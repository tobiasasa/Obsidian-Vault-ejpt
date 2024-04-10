
# Descripción

Billy Joel ha creado un blog en el ordenador de su casa y ha empezado a trabajar en él.  ¡Va a ser alucinante!
Enumera
 ¡esta caja y encuentra las 2 banderas que se esconden en ella!  Billy tiene algunas 
cosas raras en su portátil.  ¿Puedes maniobrar y conseguir lo que 
lo que necesitas?  O caerás en la madriguera del conejo...
Para que el blog funcione con AWS, tendrás que añadir MACHINE_IP blog.thm a tu archivo /etc/hosts.

Traducción realizada con la versión gratuita del traductor DeepL.com
# Reconocimiento

Añadimos blog.thm a la archivo /etc/hosts ya que dice correr en virtual hosting.

## Escaneo de maquina

![[Pasted image 20240325183132.png]]

Con el escaneo, en la cabezera http-generator se nombra el CMS, wordpress y la versión 5.0, además del samba abierto, también detecta al usuario guest, dentro del samba. 

Ingresamos a la web en busca de información y navegando un poco, detectamos dos potenciales usuarios, haciendo hover sobre los autores de los post podemos encontrar dos usuarios:

![[Pasted image 20240325183326.png]]
![[Pasted image 20240325183626.png]]

Usuario 1: kwheel
Usuario 2: bjoel

Al saber que tratamos con un wordpress, ejecutaremos un ataque de diccionario contra el panel de administración utilizando wpscan, y veremos si podemos encontrar credenciales validas, para nuestros alguno de nuestros dos usuarios

Crearemos la lista unicamente con esas dos entradas llamada users.txt

![[Pasted image 20240325184617.png]]

Obtenemos acceso con kwhel:cutiepie1 y esperamos a que siga realizando el ataque de fuerza bruta.

![[Pasted image 20240325185131.png]]

Intentamos probar las credenciales dentro de blog.htm/wp-admin, y conseguimos acceso con kwheel al dashboard

![[Pasted image 20240325185405.png]]

Enumeraremos también el samba, para ver si es posible ver algún archivo, sin uso de credenciales, pero podemos ver que no hay mucho

![[Pasted image 20240325190408.png]]
![[Pasted image 20240325195409.png]]
# Acceso inicial

Es necesario subir un archivo malicioso, pero si lo intentamos por los medios convenciones, el filtro de archivos lo bloquea, lo que me hizo listar la vulnerabilidades en busquead de algún exploit para ingresar WordPress Core 5

![[Pasted image 20240325192730.png]]

Utilizare la opcion 0, multi httpp wp crop rce

![[Pasted image 20240325193445.png]]

Ahora intentaremos buscar dentro de home la flag de user:

![[Pasted image 20240325193544.png]]

Accedemos a una shell y hacemos un tratamiento de la tty
Y podemos ver dentro del archivo de configuración del wordpress credenciales hardcodeada

![[Pasted image 20240325200018.png]]

Ingresamos dentro de la base de datos con las credenciales y buscamos dentro de wp_users

![[Pasted image 20240325200300.png]]
![[Pasted image 20240325200333.png]]

Encontramos dos entradas: 

ID 1 bjoel \$P\$BjoFHe8zIyjnQe/CBvaltzzC6ckPcO/
ID 3 kwheel \$P\$BedNwvQ29vr1TPd80CDl6WnHyjr8te.

Intentaremos determinar el tipo de cifrado/encriptado que tiene la contraseña utilizando hashid

![[Pasted image 20240325200853.png]]

# Escalada de privilegios
Dentro de los archivos con privilegios encontramos /usr/sbin/checker, al usarlo nos dice que es imposible, pero intentando con ltrace, podemos ejecutar el binario, y este nos da como salida un enviorment de administrador exportado la variable admin

![[Pasted image 20240325203350.png]]![[Pasted image 20240325204059.png]]

Habiendo ubicado el archchivo suid, luego, lo abriremos con ltrace, y exportaremos la variable admin, de esta manera obtendremos acceso de root al sistema.

![[Pasted image 20240325205326.png]]