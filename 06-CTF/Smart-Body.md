# Smart-Body — Android / Bypass de control de acceso

> Curso: CyberGames_AIEP · Categoría: Reversing / Crypto / Mobile · Valor: 80

## Objetivo

Nos piden acceder al contenido **premium** de la app **Smart-Body** sin pagar el plan. La app distingue entre usuario normal y usuario "full".

## Análisis del APK

1. Descompilamos el APK y revisamos cómo la aplicación decide si tienes plan full. Encontramos que la validación es **del lado del cliente**: la app manda **cabeceras HTTP** que le dicen al servidor qué plan tiene el usuario, en vez de que el servidor lo verifique.

2. Las cabeceras relevantes que ocupa la app son:
   ```
   X-Plan: ...
   X-App-Key: ...
   ```
   ![](../_attachments/)

## Explotación

3. Como el chequeo está en el cliente, nosotros mismos armamos la petición cambiando la cabecera del plan a **full**, saltándonos el pago:
   ```
   curl -s https://<endpoint-smartbody>/... \
     -H "X-Plan: full" \
     -H "X-App-Key: <clave-del-apk>"
   ```

4. El servidor confía en la cabecera y nos devuelve el contenido premium con la flag.
   ![](../_attachments/)

## Resultado

**FLAG: `flag{plan-full-sin-pagar-fed746}`**

## Conclusión

Clásico **bypass de control de acceso del lado del cliente** (broken access control): la autorización nunca debe depender de un valor que controla el cliente. Como la app solo mandaba `X-Plan: full` por confianza, reproducir la petición con esa cabecera nos dio acceso al plan premium sin pagar. Finalizamos.
