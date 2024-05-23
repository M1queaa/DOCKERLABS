![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/9af2e846-ec97-4dfb-823f-937a4c7c9c52)# Trust
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
![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/2300bc9c-6002-45d2-9d2e-6f490360e9be)

## ANALISIS DE VULNERABILIDADES

```

```
## EXPLOTACION

```

```

## POST-EXPLOTACION

```

```
