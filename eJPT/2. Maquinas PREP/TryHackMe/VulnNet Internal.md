# Descripción

VulnNet Entertainment es una empresa que aprende 
de sus errores. Rápidamente se dieron cuenta de que no podían hacer una aplicación web
 aplicación web segura, así que renunciaron a esa idea. En su lugar 
decidieron establecer servicios internos para propósitos de negocios. Como de costumbre, 
tienes la tarea de realizar una prueba de penetración de su red y reportar 
tus hallazgos. 
Dificultad: Fácil/Media
Sistema Operativo: Linux
Esta máquina máquina fue diseñada para ser todo lo contrario de las máquinas anteriores 
en esta serie y se centra en los servicios internos. Se supone que información interesante y usarla para obtener acceso al sistema. acceso al sistema. Reporta tus hallazgos enviando las banderas correctas. 
Nota: Todos los servicios pueden tardar entre 3 y 5 minutos en arrancar.

# Reconocimiento

nmap --min-rate 4000 -sCV -Pn -n -p- --open 10.10.221.8 -oN tcp_scan.txt

![[Pasted image 20240413170458.png]]
# Acceso inicial


# Escalada de privilegios
