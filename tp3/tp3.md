# TP3 : fix dns config

# Partie 1 : Setup Initial

🌞 ping d'un client du  `LAN1` vers un client du `LAN2`
~~~
PC1> ping 10.3.2.1

84 bytes from 10.3.2.1 icmp_seq=1 ttl=62 time=54.234 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=62 time=23.798 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=62 time=44.910 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=62 time=30.199 ms
84 bytes from 10.3.2.1 icmp_seq=5 ttl=62 time=65.667 ms
~~~

🌞 Capture Wireshark ping_partie1

    ping-partie1.pcapng

🌞 Afficher les adresses MAC des routeurs

- mac address `r1`
~~~
R1#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1               -   c401.0520.0001  ARPA   FastEthernet0/1
~~~

- mac adress `r2`
~~~
R2#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1              92   c401.0520.0001  ARPA   FastEthernet0/0
~~~

🌞 Prouvez que vous avez déjà un accès internet sur `r1`
-  Configurer `r1` pour qu'il fasse du NAT
~~~
R1(config)#interface FastEthernet0/0
R1(config-if)#ip nat outside
R1(config-if)#no shut
R1(config-if)#exit

R1(config)#interface FastEthernet1/0
R1(config-if)#ip nat inside
R1(config-if)#no shut
R1(config-if)#exit

R1(config)#interface FastEthernet0/1
R1(config-if)#ip nat inside
R1(config-if)#no shut
R1(config-if)#exit
~~~

🌞 Accès internet `LAN1`
~~~
PC1> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=57 time=65.353 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=57 time=56.593 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=57 time=53.104 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=57 time=43.414 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=57 time=43.367 ms
~~~
-Accès internet pour le deuxième LAN
~~~
R2#conf t
R2(config)#ip route 0.0.0.0 0.0.0.0 10.3.12.1
~~~

🌞 Accès internet `LAN2`
~~~
PC3> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=56 time=73.322 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=56 time=79.300 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=56 time=71.314 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=56 time=52.705 ms
~~~

# Partie II. Router-on-a-stick

🌞 Tests de ping `vlan`
~~~
PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=1.300 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=1.734 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=1.427 ms
~~~
🌞 Tests de ping `r1`
- ping `pc1`
~~~
R1#ping 10.3.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 32/53/64 ms
~~~
- ping `pc2`
~~~
R1#ping 10.3.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.2.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 36/55/64 ms
~~~
- ping `pc3`
~~~
R1#ping 10.3.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.2, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 28/39/60 ms
~~~
- ping `vlan1`et `vlan2`
~~~
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20024
RHOST:PORT  : 127.0.0.1:20025
MTU         : 1500

PC1> ping 10.3.2.1

84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=30.925 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=12.242 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=20.607 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=63 time=13.080 ms
~~~
🌞 Tests de ping
- ping internet `R1`
~~~
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 88/97/128 ms
~~~
- ping internet `PC1`
~~~
PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=39.872 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=114 time=35.426 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=114 time=39.135 ms
~~~
-ping internet `PC2`
~~~
PC2> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=42.607 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=114 time=36.053 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=114 time=43.748 ms
~~~
- ping internet `PC3`
~~~
PC3> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=39.456 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=114 time=39.689 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=114 time=36.481 ms
~~~
# III. Services dans le LAN
🌞 Prouvez avec un VPCS
~~~
PC4> dhcp
DORA IP 10.3.2.10/24 GW 10.3.2.254

PC4> show ip

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 1.1.1.1
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 40930, 41172/20586/36025
MAC         : 00:50:79:66:68:03
LPORT       : 20028
RHOST:PORT  : 127.0.0.1:20029
MTU         : 1500

PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208

84 bytes from 51.255.68.208 icmp_seq=1 ttl=53 time=40.271 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=53 time=72.421 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=53 time=44.058 ms
~~~
🌞 Tests résolutions DNS
- ping `dns`
~~~
PC4> ping dns.tp3.b2
dns.tp3.b2 resolved to dns.tp3.b2(10.3.3.1)

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=20.112 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=21.383 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=16.234 ms
~~~
- ping `efrei.fr`
~~~
PC4> ping efrei.fr
efrei.fr resolved to efrei.fr(10.3.3.1)

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=20.112 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=21.383 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=16.234 ms
~~~
🌞 Capture Wireshark
- capure wireshark `dns`
~~~
wireshark-dns.pcapng
~~~
# 3. HTTP
🌞 Preuve avec un client
~~~
[root@localhost marant]# curl web.tp3.b2 | head
<!doctype html>
<head>
<meta name='viewport" content='width=device-width, initial-scale=1'>
<title>HTTP Server Test Page powered by: Rocky Linux</title>


[root@localhost marant]# curl supersite.tp3.b2 | head
<!doctype html>
<head>
<meta name='viewport" content='width=device-width, initial-scale=1'>
<title>HTTP Server Test Page powered by: Rocky Linux</title>
~~~
# 1. DNS
# A. Transfert de zone
🌞 Requêter l'enregistrement AXFR
~~~
[marant@localhost ~]$ dig axfr @dns.tp3.b2 tp3.b2

; <<>> DiG 9.16.23-RH <<>> axfr @dns.tp3.b2 tp3.b2
; (1 server found)
;; global options: +cmd
tp3.b2. 86400 IN SOA dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
tp3.b2. 86400 IN NS dns.tp3.b2. 
coolsite.tp3.b2. 86400 IN A 10.3.3.4 
dns.tp3.b2. 86400 IN A 10.3.3.1 
meow.tp3.b2. 86400 IN A 10.3.3.6 
prout.tp3.b2. 86400 IN A 10.3.3.5 
supersite.tp3.b2. 86400 IN A 10.3.3.3 
web.tp3.b2. 86400 IN A 10.3.3.2 
web2.tp3.b2. 86400 IN A 10.3.3.4 
web3.tp3.b2. 86400 IN A 10.3.3.5 
web4.tp3.b2. 86400 IN A 10.3.3.6 
tp3.b2. 86400 IN SOA dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
~~~
# B. Flood
🌞 Spoof DNS query
~~~
from scapy.all import *

answer = sr1(IP(dst="dns.tp3.b2", src="10.3.2.10")/UDP(dport=53)/DNS(rd=1,qd=DNSQR(qname="tp3.b2", qtype="AXFR")),verbose=0)

print(answer[DNS].summary())
~~~
- spoof-dns.pcapng
# 2. TCP
# A. TCP RST
🌞 Mettre en place une attaque TCP RST