# Descripción

Esta sala está diseñada para que los usuarios se familiaricen con el Bolt CMS y cómo puede ser explotado usando Ejecución Remota de Código Autenticado. Deberá esperar al menos 3-4 minutos para que la máquina se inicie correctamente.

# Reconocimiento

![[Pasted image 20240401053111.png]]

Encontramos un servidor web, apache por default, un servidor ssh y otro servidor web, con titulo bolt. 

![[Pasted image 20240401053243.png]]

El puerto kali encontrar nada interesante, nos dirigimos a la web alojada en el puerto 8000, y vemos el código fuente, funcionalidades y revisamos cualquier información que nos ayude en el reconocimiento. 

![[Pasted image 20240401053909.png]]

Viendo la pagina podemos destacar entradas de un usuario presuntuamente administrador llamado Jake.

![[Pasted image 20240401053951.png]]

Viendo el código fuente de la pagina podemos entender que estamos tratando con un CMS llamado bolt, lo siguiente es intentar identificar la versión y las posibles vulnerabilidades existentes

![[Pasted image 20240401054343.png]]

Dentro de uno de los post de la pagina podemos encontrar una entrada del departamento de it otorgándonos la contraseña del administrador (bolt) 

![[Pasted image 20240402165456.png]]
En la descripción de la maquina nos han mencionado el uso de ejecución remota de comando autenticándonos con un usuario y contraseña, buscaremos si existe algún exploit para el CMS llamado bolt

![[Pasted image 20240402161023.png]]

Veamos el script con autenticación y ejecución de comandos 

![[Pasted image 20240402161136.png]]

Viendo el script podemos ver el enlace a un presunto panel de autenticación, comprobémoslo vía web si es posible acceder, y efectivamente, podemos ver el panel de autenticación, intentaremos entrar con las credenciales bolt:boltadmin123( podemos encontrar esta contraseña navegando por la web)

![[Pasted image 20240402161810.png]]

![[Pasted image 20240402171255.png]]

Probaremos de utilizar metasploit, con las credenciales obtenidas primero buscaremos algún script para bolt 

![[Pasted image 20240402170827.png]]

Utilizaremos el que hemos visto anteriormente autenticación con RCE, mostramos las opciones del script y estableceremos los parámetros de nuestra maquina victima

![[Pasted image 20240402171350.png]]

Lanzamos el script, y obtenemos una consola como root, ahora buscaremos las banderas y detalles del sistema

![[Pasted image 20240402171838.png]]

Dentro de la carpeta home, buscaremos la bandera, y listo, hemos completado la maquina.

![[Pasted image 20240402172045.png]]



