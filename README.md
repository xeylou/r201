# r201

## nat/pat dynamique

la spécification `overload` spécifie le pat

```
ip nat inside
ip nat outside

access-list 10 permit any
# qui est autorisé à être naté ou non
# /!\ acl a ne mettre sur aucune interface
# access-list <0-99> deny/permit <@rzo/host @ip> <netmask>

ip nat pool toto 10.2.18.4 10.2.18.8 netmask 255.255.255.0
# définition du pool d'adresse externe
# exemple pour poste titi
# ip nat pool <nom> <@ip_min> <@ip_max> netmask <netmask>

ip nat inside source list 10 pool toto overload
# activation du nat 
# ip nat outside source list <acl_num> pool <pool_name> overload
OU
ip nat inside source list <acl_num> interface <int> overload
# activation avec la seule adresse de l'interface externe
```

## nat/pat statique

### nat statique

```
ip nat inside source static <@local> <@global>
# pour naté une seule machine sur tous ses ports

ip nat inside source static <tcp/udp> <@ip_locale_equip> <port_local_utilisé> <@globale> <port_global_donné_sur_routeur>
# nat une machine avec un port spécifique, cool pour accéder à son serveur blabla de dehors
```