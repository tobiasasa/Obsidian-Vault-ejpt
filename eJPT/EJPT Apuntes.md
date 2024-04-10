# Networking
## Enrutamiento de tablas

| Comando       | Sistema Operativo |
| ------------- | ----------------- |
| `ip route`    | Linux             |
| `route print` | Windows           |
| `netstat -r`  | Mac OS X / Linux  |

## Descubrimiento de red

| Comando         | Sistema Operativo                            |
| --------------- | -------------------------------------------- |
| `ip a`          | Linux                                        |
| `ipconfig /all` | Windows                                      |
| `ifconfig`      | Unix / Mac OS X                              |
| `ip -br -c a`   | Linux - useful for fast finding IP interface |

## Comprobar puertos de escucha y conexiones TCP

| Comando                 | Sistema Operativo |
| ----------------------- | ----------------- |
| `netstat -tunp ss -tnl` | Linux             |
| `netstat -ano`          | Windows           |



# Nmap

## Reconocimiento de red, puertos, servicios y versiones

nmap --open -p- -Pn -n -sCV -T4 -f 10.2.17.47 -v -oN full_scan_tcp.txt

# Escalada de privilegios

## Linux

| Comando                              | Utilidad                                        |
| ------------------------------------ | ----------------------------------------------- |
| `find / -perm -4000 -ls 2>/dev/null` | Ver archivos con permisos SUID                  |
| `netstat -ano`                       | Listar tareas en intervalos de tiempo           |
| `file /opt/statuscheck`              | Ver tipo de archivo                             |
| `strings /opt/statuscheck`           | Ver cadena de caracteres legibles en un archivo |
## Windows