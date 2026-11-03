# HTB — Redeemer

> Plataforma: Hack The Box · Categoría: Redis / Enumeración

## Preparación

Usamos una máquina virtual de Oracle y configuramos el VPN.

![](../_attachments/Pasted%20image%2020260715091311.png)

## Tarea 1

![](../_attachments/Pasted%20image%2020260715091611.png)

## Escaneo de puertos

Corremos `nmap -A` a la máquina para ver los puertos abiertos:

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20092257%201.png)

No nos dejó ver los puertos con el comando `-A` (aggressive), entonces procedemos a escanear todos los puertos con `nmap -A -T4 -p-`. Encontramos el puerto abierto y el servicio:

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20093638.png)
![](../_attachments/Captura%20de%20pantalla%202026-07-15%20093801.png)

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20094009.png)

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20094121.png)
![](../_attachments/Captura%20de%20pantalla%202026-07-15%20094430.png)

Visualizamos la versión en el mismo escaneo:

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20094515.png)
![](../_attachments/Captura%20de%20pantalla%202026-07-15%20094729.png)

## Explotación — conexión a Redis

Nos conectamos a la base de datos Redis con el comando `redis-cli -h <ip>`, luego obtenemos las keys, y posteriormente elegimos la flag para completar el desafío.

![](../_attachments/Captura%20de%20pantalla%202026-07-15%20095003.png)
![](../_attachments/Captura%20de%20pantalla%202026-07-15%20095058.png)

**Fin.**
