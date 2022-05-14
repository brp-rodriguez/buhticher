---
title: "Módulos en Python Para Realizar Peticiones al Protocolo HTTP"
date: 2022-05-12T10:18:44-05:00
draft: false

description: "Se muestra algunos módulos importantes en Python para realizar peticiones"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opción de derecha que permite ocultar o no)
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

### Creando un cliente con urllib.request

{{< highlight Python >}}

import urllib.request
 
try:
    response = urllib.request.urlopen("http://www.python.org")
    print(response.read().decode('utf-8'))
    response.close()
except Exception as error:
    print("Ocurrió un error",error)
except HTTPError as error:
    print("Ocurrió un error",error)
except URLError as error:
    print("Ocurrió un error",error)

{{< /highlight >}} 

La función urlopen() devuelve un objeto de la clase http.client.HTTPResponse. 

### Códigos de estado

Los códigos de estado se clasifican en los siguientes grupos:

100: informativo
200: éxito
300: redirección
400: error del cliente
500: error del servidor

https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml

### Manejo de excepciones con urllib.request

{{< highlight Python >}}

import urllib.error 
from urllib.request import urlopen
 
try:
    urlopen('https://www.ietf.org/rfc/rfc0.txt')
	
except urllib.error.HTTPError as e:
    print('Exception', e)
    print('status', e.code)
    print('reason', e.reason)
    print('url', e.url)
	
{{< /highlight >}} 

### Impresi��n del emcabezado

{{< highlight Python >}}

import urllib.request
url = input("Introduce la URL:")
http_response = urllib.request.urlopen(url)
print('Código de estado: '+ str(http_response.code))
if http_response.code == 200:
    print(http_response.headers)

{{< /highlight >}} 

{{< highlight Python >}}	

import urllib.request
url = input("Introduce la URL:")
http_response = urllib.request.urlopen(url)
print('Código de estado: '+ str(http_response.code))
if http_response.code == 200:
    print(http_response.info())
	
{{< /highlight >}} 

{{< highlight Python >}}

import urllib.request
url = input("Enter the URL:")
http_response = urllib.request.urlopen(url)
if http_response.code == 200:
	print(http_response.headers)
	for key,value in http_response.getheaders():
		print(key,value)	
		
{{< /highlight >}} 

### Modificando de el user-agent para consultar

1. Crear un objeto de solicitud(Request)
2. A?adir cabeceras al objeto Request.
3. Llamar al método urlopen() para enviar el objeto Request.


{{< highlight Python >}}

