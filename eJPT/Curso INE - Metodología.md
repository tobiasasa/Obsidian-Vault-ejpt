![[Pasted image 20240325163023.png]]

![[Pasted image 20240325163940.png]]

# Reconocimiento Pasivo / Footprinting
## Objetivos y técnicas

| Objetivo de reconocimiento                     |
| ---------------------------------------------- |
| 1. Dirección IP                                |
| 2. Tecnologías web usadas                      |
| 3. Directorios ocultos en motores de busquedad |
| 4. Direcciones de correo                       |
| 5. Nombres                                     |
| 6. Numero de telefóno                          |
| 7. Dirección fiisica                           |

### DNS IP Lookup CLI

`host example.com
### Buscamos directorios/archivos no indexados:
`/robots.txt
### Buscamos un mapa del sitio web:
`/sitemap.xml

Categorías no indexadas en el frontend de la pagina web. 
Identificación nombre, dirección de correo, dentro de la web.
### Addons firefox/chromnium

BuiltWith: addon para ver tecnologías y plugins de gestores de contenido, subdominios.

![[Pasted image 20240321115342.png]]

### Descripción de tecnologías web cli

whatweb example.org
### Descarga de sitios web para comprender código fuente. 

HTTrack - Windows & Linux

Con esta herramienta descargaremos en local los archivos de una web, para analizarlos posteriormente.

### Footprintin WEB: Netcraft (pasivo)

website: netcraft.com
Services > Internet Data Mining

![[Pasted image 20240321131855.png]]

### Reconocimiento pasivo DNS
| Identificar información sobre un dominio |
| ---------------------------------------- |
| 1. Fecha de registro del dominio         |
| 2. Propietario del dominio               |
| 3. Registrador del dominio               |
| 4. Fecha de registro/expiración          |
| 5. Nombre del servidor/organización      |
#### CLI

dnsrecon -d example.org

Es muy importante si se realiza un investigación prestar atención a que CloudFlare realmente no oculta las direcciones de nuestro servidor de correo proxy. 

Registros A ip4
Registros AAAA ipv6
Registros TXT esencialmente utilizado para la información de diagnóstico.
Registros MX esencialmente apunta al servidor de correo que tenga el host configurado. 
Registros NS nombre de los servidores.
#### Vía web

dnsdumpster.com

#### Protocolo Whois

Es esencialmente un protocolo de consulta y respuesta que se usa ampliamente para consultar bases de datos que almacenan los usuarios registrados o un proveer de información en un determinado recurso de internet, como un nombre de dominio, un bloque de direcciones IP o un sistema autónomo. 

#### Whois CLI

whois hackersploit.org

El output nos dara el nombre de dominio real, el ID de registro del dominio, donde fue registrado el dominio, fecha de la ultimas actualizaciones del dominio, fecha de creación del dominio, fecha de expiación del dominio, nombre del servidor, nombre de la organización, ubicación.

Whois Cloudflare:
Veremos el rango de red, el rango de la subnet y sistema autónomo. 
### Reconocimiento de WAF

wafw00f -l (listado de proxys y firewalls que detecta)
wafw00f https:\/\/example.com (normal)
wafw00f http:\/\/example.com -a (Testing for all posible waf instances)

### Enumeración pasiva subdominios Sublist3r

sublist3r -d example.com -o subdomains.txt
sublist3r -d example.com -e google,yahoo -o subdomains.txt (especificar motor de busquedad)

### Google dorks

#### Búsqueda de dorks
exploit-db.com\/google-hacking-database
#### Búsqueda simple

site:example.com 
(Limitar busqueda al dominio en especifico)

site:example.com empleados
(Limitar busqueda al dominio en especifico, buscando por palabra clave)

site:example.com inurl:admin 
(Limitar busqueda al dominio en especifico y la palabra admin en la url)
#### Búsqueda subdominios

site:\*.example.com
(Enlistar los subdominios indexados en google)

site:\*.example.com inurl:admin
(subdominios con la palabra admin en la url)

site:\*.example.com intitle:admin
(subdominios con la palabra admin en el titulo)
#### Búsqueda archivos

site:\*.example.com filetype:pdf (xlsx,doc,docx,zip)
(buscando archivos pdf en los subdominios)
#### Búsqueda antiguas de dominios

cache:example.com
#### Búsqueda de credenciales

site:example.com inurl:auth_user_file.txt
intitle:passwd.txt
inurl:passwd.txt
inurl:wp-config.bak
### Wayback Machine

archive.org\/web
### Recolección de correos

theHarvester -d exmaple.org 
theHarvester -d example.org -b google,linkedin,yahoo,dnsdumpster,duckduckgo,crtsh
### Ver contraseña vulneradas

haveibeenpwned\.com
# Reconocimiento activo

#### Reconociminento activo de DNS
#### ¿Qué es DNS y como funciona? ¿Por que atacarlo?

DNS (Domain Name System) significa sistema de nombres de dominio, es un protocolo que se utiliza para resolver nombre de dominio o nombres de host en direcciones ip, y viceversa.

| Nombre del registro | Información                                    |
| ------------------- | ---------------------------------------------- |
| A                   | Resuelve el nombre del host en IPv4.           |
| AAAA                | Resuelve el nombre del host en IPv6.           |
| NS                  | Referencia al servidor de nombre de domino.    |
| MX                  | Resuelven un dominio en un servidor de correo. |
| CNAME               | Uso de alias de dominio.                       |
| TXT                 | Archivo de texto.                              |
| HINFO               | Información del host.                          |
| SOA                 | Autoridad del dominio.                         |
| SRV                 | Registros de servicios.                        |
| PTR                 | Resuelve un IP a su nombre de host.            |

#### Interrogación de DNS

La interrogación de DNS es el proceso de enumerar registros de DNS para un determinado domino. El objetivo de la interrogación de DNS es sondear un servidor DNS para proporcionarnos registros DNS para un dominio especifico, más específicamente, registros adicionales a los que quizás no hayamos tenido acceso anteriormente. 

El proceso de interrogación DNS, puede proporcionar información importante como la dirección IP de un dominio, subdominios o direcciones de servidores de correo entre otras. 
#### Transferencia de zona de DNS

Si se deja mal configurado el servidor DNS los atacantes pueden abusar de ello para copiar el archivo de zona del DNS principal a otro servidor DNS del atacante. 
Por lo general una transferencia de zona DNS puede proporcionar al pentester una visión holística de la red de una organización. 
En ciertos casos las direcciones de red internas se pueden encontrar en los servidores DNS de una organización, por lo tanto es posible que podamos encontrar subdominios adicionales para los dominios internos de la empresa. 

dnsenum zonetransfer.me

![[Pasted image 20240325143837.png]]
![[Pasted image 20240325144033.png]]

dig axfr @NOMBRE_SERVIDOR_DNS zonetransfer.me

fierce -dns zonetransfer.me


#### Reconocimiento de host con nmap

Nmap -sn 192.168.2.**0**/24
#### Escaneo de puertos con nmap



