---
layout: post
title: Comandos de red, consola y troubleshooting Linux
categories: [blog, net]
tags: [linux, consola, iptables, comandos, reload]
---

Imprescindibless
<pre># man "comando" entrega documentación de cada comando.</pre>
Ver y manejar procesos.
<pre># htop</pre>
Gráficas de trafico en interface
<pre># speedometer -r eth0 -t eth0</pre>
Bandwidth Monitor
<pre># bmon</pre>
iperf mide ancho de banda entre dos maquinas, se puede hacer TCP (real, con confirmación) o UDP (FTP sin confirmación de paquete). Si modificamos el tamaño de ventana vemos como aumenta el ancho de banda.
<pre>root@servidor:~# iperf -s
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size: 85.3 KByte (default)
------------------------------------------------------------
[  4] local IP_SERVIDOR port 5001 connected with IP_CLIENTE port 33475
[ ID] Interval       Transfer     Bandwidth
[  4]  0.0-10.0 sec   113 MBytes  94.1 Mbits/sec</pre>

<pre>root@cliente:~# iperf -c IP_SERVIDOR
------------------------------------------------------------
Client connecting to IP_SERVIDOR, TCP port 5001
TCP window size: 43.8 KByte (default)
------------------------------------------------------------
[  3] local IP_CLIENTE port 33475 connected with IP_SERVIDOR port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec   113 MBytes  94.4 Mbits/sec</pre>
Monitor de sistema en la consola
<pre># nmon</pre>
Ver uso de RAM
<pre># free</pre>
Ver uso de HD
<pre># df -h</pre>
Memoria virtual SWAP
<pre># vmstat</pre>
Puertos y conexiones ejemplos en: <a href="http://www.tecmint.com/20-netstat-commands-for-linux-network-management/">http://www.tecmint.com/20-netstat-commands-for-linux-network-management/</a>
<pre># netstat -tulpn</pre>
Ver trafico en interfaces, muy útil cuando nos conectamos a una red desconocida.
<pre># iptraf</pre>
Traceroute
<pre># traceroute guifi.net</pre>
Ver ARP
<pre># arp -a</pre>
Descarga directa de fichero en WEB
<pre># wget</pre>
Buscar maquinas RAPIDO   <a href="https://nmap.org/man/es/">https://nmap.org/man/es/</a>
<pre># nmap -sP 192.168.0.*</pre>
Ver negociacion de interfaces
<pre># ethtool -a eth0</pre>
Saber si un puerto está abierto:
<pre># nc -zv 192.168.122.88 80</pre>
Y no podía faltar, el todo poderoso "PING" Packet InterNet Groper
<pre># ping guifi.net</pre>
Ver gateway y tabla de rutas
<pre>#route -n</pre>

<strong>CONSOLA</strong>

Para toma de contacto con la consola y con un montón de lenguajes de programación está disponible la web 
<a href="https://www.codecademy.com/">https://www.codecademy.com/</a>, antes no era de pago.

Otro sitio para empezar de cero con la consola, <a href="http://www.webminal.org/">http://www.webminal.org/</a> podemos practicar y aprender comandos en la consola online, script, phython...

<a href="http://www.playterm.org/">http://www.playterm.org/ </a> consola online para practicar y con ejemplos.

En <a href="http://www.tecmint.com">http://www.tecmint.com</a>/category/linux-commands/ hay mucha mucha información, aunque está todo en ingles la web no tiene desperdicio, hay miles de how to, manuales, consejos, libros y en la sección de comandos, da gusto porque está todo muy bien explicado.

En <a href="http://linuxzoo.net/">linuxzoo</a> tenemos acceso a maquinas virtuales completas.

<a href="http://www.commandlinefu.com/">http://www.commandlinefu.com/</a> recopilación de útiles lineas de comando.

grc entrega resaltado con colores las salidas de los comandos.

<pre># apt-get install grc</pre>
ejemplo
<pre># grc cat /var/log/syslog</pre>
Le penemos un alias y ya está para siempre, no sirve para todos los comandos.
<pre># alias cat='grc cat'</pre>

<strong>iptables</strong>

<pre># service iptables stop start restart </pre>
Para listar reglas
<pre>
iptables -nL
iptables -L -n -v
iptables -n -L -v --line-numbers
iptables -L INPUT -n -v
iptables -L OUTPUT -n -v --line-numbers
</pre>
<pre>
-L : Muestra las reglas.
-v : Detalles.
-n : Acelera la entrega, sin traducción.
</pre>
Para borrar las reglas
<pre>
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
</pre>
<pre>
-F : Borra todas las reglas.
-X : Borra cadenas
-t table_name : Selecciona una tabla y elimina reglas
-P : Establece la polÃ­tica por defecto (como DROP, REJECT o ACCEPT)
</pre>
GUARDAR REGLAS

