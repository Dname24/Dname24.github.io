---
layout: single
title: Apuntes Hack The Box
excerpt: "En esta sección están los apuntes de una página que se usa normalmente para hackear"
date: 2020-01-29
classes: wide
header:
  teaser: # /assets/images/hospital-dicom/dicom.png
  teaser_home_page: true
categories:
  - Research
tags:
  - Hack The Box
  - Vulnerabilidad
  - Write Up
---

### Apuntes básicos:

Primero hacer **ping** a la **ip** para vertificar si esta recibe paquetes

Después debemos realizar un escaneo de puertos con **nmap**
{% highlight shell %}
sudo nmap -sV -O {IP} 
{% endhighlight %}
Como vemos el comando lleva 

 **-sV** sirve para la **Detección de versiones**

 **-O** 	sirve para la **Detección del sistema operatico**

Después de ejecutar el código nos damos cuenta que tiene un servicio llamado **FTP**
**(File Transfer Protocol)** el que se encarga de transferir archivos de manera manual o 
automática por medio de client-server, a veces está mal configurada y se puede explotar.
Tener en cuenta servicios adicionales como *SSH/HTTPD (Secure Shell Protocol/Web Server)*
que permite **recibir y enviar archivos** que se pueden usar para configurar el **servidor web**
o servir registros a una fuente externa.

<div>
<p style = 'text-align:center;'>
<img src="https://i1.wp.com/nksistemas.com/wp-content/uploads/2012/06/puertos.png?ssl=1" alt="JuveYell" width="500px">
</p>
</div>

Como se ve en la imagen tenemos sus puerto con sus servicios, tener en cuenta **port 21:ftp**
**port 22:SSH** **port 80:Web Server** pero como es una máquina simple no tendrá seguridad y 
podremos ingresar con los siguientes comandos:

{% highlight shell %}
ftp {IP}
Connected to {IP}.
220 (vsFTPd 3.0.3)
Name ({IP }:{username}):
331 Please specify the password.
Password:
{% endhighlight %}

Nos pide un usuario y contraseña el cual si está mal configurado se podrá acceder con username: ~~anonymous~~
y con cualquier contraseña ya que el servicio ignorará la contraseña.

### smbclient

Alojado en el puerto **445**, utilizaremos un script llamado **smbclient -h**

{% highlight shell %}
smbclient -L {IP}
{% endhighlight %}

**--List = Host**

Con el comando L se selecciona el hostde destino para la solicitud de conexión, con esto esperamos
la siguiente respuesta aunque no siempre es igual en todas las maquinas:

{% highlight shell %}
    ADMIN$: Los recursos compartidos de red ocultas creado por windows, 
	    permite tener acceso remoto(no se elimina, solo se deshabilitan)
        C$: Sistema Operativo
      IPC$: Recursos compartidos
WorkShares: Custom Share
{% endhighlight %}

Para poder ver los archivos que se encuentran dentro de estos recursos compartidos usamos el sgte comando:
 
{% highlight shell %}
smbclient \\\{IP}\\{Recurso-Compartido}
{% endhighlight %}

### xfreerdp

Alojado en el puerto **3389** normalmente usado para **RDP(Remote Desktop Protocol)**, utilizaremos un script
llamado **xfreerdp** que permitirá ver la pantalla de la victima.

{% highlight shell  %}
xfreerdp [file] [options] [/v:<server>[:port]]
{% endhighlight %}

Como es una maquina simple solo usaremos el sgte comando **xfreerdp /v:{IP}**, nos pedirá un usuario y contraseña
podríamos usar los sgtes nombres:"admin,  administrator, root, user"

{% highlight shell  %}
xfreerdp /v:{IP} /u:Administrator
{% endhighlight %}


## SQL Injection

Modificamos el comando aumentandolé un nuevo parametro:

{% highlight shell %}
sudo nmap -sC -sV -O {IP}
{% endhighlight  %}

**-sC** sirve para realizar un analisis de secuencia de comandos. Es equivalente a --script = default.

Cuando tenemos el **puerto: 80** abierto que es referente a un servidor web, como ya
sabemos las páginas web tienen directorios los cuales tenemos que visitar para poder encontrar una vulnerabilidad
y poder explotarla, directorios como: **Home , About , Contact , Register , and Log-in pages**
Para esto usaremos una herramienta llamada [GoBuster](https://github.com/OJ/gobuster.git) que se encargará de 
encontrar esos directorios.

Lo instalamos:
{% highlight shell  %}
go get && go build
go install
gobuster --help
{% endhighlight  %}

```
SQL Service 
  →Database
   →Tables
    →Columns↘
             Data	
    →Rows   ↗
```
En el "grafico" anterior vemos como funciona un servicion SQL

```
       User Search                 Permissions Check
       ___________                 _________________
      /           ↘               /                 ↘	 
Client             Web Aplication                    SQL Service 
   
HTTP 200 la pagína o recurso existe así que procede con el envio de los datos
     404 no encontrado, la página o recurso no existe
     302 página o recurso encontrado pero mediante redireción
```

Ejemplo de SQL Injection

{% highlight PHP  %}
<?php
mysql_connect("localhost", "db_username", "db_password"); 
# Connection to the SQLDatabase.
mysql_select_db("users"); 
# Database table where user information is stored.

$username=$_POST['username']; 
# User-specified username.
$password=$_POST['password']; 
#User-specified password.

$sql="SELECT * FROM users WHERE username='$username' AND password='$password'";
# Query for user/pass retrieval from the DB.

$result=mysql_query($sql);
# Performs query stored in $sql and stores it in $result.

$count=mysql_num_rows($result);
# Sets the $count variable to the number of rows stored in $result.

if ($count==1){
  # Checks if there's at least 1 result, and if yes:
 $_SESSION['username'] = $username;
 # Creates a session with the specified $username.
 $_SESSION['password'] = $password;
 # Creates a session with the specified $password.
 header("location:home.php");
 # Redirect to homepage.
}
else {
 # If there's no singular result of a user/pass combination:
  header("location:login.php");
 # No redirection, as the login failed in the case the $count 
 #variable is not equal to
 #1, HTTP Response code 200 OK.
}
?>
{% endhighlight  %}


Esto es un concepto básico pero util en algunos casos como se mira en el código **php** el **#**
sirve para hacer comentarios, lo podemos usar para ingresar como un usuario admin usando un fallo
en el código ingresando así
```
user: admin'#
password: #
```

En verdad la contraseña no importa ya que con el **#** lo toma como un comentario y ponemos 
cualquier cosa en contraseña ya que no podemos ingresar si no colocamos un texto en ese recuadro.



### Fuzzing

Estaremos usando la herramienta *WFUZZ* con la siguiente sintaxis

```BASH
wfuzz -c --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt  -u http://IP/FUZZ
```
Para buscar subdominios se cambia la sintaxis por la siguiente: 

```BASH
wfuzz -c --hc=404,400 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt  -H "Host: FUZZ.IP" http://IP.htb/
```
### Server interno

Para montarnos nuestro propio servidor python lo usamos con el siguiente comando

```BASH
python3 -m http.server
```
