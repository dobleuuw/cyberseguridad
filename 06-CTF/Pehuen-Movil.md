# Pehuén Móvil — Android / Reversing de APK

> Curso: CyberGames_AIEP · Categoría: Reversing / Crypto / Mobile · Valor: 80

## Objetivo

Nos piden analizar la aplicación Android **Pehuén Móvil** y encontrar la flag. La pista apunta a que la app guarda información sensible dentro del propio paquete.

## Preparación

1. Nos armamos un entorno con **androguard** en un venv de Python para analizar el APK sin tener que instalarlo:
   ```
   python3 -m venv agvenv
   ./agvenv/bin/pip install androguard
   ```
   ![](../_attachments/)

## Análisis del APK

2. Un APK es solo un ZIP: dentro está el código en **DEX** (`classes.dex`). Empezamos buscando strings interesantes (credenciales, URLs, "flag", claves) directamente en el DEX:
   ```
   ./agvenv/bin/python -c "from androguard.misc import AnalyzeAPK; a,d,dx=AnalyzeAPK('pehuen.apk'); [print(s.get_value()) for s in dx.get_strings()]"
   ```

3. Revisando las clases decompiladas encontramos que la app lleva **secretos hardcodeados** en el código (no en el servidor). El desarrollador dejó la información sensible incrustada en el APK, que es justo lo que no se debe hacer.
   ![](../_attachments/)

## Resultado

**FLAG: `flag{secretos-en-el-apk-8ecc7f}`**

## Conclusión

El típico error de **hardcodear secretos en el cliente**: cualquiera que descargue el APK lo puede descomprimir y leer el código. El APK no es una caja negra, es código que viaja con el usuario. Finalizamos.