Una vez que se apliquen reglas en iptables, se perderán cuando se reinicia el sistema. Para que se mantengan (una vez aplicadas las que queremos), deberemos hacer lo siguiente:
<pre># iptables-save &gt; /etc/firewall.conf </pre>
Crearemos el archivo /etc/network/if-up.d/iptables y dentro pondremos:
<pre>#!/bin/sh
iptables-restore &lt; /etc/firewall.conf</pre>
Hacerlo ejecutable y listo:
<pre>chmod +x /etc/network/if-up.d/iptables</pre>
Cualquier cambio que queramos hacer será modificando el archivo /etc/firewall.conf o ejecutando las reglas por comandos y salvando de nuevo como al principio:
<pre># iptables-save &gt; /etc/firewall.conf</pre>

* Habilitar el enrutamiento entre tarjetas de red de nuestro equipo:
<pre>#echo 1 &gt; /proc/sys/net/ipv4/ip_forward</pre>

Ociones de tablas:

Para la tabla filtros:
<pre>
INPUT: Indica paquetes recibidos.
OUTPUT: Indica paquetes salientes.
FORWARD: Indica paquetes que se reciben pero que no son para nosotros sino que se enroutan de nuevo.
</pre>
Para la tabla NAT:
<pre>
PREROUTING: Permite traducir direcciones de paquetes entrantes a ips NAT.
POSTROUTING: Permite traducir direcciones NAT de paquetes salientes a la correspondiente ip.
OUTPUT: Indica paquetes salientes.
</pre>
Para la tabla Mangle:
<pre>
INPUT: Indica paquetes recibidos.
PREROUTING: Permite traducir direcciones de paquetes entrantes a ips NAT.
POSTROUTING: Permite traducir direcciones NAT de paquetes salientes a la correspondiente ip.
OUTPUT: Indica paquetes salientes.
FORWARD: Indica paquetes que se reciben pero que no son para nosotros sino que se enrutan de nuevo.
</pre>
Para la tabla RAW:
<pre>
PREROUTING: Permite traducir direcciones de paquetes entrantes a ips NAT.
OUTPUT: Indica paquetes salientes.
</pre>
* Máxima restricción:
<pre>
# IPTABLES -P INPUT   DROP
# IPTABLES -P OUTPUT  DROP
# IPTABLES -P FORWARD DROP
</pre>
TABLA FILTER Tabla por defecto permite, detiene, redirecciona, según cadena input output forward hay 3 cadenas por defecto.

<pre>TABLA NAT
 # iptables -t nat -L
 Chain PREROUTING (policy ACCEPT)
 target     prot opt source               destination

Chain INPUT (policy ACCEPT)
 target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
 target     prot opt source               destination

Chain POSTROUTING (policy ACCEPT)
 target     prot opt source               destination</pre>

TABLA MANGLE  Combina las tablas, permite QOS nat filtrado y mas cosas

<pre># iptables -t mangle -L
 Chain PREROUTING (policy ACCEPT)
 target     prot opt source               destination

Chain INPUT (policy ACCEPT)
 target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
 target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
 target     prot opt source               destination

Chain POSTROUTING (policy ACCEPT)
 target     prot opt source               destination</pre>
 
* El iptables-persistent sirve para almacenar las reglas.

NOTA: Al configurar un firewall sobre un sistema remoto, se puede configurar un cron job que ejecute service iptables-persistent restart periódicamente (por ejemplo cada 5 minutos) para no quedar "fuera", o sin acceso al sistema. De esta forma, si se agrega una nueva regla, y la misma nos deja fuera del sistema, sólo hay que esperar un período de tiempo hasta que el firewall se reinicie automáticamente. En cambio, si la regla funciona, es posible guardarla ejecutando service iptables-persistent save. Al finalizar con las modificaciones eliminar el cron job.

<strong>RELOAD IN en Linux</strong>

<strong>Apagar</strong> la maquina ya:
<pre><strong># </strong>sudo shutdown -<strong>r</strong></pre>
<strong>Apagar</strong> la maquina después de 10 minutos:
<pre> # sudo shutdown -<strong>h</strong> +10
The system is going down for power-off at ...</pre>
<strong>Apagar</strong> la maquina a una hora determinada:
<pre><strong>#</strong> shutdown -<strong>h</strong> 15:30
The system is going down for power-off at ...</pre>
<strong>Reiniciar</strong> la maquina ya:
<pre><strong># </strong>shutdown -<strong>r</strong> now</pre>
<strong>Reiniciar</strong> la maquina después de 7 minutos:
<pre> # shutdown -<strong>r</strong> +7
The system is going down for reboot at ...</pre>
<strong>Reiniciar</strong> a las 5 de la tarde:
<pre><strong>#</strong> shutdown -<strong>r</strong> 17:00
The system is going down for reboot at ...</pre>
<strong>Cancelar orden:</strong>
<pre><strong># shutdown -c</strong></pre>
<pre>The system shutdown has been cancelled at ...</pre>


