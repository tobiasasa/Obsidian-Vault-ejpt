
# Descripción

Una máquina de nivel fácil con múltiples formas de escalar privilegios.
# Reconocimiento

![[Pasted image 20240404032817.png]]

Se puede apreciar un servicio apache de version 2.4.18, que parece ser un wordpress, y un servicio OpenSSH 7.2p2, comencemos navegando por la web

![[Pasted image 20240404034012.png]]

También vemos un comentario de un tal Hott

![[Pasted image 20240404034136.png]]

Dado que estamos trabajando con el cms de wordpress, lo próximo seria intentar enumerar con wpscan todo lo que sea posible, para intentar tener credenciales

![[Pasted image 20240404042421.png]]

Dentro de los detalles que nos da wpscan podemos destacar la información de listado de usuarios.

![[Pasted image 20240404042505.png]]

Guardaremos los usuarios en un archivo llamado users.txt para poder usarlos posteriormente

![[Pasted image 20240404042936.png]]

Intenteremos conseguir la contraseña de alguno de estos usuarios utilizando fuerza de diccionario con rockyou
`wpscan -U users.txt -P /usr/share/wordlists/rockyou.txt --url http://10.10.165.98/`

Lanzamos script, y conseguimos credenciales validas

![[Pasted image 20240404045340.png]]

Ingresaremos desde /wp-login.php con las credenciales que nos han propocionado

![[Pasted image 20240404045613.png]]

# Acceso inicial wp php theam

He intentando crear un post, para intentar subir un archivo php, pero esta sanetizado, podriamos intentar agregar codigo php de una shell inversa, editare el archivo header.php

Utilizare la reverse php shell de pentestmonkey https://github.com/pentestmonkey/php-reverse-shell/

![[Pasted image 20240404053524.png]]

Cambiamos los valores de la ip y el puerto, y ponemos en escucha con netcat, en el puerto 1337, guardamos los cambios y al abrir el sitio web, tenemos acceso al sistema.

![[Pasted image 20240404053733.png]]

Viendo puertos a la escucha y conexiones tcp podemos ver, el servicio 4512, correspondiente al ssh, una base de datos en el 3306, para ver las credenciales podriamos ver el archivo wp-config.php que se encuentra en /etc/www/html 

![[Pasted image 20240404054443.png]]

![[Pasted image 20240404054755.png]]

Podemos ver informacion relevante: 
base de datos  colddbox
usuario c0ldd
contraseña cybersecurity

Nos conectaremos a la base de datos con las credenciales para ver todo el contenido, y posibles contraseñas de usuarios


![[Pasted image 20240404055218.png]]

![[Pasted image 20240404055249.png]]

c0ldd `$P$BJs9aAEh2WaBXC2zFhhoBrDUmN1g0i1`
hugo `$P$B2512D1ABvEkkcFZ5lLilbqYFT1plC/`
philip `$P$BXZ9bXCbA1JQuaCqOuuIiY4vyzjK/Y.`

Obtendremos las contraseñas para ver si ha reutilizado contraseñas a nivel de sistema, en esta caso usare john the ripper

![[Pasted image 20240404061034.png]]

c0ldd:9876543210
hugo:password123456

Probamos la credenciales para el usuario c0ldd pero no son validos, probando todas las credenciales que hemos obtenido hasta ahora, c0ldd reutilizo la contraseña de la base de datos a su usuario de sistema

c0ldd:cybersecurity

![[Pasted image 20240404071244.png]]

# Escalada de privilegios

Conseguimos la primera bandera, deberemos conseguir acceso al usuario root, para eso usare GTFObins, primero listaremos los permisos que puede ejecutar como root el usuario c0ldd

![[Pasted image 20240404071645.png]]

![[Pasted image 20240404071713.png]]

Siguiendo los pasos de GTFObins, podemos ver que obtenemos acceso al usuario root desde ftp.

![[Pasted image 20240404071759.png]]

![[Pasted image 20240404072007.png]]
![[Pasted image 20240404072609.png]]

Cualquiera de las dos opciones nos permite obtener usuario root