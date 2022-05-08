---
title: "Aprendiendo Sockets con Python"
date: 2022-05-07T20:48:46-05:00
draft: false

description: "Breves conceptos de sockets con Python"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 馃榿

#(Etiquetas de referencia al contenido)
tags:
- Python
- Sockets

categories:
- Programación

image: images/SocketsPython/Socket.png
---

Resumen de conceptos básicos de sockets en Python

## Fundamentos de Sockets 

### ¿Qué son los sockets?

Entendamos los sockets como un canal de comunicación punto a punto entre un cliente y un servidor. Es representado mediante la instancia de una conección a una IP y puerto mediante un protocolo en específico. La finalidad de un socket es comunicar datos o procesos a través de la red. 
 
#### Tipos de Sockets 

Hay dos tipos de sockets 
1. Sockets de flujo TCP (socket.SOCK_STREAM) 
2. Sockets de datagramas UDP (socket.SOCK_DGRAM)

Por un tema de simplicidad se ralizará mayor énfasis a los sockets de flujo TCP

#### Instanciando sockets en Python

En Python para crear un socket se utiliza el constructor scoket.socket() que puede tomar como parámetros adicionales la familia, tipo del flujo. El tipo de familia de direcciones del socket más usual es AF_INET (direcciones de IPv4), AF_INET6 (direcciones IPv6). También existen otras familias. Por ejemplo, AF_UNIX, AF_IPX, AF_IRDA, AF_BLUETOOTH, entre otros. 

### Código 
{{< highlight Python >}}
import socket
sock1 = socket.socket(AF_INET,SOCK_STREAM)
sock2 = socket.socket()
'''
"sock1 y sock2" : Variables u objetos instanciados del socket 
"socket"   : Llamada al módulo de socket  
"socket(AF_INET,SOCK_STREAM)" : Contructor del socket con parámetros
"socket()" : Constructor sin parámetros
"AF_INET"  : Familia de direcciones IPv4
"SOCK_STREAM" : Tipo del flujo de la comunicación del socket 
'''

{{< /highlight >}} 