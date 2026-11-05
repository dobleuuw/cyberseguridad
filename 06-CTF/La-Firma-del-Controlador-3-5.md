# La Firma del Controlador 3/5 — Modbus / OT

> Curso: CyberGames_AIEP · Categoría: TI/OT Maqueta Smartcity CUADRANTE 1 · Valor: 500

## Objetivo

Nos piden descubrir la ubicación del **PLC** dentro de la red industrial, interrogar el servicio Modbus e **identificar la firma del dispositivo**. Trabajamos desde el puente Ubuntu (10.10.10.20) donde ya teníamos root.

## Ubicación del PLC

1. En el `/root` del puente había capturas de tráfico ICS. Las analizamos con **tcpdump** buscando tráfico Modbus (puerto **502**):
   ```
   tcpdump -nn -r captura_ics.pcap "tcp port 502"
   ```
   Identificamos que el puente conversa con **172.16.0.25:502** → ese es el **PLC** (red OT 172.16.0.0/24, alcanzable vía PfSense 10.10.10.100).
   ![](../_attachments/)

2. Confirmamos que el puerto está abierto:
   ```
   bash -c 'cat </dev/null >/dev/tcp/172.16.0.25/502 && echo ABIERTO'
   ```

3. De una trama real del pcap sacamos el **Unit ID = 0x01** (ocupaban un `Write Single Register` FC 0x06).

## Interrogación del servicio (firma)

4. Para sacar la firma del dispositivo ocupamos la función Modbus **Read Device Identification** (FC **0x2B** / MEI **0x0E**), que devuelve los campos de identificación del fabricante. Armamos un pequeño cliente en Python que arma la trama MBAP + PDU y pide los objetos básicos (VendorName, ProductCode, Revision):
   ```python
   pdu = bytes([0x2B, 0x0E, 0x01, 0x00])          # FC, MEI, ReadDevIDCode=basico, ObjectID=0
   frame = struct.pack(">HHHB", 1, 0, len(pdu)+1, 1) + pdu
   ```
   ![](../_attachments/)

5. El PLC nos responde con su **firma**:
   ```
   [0] VendorName        : ciberlab{m0dbus_d1sc0v3r_1d3nt1f13d}
   [1] ProductCode       : f3lip3.operador:dl@QvNH@Skt0fhg29i
   [2] MajorMinorRevision: 1.0.0
   ```

## Resultado

**FLAG: `ciberlab{m0dbus_d1sc0v3r_1d3nt1f13d}`** (campo **VendorName**)

Además, de regalo el **ProductCode** filtró una credencial que ocuparemos en el siguiente desafío:
**`f3lip3.operador : dl@QvNH@Skt0fhg29i`**

## Conclusión

Los equipos industriales no se anuncian como los servicios IT: hay que mirar el **tráfico Modbus** para encontrarlos. Una vez ubicado el PLC, el protocolo Modbus no tiene autenticación, así que con una consulta estándar (**Read Device Identification**) obtenemos la firma del controlador y, en este caso, hasta una credencial filtrada en los campos de identificación. Finalizamos.
