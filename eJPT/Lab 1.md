Se le proporciona una máquina Kali GUI y una máquina objetivo. La máquina objetivo está ejecutando un Firewall de Windows. 

Su tarea es descubrir los hosts activos disponibles y sus puertos abiertos utilizando Nmap e identificar los servicios y aplicaciones en ejecución.

Instrucciones:

Su máquina Kali tiene una interfaz con dirección IP 10.10.X.Y. Ejecute "ip addr" para conocer los valores de X e Y.
La dirección IP de la máquina objetivo se menciona en el archivo "/root/Desktop/target".
No atacar la puerta de enlace situada en la dirección IP 192.V.W.1 y 10.10.X.1

![[Pasted image 20240325154725.png]]


nmap --open -p- -Pn -n -sCV -T4 -f 10.2.17.47 -v -oN full_scan_tcp.txt

