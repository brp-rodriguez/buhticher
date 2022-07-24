---
title: "Conexión por SSH"
date: 2022-06-11T19:27:58-05:00
draft: false

description: "Conectándonos por el servicio SSH de Kali Linux"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- SSH
- Tips

categories:
- Ciberseguridad
- Kali-Linux
image: images/TIPS/ConexionSSH.jpg

---

### Cómo abrir el puerto 22 para ejecutar el servicio SSH

sudo apt-get update
sudo apt-get install ssh 

#### Habilitamos el uso del servicio SSH 
sudo systemctl enable ssh

#### Iniciamos el servicio
sudo service ssh start

#### Abrimos el archivo sshd_config para editarlo y permite la conexion por usuario root
sudo nano /etc/ssh/sshd_config

Busca en el archivo y descomenta la siguiente linea y edita el valor "prohibit-password" a "yes" (Sin comillas)
Cambia esto
#PermitRootLogin prohibit-password

Por esto 
PermitRootLogin yes

#### Reinicia el servicio
sudo service ssh restart

#### Prueba y accede 