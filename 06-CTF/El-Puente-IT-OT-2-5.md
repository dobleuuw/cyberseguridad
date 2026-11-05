# El Puente IT/OT 2/5 — Pivote y Escalamiento en Linux

> Curso: CyberGames_AIEP · Categoría: TI/OT Maqueta Smartcity CUADRANTE 1 · Valor: 250

## Objetivo

Nos piden saltar desde la red IT hacia la red OT a través de un servidor **Ubuntu** que actúa como puente (**10.10.10.20**). Nos entregan una clave SSH del usuario local **monitoreo**. La misión es entrar, investigar los permisos y conseguir **root**.

## Acceso inicial

1. La clave `id_rsa_monitoreo.txt` venía con **saltos de línea de Windows (CRLF)**, por eso SSH la rechazaba con "invalid format". La limpiamos:
   ```
   tr -d '\r' < id_rsa_monitoreo.txt > id_rsa_mon
   chmod 600 id_rsa_mon
   ```

2. Nos conectamos al puente:
   ```
   ssh -i id_rsa_mon monitoreo@10.10.10.20
   ```
   Entramos como **monitoreo@ubuntubridge**.
   ![](../_attachments/)

## Enumeración de privilegios

3. Revisamos sudo, SUID y capabilities. **No había sudo sin contraseña** ni binarios SUID raros. El `sudo` era versión **1.9.15p5**, pero al revisar el changelog el parche de **CVE-2025-32463** ya estaba aplicado, así que ese vector quedó descartado.

4. Revisando los procesos y archivos de otros usuarios, encontramos que el usuario **operador** tenía una sesión activa ejecutando:
   ```
   ssh -i /tmp/key2 -o StrictHostKeyChecking=no root@127.0.0.1
   ```
   O sea, operador se loguea como **root por clave** en la misma máquina.

5. Y lo importante: en `/tmp` había archivos de operador **legibles por todos**:
   ```
   -rw-rw-r-- operador operador /tmp/key1
   -rw-rw-r-- operador operador /tmp/_rk
   ```
   ![](../_attachments/)

## Explotación

6. Al abrir `/tmp/key1` vemos que es una clave privada SSH, pero **con las líneas invertidas** (empieza por `-----END...` y termina en `-----BEGIN...`). El comentario de la clave decodificaba a **root@ubuntubridge**, así que es la clave de root ofuscada.

7. La reconstruimos invirtiendo el orden de las líneas con **`tac`**:
   ```
   tac /tmp/key1 > root_key
   chmod 600 root_key
   ssh-keygen -y -f root_key    # validamos que la clave quedó bien
   ```

8. Nos conectamos como **root**:
   ```
   ssh -i root_key root@10.10.10.20
   id   # uid=0(root) gid=0(root)
   ```
   ![](../_attachments/)

## Obtención de la flag

9. Leemos la flag en el home de root:
   ```
   cat /root/flag_2.txt
   ```

**FLAG: `ciberlab{acl_abus3_gr4nted}`**

## Conclusión

La escalada no fue por un exploit, fue por **malos permisos (ACL abuse)**: una clave privada de root quedó legible en `/tmp` para cualquier usuario, solo ofuscada invirtiendo las líneas. Con reconstruirla y reutilizarla pasamos de `monitoreo` a `root`. Este puente es ahora nuestra entrada a la red OT (172.16.0.0/24). Finalizamos.
