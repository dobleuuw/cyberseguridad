
Usamos una maquina virtual de Oracle y configuramos el VPN
![](cyberseguridad/Imagenes/Pasted%20image%2020260715091311.png)

Tarea 1
![](cyberseguridad/Imagenes/Pasted%20image%2020260715091611.png)

Corremos nmap -A a la maquina para ver los puertos abiertos:
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20092257%201.png)
No nos dejó ver los puertos con el comando -A agressive entonces procedemos a escanear todos los puertos con nmap -A -T4 -p-, encontramos el puerto abierto y el servicio:
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20093638.png)
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20093801.png)

![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20094009.png)

![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20094121.png)
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20094430.png)

Visualizamos la versión en el mismo escaneo:

![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20094515.png)
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20094729.png)
Nos conectamos a la base de datos Redis con el comando redis-cli -h ip, luego obtenermos las keys, y posterior eligimos la flag, para completar el desafio.

![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20095003.png)
![](cyberseguridad/Imagenes/Captura%20de%20pantalla%202026-07-15%20095058.png)
Fin.
