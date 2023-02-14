- [Cheat Sheet - eCPPTv2 !](#cheat-sheet---ecpptv2--)
  * [Common Network Commands](#common-network-commands)
    + [Interfaces de red.](#interfaces-de-red)
    + [Arp -a and ip n](#arp--a-and-ip-n)
    + [route and ip r](#route-and-ip-r)
    + [ping](#ping)
    + [netstat](#netstat)
  * [Services](#services)
  * [Basic Ping discovery](#basic-ping-discovery)

# Cheat Sheet - eCPPTv2 !
Cheat Sheet eCPPTv2 para el examen de certificación eCPPTv2.

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
