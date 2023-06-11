

# AS2

## rt15（R2）

```
sys
sysname rt15

router id 2.1.1.15 

interface GigabitEthernet0/0/0
 ip address 95.0.0.254 255.255.255.252 
 
 interface GigabitEthernet0/0/1
 ip address 95.2.2.15 255.255.255.0
 
 interface GigabitEthernet0/0/2
 ip address 95.2.3.15 255.255.255.0
 
interface LoopBack0
 ip address 2.1.1.15 255.255.255.255 

interface LoopBack1
 ip address 192.168.2.15 255.255.255.255
 quit
 
ip ip-prefix permit_snmp_in index 10 permit 192.168.2.0 24
ip ip-prefix permit_snmp_out index 10 permit 192.168.1.0 24
ip ip-prefix permit_snmp_out index 20 permit 192.168.3.0 24
ip ip-prefix permit_export index 10 permit 95.4.126.242 32
ip ip-prefix permit_export index 20 permit 192.168.2.0 24
ip ip-prefix permit_export index 30 permit 95.4.10.0 24
ip ip-prefix permit_export index 40 permit 95.4.11.0 24
ip ip-prefix permit_export index 50 permit 95.4.12.0 24
ip ip-prefix permit_export index 60 permit 95.4.13.0 24
 
 route-policy import_ospf permit node 10 
 if-match ip-prefix permit_snmp_in 
 
route-policy import_bgp permit node 10 
 if-match ip-prefix permit_snmp_out 
 
 
 bgp 2
 peer 95.0.0.253 as-number 1 
 group as2 internal
 peer as2 connect-interface LoopBack0
 peer 2.1.1.16 as-number 2 
 peer 2.1.1.16 group as2 
 peer 2.1.1.17 as-number 2 
 peer 2.1.1.17 group as2 
 peer 2.1.1.18 as-number 2 
 peer 2.1.1.18 group as2 
  undo synchronization
  default local-preference 200
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.253 enable
  peer 95.0.0.253 ip-prefix permit_export export
  peer as2 enable
  peer as2 next-hop-local 
  peer 2.1.1.16 enable
  peer 2.1.1.16 group as2 
  peer 2.1.1.17 enable
  peer 2.1.1.17 group as2 
  peer 2.1.1.18 enable
  peer 2.1.1.18 group as2
  quit
  
 ospf 1 
 import-route bgp route-policy import_bgp
 area 0.0.0.0 
  network 2.1.1.15 0.0.0.0 
  network 192.168.2.15 0.0.0.0 
  network 95.2.0.0 0.0.255.255 
  quit
  quit
  
```

## rt16（S2）

```
sys
sysname rt16

router id 2.1.1.16 

interface GigabitEthernet0/0/0
 ip address 95.0.0.250 255.255.255.252 
#
interface GigabitEthernet0/0/1
 ip address 95.2.2.16 255.255.255.0 
#
interface GigabitEthernet0/0/2
 ip address 95.2.3.16 255.255.255.0 
interface LoopBack0
 ip address 2.1.1.16 255.255.255.255 
#
interface LoopBack1
 ip address 192.168.2.16 255.255.255.255 

ip ip-prefix permit_snmp_in index 10 permit 192.168.2.0 24
ip ip-prefix permit_snmp_out index 10 permit 192.168.1.0 24
ip ip-prefix permit_snmp_out index 20 permit 192.168.3.0 24
ip ip-prefix permit_export index 10 permit 95.4.126.242 32
ip ip-prefix permit_export index 20 permit 192.168.2.0 24
ip ip-prefix permit_export index 30 permit 95.4.10.0 24
ip ip-prefix permit_export index 40 permit 95.4.11.0 24
ip ip-prefix permit_export index 50 permit 95.4.12.0 24
ip ip-prefix permit_export index 60 permit 95.4.13.0 24

 
route-policy import_ospf permit node 10 
 if-match ip-prefix permit_snmp_in 
#
route-policy import_bgp permit node 10 
 if-match ip-prefix permit_snmp_out 
 
bgp 2
 peer 95.0.0.249 as-number 1 
 group as2 internal
 peer as2 connect-interface LoopBack0
 peer 2.1.1.15 as-number 2 
 peer 2.1.1.15 group as2 
 peer 2.1.1.17 as-number 2 
 peer 2.1.1.17 group as2 
 peer 2.1.1.18 as-number 2 
 peer 2.1.1.18 group as2 
  undo synchronization
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.249 enable
  peer 95.0.0.249 ip-prefix permit_export export
  peer as2 enable
  peer as2 next-hop-local 
  peer 2.1.1.15 enable
  peer 2.1.1.15 group as2 
  peer 2.1.1.17 enable
  peer 2.1.1.17 group as2 
  peer 2.1.1.18 enable
  peer 2.1.1.18 group as2
  quit
  
ospf 1 
 import-route bgp route-policy import_bgp
 area 0.0.0.0 
  network 2.1.1.16 0.0.0.0 
  network 192.168.2.16 0.0.0.0 
  network 95.2.0.0 0.0.255.255 
  quit
  quit
  
```



