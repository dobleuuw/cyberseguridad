# AIEP — Informe Prueba 2

> Curso: AIEP · Tipo: Informe de evaluación · Fecha: 2026-09-30

![[Screenshot_2026-09-30_10_05_35.png]]
![[Screenshot_2026-09-30_09_53_11.png]]

## 1. ¿Cuál es el banner exacto del software y la versión que devuelve el servicio FTP?

![[Screenshot_2026-09-30_09_48_04.png]]

Llegamos a la conclusión de que el puerto FTP 21 está cerrado; por ende vemos los servicios y tenemos el 2121 vsftpd.

![[Screenshot_2026-09-30_09_50_05.png]]

Procedemos a escanear el puerto 2121: obtenemos el banner y versión OS: **unix vsftpd 3.0.2**.

![[Screenshot_2026-09-30_09_53_11 1.png]]

## 2. Escanea el servicio Redis: ¿Cuál es la versión? ¿OS? ¿Role?

Tiramos el script de redis en nmap:

![[Screenshot_2026-09-30_10_05_35 1.png]]

- **Versión:** 5.0.14
- **OS:** Linux 7.1.5+kali-amd64 x86_64
- **Role:** master

## 3. ¿Cuál es el 'Protocol' y 'Salt' del servicio de DB?

![[Screenshot_2026-09-30_10_15_15.png]]

Tiramos un scan general; intenté buscar scripts pero no hay para MySQL o no los encontré.

- **Protocol:** 10
- **Salt:** `4\x01\x1D\x08\x0F\x01\x17%]J@o\x11_\x15e9\x03`

## 4. ¿Cuál es el nivel de severidad asignado a la vulnerabilidad Gateway Web?

![[Screenshot_2026-09-30_10_56_19.png]]

Según Nessus, es la única vulnerabilidad encontrada en el Gateway Web, con un nivel de **6.5**.

## 5. ¿Qué CVE y puntuación CVSS tiene?

- **CVE-2025-9293:** TLS Certificate Validation Vulnerability
- **CVSS:** 6.5

## 6. ¿En qué consiste la solución sugerida para reparar la vulnerabilidad de Redis?

Redis es una base de datos en memoria de código abierto que persiste en el disco. Desde la versión 2.8 hasta versiones anteriores a la 8.0.3, 7.4.5, 7.2.10 y 6.2.19, un usuario autenticado podía usar una cadena especialmente manipulada para activar una escritura fuera de los límites de pila/montón en operaciones de hyperloglog, lo que podría provocar la ejecución remota de código. Es probable que el error afecte a todas las versiones de Redis con operaciones de hyperloglog implementadas. Esta vulnerabilidad se corrigió en las versiones 8.0.3, 7.4.5, 7.2.10 y 6.2.19. Otra solución alternativa para mitigar el problema sin parchear el ejecutable redis-server es impedir que los usuarios ejecuten operaciones de hyperloglog, lo que se puede lograr mediante ACL para restringir los comandos HLL.

![[Screenshot_2026-09-30_11_04_42.png]]

## 7. ¿Qué fecha/hora absoluta devuelve la cabecera 'Date' HTTP del servidor de aplicaciones?

![[Screenshot_2026-09-30_11_06_09.png]]
