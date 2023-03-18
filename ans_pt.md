5.2.1
Router0(config)#access-list 1 deny host 192.168.1.20
Router0(config)#access-list 1 permit any
Router0(config)#int f0/0
Router0(config-if)#ip access-group 1 in
Router0(config-if)#no ip access-group 1 in


5.2.2
Router0(config)#access-list 1 deny host 192.168.1.10
Router0(config)#access-list 1 permit any
Router0(config)#int s0/0/0
Router0(config-if)#ip access-group 1 out
Router0(config-if)#no ip access-group 1 out


5.2.3
Router0(config)#access-list 1 deny 192.168.2.0 0.0.0.255
Router0(config)#access-list 1 permit any
Router0(config)#int s0/0/0
Router0(config-if)#ip access-group 1 out
Router0(config-if)#no ip access-group 1 out


5.3.1
Router0(config)#ip access-list standard anti-pc2
Router0(config-ext-nacl)#deny 192.168.2.0 0.0.0.255
Router0(config-ext-nacl)#permit any 
Router0(config-ext-nacl)#int s0/0/0
Router0(config-if)#ip access-group anti-pc2 in


5.4.1
avec une acl standard on ne peut préciser quel host ne peut communiquer avec quel host


5.4.2
Router0(config)#access-list 100 deny ip host 192.168.1.10 host 192.168.2.10
Router0(config)#access-list 100 permit ip any any 
Router0(config)#int s0/0/0
Router0(config-if)#ip access-group 100 out
Router0(config-if)#no ip access-group 100 out


5.4.3
Router1(config)#access-list 100 deny icmp any host 192.168.4.12
Router1(config)#access-list 100 permit ip any any
Router1(config-if)#ip access-group 100 out
Router1(config-if)#no ip access-group 100 out


5.4.4
Router0(config)#access-list 100 deny ip host 192.168.1.10 host 192.168.4.12
Router0(config)#access-list 100 permit ip any any
Router0(config)#int s0/0/0
Router0(config-if)#ip access-group 100 out

5.4.5
Router1(config)#access-list 100 deny icmp host 192.168.1.20 host 192.168.4.12
Router1(config)#access-list 100 deny tcp host 192.168.1.20 host 192.168.4.12 eq www
Router1(config)#access-list 100 permit ip any any
Router1(config)#int f0/1
Router1(config-if)#ip access-group 101 out
Router1(config-if)#no ip access-group 101 out
 
5.5.1
Router1(config)#ip access-list extended anti-web2
Router1(config-ext-nacl)#deny icmp host 192.168.1.20 host 192.168.4.12
Router1(config-ext-nacl)#permit ip any any 
Router1(config-ext-nacl)#int f0/1
Router1(config-if)#ip access-group anti-web2 out
Router1(config-if)#no ip access-group anti-web2 out

5.6.1
note: les acl crées en ipv4 de 5.2 à 5.5 avec 5.4

Router0(config)#ipv6 access-list anti-pc2
Router0(config-ipv6-acl)#deny ipv6 host fc00:1::20 any
Router0(config-ipv6-acl)#permit ipv6 any any
Router0(config)#int s0/0/0
Router0(config-if)#ipv6 traffic-filter anti-pc2 out
Router0(config-if)#no ipv6 traffic-filter anti-pc2 out

Router0(config)#ipv6 access-list anti-pc1
Router0(config-ipv6-acl)#deny ipv6 host fc00:1::10 host fc00:2::10
Router0(config-ivp6-acl)#deny ipv6 host fc00:1::10 host fc00:2::20
Router0(config-ipv6-acl)#permit ipv6 any any
Router0(config)#int s0/0/0
Router0(config-if)#ipv6 traffic-filter anti-pc1 out
Router0(config-if)#no ipv6 traffic-filter anti-pc1 out

Router0(config)#ipv6 access-list anti-rzo1
Router0(config-ipv6-acl)#deny ipv6 fc00:1::/64 fc00:2::/64
Router0(config-ipv6-acl)#permit ipv6 any any
Router0(config-ipv6-acl)#int s0/0/0
Router0(config-if)#ipv6 traffic-filter anti-rzo1 out
Router0(config-if)#no ipv6 traffic-filter anti-rzo1 out

Router0(config)#ipv6 access-list pc1-pc3
Router0(config-ipv6-acl)#deny ipv6 host fc00:1::10 host fc00:2::20
Router0(config-ipv6-acl)#permit ipv6 any any
Router0(config-ipv6-acl)#int s0/0/0
Router0(config-if)#ipv6 traffic-filter pc1-pc3 out
Router0(config-if)#no ipv6 traffic-filter pc1-pc3 out

Router1(config)#ipv6 access-list anti-ping
Router1(config-ipv6-acl)#deny icmp any host fc00:4::12
Router1(config-ipv6-acl)#permit ipv6 any any
Router1(config-ipv6-acl)#int f0/1
Router0(config-if)#ipv6 traffic-filter anti-ping out
Router0(config-if)#no ipv6 traffic-filter anti-ping out

je pense avoir compris, je ne fais pas avant dernière et dernière du 5.4












