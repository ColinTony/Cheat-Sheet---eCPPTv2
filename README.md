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
