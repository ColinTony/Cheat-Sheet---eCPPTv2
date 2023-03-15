
# Cheat Sheet - eCPPTv2 !
Cheat Sheet eCPPTv2 para el examen de certificación eCPPTv2.
- [Cheat Sheet - eCPPTv2 !](#cheat-sheet---ecpptv2--)
  * [Common Network Commands](#common-network-commands)
    + [Interfaces de red.](#interfaces-de-red)
    + [Arp -a and ip n](#arp--a-and-ip-n)
    + [route and ip r](#route-and-ip-r)
    + [ping](#ping)
    + [netstat](#netstat)
  * [Services](#services)
  * [Basic Ping discovery](#basic-ping-discovery)
- [Enumeration](#enumeration)
  * [Scanning - Nmap](#scanning---nmap)
  * [SMB - Enumeration](#smb---enumeration)
    + [CheatSheet - 1.](#cheatsheet---1)
    + [CheatSheet -2](#cheatsheet--2)
    + [Vulnerabilidades conocidas.](#vulnerabilidades-conocidas)
  * [Enumeration SSH](#enumeration-ssh)
    + [Recon](#recon)
    + [BruteForce](#bruteforce)
- [Researching Potential Vulnerabilities](#researching-potential-vulnerabilities)

## Common Network Commands
### Interfaces de red.
Permiten ver y configurar las direcciones IPv4 y IPv6 asociadas a las diferentes interfaces de red. Cada interfaz debe tener al menos, una dirección IP configurada:

     - ip a
     - ip address
     - ifconfig
     - iwconfig

  ### Arp -a and ip n
  Permiten saber la MAC asociada a una IP usando ARP.
  
     ip n
     arp -a
### route and ip r
Muestrán la routing table.

    ip r
    route
   Aveces se necesita modificar la router table para poder alcanzar alguna IP. Aqui dejo algunos ejemplos
   

    ip route add <network_ip>/<cidr> via <gateway_ip> dev <network_card_name>
    ip route add 10.0.3.0/24 via 10.0.3.1
   
   Un link con mas información. [saber más sobre route](https://devconnected.com/how-to-add-route-on-linux/)
   ### ping
   Este comando se usa para saber si un IP esta activa, si nos contesta, lo hace enviando paquetes de ICMP.
   

    ping <IP>
    ping -c 5 <IP>
   si especificamos con **-c** seguido de un número , enviara n cantidad de paquetes ICMP.  
   
### netstat
Se ocupa normalmente para identificar puertos abiertos ó servicios corriendo.

    netstat -ano
## Services
Un servicio (daemon o demonio) **es un programa que se inicia cuando se carga el sistema operativo y que se ejecuta en segundo plano**. Estos programas pueden ser servidores web, de correo, de base de datos, un cortafuegos, entre otras cosas.

    sudo service apache2 start

Tambien puede usarse el systemctl.

    sudo systemctl start apache2
    sudo systemctl stop apache2
    sudo systemctl reload nginx

Para habilitar o deshabilitar un servicio se ocupa

    sudo systemctl enable ssh

Para deshabilitarlo seria:

    sudo systemctl disable ssh

Si queremos saber el estado del servicio usamos 

    sudo systemctl status ssh

siendo que **start** se puede remplazar por restart, stop o reload. El servicio **apache2** se puede remplazar por el nombre del servicio que deseas en los ejemplos usamos los servicios de apache2, ssh, nginx.
## Basic Ping discovery 
Script basico para descubrir host activos ./pingSweep 192.168.1

    #!/bin/bash 
    if [ "$1" == "" ] then 
	    echo "You forgot an IP address!" 
	    echo "Syntax: ./ipsweep.sh 192.168.1" 
	else for ip in `seq 1 254`; do
		ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" & 
	done fi
# Enumeration
Cheatsheet para la enumeración.
## Scanning - Nmap
Usamos la metodologia de s4vitar. 

    nmap -p- --open -sS --min-rate 5000 -n -Pn -vvv <IP> -oG allPorts

Nos generará un archivo llamado allPorts , el cual esta en formato grep, nosotros con la herramienta de "extractPorts" podremos copiarnos los puertos abiertos para luego ejecutar:

    nmap -p<puertos copiados> -sC -sV <IP> -oN target

Creara un nuevo archivo en cual nos mostrará la enumeracion de nuestros puertos abiertos.
## SMB - Enumeration
Escaneando el SMB.
### CheatSheet - 1.
**domains:**
   

     smb-enum-domains

**grupos:**

    smb-enum-groups

**process:**

    smb-enum-process

**sessions:**

    smb-enum-sessions

**System Operating:**

    smb-os-discovery

**Stats:**

    smb-server-stats

**Info:**

    smb-system-info

### CheatSheet -2 
**Listar :**

    smbclient -L \\<IP> - Listing OR
    smbclient -L <IP> - Listing.

**Null Session**

    smbclient -N <IP> - Login Null Session. OR
    smbclient -N \\<IP> - Login Null Session.
**Banner Information**

     crackmapexec smb <IP> - Banner and information.

 **Connect**
	Despues de listar podemos intentar conectarnos con:
 

    smbclient \\\\x.x.x.x\\share

**Si tenemos credenciales podemos conectarnos con** 

    smbclient -U “DOMAINNAME\Username” \\\\IP\\IPC$ password
    smbclient -U "username" -P "Password" \\\\IP\\IPC$


### Vulnerabilidades conocidas.
```bash
smb-vuln-conficker (dangerous, can crash target)
smb-vuln-ms06-025 (Buffer overflow in RRAS)
smb-vuln-ms07-029 (Buffer overflow which can crash the RPC intrface in the DNS Server)
smb-vuln-ms08-067 (Buffer overflow/RCE. Dangerous, can crash the target)
smb-vuln-ms10-054 (Remote Memory Corruption. Result is BSOD -> DANGEROUS)
smb-vuln-ms10-061 (Print vulnerability. Safe and can\'t crash the target)
smb-vuln-ms17-010 (RCE, just checking if vulnerable)
```
## Enumeration SSH
Podemos checar que sea una version inverior a 7.7 el cual es vulnerable a User Enumeration.

### Recon
Podemos tratar de obtener el banner grabbing.

    telnet <IP> 22

### BruteForce
Fuerza bruta con hydra y un diccionario.

 **List of users using wordlists**

    hydra -L users.txt -P <passwordList> -t 3 -s port <IP> ssh

**Only one user and wordlist passwords**

    hydra -l root -P <passwordList> -t 3 -s port <IP> ssh


# Researching Potential Vulnerabilities
Para esta parte de busqueda de vulnerabilidades podemos usar tal cual google o buscar directamente en las siguientes paginas:

**exploit-db.com
github.com
cvedetails.com**

Tambien podemos usar la herramienta en consola **searchsploit**:

**Buscar vulnerabilidades y PoC a OpenSSH version 2.x.x**

    searchsploit OpenSSH 2.
    
   **Copia el PoC 37292.c a tu ruta actual de trabajo.**

    searchsploit -m linux/local/37292.c
 **Ver el exploit 37292.c sin copiarlo.**
 
     searchsploit -x linux/local/37292.c 