## LSW7

```
sys
sysn ls7
inter g0/0/1
port link-type access
inter g0/0/2
port link-type access
inter g0/0/3
port link-type access
vlan 2
port g0/0/1 
port g0/0/2
port g0/0/3
inter vlan 2
ip add 95.2.2.254 24


inter loop1
ip add 192.168.2.7 32

router id 2.2.1.7
ospf
area 0
network 95.2.2.0 0.0.0.255
network 192.168.2.7 0.0.0.0
quit
quit
```

## LSW8

```
sys
sysn ls8
inter g0/0/1
port link-type access
inter g0/0/2
port link-type access
inter g0/0/3
port link-type access
inter g0/0/4
port link-type access
vlan 2
port g0/0/4
inter vlan 2
ip add 192.168.2.1 24
vlan 3
port g0/0/1
port g0/0/2
port g0/0/3
inter vlan3
ip add 95.2.3.254 24

inter loop1
ip add 192.168.2.8 32

quit
router id 2.2.1.8
ospf
area 0
network 95.2.3.0 0.0.0.255
network 192.168.2.0 0.0.0.255
network 192.168.2.8 0.0.0.0
quit
quit
```

## rt17

```
sys
sysn rt17
inter g4/0/0
ip add 95.0.0.233 30
inter g0/0/0
ip add 95.2.2.17 24
inter g0/0/1
ip add 95.2.3.17 24
inter loop1
ip add 192.168.2.17 32
inter loop0 
ip add 2.1.1.17 32

quit
router id 2.1.1.17

ip ip-prefix permit_snmp_in index 10 permit 192.168.2.0 24
ip ip-prefix permit_snmp_out index 10 permit 192.168.4.0 24
ip ip-prefix permit_export index 10 permit 172.16.1.0 24
ip ip-prefix permit_export index 20 permit 95.3.112.242 32 
ip ip-prefix permit_export index 30 permit 192.168.2.0 24

route-policy import_ospf permit node 10 
 if-match ip-prefix permit_snmp_in 

route-policy import_bgp permit node 10 
 if-match ip-prefix permit_snmp_out 

ospf
import-route bgp route-policy import_bgp
area 0
network 2.1.1.17 0.0.0.0
network 95.2.2.0 0.0.0.255
network 95.2.3.0 0.0.0.255
network 192.168.2.17 0.0.0.0
quit
quit

bgp 2
peer 95.0.0.234 as-number 4
group as2 internal
peer as2 connect-interface loop0
peer 2.1.1.15 as-number 2
peer 2.1.1.15 group as2
peer 2.1.1.16 as-number 2
peer 2.1.1.16 group as2
peer 2.1.1.18 as-number 2 
peer 2.1.1.18 group as2 
undo synchronization
	network 192.168.2.0 
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.234 enable
  peer 95.0.0.234 ip-prefix permit_export export
  peer as2 enable
  peer as2 next-hop-local 
  peer 2.1.1.15 enable
  peer 2.1.1.15 group as2 
  peer 2.1.1.16 enable
  peer 2.1.1.16 group as2 
  peer 2.1.1.18 enable
  peer 2.1.1.18 group as2 

quit
quit

```

