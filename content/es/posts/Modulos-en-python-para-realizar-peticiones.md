---
title: "M贸dulos en Python Para Realizar Peticiones al Protocolo HTTP"
date: 2022-05-12T10:18:44-05:00
draft: false

description: "Se muestra algunos m贸dulos importantes en Python para realizar peticiones"
hideToc: false #(false: se muestra el indice a la derecha ... true no se muestra)
enableToc: true #(Se quita o no la opci贸n de derecha que permite ocultar o no)
enableTocContent: false  #(Indice pero vertical)
author: Brayan Rodriguez #(Autor)
authorEmoji: 馃懞

#(Etiquetas de referencia al contenido)
tags:
- Python
- Sockets
- Modules-Python

categories:
- Programaci贸n

image: images/03_Modulos_Python/Intro.jpg
---

Algunos M贸dulos en Python para realizar peticiones 

###  Protocolo HTTP 

El protocolo HTTP es un canal de transferencia de datos de hyper-texto que no posee estado y que no almacena informaci贸n. Debido a que no posee estado, se utiliza cookies para guardar informaci贸n. 
El protocolo HTTP define una serie de m茅todos para realizar consultas, entre estos se encuentran: 

GET. Pide una representaci贸n del recurso especificado. Por seguridad no deber铆a ser usado por aplicaciones que causen efectos ya que transmite informaci贸n a trav茅s de la URI agregando par谩metros a la URL.

HEAD. Pide una respuesta id茅ntica a la que corresponder铆a a una petici贸n GET, pero en la petici贸n no se devuelve el cuerpo. Esto es 煤til para poder recuperar los metadatos de los encabezados de respuesta, sin tener que transportar todo el contenido.

POST. Env铆a los datos para que sean procesados por el recurso identificado. Los datos se incluir谩n en el cuerpo de la petici贸n. Esto puede resultar en la creaci贸n de un nuevo recurso o de las actualizaciones de los recursos existentes o ambas cosas.

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
    print("Ocurri贸 un error",error)
except HTTPError as error:
    print("Ocurri贸 un error",error)
except URLError as error:
    print("Ocurri贸 un error",error)

{{< /highlight >}} 

La funci贸n urlopen() devuelve un objeto de la clase http.client.HTTPResponse. 

### C贸digos de estado

Los c贸digos de estado se clasifican en los siguientes grupos:

100: informativo
200: 茅xito
300: redirecci贸n
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

### Impresi锟斤拷n del emcabezado

{{< highlight Python >}}

import urllib.request
url = input("Introduce la URL:")
http_response = urllib.request.urlopen(url)
print('C贸digo de estado: '+ str(http_response.code))
if http_response.code == 200:
    print(http_response.headers)

{{< /highlight >}} 

{{< highlight Python >}}	

import urllib.request
url = input("Introduce la URL:")
http_response = urllib.request.urlopen(url)
print('C贸digo de estado: '+ str(http_response.code))
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
3. Llamar al m茅todo urlopen() para enviar el objeto Request.


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

### Obteniendo emails de la p谩gina principal de una web

{{< highlight Python >}}
import urllib.request
import re
web = input("Introduce una url(sin http://): ")
#https://www.adrformacion.com/
#obtener la respuesta
response = urllib.request.Request('http://'+web)
#obtener el contenido de la p谩gina a partir de la respuesta
content = urllib.request.urlopen(response).read()
# expression regular para detectar emails
pattern = re.compile("[-a-zA-Z0-9._]+@[-a-zA-Z0-9_]+.[a-zA-Z0-9_.]+")
#obtener emails a partir de una expresi锟斤拷n regular
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

Una caracter铆stica interesante es que le podemos indicar el n煤mero de conexiones que vamos a reservar para el pool de conexiones que estamos creando utilizando la clase PoolManager.

Esta clase se encarga de gestionar la conexi贸n de forma persistente y reutilizar las conexiones HTTP que va creando gracias a un pool de conexiones.

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

