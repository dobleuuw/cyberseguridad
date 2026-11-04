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

6. **Cuantos servicios están corriendo en el puerto con MQTT**: según mi lab no hay servicios corriendo en el puerto con MQTT puerto 58080.

7. **Cual es la flag que se encuentra explotando la vulnerabilidad SQLi.**: abrimos la web e ingresamos en Usuario el SQLi ' OR 1=1-- - con clave "x", procedemos a acceder con SuperAdmin y encontramos la flag.
   ![](../_attachments/Pasted%20image%2020261104115121.png)
**Rol: SuperAdmin**

**FLAG{SQLI_AUTH_BYPASS_FOUND}**

9. **Cual es la flag que se encuentra explotando la vulnerabilidad de Path**  
**Traversal. (etc/flag.txt).**
   
   Intentamos ingresar el Path Traversal ../../../etc/flag.txt![](../_attachments/Pasted%20image%2020261104115536.png)
![](../_attachments/Pasted%20image%2020261104120355.png)

9. Obtenemos la flag.![](../_attachments/Pasted%20image%2020261104115846.png)


10. Cuál es la flag que se encuentra explotando la vulnerabilidad SSRF.  
(srv_internal_api:8888):

