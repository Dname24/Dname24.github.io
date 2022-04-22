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
Nota: En el laboratorio para identificar el tipo de protocolo que usaba la pagina local 
usamos Wireshark pero antes de elegir la interfaz de conectada debemos ver de donde 
vienen las conexiones, tambien mirar siempre que hosts tiene en el /etc/hosts.
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

## Link Layer Devices and Protocols 

### -Direcciones MAC

Las direcciones IP son la tercera capa (Network Layer) esquema de direccionamiento usado para identificar un host en una red
mientras que las direcciones MAC solo identifican la tarjeta de red, para saber tu direccion MAC se necesita el siguiente 
comando.


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

## Firewall y Defensa de red