1. response.status_code: este es el c贸digo HTTP devuelto por el servidor.
2. response.content: Aqu铆 encontraremos el contenido de la respuesta del servidor.
3. response.json(): en el caso de que la respuesta sea un JSON, este m锟斤拷todo serializa la cadena y devuelve una estructura de diccionario con la estructura JSON correspondiente. En el caso de no recibir un JSON para cada respuesta, el m锟斤拷todo devolver锟斤拷a una excepci锟斤拷n que podr锟斤拷amos controlar

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
    print("N煤mero de palabras :",num)

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


La instrucci贸n response.headers proporciona las cabeceras de la respuesta del servidor web. B谩sicamente, la respuesta es un diccionario de objetos, y con el m茅todo items(), podemos iterar con el formato clave-valor para acceder a las diferentes cabeceras.

### Ventajas del m贸dulo requests

El m贸dulo requests facilita el uso de peticiones HTTP en Python en comparaci贸n con urllib. A menos que tenga un requisito para usar urllib, siempre recomendar铆a el uso de requests para sus proyectos en Python. Entre las principales ventajas del m贸dulo de requests podemos destacar las siguientes:

Una biblioteca enfocada en la creaci贸n de clientes HTTP completamente funcionales.
Soporta todos los m茅todos y caracter铆sticas definidos en el protocolo HTTP.
Es "Pythonic", es decir, est谩 completamente escrito en Python y todas las operaciones se realizan de manera simple y con solo unas pocas l铆neas de c贸digo.
Tareas como la integraci贸n con servicios web, la creaci贸n de un pool de conexiones HTTP, la codificaci贸n de datos POST en formularios y el manejo de cookies, se manejan autom谩ticamente.
Se trata de una librer铆a que implementa las funcionalidades de urllib3 y las extiende.

### Get API Rest 

{{< highlight Python >}}

import requests,json
 
response = requests.get("http://httpbin.org/get",timeout=5)
 
print("C贸digo de estado HTTP: " + str(response.status_code))
 
print(response.headers)
 
if response.status_code == 200:
    results = response.json()
    for result in results.items():
        print(result)
    print("Cabeceras de la respuesta: ")
    for header, value in response.headers.items():
        print(header, '-->', value)
    print("Cabeceras de la petici贸n : ")
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
    print("Impresi贸n de resultados formato linea por linea")
    for res in resultados.items():
        print(res)
    print("Cabecera de la respuesta")
    for header, value in response.headers.items():
        print(header, " -> ", value)
    print("Cabecera de la petici贸n")
    for header, value in response.request.headers.items():
        print(header, " -> ", value)
else:
    print("HTTP code error %s" %res.status_code)

{{< /highlight >}} 


### Consumo a trav茅s de proxy

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

Establecer la conexi贸n con un host con m贸dulo http.client utilizando la clase http.client.HTTPConnection.
Establecer la conexi贸n con un host con m贸dulo urllib.request utilizando el m茅todo urlopen(), incluyendo la obtenci贸n del c贸digo de estado, cabeceras de la respuesta y de la petici贸n a partir el objeto de respuesta response.
Obtener informaci贸n de las cabeceras de la respuesta y de la petici贸n de diferentes formas como utilizando los m茅todos info() y getheaders().
Personalizar las cabeceras que se env铆an en la petici贸n a trav茅s del par谩metro headers urllib.request.Request(url,headers=headers).
Extraer informaci贸n de una p谩gina web como enlaces e im谩genes utilizando el m贸dulo re para expresiones regulares.
Establecer la conexi贸n con un host con m贸dulo urllib3 utilizando la clase PoolManager.
Establecer la conexi贸n con un dominio con m贸dulo requests utilizando el m茅todo get(), incluyendo la obtenci贸n del c贸digo de estado, cabeceras de la respuesta y de la petici贸n con el objeto de respuesta.
Realizar una petici贸n a una API Rest utilizando el m贸dulo request a trav茅s de los m茅todos get() y post().