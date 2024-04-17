
`Mapeo del samba`
smbmap -H =IP=

`Teniendo permisos de lectura, listar directorio samba`
smbmap -H =IP= -r =USERVIEWPERMISSIONS=

`Descargar archivos con permisos de lectura`

smbmap -H =IP= --download USERVIEWPERMISSIONS/file.txt

`Inicio de sesión con samba`
smbmap -H =IP= -u USER -p PASSWORD

`Descargar archivos con usuario`
smbmap -H =IP= -u USER -p PASSWORD --download =USERVIEWPERMISSIONS/file.txt