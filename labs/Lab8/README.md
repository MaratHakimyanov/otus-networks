# Лабораторная работа №8. EIGRP
### Цели:
1. Настроить EIGRP в "С.-Петербург", использовать named EIGRP

### Описание:
1. В офисе "С.-Петербург" настроить EIGRP.
2. R32 получает только маршрут по умолчанию.
3. R16-17 анонсируют только суммарные префиксы.
4. Использовать EIGRP named-mode для настройки сети. Настройка осуществляется одновременно для IPv4 и IPv6.

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab8/Lab8_Topology.JPG)

### Решение:
### 1. Настройка EIGRP в "С.-Петербург", использовать named EIGRP

Настроим именованный EIGRP на R18.
#### Маршрутизатор R18:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 18.18.18.18
  no shutdown
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int
  exit-address-family

address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 18.18.18.18
  no shutdown
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int
  exit-address-family
```


На R17 и R16 также необходимо настроить суммаризацию.
#### Маршрутизатор R17:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    auto-summary
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 17.17.17.17
  af-int e0/3
    passive-int
  no shutdown
  exit-address-family

address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 17.17.17.17
  af-int e0/3
    passive-int
  no shutdown
  af-int e0/0  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/1  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/2  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/3  
    summary-address FDE8:8A:FC:1:10::/80
  exit-address-family
```

#### Маршрутизатор R16:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    auto-summary
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 16.16.16.16
  no shutdown
  exit-address-family

address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 16.16.16.16
  no shutdown
  af-int e0/0  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/1  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/2  
    summary-address FDE8:8A:FC:1:10::/80
  af-int e0/3  
    summary-address FDE8:8A:FC:1:10::/80
  exit-address-family
```

На L3-коммутаторах SW9 и SW10 настроим именованный EIGRP.
#### L3-коммутатор SW9:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 9.9.9.9
  no shutdown
  af-int e0/1
    passive-int
  af-int e1/1
    passive-int
  af-int e1/2
    passive-int
  af-int e1/3
    passive-int  
  exit-address-family

  address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 9.9.9.9
  no shutdown
  af-int e0/1
    passive-int
  af-int e1/1
    passive-int
  af-int e1/2
    passive-int
  af-int e1/3
    passive-int  
  exit-address-family
```

#### L3-коммутатор SW10:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 10.10.10.10
  no shutdown
  af-int e0/1
    passive-int
  af-int e1/1
    passive-int
  af-int e1/2
    passive-int
  af-int e1/3
    passive-int  
  exit-address-family

  address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 10.10.10.10
  no shutdown
  af-int e0/1
    passive-int
  af-int e1/1
    passive-int
  af-int e1/2
    passive-int
  af-int e1/3
    passive-int  
  exit-address-family
```

R32 должен получать только маршрут по умолчанию. Для этого необходимо настроить маршрут по умолчанию на R18, настроить команду **redistribute static** на R32. Также на на R32 необходимо прописать access-list.

Пропишем маршрут по умолчанию на R18.
#### Маршрутизатор R18:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.34
ip route 0.0.0.0 0.0.0.0 46.12.1.38
ipv6 route ::/0 2DE8:8A:FC:1:10:A3:0:37
ipv6 route ::/0 2DE8:8A:FC:1:10:A3:0:39
```

Настроим именованный EIGRP на R32.

#### Маршрутизатор R32:
```
router eigrp LAB8
  address-family ipv4 unicast autonomous-system 1
  topology base
    redistribute static
    exit-af-topology  
  network 192.168.4.0
  eigrp router-id 32.32.32.32
  no shutdown
  af-int e0/1
    passive-int
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int 
  exit-address-family

  address-family ipv6 unicast autonomous-system 1
  topology base
    redistribute static
    exit-af-topology  
  eigrp router-id 32.32.32.32
  no shutdown
  af-int e0/1
    passive-int
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int  
  exit-address-family
```

C помощью команды **show ip route eigrp** и **show ipv6 route eigrp** проверим, что R32 получает маршруты по умолчанию.

#### Маршрутизатор R32:
```
R32#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.4.9 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 192.168.4.9, 00:00:33, Ethernet0/0
      192.168.4.0/24 is variably subnetted, 10 subnets, 3 masks
D        192.168.4.0/30 [90/2048000] via 192.168.4.9, 00:25:29, Ethernet0/0
D        192.168.4.4/30 [90/1536000] via 192.168.4.9, 00:25:29, Ethernet0/0
D        192.168.4.12/30 [90/2048000] via 192.168.4.9, 00:25:24, Ethernet0/0
D        192.168.4.16/30 [90/2048000] via 192.168.4.9, 00:25:24, Ethernet0/0
D        192.168.4.20/30 [90/1536000] via 192.168.4.9, 00:25:29, Ethernet0/0
D        192.168.4.24/30 [90/1536000] via 192.168.4.9, 00:25:29, Ethernet0/0
D        192.168.4.28/30 [90/2048000] via 192.168.4.9, 00:25:24, Ethernet0/0
D        192.168.4.128/25 [90/2048000] via 192.168.4.9, 00:25:24, Ethernet0/0

R32#sh ipv6 route eigrp
IPv6 Routing Table - default - 5 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
D   ::/0 [90/2048000]
     via FE80::16, Ethernet0/0
D   FDE8:8A:FC:1:10::/80 [90/1536000]
     via FE80::16, Ethernet0/0
```



