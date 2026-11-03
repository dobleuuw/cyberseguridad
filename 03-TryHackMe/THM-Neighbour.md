# THM — Neighbour

> Plataforma: TryHackMe · Categoría: Web

## Tarea 1

Descubre nuestro nuevo servicio en la nube, "Autenticación en Cualquier Lugar": ¡inicia sesión desde donde quieras! Los usuarios pueden ingresar su nombre de usuario y contraseña para un proceso de inicio de sesión totalmente seguro. Seguro que no encontrarás ningún secreto en el perfil de otras personas, ¿verdad?

Revisamos el código fuente:

![](../_attachments/Pasted%20image%2020260527201621.png)

Cambiamos `guest->` por `admin` para entrar al usuario viendo el código fuente.