## rt18

```
sys
sysn rt18
inter g4/0/0
ip add 95.0.0.229 30
inter g0/0/1
ip add 95.2.2.18 24
inter g0/0/0
ip add 95.2.3.18 24
inter loop0 
ip add 2.1.1.18 32
inter loop1
ip add 192.168.2.18 32
quit


router id 2.1.1.18


ip ip-prefix permit_snmp_in index 10 permit 192.168.2.0 24
ip ip-prefix permit_snmp_out index 10 permit 192.168.4.0 24
ip ip-prefix permit_export index 10 permit 172.16.1.0 24
ip ip-prefix permit_export index 20 permit 95.3.112.242 32
ip ip-prefix permit_export index 30 permit 192.168.2.0 24

route-policy import_ospf permit node 10 
 if-match ip-prefix permit_snmp_in

route-policy import_bgp permit node 10 
 if-match ip-prefix permit_snmp_out 

ospf
import-route bgp route-policy import_bgp
area 0
network 2.1.1.18 0.0.0.0 
  network 95.2.0.0 0.0.255.255 
  network 192.168.2.18 0.0.0.0 
quit
quit

bgp 2
 peer 95.0.0.230 as-number 4 
 group as2 internal
 peer as2 connect-interface LoopBack0
 peer 2.1.1.15 as-number 2 
 peer 2.1.1.15 group as2 
 peer 2.1.1.16 as-number 2 
 peer 2.1.1.16 group as2 
 peer 2.1.1.17 as-number 2 
 peer 2.1.1.17 group as2 
 
  undo synchronization
  network 192.168.2.0 
  default local-preference 200
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.230 enable
  peer 95.0.0.230 ip-prefix permit_export export
  peer as2 enable
  peer as2 next-hop-local 
  peer 2.1.1.15 enable
  peer 2.1.1.15 group as2 
  peer 2.1.1.16 enable
  peer 2.1.1.16 group as2 
  peer 2.1.1.17 enable
  peer 2.1.1.17 group as2 

quit
quit
```





# AS4

## rt19

```
sys
sysn rt19
inter g4/0/0
ip add 95.0.0.234 30
inter g0/0/0
ip add 95.4.2.19 24
inter g0/0/1
ip add 95.4.3.19 24
inter loop0
ip add 4.1.1.19 32
inter loop1
ip add 192.168.4.19 32

quit

router id 4.2.1.19

ip ip-prefix permit_users index 10 permit  95.4.10.0 24
ip ip-prefix permit_users index 20 permit 95.4.11.0 24
ip ip-prefix permit_users index 30 permit 95.4.12.0 24
ip ip-prefix permit_users index 40 permit 95.4.13.0 24
ip ip-prefix permit_inet index 10 permit 172.16.1.0 24
ip ip-prefix permit_voip_in index 10 permit 95.4.126.242 32
ip ip-prefix permit_voip_out index 10 permit 95.3.112.242 32
ip ip-prefix permit_snmp_out index 10 permit 192.168.2.0 24

route-policy import_bgp permit node 10 
 if-match ip-prefix permit_inet 
#
route-policy import_bgp permit node 20 
 if-match ip-prefix permit_voip_out 
#
route-policy import_bgp permit node 30 
 if-match ip-prefix permit_snmp_out 
#
route-policy import_ospf permit node 10 
 if-match ip-prefix permit_users 
#
route-policy import_ospf permit node 20 
 if-match ip-prefix permit_voip_in 
#
route-policy import_ospf permit node 30 
 if-match ip-prefix permit_snmp_in 
#


ospf 1 
 import-route bgp cost 2 route-policy import_bgp
 preference ase 255 
 area 0.0.0.0 
  network 4.1.1.19 0.0.0.0 
  network 192.168.4.19 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit
quit

bgp 4
 peer 95.0.0.233 as-number 2 
 group as4 internal
 peer as4 connect-interface LoopBack0
 peer 4.1.1.20 as-number 4 
 peer 4.1.1.20 group as4 
  undo synchronization
  aggregate 192.168.4.0 255.255.255.0 detail-suppressed 
  network 192.168.4.0 
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.233 enable
  peer as4 enable
  peer 4.1.1.20 enable
  peer 4.1.1.20 group as4 
quit



```

