# Prime — Reversing

> Curso: CyberGames_AIEP · Categoría: Reversing / Crypto / Mobile · Valor: 90

## Objetivo

Nos piden encontrar el **menor input decimal** que haga "crashear" el programa. La flag es ese número. Nos entregan `ex34_prime.elf` (ELF 64-bit, PIE, **no stripped**).

## Análisis

1. Al estar **no stripped**, vemos las funciones directo:
   ```
   nm ex34_prime.elf    ->   check_input , main
   ```

2. Desensamblamos `check_input` con objdump. La lógica es una validación matemática:
   ```
   rax = input * 0x3039        (12345 * x)
   rcx = rax + 0x1a85          (+ 6789)
   rax = rcx mod 0xfff1        (mod 65521, con truco de división por constante)
   cmp rax, 0x3e95             (compara con 16021)
   ```
   O sea, el programa acepta cuando:
   **`(12345 · x + 6789) mod 65521 == 16021`**

   El "crash/fuzz" del enunciado es solo temático: no hay segfault, es una validación.

## Resolución

3. No hace falta fuzzear nada: lo resolvemos con **inverso modular** (65521 es primo, así que 12345 es invertible):
   ```python
   M = 65521
   tgt = (16021 - 6789) % M
   x = (tgt * pow(12345, -1, M)) % M     # -> 52667
   ```

4. Verificamos ejecutando el binario:
   ```
   ./ex34_prime.elf 52667   ->  Good way / Congratulations!   (exit 0)
   ./ex34_prime.elf 52666   ->  Bad way / Try again!          (exit 1)
   ```
   ![](../_attachments/)

## Resultado

**FLAG: `52667`**

## Conclusión

Leyendo el ensamblador identificamos que la "condición de crash" era en realidad una **congruencia lineal**. En vez de probar millones de números por fuerza bruta, la resolvimos de forma directa con inverso modular. Finalizamos.
