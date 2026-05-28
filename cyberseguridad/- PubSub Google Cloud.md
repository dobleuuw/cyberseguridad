1)Creamos un ambiente virtual:

![](Imagenes/Pasted%20image%2020260527201958.png)

2)Verificamos:

![](Imagenes/Pasted%20image%2020260527202014.png)

3)Construimos el build y activamos:

![](Imagenes/Pasted%20image%2020260527202103.png)

4)Instalamos la libreria con pip install --upgrade google-cloud-pubsub :

![](Imagenes/Pasted%20image%2020260527202140.png)

5)Obtenemos el código de prueba clonando el repositorio de GitHub:

![](Imagenes/Pasted%20image%2020260527202206.png)

6)Hacemos echo $GOOGLE_CLOUD_PROJECT:

![](Imagenes/Pasted%20image%2020260527202239.png)

7)Corremos el publisher script para crear un Pub/Sub:

![](Imagenes/Pasted%20image%2020260527202317.png)

8)Verificamos que hayamos creado MyTopic:

![](Imagenes/Pasted%20image%2020260527202345.png)

9)Creamos una subscripción:

![](Imagenes/Pasted%20image%2020260527202419.png)

10)Publicamos "Hello" en el Topic:

![](Imagenes/Pasted%20image%2020260527202454.png)

11)Creamos otro mensaje:

![](Imagenes/Pasted%20image%2020260527202524.png)

12)Creamos otro mensaje más debajo del anterior:

![](Imagenes/Pasted%20image%2020260527202603.png)

13)Otro más:

![](Imagenes/Pasted%20image%2020260527202627.png)

14)Vemos los mensajes python subscriber.py $GOOGLE_CLOUD_PROJECT receive MySub :

![](Imagenes/Pasted%20image%2020260527202726.png)




















