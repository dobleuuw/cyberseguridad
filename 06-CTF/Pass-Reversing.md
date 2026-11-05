# Pass — Reversing

> Curso: CyberGames_AIEP · Categoría: Reversing / Crypto / Mobile · Valor: 90

## Objetivo

Nos piden encontrar el número correcto que valida el binario **ex32_pass.elf** (ELF 64-bit, PIE, **stripped**). La flag es ese número.

## Análisis

1. Al estar **stripped** no tenemos nombres de funciones, así que ocupamos **radare2** para analizar:
   ```
   r2 -A ex32_pass.elf
   pdf @ fcn.00001249        # función de chequeo
   ```
   ![](../_attachments/)

2. Leyendo el ensamblador reconstruimos la lógica del validador. El binario:
   - Toma nuestro número **N** y calcula el **MD5** de su representación decimal (`str(N)`).
   - Tiene una **clave** fija, que son los bytes (little-endian) de la suma de dos constantes:
     ```
     key = 0x25b6f31479e939ad + 0xa4b69adc59b31559
     ```
   - Calcula un acumulador:
     **`Int = ( Σ  key[i] × MD5(str(N))[i] )  mod 65536`**
   - Acepta cuando **`Int == 0xe850`** (59472).

## Resolución

3. Como la entrada es un número y la validación es determinista, lo resolvemos por **fuerza bruta** replicando el algoritmo en Python:
   ```python
   import hashlib, struct
   key = (0x25b6f31479e939ad + 0xa4b69adc59b31559).to_bytes(16, "little")
   for N in range(0, 1_000_000):
       h = hashlib.md5(str(N).encode()).digest()
       acc = sum(key[i] * h[i] for i in range(len(key))) % 65536
       if acc == 0xe850:
           print("N =", N); break
   ```

4. El número que cumple la condición es:

**FLAG: `56026`**

## Conclusión

Detrás del "password" había un **checksum basado en MD5**: el binario no comparaba contra una clave guardada, sino que validaba una operación matemática sobre el hash del número. Reconstruyendo el algoritmo en ensamblador y replicándolo en Python, encontramos el valor correcto por fuerza bruta en segundos. Finalizamos.
