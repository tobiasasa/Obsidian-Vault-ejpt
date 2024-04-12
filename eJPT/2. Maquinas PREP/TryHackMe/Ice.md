# Descripción

¡Conéctate a la red TryHackMe! Tenga en cuenta que esta máquina no responde al ping (ICMP) y puede tardar unos minutos en arrancar.

# Reconocimiento

Comenzamos con un script de reconocimiento en búsqueda de versiones y servicios 

![[Pasted image 20240411130838.png]]
![[Pasted image 20240411130921.png]]

Podemos ver que estamos frente un OS, windows, podemos contemplar el nombre de un host llamado DARK-PC, y encontramos el puerto 135, 139, 445, 3389, 5357, 8000, 49153, 49154, 49159, 49160

Listando el samba podemos ver que no es accesible pero igualmente podemos identificar un host conectado llamado DARK-PC, corriendo Windows 7.
Podríamos ver si es vulnerable a eternal blue, pero antes revisare los servicios http, de los puertos  5357 y 8000.

La web por el puerto 5357 parece no ser accesible. 

![[Pasted image 20240411131537.png]]

Y en el servicio web del puerto 8000, nos dice que no se encuentra el recurso disponible

![[Pasted image 20240411131712.png]]

Por lo que intentaremos hacer una ataque de diccionario para listar subdirecotrios, con dirb, no encontramos nada, probaremos si es vulnerable a eternal blue.

![[Pasted image 20240411134200.png]]
![[Pasted image 20240411134329.png]]

Podemos confirmar que es vulnerable a MS17-010
# Acceso inicial

Explotaremos con el modolo de metasploit ms17-010, el objetivo, y crearemos un shell para poder interactuar con el sistema windows, podemos confirmar que somos el usuario con privilegios maximos. 

![[Pasted image 20240411134757.png]]

# Escalada de privilegios


Otro método sería verificar el servicio, que corre en el puerto 8000, que es vulnerable a un bufferoverflow, que permite ejecución remota de comandos, el servicio llamado ICECAST

![[Pasted image 20240411140116.png]]

Buscamos en metasploit, y podemos encontrar el mismo exploit disponible, un bufferoverflow en el header.

![[Pasted image 20240411140450.png]]

![[Pasted image 20240411140725.png]]

Revisaremos los privilegios, y usaremos el modulo de post explotación de metasploit

![[Pasted image 20240411142609.png]]

getprivs para visualizar los privilegios de DARK-PC

Y luego ejecutaremos el modulo para que nos sugiera exploits para escalar privilegios en la etapa de la post explotación

run post/multi/recon/local_exploit_suggester

![[Pasted image 20240411142802.png]]

Encontramos 10 exploits validos para escalar privilegios, dentro de el usuario DARK-PC, utilizaremos el exploit exploit/windows/local/bypassuac_eventvwr, para escalar privilegios 

![[Pasted image 20240411143247.png]]

Revisamos nuevamente los privilegios del usuario dark y podemos ver que en la segunda sessión del meterpreter, tenemos todos los privilegios

![[Pasted image 20240411143554.png]]

Listamos todos los procesos que corre el sistema, migramos la proceso spoolscv

![[Pasted image 20240411144332.png]]

![[Pasted image 20240411144601.png]]![[Pasted image 20240411144629.png]]


# Post Explotación

Cargamos el modulo de kiwi para poder utilizar mimikatz e interactuar con las credenciales del sistema

![[Pasted image 20240411144947.png]]

Hemos obtenido las credenciales de dark-pc, intentemos dumpear los hashes para verlos todo e intentar hacer una posible fuerza bruta. pero con eso finaliza la maquina.

![[Pasted image 20240411145128.png]]
