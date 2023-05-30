## real-R1

```
sys
router id 1.1.1.1

inter Serial1/0
ip address 95.0.0.253 255.255.255.252
interface LoopBack0
ip address 1.1.1.1 255.255.255.255
interface LoopBack1
ip address 192.168.1.1 255.255.255.255
interface GigabitEthernet0/0
ip address 95.1.2.1 255.255.255.0
interface GigabitEthernet0/1
ip address 95.1.3.1 255.255.255.0
quit

ospf
import-route bgp
preference ase 200
area 0
network 1.1.1.1 0.0.0.0
network 95.0.0.252 0.0.0.3
network 95.1.2.0 0.0.0.255
network 95.1.3.0 0.0.0.255
network 192.168.1.1 0.0.0.0
quit
quit

acl basic 2001
rule 0 permit source 95.3.112.240 0.0.0.3
rule 5 deny
quit

route-policy per1 permit node 10
if-match ip address acl 2001
apply cost 2000
quit

bgp 1
peer 95.0.0.254 as-number 2
peer 95.1.2.5 as-number 1
peer 95.1.3.5 as-number 1
address-family ipv4 unicast
default med 2000
import-route ospf 1 med 800 route-policy per1
network 1.1.1.1 255.255.255.255
peer 95.0.0.254 enable
peer 95.1.2.5 enable
peer 95.1.2.5 next-hop-local
peer 95.1.3.5 enable
peer 95.1.3.5 next-hop-local
quit
quit

ip unreachables enable
ip ttl-expires enable


```

## real-R2

```
sys
sysname R2
router id 2.1.1.15

ospf 1
area 0.0.0.0
  network 95.0.0.252 0.0.0.3
  network 95.2.2.0 0.0.0.255
  network 95.2.3.0 0.0.0.255
quit
quit

 ip unreachables enable
 ip ttl-expires enable

interface Serial1/0
 ip address 95.0.0.254 255.255.255.252
interface GigabitEthernet0/0
 ip address 95.2.2.15 255.255.255.0
interface GigabitEthernet0/1
 ip address 95.2.3.15 255.255.255.0
quit

bgp 2
 peer 95.0.0.253 as-number 1
 peer 95.2.2.16 as-number 2
 peer 95.2.2.17 as-number 2
 peer 95.2.2.18 as-number 2
 peer 95.2.3.17 as-number 2
 peer 95.2.3.18 as-number 2
  address-family ipv4 unicast
  import-route ospf 1
  peer 95.0.0.253 enable
  peer 95.2.2.16 enable
  peer 95.2.2.17 enable
  peer 95.2.2.18 enable
  peer 95.2.3.17 enable
  peer 95.2.3.18 enable
 quit
quit


```

## real-S1

```
sys

ospf 1
 area 0.0.0.0
  network 95.0.0.248 0.0.0.3
  network 95.1.2.0 0.0.0.255
quit
quit

 ip unreachables enable
 ip ttl-expires enable
 
vlan 10
port g1/0/1
port g1/0/2
port g1/0/13
port g1/0/14
quit
vlan 20
port g1/0/24
quit

interface Vlan-interface10
 ip address 95.1.2.2 255.255.255.0
interface Vlan-interface20
 ip address 95.0.0.249 255.255.255.252
quit

bgp 1
 peer 95.0.0.250 as-number 2
 peer 95.1.2.5 as-number 1
 peer 95.1.3.5 as-number 1
  address-family ipv4 unicast
  peer 95.0.0.250 enable
  peer 95.1.2.5 enable
  peer 95.1.2.5 next-hop-local
  peer 95.1.3.5 enable
  peer 95.1.3.5 next-hop-local
quit
quit

```

## real-S2

```
sys
sysname S2

ospf 1
 area 0.0.0.0
  network 95.0.0.248 0.0.0.3
  network 95.2.2.0 0.0.0.255
quit
quit

ip unreachables enable
ip ttl-expires enable
 
vlan 10
port g1/0/1
port g1/0/2
port g1/0/13
port g1/0/14
quit
vlan 20
port g1/0/24

interface Vlan-interface10
 ip address 95.2.2.16 255.255.255.0
interface Vlan-interface20
 ip address 95.0.0.250 255.255.255.252
quit

bgp 2
 peer 95.0.0.249 as-number 1
 peer 95.2.2.15 as-number 2
 peer 95.2.2.17 as-number 2
 peer 95.2.2.18 as-number 2
 peer 95.2.3.15 as-number 2
 peer 95.2.3.17 as-number 2
 peer 95.2.3.18 as-number 2
 address-family ipv4 unicast
  peer 95.0.0.249 enable
  peer 95.2.2.15 enable
  peer 95.2.2.17 enable
  peer 95.2.2.18 enable
  peer 95.2.3.15 enable
  peer 95.2.3.17 enable
  peer 95.2.3.18 enable
 quit
quit


 
```