import urllib.request
url = "http://www.python.org"
#https://www.whatismybrowser.com/guides/the-latest-user-agent/chrome
headers= {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.61 Safari/537.36'}
request = urllib.request.Request(url,headers=headers)
response = urllib.request.urlopen(request)
print('User-agent',request.get_header('User-agent'))
if response.code == 200:
    print(response.headers)


{{< /highlight >}} 

### Obteniendo emails de la página principal de una web

{{< highlight Python >}}
import urllib.request
import re
web = input("Introduce una url(sin http://): ")
#https://www.adrformacion.com/
#obtener la respuesta
response = urllib.request.Request('http://'+web)
#obtener el contenido de la página a partir de la respuesta
content = urllib.request.urlopen(response).read()
# expression regular para detectar emails
pattern = re.compile("[-a-zA-Z0-9._]+@[-a-zA-Z0-9_]+.[a-zA-Z0-9_.]+")
#obtener emails a partir de una expresi��n regular
mails = re.findall(pattern,str(content))
print(mails)
{{< /highlight >}} 


{{< highlight Python >}}

from urllib.request import urlopen
import re
 
def download_page(url):
    return urlopen(url).read()
 
def extract_links(page):
    link_regex = re.compile('<a[^>]+href=["\'](.*?)["\']',re.IGNORECASE)
    return link_regex.findall(page)
 
if __name__ == '__main__':
    target_url = 'http://www.adrformacion.com'
    content = download_page(target_url)
    links = extract_links(str(content))
    for link in links:
        print(link)
		
{{< /highlight >}} 

### Obteniendo imagenes

{{< highlight Python >}}
from urllib.request import urlopen, urljoin
import re

def download_page(url):
	return urlopen(url).read()
	
def extract_image_locations(page):
    img_regex = re.compile('<img[^>]+src=["\'](.*?)["\']',re.IGNORECASE)
    return img_regex.findall(page)

if __name__ == '__main__':
    target_url = 'https://www.adrformacion.com/'
    content = download_page(target_url)
    #print(content)
    image_locations = extract_image_locations(str(content))
    print(image_locations)
    for src in image_locations:
        print(target_url, src)
{{< /highlight >}} 		


### urllib3

Una característica interesante es que le podemos indicar el número de conexiones que vamos a reservar para el pool de conexiones que estamos creando utilizando la clase PoolManager.

Esta clase se encarga de gestionar la conexión de forma persistente y reutilizar las conexiones HTTP que va creando gracias a un pool de conexiones.

{{< highlight Python >}}

import urllib3
pool = urllib3.PoolManager(10)
response = pool.request('GET','http://www.python.org')
print(response.status)
print("Keys\n-------------")
print(response.headers.keys())
print("Values\n-------------")
print(response.headers.values())
for header,value in response.headers.items():
    print(header + ":" + value)
{{< /highlight >}} 		


### Cliente HTTP con requests

{{< highlight Python >}}

import requests
response = requests.get('http://www.python.org')

{{< /highlight >}} 		

1. response.status_code: este es el código HTTP devuelto por el servidor.
2. response.content: Aquí encontraremos el contenido de la respuesta del servidor.
3. response.json(): en el caso de que la respuesta sea un JSON, este m��todo serializa la cadena y devuelve una estructura de diccionario con la estructura JSON correspondiente. En el caso de no recibir un JSON para cada respuesta, el m��todo devolver��a una excepci��n que podr��amos controlar

{{< highlight Python >}}

>>> response.status_code
200
 
>>> response.reason
'OK'
>>> response.url
'http://www.python.org'
>>> response.headers['content-type']
'text/html; charset=utf-8'

>>> response.request.headers
{'User-Agent': 'python-requests/2.23.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}

{{< /highlight >}} 

### Contando palabras

{{< highlight Python >}}

import requests
import sys
import re 
def contar_palabras(url):    
    try:
        fichero_url = f'{url}'
        response = requests.get(fichero_url)
        #print(response.text)
        texto = response.text 
        sp = ' |\t|\r|\n'
        palabras = re.split(sp,texto)
        #print(palabras)
        return len(palabras)
    except requests.exceptions.HTTPError as e:
        print ("Http Error:",e)
    except requests.exceptions.Timeout as e:
        print("Timeout Error: ",e)
    except requests.exceptions.RequestException as e:
        print("Error Desconocido: ",e)

if __name__ == "__main__":
    url = "ht.txt"
    num = contar_palabras(url)
    print("Número de palabras :",num)

{{< /highlight >}} 

### Cabeceras de las peticiones

{{< highlight Python >}}
import requests
 
response = requests.get("http://www.python.org")
 
print(response.content)
 
print("Status code: "+str(response.status_code))
 
print("Cabeceras de respuesta: ")
for header, value in response.headers.items():
    print(header, '-->', value)
 
print("Cabeceras de la peticion:")
for header, value in response.request.headers.items():
    print(header, '-->', value)
	
{{< /highlight >}} 


La instrucción response.headers proporciona las cabeceras de la respuesta del servidor web. Básicamente, la respuesta es un diccionario de objetos, y con el método items(), podemos iterar con el formato clave-valor para acceder a las diferentes cabeceras.

### Ventajas del módulo requests

El módulo requests facilita el uso de peticiones HTTP en Python en comparación con urllib. A menos que tenga un requisito para usar urllib, siempre recomendaría el uso de requests para sus proyectos en Python. Entre las principales ventajas del módulo de requests podemos destacar las siguientes:

Una biblioteca enfocada en la creación de clientes HTTP completamente funcionales.
Soporta todos los métodos y características definidos en el protocolo HTTP.
Es "Pythonic", es decir, está completamente escrito en Python y todas las operaciones se realizan de manera simple y con solo unas pocas líneas de código.
Tareas como la integración con servicios web, la creación de un pool de conexiones HTTP, la codificación de datos POST en formularios y el manejo de cookies, se manejan automáticamente.
Se trata de una librería que implementa las funcionalidades de urllib3 y las extiende.

### Get API Rest 

{{< highlight Python >}}

import requests,json
 
response = requests.get("http://httpbin.org/get",timeout=5)
 
print("Código de estado HTTP: " + str(response.status_code))
 
print(response.headers)
 
if response.status_code == 200:
    results = response.json()
    for result in results.items():
        print(result)
    print("Cabeceras de la respuesta: ")
    for header, value in response.headers.items():
        print(header, '-->', value)
    print("Cabeceras de la petición : ")
    for header, value in response.request.headers.items():
        print(header, '-->', value)
    print("Server:" + response.headers['server'])
else:
    print("Error code %s" % response.status_code)

{{< /highlight >}} 

### Ejercicio de consumir POST 


{{< highlight Python >}}

import requests

data_dictionary = {"id": "0123456789"}
headers = {"Content-Type" :"application/json","Accept":"application/json"}
response = requests.post("http://httpbin.org/post",data=data_dictionary,headers=headers)

print("HTTP Status Code: " + str(response.status_code))

if response.status_code == 200:
    resultados = response.json()
    print("Impresión de resultados formato linea por linea")
    for res in resultados.items():
        print(res)
    print("Cabecera de la respuesta")
    for header, value in response.headers.items():
        print(header, " -> ", value)
    print("Cabecera de la petición")
    for header, value in response.request.headers.items():
        print(header, " -> ", value)
else:
    print("HTTP code error %s" %res.status_code)

{{< /highlight >}} 


### Consumo a través de proxy

{{< highlight Python >}}

proxy = {"protocol":"ip:port", ...}

response = requests.get(url,headers=headers,proxies=proxy)

import requests
http_proxy = "http://<direccion_ip>:<puerto>"
proxy_dictionary = { "http" : http_proxy}
requests.get("http://dominio.org", proxies=proxy_dictionary)


{{< /highlight >}} 

### Resumen 

En esta unidad hemos aprendido:

Establecer la conexión con un host con módulo http.client utilizando la clase http.client.HTTPConnection.
Establecer la conexión con un host con módulo urllib.request utilizando el método urlopen(), incluyendo la obtención del código de estado, cabeceras de la respuesta y de la petición a partir el objeto de respuesta response.
Obtener información de las cabeceras de la respuesta y de la petición de diferentes formas como utilizando los métodos info() y getheaders().
Personalizar las cabeceras que se envían en la petición a través del parámetro headers urllib.request.Request(url,headers=headers).
Extraer información de una página web como enlaces e imágenes utilizando el módulo re para expresiones regulares.
Establecer la conexión con un host con módulo urllib3 utilizando la clase PoolManager.
Establecer la conexión con un dominio con módulo requests utilizando el método get(), incluyendo la obtención del código de estado, cabeceras de la respuesta y de la petición con el objeto de respuesta.
Realizar una petición a una API Rest utilizando el módulo request a través de los métodos get() y post().