## rt20

```
sys
sysn rt20
inter g4/0/0
ip add 95.0.0.230 30
inter g0/0/1
ip add 95.4.2.20 24
inter g0/0/0
ip add 95.4.3.20 24

interface LoopBack0
 ip address 4.1.1.20 255.255.255.255 
interface LoopBack1
 ip address 192.168.4.20 255.255.255.255 
quit 

router id 4.1.1.20

ip ip-prefix permit_users index 10 permit 95.4.10.0 24
ip ip-prefix permit_users index 20 permit 95.4.11.0 24
ip ip-prefix permit_users index 30 permit 95.4.12.0 24
ip ip-prefix permit_users index 40 permit 95.4.13.0 24
ip ip-prefix permit_inet index 10 permit 172.16.1.0 24
ip ip-prefix permit_voip_in index 10 permit 95.4.126.242 32
ip ip-prefix permit_voip_out index 10 permit 95.3.112.242 32
ip ip-prefix permit_snmp_out index 10 permit 192.168.2.0 24

route-policy import_bgp permit node 10 
 if-match ip-prefix permit_inet 
#
route-policy import_bgp permit node 20 
 if-match ip-prefix permit_voip_out 
#
route-policy import_bgp permit node 30 
 if-match ip-prefix permit_snmp_out 
#
route-policy import_ospf permit node 10 
 if-match ip-prefix permit_users 
#
route-policy import_ospf permit node 20 
 if-match ip-prefix permit_voip_in 
#
route-policy import_ospf permit node 30 
 if-match ip-prefix permit_snmp_in 

ospf 1 
 import-route bgp route-policy import_bgp
 preference ase 255 
 area 0.0.0.0 
  network 4.1.1.20 0.0.0.0 
  network 192.168.4.20 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit  
quit

bgp 4
 peer 95.0.0.229 as-number 2 
 group as4 internal
 peer as4 connect-interface LoopBack0
 peer 4.1.1.19 as-number 4 
 peer 4.1.1.19 group as4 
 
  undo synchronization
  default local-preference 200
  aggregate 192.168.4.0 255.255.255.0 detail-suppressed 
  network 192.168.4.0 
  import-route ospf 1 route-policy import_ospf
  peer 95.0.0.229 enable
  peer as4 enable
  peer 4.1.1.19 enable
  peer 4.1.1.19 group as4 
 quit

```

## ls9

```
sys
sysn ls9
inter g0/0/1
port link-type access
inter g0/0/2
port link-type access
inter g0/0/3
port link-type access
inter g0/0/4
port link-type access
inter g0/0/5
port link-type access
vlan 2
port g0/0/3
port g0/0/4
inter vlan 2
ip add 95.4.2.9 24
vlan 3
port g0/0/5
inter vlan 3
ip add 95.4.0.249 24
vlan 4
port g0/0/1
port g0/0/2
inter vlan 4
ip add 95.4.4.9 24
interface LoopBack1
 ip address 192.168.4.9 255.255.255.255
quit


router id 4.2.1.9
ospf 1
 area 0.0.0.0
  network 95.4.0.0 0.0.255.255
  network 192.168.4.9 0.0.0.0
quit
quit
```

## ls10

