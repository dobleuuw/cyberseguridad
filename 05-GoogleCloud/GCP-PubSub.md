# GCP — Pub/Sub

> Plataforma: Google Cloud · Categoría: Pub/Sub (mensajería)

1. Creamos un ambiente virtual:

![](../_attachments/Pasted%20image%2020260527201958.png)

2. Verificamos:

![](../_attachments/Pasted%20image%2020260527202014.png)

3. Construimos el build y activamos:

![](../_attachments/Pasted%20image%2020260527202103.png)

4. Instalamos la librería con `pip install --upgrade google-cloud-pubsub`:

![](../_attachments/Pasted%20image%2020260527202140.png)

5. Obtenemos el código de prueba clonando el repositorio de GitHub:

![](../_attachments/Pasted%20image%2020260527202206.png)

6. Hacemos `echo $GOOGLE_CLOUD_PROJECT`:

![](../_attachments/Pasted%20image%2020260527202239.png)

7. Corremos el publisher script para crear un Pub/Sub:

![](../_attachments/Pasted%20image%2020260527202317.png)

8. Verificamos que hayamos creado `MyTopic`:

![](../_attachments/Pasted%20image%2020260527202345.png)

9. Creamos una suscripción:

![](../_attachments/Pasted%20image%2020260527202419.png)

10. Publicamos "Hello" en el Topic:

![](../_attachments/Pasted%20image%2020260527202454.png)

11. Creamos otro mensaje:

![](../_attachments/Pasted%20image%2020260527202524.png)

12. Creamos otro mensaje más debajo del anterior:

![](../_attachments/Pasted%20image%2020260527202603.png)

13. Otro más:

![](../_attachments/Pasted%20image%2020260527202627.png)

14. Vemos los mensajes con `python subscriber.py $GOOGLE_CLOUD_PROJECT receive MySub`:

![](../_attachments/Pasted%20image%2020260527202726.png)
