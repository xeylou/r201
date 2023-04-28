## les acl cisco & leur fonctionnement

définissent un ensemble de règles sur un réseau afin de filtrer certais flux, au niveau de la couche 3 (ip) ou 4 (tcp/udp) sur un routeur cisco

### définition vidéo bascou 

peuvent être mises avant la décision de routage ou après au niveau de la sortie quand l'interface a été définie. pas de formes de procès ni d'avertissement à l'expéditeur, suppression seule

1 unique acl par sens, par interface & par proto. de couche 3 (ipv4 ou ipv6)

<condition> <action> 
<proto> <permit/deny>

**lues dans l'ordre, extrêmement important /!\**

par défaut interdit

tout ce qu'on fait avec une acl standard on peut la faire avec une acl étendue

en ipv6 que des étendues

acl numérique peut pas supprimer erreur, donc tout supprimer

acl ne porte que sur l'@ip_source

access-list 1 permit any
access-list 1 deny any

**/!\ MISE EN IN OU EN OUT**
  
```
ipv6 traffic-filter MonACL in/out
```
  
tout dans la running config



### acl standard

regarde l'@ip_source d'une trame, accorde ensuite l'envoi vers 
l'@ip_destination si permit

leur identifiant vont de 1 -> 99 ou de 1300 -> 1999

peuvent être utilisées pour autoriser ou interdire la communication
d'une machine ou d'une partie d'un réseau à une autre partie d'un
réseau ou à un groupement de machines du réseau


exemple:
interdire au réseau 10.1.1.0/24 de communiquer avec 10.1.2.0/24 mais lui
laisser la possibilité de communiquer avec 10.1.3.0/24
```
Robert(config)# access-list 1 deny 10.1.2.0 0.0.0.255
Robert(config)# access-list 1 permit 10.1.3.0 0.0.0.255
```
ce sont des /24, les 0 & les 255 sont inversés

applications des acl à un réseau
```
Router(config)# interface gigabitEthernet 0/2
Router(config-if)# ip access-group 1 out
```
la règle est appliquée aux paquets sortants *out* de l'interface g0/2


### acl étendues

les acl étendues peuvent vérifier plus de champs que les acl standards

numéro des acl étendues:
100 -> 199 & 200 -> 2699

```
Robert(config)# access-list 100 deny icmp 10.1.3.0 0.0.0.255 10.1.2.0 0.0.0.255
Robert(config)# access-list 100 permit icmp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255
Robert(config)# interface gig 0/2
Robert(config-if)# ip access-group 100 in
```
pour les machines sur une interface (réseau 10.1.2.0/24):
empêche les pings en interdisant l'utilisation du protocole icmp vers le réseau 10.1.3.0/24
mais l'autorise vers les équipements des machines sur le réseau 10.1.1.0/24
```
Robert(config)# access-list 100 deny ip host 192.168.0.1 any
Robert(config)# access-list 100 deny ip 192.168.0.0 0.0.0.255 any
```
__partie source & distination après__

note: on peut préciser aux acl étendues quel port spécifique nous souhaitons bloquer la
communication avec, *eq* voulant dire égal pour le port
```
Robert(config)# access-list 100 permit tcp 10.1.1.0 0.0.0.255 10.1.2.0 0.0.0.255 eq 21
```
*autorise le réseau 10.1.1.0/24 à communiquer en utilisant le protocol ftp avec 10.1.2.0/24*

que des nommées pour ipv6



### exemple plus complets

```
Robert(config)# access-list 101 deny tcp host 192.168.0.1 192.168.1.0 0.0.0.255 eq ftp
Robert(config)# access-list 101 deny tcp host 192.168.0.1 host 172.16.1.100 eq www
Robert(config)# access-list 101 permit any any
```
*interdit la machine avec l'@ip 192.168.0.1 de communiquer avec le réseau 192.68.1.0/24
en utilisant ftp*
*interdiction de communication par échange web entrer deeux machines: 192.168.0.1 & 172.16.1.100*
__/!\ mais autorise l'utilisation d'autres ports__
```
Robert(config)# int g0/0
Robert(config)# ip access-group 101 in
```


### acl nommées

permet d'utiliser un nom plutôt qu'un nombre pour identifier les acl étendues ou standard

```
Robert(config)# ip access-list extended David-HTTP
```
*création d'une acl nommée + rentre dans la configuration de cette acl*
```
Robert(config)# deny tcp host 192.168.0.1 host 172.16.1.100 eq www
```
*création de la règle interdisant toute communication web entre deux machines*
```
Robert(config)# permit ip any any
```


### réflexions finales

acl standard sont placés généralement près de l'host (car très restrictives)
acl étendues près de la destination
  
