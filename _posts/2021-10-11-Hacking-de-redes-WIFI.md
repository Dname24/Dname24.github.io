---
layout: single
title: Hacking de Redes Inalámbricas (WiFi) 
excerpt: "Todo sobre hacking de redes wifi"
date: 2020-01-29
classes: wide
header:
  teaser: #
  teaser_home_page: true
categories:
  - Research
tags:
  - Wifi
  - Apuntes
  - WPA
---

# Vulneración de redes WPA/WPA2
## Modo monitor 
Lo primero será poner nuestra tarjeta de red en modo monitor para capturar y escuchar todos los paquetes que viajan en el aire, se hace con los siguientes comandos:

```bash
iwconfig
sudo airmon-ng start wlan0mon #Activamos el modo monitor de nuestra placa
ifconfig wlan0mon up #Levantamos la red
killall dhclient wpa_supplicant #Termina los procesos por atrás ya que haremos todo offline
/etc/init.d/networking restart #Reinicia el servicio de conexión
```

## Falsificación de nuestra dirección MAC 

Con el comando `macchanger` podremos cambiar nuestra dirección MAC para no tener dificultades al momento de auditar una red

```bash
macchanger -s wlan0mon #Muestra la ip actual
macchanger -l | grep "NATIONAL SECURITY AGENCY"
ifconfig wlan0mon down
macchanger --mac=00:20:91:af:af:25 wlan0mon
ifconfig wlan0mon up
```

![](/assets/images/Hackin-redes-WIFI/mac.png)

## Uso de Airodump para efectuar un análisis del entorno



