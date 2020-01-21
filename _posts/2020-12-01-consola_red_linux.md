---
layout: linux
title: Comandos de red, consola y troubleshooting 
---
<ol>
 	<li>Solución de problemas y comandos a tener en cuenta</li>
</ol>
<pre># man "comando" entrega documentación de cada comando.</pre>

Ver y manejar procesos.
<pre># htop</pre>
Gráficas de trafico en interface
<pre># speedometer -r eth0 -t eth0</pre>
Bandwidth Monitor
<pre># bmon</pre>
Medir ancho de banda entre dos maquinas, se puede hacer TCP (real, con confirmación) o UDP (FTP sin confirmación de paquete). Si modificamos el tamaño de ventana vemos como aumenta el ancho de banda.
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
<pre># free

</pre>
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
Saber si un puerto está o no abierto:<span id="ezoic-pub-ad-placeholder-107" class="ezoic-adpicker-ad" data-ezadpicker="107" data-ez-position-type="long_content"></span>

<code>nc -zv 192.168.122.88 80</code>

Y no podía faltar, el todo poderoso "PING" <b>Packet InterNet Groper</b>
<pre># ping guifi.net</pre>
