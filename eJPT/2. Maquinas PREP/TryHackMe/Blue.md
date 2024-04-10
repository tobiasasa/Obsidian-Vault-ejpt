# Descripción

Puede y aprende a qué exploit es vulnerable esta máquina. Tenga en cuenta que esta máquina no responde al ping (ICMP) y puede tardar unos minutos en arrancar. Esta sala no pretende ser un CTF de boot2root, sino una serie educativa para principiantes. Los profesionales probablemente obtendrán muy poco de esta sala más allá de la práctica básica, ya que el proceso aquí está pensado para principiantes. 

# Reconocimiento

Podemos ver un Windows 7 Profesional, un puerto de escritorio remoto que nos muestra la información de un dispositivo con el nombre JON-PC, podríamos probar ver si el sevidor windows 7 es vulnerable a MS17-010 (EternalBlue)

msf6 > use auxiliary/scanner/smb/smb_ms17_010


![[Pasted image 20240326152349.png]]

Podemos confirmar que el objetivo es vulnerable a eternalblue

# Acceso inicial

Buscamos un exploit para eternalblue, en este caso usare el windows/smb/ms17_010_eternalblue, podríamos buscar otras utilizando el comando search eternalblue



use windows/smb/ms17_010_eternalblue

![[Pasted image 20240326153118.png]]

Ahora podremos ver las especificaciones del payload, y el modulo, ponemos los valores correspondientes a nuestra maquina vicitima.

![[Pasted image 20240326153153.png]]

Vemos nuestra ip de atacante ip a

![[Pasted image 20240326153348.png]]

Con todos los parámetros listos lanzaremos el ataque, estableciendo la ip vicitma y nuestra ip de atacante.

![[Pasted image 20240326153550.png]]

y lanzaremos el exploit obteniendo una sesión de meterpreter

![[Pasted image 20240326153906.png]]

Encontrando el usuario del sistema, y crackeando su contraseña.

