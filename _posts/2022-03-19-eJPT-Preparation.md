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

## Conceptos de criptografia y VPN

Los protocolos de criptografia encriptan la informacion transmitida para proteger la comunicacion, un gran protocolo de es el *VPN* (Vitual Private Network).
El VPN usa criptografia para extender una red privada sobre una publica, como internet. 

Cuado uno esta conectado por vpn actualmente esta corriendo los mismos protocolos de una red privada, esto le permite realizar operaciones de red de bajo nivel los cuales puedes interceptar con **Wireshark**

### Introduccion a Wireshark 

Wireshark es una herramienta rastreadora(sniffer) de red, un sniffer te permite ver los datos transmitidos a través de la red hacia y desde su computadora.

```
Nota: En el laboratorio para identificar el tipo de protocolo que usaba la pagina local 
usamos Wireshark pero antes de elegir la interfaz de conectada debemos ver de donde 
vienen las conexiones, tambien mirar siempre que hosts tiene en el /etc/hosts.
```

### Aritmetica Binaria Basica

Convertir numeros de sistema decimal a sistema binario es algo sencillo, tambien se puede hacer con operadores logicos como: 

-"NOT" Este operador se encarga de cambiar los **0** por **1** y viceversa

-"AND" Este operador se encarga de comparar un bit con el otro dependiendo de su orden y lo cambia si no cumple la condicion

-"OR"  Este operador se encarga de comparar un bit con el otro dependiendo de su orden, si no cumple la condicion se cambia

-"XOR" Este operador hace alusion al 'OR exclusivo' 

Tener en cuenta que la base hexadecimal tambien es usada en la informatica asi que es bueno saber como funciona

## Protocolos

### Paquetes

Cada paquete en todo protocolo tiene la siguiente estructura:

-"Header" El encabezado tiene una estructura especifica del protocolo: Esto garantiza que el host receptor pueda interpretar correctamente el payload y manejar la comunicacion general.

-"Payload" Es la informacion actualmente

### ISO/OSI

OSI/ISO consiste en 7 capas y su uso como referencia para la implementacion de la actual capa de protocolos.

![](/assets/images/OSI_Model_v1.png)


