---
title: "Modulos en Python Para Realizar Peticiones al Protocolo HTTP"
date: 2022-05-12T10:18:44-05:00
draft: false

description: "Se muestra algunos módulos importantes en Python para realizar peticiones"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 👺

#(Etiquetas de referencia al contenido)
tags:
- Python
- Sockets
- Modules-Python

categories:
- Programación

image: images/03_Modulos_Python/Intro.jpg
---

Algunos Módulos en Python para realizar peticiones 

###  Protocolo HTTP 

El protocolo HTTP es un canal de transferencia de datos de hyper-texto que no posee estado y que no almacena información. Debido a que no posee estado, se utiliza cookies para guardar información. 
El protocolo HTTP define una serie de métodos para realizar consultas, entre estos se encuentran: 

GET. Pide una representación del recurso especificado. Por seguridad no debería ser usado por aplicaciones que causen efectos ya que transmite información a través de la URI agregando parámetros a la URL.

HEAD. Pide una respuesta idéntica a la que correspondería a una petición GET, pero en la petición no se devuelve el cuerpo. Esto es útil para poder recuperar los metadatos de los encabezados de respuesta, sin tener que transportar todo el contenido.

POST. Envía los datos para que sean procesados por el recurso identificado. Los datos se incluirán en el cuerpo de la petición. Esto puede resultar en la creación de un nuevo recurso o de las actualizaciones de los recursos existentes o ambas cosas.

### Creando un cliente con httplib.client

{{< highlight Python >}}

import http.client
import argparse

parser = argparse.ArgumentParser(description='obtener respuesta de un dominio')
parser.add_argument("-target", dest="target", help="IP /dominio", required=True)
parsed_args = parser.parse_args()
print(parsed_args.target)
connection = http.client.HTTPConnection(parsed_args.target)
connection.request("GET", "/")
data = connection.getresponse()
print (data.code)
print (data.headers)
texto = data.readlines()
print(texto)

{{< /highlight >}} 