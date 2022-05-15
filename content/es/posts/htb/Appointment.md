---
title: "Appointment"
date: 2022-05-14T14:48:02-05:00
draft: false

description: "Resolución del reto Appointment de Hack The Box"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- SQL-Injection
- Fundamentos

categories:
- Ciberseguridad
- Hack-the-box

image: images/HTB/01_Appointment/Appointment.png
---

#### TASK 1: What does the acronym SQL stand for?

ANS: Structured Query language

#### TASK 2: What is one of the most common type of SQL vulnerabilities?

ANS: SQL injection

#### TASK 3: What does PII stand for?

ANS: personally identifiable information

#### TASK 4: What does the OWASP Top 10 list name the classification for this vulnerability?

ANS: A03:2021-Injection

#### TASK 5: What service and version are running on port 80 of the target?

ANS: Apache httpd 2.4.38 ((Debian))

#### TASK 6: What is the standard port used for the HTTPS protocol?

ANS: 443

#### TASK 7: What is one luck-based method of exploiting login pages?

ANS: brute-forcing

#### TASK 8: What is a folder called in web-application terminology?

ANS: directory

#### TASK 9: What response code is given for "Not Found" errors?

ANS: 404

#### TASK 10: What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

ANS: dir

#### TASK 11: What symbol do we use to comment out parts of the code?

ANS: #

#### SUBMIT FLAG: Submit root flag

Ingresamos a la p+agina web expuesta http://{IP}:80

En el formulario de inicio de sesión injectamos una admin' or 1=1;#
o también podemos utilizar a' or '1'='1 en los campos usuario y contraseña. 

ANS: e3d0796d002a446c0e622226f42e9672