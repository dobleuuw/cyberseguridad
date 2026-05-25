Creamos un ambiente virtual:

![[Pasted image 20260521135443.png]]

Verificamos
![[Pasted image 20260521135524.png]]
Construimos el Build y Activamos:
![[Pasted image 20260521135619.png]]
Instalamos la Libreria con pip install --upgrade google-cloud-pubsub
![[Pasted image 20260521135709.png]]
Obtenemos el código de prueba clonando el repositorio de GitHub:
![[Pasted image 20260521135808.png]]
Creamos el Echo GOOUD_PROJECT
![[Pasted image 20260521135907.png]]
Corremos el Publisher script para crear un Pub/Sub:
![[Pasted image 20260521140026.png]]
Verificamos que hayamos creado MyTopic:
![[Pasted image 20260521140231.png]]
Creamos una Subscripción:
![[Pasted image 20260521140318.png]]

Publicamos "Hello" en el Topic:
![[Pasted image 20260521140408.png]]

Creamos otro mensaje:
![[Pasted image 20260521140448.png]]
Ponemos otro mensaje que va debajo:
![[Pasted image 20260521140522.png]]

Otro más:
![[Pasted image 20260521140539.png]]

Vemos los mensajes:
python subscriber.py $GOOGLE_CLOUD_PROJECT receive MySub

![[Pasted image 20260521140611.png]]