```
sys
sysn ls10
inter g0/0/1
port link-type access
inter g0/0/2
port link-type access
inter g0/0/3
port link-type access
inter g0/0/4
port link-type access
inter g0/0/5
port link-type access
vlan 2
port g0/0/3
port g0/0/4
inter vlan 2
ip add 95.4.3.10 24
vlan 3
port g0/0/5
inter vlan 3
ip add 95.4.0.250 24
vlan 4
port g0/0/1
port g0/0/2
inter vlan 4
ip add 95.4.5.10 24
interface LoopBack1
 ip address 192.168.4.10 255.255.255.255
quit


router id 4.2.1.10
ospf 1
 area 0.0.0.0
  network 95.4.0.0 0.0.255.255
  network 192.168.4.10 0.0.0.0
quit
quit

```

## rt21

```
sys
sysn rt21
inter g0/0/0
ip add 95.4.4.21 24
inter g0/0/1
ip add 95.4.5.21 24
inter S4/0/1
ip add 95.4.7.21 24
link-protocol ppp
shutdown
undo shutdown
inter S4/0/0
ip add 95.4.6.21 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack1
 ip address 192.168.4.21 255.255.255.255 
quit


router id 4.1.1.21
ospf 1 
 area 0.0.0.0 
  network 192.168.4.21 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit
quit

```

## rt22

```
sys
sysn rt22
inter g0/0/0
ip add 95.4.5.22 24
inter g0/0/1
ip add 95.4.4.22 24
inter S4/0/1
ip add 95.4.9.22 24
link-protocol ppp
shutdown
undo shutdown
inter S4/0/0
ip add 95.4.8.22 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack1
 ip address 192.168.4.22 255.255.255.255 
quit


router id 4.1.1.22
ospf
ospf 1 
 area 0.0.0.0 
  network 192.168.4.22 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit
quit
```

## rt23

```
sys
sysn rt23

inter g0/0/0
ip add 95.4.10.23 24
vrrp vrid 1 virtual-ip 95.4.10.1
inter g0/0/1
ip add 95.4.11.23 24
vrrp vrid 2 virtual-ip 95.4.11.1
inter S4/0/0
ip add 95.4.6.23 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack0
 ip address 95.4.126.242 255.255.255.255
interface LoopBack1
 ip address 192.168.4.23 255.255.255.255 
quit
router id 4.1.1.23

ospf 1 
 area 0.0.0.0 
  network 192.168.4.23 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
  network 95.4.126.242 0.0.0.0 
quit
quit

```

## rt24

```
sys
sysn rt24
inter g0/0/0
ip add 95.4.10.24 24
vrrp vrid 1 virtual-ip 95.4.10.1
inter g0/0/1
ip add 95.4.11.24 24
vrrp vrid 2 virtual-ip 95.4.11.1
inter S4/0/0
ip add 95.4.8.24 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack1
 ip address 192.168.4.24 255.255.255.255 
quit
router id 4.1.1.24

ospf 1 
 area 0.0.0.0 
  network 192.168.4.24 0.0.0.0 
  network 95.4.0.0 0.0.255.255 

quit
quit
```

## rt25

```
sys
sysn rt25
inter g0/0/0
ip add 95.4.12.25 24
vrrp vrid 1 virtual-ip 95.4.12.1
inter g0/0/1
ip add 95.4.13.25 24
vrrp vrid 2 virtual-ip 95.4.13.1
inter S4/0/0
ip add 95.4.7.25 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack1
 ip address 192.168.4.25 255.255.255.255 

quit
router id 4.1.1.25

ospf 1 
 area 0.0.0.0 
  network 192.168.4.25 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit
quit
```

## rt26

```
sys
sysn rt26
inter g0/0/0
ip add 95.4.12.26 24
vrrp vrid 1 virtual-ip 95.4.12.1
inter g0/0/1
ip add 95.4.13.26 24
vrrp vrid 2 virtual-ip 95.4.13.1
inter S4/0/0
ip add 95.4.9.26 24
link-protocol ppp
shutdown
undo shutdown
interface LoopBack1
 ip address 192.168.4.26 255.255.255.255 
quit
router id 4.1.1.26

ospf 1 
 area 0.0.0.0 
  network 192.168.4.26 0.0.0.0 
  network 95.4.0.0 0.0.255.255 
quit
quit
```