## ls7

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
router id 2.2.1.7
ospf
area 0
network 95.2.2.0 0.0.0.255
quit
quit
```

## ls8

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
quit
router id 2.2.1.8
ospf
area 0
network 95.2.3.0 0.0.0.255
network 192.168.2.0 0.0.0.255
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
quit
router id 2.1.1.17
ospf
area 0
network 95.2.2.0 0.0.0.255
network 95.2.3.0 0.0.0.255
network 95.0.0.233 0.0.0.3
quit
quit
bgp 2
peer 95.0.0.234 as-number 4
peer 95.2.2.16 as-number 2
peer 95.2.3.16 as-number 2
peer 95.2.2.15 as-number 2
peer 95.2.3.15 as-number 2
peer 95.2.2.18 as-number 2
peer 95.2.3.18 as-number 2
quit

bgp 2
default local-pref 100
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
quit
router id 2.1.1.18
ospf
area 0
network 95.2.3.0 0.0.0.255
network 95.2.2.0 0.0.0.255
network 95.0.0.229 0.0.0.3
quit
quit
bgp 2
peer 95.0.0.230 as-number 4
peer 95.2.2.16 as-number 2
peer 95.2.3.16 as-number 2
peer 95.2.2.15 as-number 2
peer 95.2.3.15 as-number 2
peer 95.2.2.17 as-number 2
peer 95.2.3.17 as-number 2
quit

bgp 2
default local-pref 800
quit

```



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
quit
router id 4.2.1.19
ospf
area 0
network 95.0.0.234 0.0.0.3
network 95.4.2.19 0.0.0.255
network 95.4.3.19 0.0.0.255
quit
quit
bgp 4
peer 95.0.0.233 as-number 2
quit


acl number 2001
rule permit source 95.4.126.242 0.0.0.0
rule deny source any
quit
route-policy permit_242_deny_any permit node 10
if-match acl 2001
apply cost 200
quit
bgp 4
import-route ospf 1 route-policy permit_242_deny_any



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
quit
router id 4.1.1.20
ospf
area 0
network 95.0.0.230 0.0.0.3
network 95.4.2.20 0.0.0.255
network 95.4.3.20 0.0.0.255
quit
quit
bgp 4
peer 95.0.0.229 as-number 2
quit

acl number 2001
rule permit source 95.4.126.242 0.0.0.0
rule deny source any
quit
route-policy permit_242_deny_any permit node 10
if-match acl 2001
apply cost 100
quit
bgp 4
import-route ospf 1 route-policy permit_242_deny_any

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
quit
router id 4.2.1.9
ospf
area 0
network 95.4.2.9 0.0.0.255
network 95.4.0.249 0.0.0.255
network 95.4.4.9 0.0.0.255
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
quit
router id 4.2.1.10
ospf
area 0
network 95.4.3.10 0.0.0.255
network 95.4.0.250 0.0.0.255
network 95.4.5.10 0.0.0.255
quit
quit

inter vlan 4
ospf cost 1
quit
inter vlan 2
ospf cost 1
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
quit
router id 4.1.1.21
ospf
area 0
network 95.4.4.21 0.0.0.255
network 95.4.5.21 0.0.0.255
network 95.4.6.21 0.0.0.255
network 95.4.7.21 0.0.0.255
quit
quit

inter S4/0/0
ospf cost 1
shutdown
undo shutdown

inter g0/0/1
ospf cost 1
quit

inter g0/0/0
ospf cost 800
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
quit
router id 4.1.1.22
ospf
area 0
network 95.4.4.22 0.0.0.255
network 95.4.5.22 0.0.0.255
network 95.4.8.22 0.0.0.255
network 95.4.9.22 0.0.0.255
quit
quit
```

## rt23

```
sys
sysn rt23

inter loop1
ip add 95.4.126.242 32
quit

inter g0/0/0
ip add 95.4.10.23 24
inter g0/0/1
ip add 95.4.11.23 24
inter S4/0/0
ip add 95.4.6.23 24
link-protocol ppp
shutdown
undo shutdown
quit
router id 4.1.1.23
ospf
area 0
network 95.4.10.23 0.0.0.255
network 95.4.11.23 0.0.0.255
network 95.4.6.23 0.0.0.255
network 95.4.126.242 0.0.0.0
quit
quit

inter S4/0/0
ospf cost 1
shutdown 
undo shutdown
```

## rt24

```
sys
sysn rt24
inter g0/0/0
ip add 95.4.10.24 24
inter g0/0/1
ip add 95.4.11.24 24
inter S4/0/0
ip add 95.4.8.24 24
link-protocol ppp
shutdown
undo shutdown
quit
router id 4.1.1.24
ospf
area 0
network 95.4.10.24 0.0.0.255
network 95.4.11.24 0.0.0.255
network 95.4.8.24 0.0.0.255
quit
quit
```

## rt25

```
sys
sysn rt25
inter g0/0/0
ip add 95.4.12.25 24
inter g0/0/1
ip add 95.4.13.25 24
inter S4/0/0
ip add 95.4.7.25 24
link-protocol ppp
shutdown
undo shutdown
quit
router id 4.1.1.25
ospf
area 0
network 95.4.12.25 0.0.0.255
network 95.4.13.25 0.0.0.255
network 95.4.7.25 0.0.0.255
quit
quit
```

## rt 26

```
sys
sysn rt26
inter g0/0/0
ip add 95.4.12.26 24
inter g0/0/1
ip add 95.4.13.26 24
inter S4/0/0
ip add 95.4.9.26 24
link-protocol ppp
shutdown
undo shutdown
quit
router id 4.1.1.26
ospf
area 0
network 95.4.12.26 0.0.0.255
network 95.4.13.26 0.0.0.255
network 95.4.9.26 0.0.0.255
quit
quit
```

