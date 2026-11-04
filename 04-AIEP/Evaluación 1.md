1. Primero empezamos con un escaneo nmap básico de todo con 
   nmap -p- 127.0.0.1
   ![](../_attachments/Pasted%20image%2020261104110332.png)
2. Identificamos los servicios activos. 

3. Nos piden lo siguiente: **Cual es el banner completo del servicio Sendmail**, entonces ocuparemos el nmap con -sC para ocupar scripts, -sV para detectar versiones y -p para los puertos activos, encontramos que Sendmail está en el puerto 24444:
   ![](../_attachments/Pasted%20image%2020261104110855.png)

4. Luego de identificar el puerto donde Sendmail está, nos conectamos a través de netcat, y verificamos que la respuesta nos entregue el banner:
   ![](../_attachments/Pasted%20image%2020261104111140.png)
   Identificamos el **Banner: 220 mail.corp-internal.lan**

5. **Cual es el http-title del puerto con la página web**, identificamos el http-tittle con el mismo scan anterior. **http-title: Panel de Monitorizaci\xC3\xB3n y M\xC3\xA9tricas**


![](../_attachments/Pasted%20image%2020261104111445.png)

6. **Cuantos servicios están corriendo en el puerto con MQTT** 