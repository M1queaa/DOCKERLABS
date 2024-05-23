# FirstHacking
https://dockerlabs.es

*23/5/2024*


## ENUMERACION

``
┌──(kali㉿kali)-[~] 
└─$ nmap -p- --min-rate 5000 -sCV 172.17.0.2 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-23 10:29 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00014s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
Service Info: OS: Unix
``

## ANALISIS DE VULNERABILIDADES
┌──(kali㉿kali)-[~]
└─$ searchsploit vsftpd 2.3.4
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                            |  Path
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
vsftpd 2.3.4 - Backdoor Command Execution                                                                                                                                                                 | unix/remote/49757.py
vsftpd 2.3.4 - Backdoor Command Execution (Metasploit)                                                                                                                                                    | unix/remote/17491.rb
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results


## EXPLOTACION

## POST-EXPLOTACION
