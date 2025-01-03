## 1 Most simplest LAN

#### Déterminer l'adresse MAC de vos deux machines
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```
-> L'adresse MAC de PC 1 est 00:65:69:71:50:00

```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
-> L'adresse MAC de PC 2 est 00:65:69:71:50:01

#### Définir une IP statique sur les deux machines
On donne une IP à PC1 :
```
PC1> ip 10.1.1.1/24
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0
```

On vérifie que le changement d'IP est effectif pour PC 1 :
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```

On donne une IP à PC2 :
```
PC2> ip 10.1.1.2/24
Checking for duplicate address...
PC2 : 10.1.1.2 255.255.255.0
```

On vérifie que le changement d'IP est effectif pour PC 2:
```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```

####  Effectuer un `ping` d'une machine à l'autre

```
PC1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.302 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=1.539 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=1.405 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=1.170 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.281 ms
```

```
PC2> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=0.185 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.165 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=0.367 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=64 time=0.606 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=64 time=0.453 ms
```

#### Wireshark !

[Ping entre les deux machines pcapng](ping_deux_machines.pcapng)

C'est le protocole ICMP est utilisé pour envoyer le message `ping`.

#### ARP

```
PC1> arp          

00:65:69:71:50:01  10.1.1.2 expires in 108 seconds
```

```
PC2> arp

00:65:69:71:50:00  10.1.1.1 expires in 106 seconds
```

## 2 Ajoutons un switch
#### Déterminer l'adresse MAC de vos trois machines
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```
```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
```
PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```

#### Définir une IP statique sur les trois machines
```
PC3> ip 10.1.1.3/24
Checking for duplicate address...
PC3 : 10.1.1.3 255.255.255.0
```
```
PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         : 
MAC         : 00:65:69:71:50:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```

#### Effectuer des ping d'une machine à l'autre
```
PC3> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=2.453 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.547 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.702 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.451 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.334 ms
```

```
PC1> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.471 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=1.475 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=0.353 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.437 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.376 ms
```

## 3 Serveur DHCP
#### Donner un accès Internet à la machine dhcp.tp1.efrei
```
[toto@efrei-xmg4agau1 ~]$ ping google.com
PING google.com (216.58.214.174) 56(84) bytes of data.
64 bytes from mad01s26-in-f174.1e100.net (216.58.214.174): icmp_seq q = 1 ttl I = 63 time 37.0 ms
64 bytes from par10s42-in-f14.1e100.net (216.58.214.174): icmp_seq=2 ttl I = 63 time=29.4 ms
```

#### Installer et configurer un serveur DHCP

```
dnf-y install dhcp-server
vi /etc/dhcp/dhcpd.conf
systemctl enable --now dhcpd
```

```fichier_dhcpd_conf
authoritative;
subnet 10.1.1.0 netmask 255.255.255.0 {
	range 10.1.1.10 10.1.1.50;
	option broadcast-address 10.1.1.1;
	option routers 10.1.1.1;
}
```

#### Récupérer une IP automatiquement depuis les 3 nodes
```
PC1> dhcp
DORA IP 10.1.1.10/24 GW 10.1.1.1

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 10.1.1.1
DNS         : 
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 563, 570/285/498
MAC         : 00:65:69:71:50:01
LPORT       : 20007
RHOST:PORT  : 127.0.0.1:20008
MTU         : 1500
```

```
PC2> dhcp
DDORA IP 10.1.1.12/24 GW 10.1.1.1

PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.12/24
GATEWAY     : 10.1.1.1
DNS         : 
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 594, 599/299/524
MAC         : 00:65:69:71:50:00
LPORT       : 20009
RHOST:PORT  : 127.0.0.1:20010
MTU         : 1500
```

```
PC3> dhcp
DDORA IP 10.1.1.11/24 GW 10.1.1.1

PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.11/24
GATEWAY     : 10.1.1.1
DNS         : 
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 584, 599/299/524
MAC         : 00:50:79:66:68:02
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500
```

#### Wireshark !
[Echange DHCP pcapng](echange_dhcp.pcapng)

## 4 DHCP spoofing

#### Configurez dnsmasq
- Installation de dnsmasq
```
dnf install -y dnsmasq
```
- Configuration de dnsmasq
```
port=0 # pour désactiver DNS
dhcp-range=10.1.1.210,10.1.1.250,255.255.255.0,12h
dhcp-authoritative
interface=enp0s3
```

#### Test !
```
PC4> dhcp
DDORA IP 10.1.1.248/24 GW 10.1.1.50
```

```
PC4> show ip

NAME        : PC4[1]
IP/MASK     : 10.1.1.248/24
```

#### Now race !

```
PC2> dhcp
DDORA IP 10.1.1.246/24 GW 10.1.1.50
```
```
PC3> dhcp
DDORA IP 10.1.1.247/24 GW 10.1.1.50
```
```
PC4> dhcp
DORA IP 10.1.1.21/24 GW 10.1.1.1
```

#### Wireshark !

[Race pcapng](race.pcapng)
