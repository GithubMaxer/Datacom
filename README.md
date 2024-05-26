Pc > cmd

ipconfig 

ssh -l (Admin) (default gateway ip adres)
admin wachtwoord

show lldp neighbors (Hier kan je zien hoeveelrouters, switches, access points, etc. zijn verbonden0

show lldp neighbors detail (hier zie je heel wat info zoals de snelheid van de kable, 1000baseT (HD) = 1GB)

(Tip, sinds j een tekening moet maken maar dit meteen terwijl je naar alle connecties kijkt, noteerd meteen alles)

enable
(log in)

sh run (Nu kan je de ip's van de andere routers zien)

Kijk nu naar het ip adres van 1 van de apparaten waarmee de router verbonden is, je kan met ssh verbinden met het apparaat. Voer dan de zelfde stappen weer uit. Doe dit tot dat je het hele netwerk in kaart heb gebragd

De commando show cdp neighbors kan ook gebruikt worden als vervanger voor show lldp neighbors
De commando show cdp entry * kan gebruikt worden om de op adressen te zien van de routers waarmee je moet verbinden

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
DHCP exclution

om een ip range te excluten zo dat het niet word gegeven aan apparaten gebruik je de volgende commando.

Switch(config)#ip dhcp excluded-address 192.168.20.1 192.168.20.100

Je moet de range starten bij x.1 anders werkt het niet

--------------------------------------------------------------------------------------------------------------------------------------------------------

Router> enable

Router# configure terminal

Router(config)# lldp run

Router(config)# end

Router# write memory

Testen van LLDP
Om LLDP te testen en te verifiëren dat apparaten elkaar zien:

Verbind apparaten:
Zorg ervoor dat alle apparaten die je wilt monitoren met LLDP zijn verbonden via de juiste poorten.

Gebruik de show lldp neighbors-commando op meerdere apparaten:

Voer dit commando uit op verschillende routers en switches in je netwerk om te verifiëren dat ze hun buren detecteren.
Controleer gedetailleerde informatie:

Gebruik het commando show lldp neighbors detail voor meer gedetailleerde informatie over elk gedetecteerd apparaat

---------------------------------------------------------------------------------------------------------------------------------------------------

How to configure MLS for vlans

en

conf t

vlan (vlan nummer)

interface vlan (vlan nummer)

(nu word de vlan status verandered naar up)

ip address (ip adress) (subnetmask)

exit

(doe dit voor alle vlans die je moet aan maken)

----------------------------------------------------
VTP configuratie

vtp domain (naam die je aan het domein wilt geven)

vtp mode server (De MLS (mulity layer switch) word als server in gesteld, de normalen switches worden als client ingesteld)

------------------------------------------------------------------------------------------------------------------------------------
DHCP configuratie

ip dhcp pool (naam van de dhcp, als je de dhcp aan maakt voor een vlan geeft het gewoon de naam van de vlan voor handigheid)

network (ip adress) (dus als jouw ip adres 192.168.10.0/24 is dan word de commando network 192.168.10.0 255.255.255.0)

default-router (ip adres van de vlan) (dus als je bij de vlan het ip adres 192.168.10.1 hebt gegeven dan word de commando: default-router 192.168.10.1)

---------------------------------------------------------------------------------------------------------------------------------------------------------------
interfaces in trunk zetten

De interfaces moeten in trunk gezet worden zodat meerdere ip adressen over de interfaces kunnen gaan dit doe je met de volgende commando

Router(config#)interface range Gigabitethernet1/0/1 - Gigabitethernet1/0/4 (Gigabitethernet is een voorbeeld je kan een grotere range aan geven)
Switch(config-if-range)# switchport mode trunk

-------------------------------------------------------------------------------------------------------------------------------------------------------------------
Switch configuratie

Om dit alles nu uit te delen op de switch doe je het volgende 

en

conf t

vtp domain (vtp domain naam)

vtp mode client

interface range fastethernet0/2 - fastethernet0/4

Switch(config-if-range)# switchport mode acces

Switch(config-if-range)# switchport access vlan 10(vlan 10 is een voorbeeld, zet het juiste vlan nummer neer)

Switch(config-if-range)# no shutdown (no shutdown hoef je alleen uittevoeren als de kabel status down is, dit kan je zien aan de rode puntjes op de kabels. De no shutdown commando verandered de status van de kabel van down naar up)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PC configuratie

Ga naar de pc en zet het ip naar DHCP.

Als je de error krijgt "DHCP failed. APIPA is being used" dan moet je gaan trouble shooten.

check dat alle kabels die op trunk horen te staan op trunk zijn.

check dat alle kabels die op access moeten zijn op access zijn

check de dhcp configuratie

check de vtp configuratie

zet ip routing aan indien nodig. Dit doe je met de commando: ip routing

----------------------------------------------------------------------------------------------------------------------
Dit is een voorbeeld voor de configuratie op de MLS

!
version 16.3.2

no service timestamps log datetime msec

no service timestamps debug datetime msec

no service password-encryption

hostname Switch


ip dhcp pool vlan10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 
 dns-server 10.10.10.1
ip dhcp pool vlan20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 
ip dhcp pool vlan30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 
ip dhcp pool vlan40
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 
ip dhcp pool vlan50
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.1

no ip cef
no ipv6 cef


spanning-tree mode pvst

interface GigabitEthernet1/0/1
 no switchport
 ip address 10.10.10.2 255.255.255.252
 duplex auto
 speed auto

interface GigabitEthernet1/0/2
 switchport access vlan 50
 switchport mode trunk

interface GigabitEthernet1/0/3
 switchport access vlan 50
 switchport mode trunk

interface GigabitEthernet1/0/4
 switchport access vlan 50
 switchport mode trunk

interface GigabitEthernet1/0/5

interface GigabitEthernet1/0/6

interface GigabitEthernet1/0/7

interface GigabitEthernet1/0/8

interface GigabitEthernet1/0/9

interface GigabitEthernet1/0/10

interface GigabitEthernet1/0/11

interface GigabitEthernet1/0/12

interface GigabitEthernet1/0/13

interface GigabitEthernet1/0/14

interface GigabitEthernet1/0/15

interface GigabitEthernet1/0/16

interface GigabitEthernet1/0/17

interface GigabitEthernet1/0/18

interface GigabitEthernet1/0/19

interface GigabitEthernet1/0/20

interface GigabitEthernet1/0/21

interface GigabitEthernet1/0/22

interface GigabitEthernet1/0/23

interface GigabitEthernet1/0/24

interface GigabitEthernet1/1/1

interface GigabitEthernet1/1/2

interface GigabitEthernet1/1/3

interface GigabitEthernet1/1/4

interface Vlan1
 no ip address
 shutdown

interface Vlan10
 mac-address 0009.7c09.8801
 ip address 192.168.10.1 255.255.255.0

interface Vlan20
 mac-address 0009.7c09.8802
 ip address 192.168.20.1 255.255.255.0

interface Vlan30
 mac-address 0009.7c09.8803
 ip address 192.168.30.1 255.255.255.0

interface Vlan40
 mac-address 0009.7c09.8804
 ip address 192.168.40.1 255.255.255.0

interface Vlan50
 mac-address 0009.7c09.8805
 ip address 192.168.50.1 255.255.255.0

--------------------------------------------------------------------------------------------------------------------------------------
Dit is een voorbeeld van de configuratie op de switch


version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption

hostname Switch



spanning-tree mode pvst
spanning-tree extend system-id

interface FastEthernet0/1
 switchport mode trunk

interface FastEthernet0/2
 switchport access vlan 10
 switchport mode access

interface FastEthernet0/3
 switchport access vlan 10
 switchport mode access

interface FastEthernet0/4
 switchport access vlan 10
 switchport mode access

interface FastEthernet0/5
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/6
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/7
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/8
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/9
 switchport access vlan 20
 switchport mode access

interface FastEthernet0/10
 switchport access vlan 30
 switchport mode access

interface FastEthernet0/11
 switchport access vlan 30
 switchport mode access

interface FastEthernet0/12
 switchport access vlan 30
 switchport mode access

interface FastEthernet0/13
 switchport access vlan 30
 switchport mode access

interface FastEthernet0/14
 switchport access vlan 30
 switchport mode access


interface FastEthernet0/15
 switchport access vlan 40
 switchport mode access

interface FastEthernet0/16
 switchport access vlan 40
 switchport mode access

interface FastEthernet0/17
 switchport access vlan 40
 switchport mode access

interface FastEthernet0/18
 switchport access vlan 40
 switchport mode access

interface FastEthernet0/19
 switchport access vlan 40
 switchport mode access

interface FastEthernet0/20
 switchport access vlan 50
 switchport mode access

interface FastEthernet0/21
 switchport access vlan 50
 switchport mode access

interface FastEthernet0/22
 switchport access vlan 50
 switchport mode access

interface FastEthernet0/23
 switchport access vlan 50
 switchport mode access

interface FastEthernet0/24
 switchport access vlan 50
 switchport mode access

interface GigabitEthernet0/1

interface GigabitEthernet0/2

interface Vlan1
 no ip address
 shutdown

line con 0

line vty 0 4
 login
line vty 5 15
 login


end

------------------------------------------------------------------------------------------------------------

OSPF instellen

Router

Router> enable

Router# configure terminal

Router(config)# router ospf 1

Router(config-router)# network 10.10.10.0 0.0.0.3 area 0

Router(config-router)# network 10.10.11.0 0.0.0.3 area 0

Router(config-router)# network 192.168.10.0 0.0.0.255 area 0

Router(config-router)# end

Router#

Letop: wanneer je OSPF configureerd moet elk netwerk in de zelfde aera zitten, in dit geval is dat area 0.
Als je dat niet doet ga je errors krijgen. 

Het genen na het ip adres is de wild card mask van het ip adres. Als je niet weet wat de wildcard mask is van jou subnet mask kan je dit makkelijk opzoeken.

De wildcard mask van een /24 adres (dus 255.255.255.0) is 0.0.0.255.

De wildcard mask van een /30 adres (dus 255.255.255.252) is 0.0.0.3

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
MLS configuraties.

Switch> enable

Switch# configure terminal

Switch(config)# ip routing (ip routing moet je instellen anders kan je ospf niet instellen)

Switch(config)# router ospf 1

Switch(config-router)# network 10.10.10.0 0.0.0.3 area 0

Switch(config-router)# network 192.168.10.0 0.0.0.255 area 0

Switch(config-router)# network 192.168.20.0 0.0.0.255 area 0

Switch(config-router)# network 192.168.30.0 0.0.0.255 area 0

Switch(config-router)# network 192.168.40.0 0.0.0.255 area 0

Switch(config-router)# network 192.168.50.0 0.0.0.255 area 0

Switch(config-router)# end

Switch#

----------------------------------------------------------------------------------------------------------------------------------------------------------------

Om ospf te controleren kan je de volgende commandos gebruiken
Router# show ip ospf neighbor
Switch# show ip ospf neighbor

Router# show ip route ospf
Switch# show ip route ospf

-------------------------------------------------------------------------------------------------------

SSH configureren

1.	Typ enable en dan config t
2.	
3.	Configureer de hostname met de commando hostname <host name>
4.	
5.	Typ nu de commando ip domain-name <domain name>
6.	
7.	Type nu de commando crypto key generate rsa. Wanneer het je vraagt hoeveel bits typ 2048 
  
8.	Voer nu de commando ip ssh version 2
   
10.	Typ nu de commando’s line vty 0 4 > transport input ssh > login local
    
12.	Typ nu de command username <username> password <wachtwoord> en dan exit
    
---------------------------------------------------------------------------------------------------------------
Router>en

Router#conf t

Enter configuration commands, one per line.  End with CNTL/Z.

Router(config)#hostname test123

test123(config)#ip domain-name Bomba

test123(config)#cry

test123(config)#crypto k

test123(config)#crypto key g

test123(config)#crypto key generate r

test123(config)#crypto key generate rsa 

The name for the keys will be: test123.Bomba

Choose the size of the key modulus in the range of 360 to 4096 for your

  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 2048

% Generating 2048 bit RSA keys, keys will be non-exportable...[OK]

test123(config)#ip ssh ver

*Mar 1 1:38:12.701: %SSH-5-ENABLED: SSH 1.99 has been enabled

test123(config)#ip ssh version 2

test123(config)#lin

test123(config)#line v

test123(config)#line vty 0 4

test123(config-line)#tr

test123(config-line)#transport i

test123(config-line)#transport input s

test123(config-line)#transport input ssh 

test123(config-line)#lo

test123(config-line)#login loca

test123(config-line)#login local 

test123(config-line)#us

test123(config-line)#user

test123(config-line)#username amdin password admin

Router0(config)#enable password admin

test123(config)#end

test123#

%SYS-5-CONFIG_I: Configured from console by console

test123#copy run start

Destination filename [startup-config]? 

Building configuration...

[OK]

test123#
