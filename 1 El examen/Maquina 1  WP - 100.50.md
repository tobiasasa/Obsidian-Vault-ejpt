````
Nmap scan report for 192.168.100.50
Host is up (0.00045s latency).
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Apache httpd 2.4.51 ((Win64) PHP/7.4.26)
|_http-title: WAMPSERVER Homepage
|_http-server-header: Apache/2.4.51 (Win64) PHP/7.4.26
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows Server 2012 R2 Standard 9600 microsoft-ds
3307/tcp  open  opsession-prxy?
| fingerprint-strings: 
|   NULL: 
|_    Host 'ip-12-168-100-5.eu-central-1.compute.internal' is not allowed to connect to this MariaDB server
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: WINSERVER-01
|   NetBIOS_Domain_Name: WINSERVER-01
|   NetBIOS_Computer_Name: WINSERVER-01
|   DNS_Domain_Name: WINSERVER-01
|   DNS_Computer_Name: WINSERVER-01
|   Product_Version: 6.3.9600
|_  System_Time: 2024-04-15T00:03:43+00:00
| ssl-cert: Subject: commonName=WINSERVER-01
| Not valid before: 2024-04-13T23:49:39
|_Not valid after:  2024-10-13T23:49:39
|_ssl-date: 2024-04-15T00:03:53+00:00; -1s from scanner time.
5985/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
47001/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
49178/tcp open  msrpc              Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3307-TCP:V=7.92%I=7%D=4/15%Time=661C6E74%P=x86_64-pc-linux-gnu%r(NU
SF:LL,6D,"i\0\0\x01\xffj\x04Host\x20'ip-192-168-100-5\.eu-central-1\.compu
SF:te\.internal'\x20is\x20not\x20allowed\x20to\x20connect\x20to\x20this\x2
SF:0MariaDB\x20server");
MAC Address: 02:AC:90:76:9E:3B (Unknown)
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 0s, deviation: 2s, median: -1s
|_nbstat: NetBIOS name: WINSERVER-01, NetBIOS user: <unknown>, NetBIOS MAC: 02:ac:90:76:9e:3b (unknown)
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: WINSERVER-01
|   NetBIOS computer name: WINSERVER-01\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-04-15T00:03:48+00:00
| smb2-time: 
|   date: 2024-04-15T00:03:43
|_  start_date: 2024-04-14T23:49:30
````` 

Samba 

![[Pasted image 20240414213605.png]]

![[Pasted image 20240414213738.png]]![[Pasted image 20240414215201.png]]

![[Pasted image 20240414215315.png]]

![[Pasted image 20240414220836.png]]

Please contact the server administrator at wampserver@wampserver.invalid

![[Pasted image 20240414222208.png]]

wordpress admin

![[Pasted image 20240414222247.png]]
\
## admin:estrella

multi/http/wp_responsive_thumbnail_slider_upload

