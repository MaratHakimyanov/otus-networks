# Лабораторная работа №7. IS-IS
### Цели:
1. Настроить IS-IS в офисе "Триада"

### Описание:
1. Настроить IS-IS в ISP "Триада".
2. R23 и R25 находятся в зоне 2222.
3. R24 находится в зоне 24.
4. R26 находится в зоне 26. Настройка осуществляется одновременно для IPv4 и IPv6. 

### Решение:
### 1. Настройка IS-IS в офисе "Триада"

Для настройки IS-IS необходимо глобально включить на маршрутизаторах isis, назначить iso адрес, на интерфейсах запустить ipv4 isis, ipv6 isis и назначить необходимый уровень взаимодействия на интерфейсах в соответствии с топологией.

#### Маршрутизатор R23:
```
router isis
  net 49.2222.0230.2302.3023.00
ipv6 router isis
  net 49.2222.0230.2302.3023.00
int e0/1
  ip router isis
  ipv6 router isis
  isis circuit-type level-1 
int e0/2
  ip router isis
  ipv6 router isis
  isis circuit-type level-2-only
```

#### Маршрутизатор R24:
```
router isis
  net 49.0024.0240.2402.4024.00
ipv6 router isis
  net 49.0024.0240.2402.4024.00
int range e0/1-2
  ip router isis
  ipv6 router isis
  isis circuit-type level-2-only
```

#### Маршрутизатор R25:
```
router isis
  net 49.2222.0250.2502.5025.00
ipv6 router isis
  net 49.2222.0250.2502.5025.00
int e0/0
  ip router isis 
  ipv6 router isis
  isis circuit-type level-1
int e0/2
  ip router isis
  ipv6 router isis
  isis circuit-type level-2-only
```

#### Маршрутизатор R26:
```
router isis
  net 49.0026.0260.2602.6026.00
ipv6 router isis
  net 49.0026.0260.2602.6026.00
int range e0/0,e0/2
  ip router isis
  ipv6 router isis
  isis circuit-type level-2-only
```

### 2. Проверка правильности настройки IS-IS

Чтобы проверить, что маршрутизаторы видят соседей, получают маршруты и их взаимодействие настроено корректно, необходимо ввести команды **show isis neighbors**, **show ip route isis**, **show ipv6 route isis**, **show isis database**.

#### Маршрутизатор R23:
```
R23#sh isis nei

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/2       192.168.2.2     UP    8        R24.02             
R25            L1   Et0/1       192.168.2.6     UP    7        R25.01 

R23#sh ip route isis
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.2.0/24 is variably subnetted, 6 subnets, 2 masks
i L2     192.168.2.8/30 [115/20] via 192.168.2.2, 00:07:37, Ethernet0/2
i L2     192.168.2.12/30 [115/30] via 192.168.2.2, 00:02:18, Ethernet0/2

R23#sh ipv6 route isis
IPv6 Routing Table - default - 9 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I2  FDE8:8A:FC:1:10:A3:0:46/127 [115/20]
     via FE80::24, Ethernet0/2
I2  FDE8:8A:FC:1:10:A3:0:48/127 [115/30]
     via FE80::24, Ethernet0/2

R23#sh isis database 

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00           * 0x0000000B   0x1A11        840               1/0/0
R25.00-00             0x0000000A   0x0ED4        983               1/0/0
R25.01-00             0x00000002   0x15B7        835               0/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00           * 0x0000000E   0x40A4        864               0/0/0
R24.00-00             0x00000008   0x495A        1018              0/0/0
R24.02-00             0x00000002   0x7721        704               0/0/0
R25.00-00             0x0000000A   0xE257        1016              0/0/0
R26.00-00             0x00000005   0x09EB        1015              0/0/0
R26.01-00             0x00000002   0xCC22        1013              0/0/0
R26.02-00             0x00000002   0xC704        1015              0/0/0
```

#### Маршрутизатор R24:
```
R24#sh isis nei

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L2   Et0/2       192.168.2.1     UP    25       R24.02             
R26            L2   Et0/1       192.168.2.10    UP    9        R26.01 

R24#sh ip route isis
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.2.0/24 is variably subnetted, 6 subnets, 2 masks
i L2     192.168.2.4/30 [115/20] via 192.168.2.1, 00:10:20, Ethernet0/2
i L2     192.168.2.12/30 [115/20] via 192.168.2.10, 00:05:01, Ethernet0/1

R24#sh ipv6 route isis
IPv6 Routing Table - default - 11 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I2  FDE8:8A:FC:1:10:A3:0:44/127 [115/20]
     via FE80::23, Ethernet0/2
I2  FDE8:8A:FC:1:10:A3:0:48/127 [115/20]
     via FE80::26, Ethernet0/1
     
R24#sh isis database  

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R24.00-00           * 0x00000005   0x2561        540               1/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x0000000E   0x40A4        700               0/0/0
R24.00-00           * 0x00000008   0x495A        857               0/0/0
R24.02-00           * 0x00000002   0x7721        543               0/0/0
R25.00-00             0x0000000A   0xE257        855               0/0/0
R26.00-00             0x00000005   0x09EB        855               0/0/0
R26.01-00             0x00000002   0xCC22        853               0/0/0
R26.02-00             0x00000002   0xC704        854               0/0/0
```

#### Маршрутизатор R25:
```
R25#sh isis nei

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L1   Et0/0       192.168.2.5     UP    24       R25.01             
R26            L2   Et0/2       192.168.2.14    UP    8        R26.02 

R25#sh ip route isis
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.2.0/24 is variably subnetted, 6 subnets, 2 masks
i L2     192.168.2.0/30 [115/30] via 192.168.2.14, 00:07:28, Ethernet0/2
i L2     192.168.2.8/30 [115/20] via 192.168.2.14, 00:07:28, Ethernet0/2

R25#sh ipv6 route isis
IPv6 Routing Table - default - 11 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I2  FDE8:8A:FC:1:10:A3:0:42/127 [115/30]
     via FE80::26, Ethernet0/2
I2  FDE8:8A:FC:1:10:A3:0:46/127 [115/20]
     via FE80::26, Ethernet0/2
     
R25#sh isis database  

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x0000000B   0x1A11        565               1/0/0
R25.00-00           * 0x0000000A   0x0ED4        712               1/0/0
R25.01-00           * 0x00000002   0x15B7        564               0/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x0000000E   0x40A4        586               0/0/0
R24.00-00             0x00000008   0x495A        742               0/0/0
R24.02-00             0x00000002   0x7721        429               0/0/0
R25.00-00           * 0x0000000A   0xE257        748               0/0/0
R26.00-00             0x00000005   0x09EB        744               0/0/0
R26.01-00             0x00000002   0xCC22        742               0/0/0
R26.02-00             0x00000002   0xC704        743               0/0/0
```

#### Маршрутизатор R26:
```
R26#sh isis nei

System Id      Type Interface   IP Address      State Holdtime Circuit Id
R24            L2   Et0/0       192.168.2.9     UP    24       R26.01             
R25            L2   Et0/2       192.168.2.13    UP    26       R26.02 

R26#sh ip route isis
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      192.168.2.0/24 is variably subnetted, 6 subnets, 2 masks
i L2     192.168.2.0/30 [115/20] via 192.168.2.9, 00:09:08, Ethernet0/0
i L2     192.168.2.4/30 [115/20] via 192.168.2.13, 00:09:08, Ethernet0/2

R26#sh ipv6 route isis
IPv6 Routing Table - default - 11 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
I2  FDE8:8A:FC:1:10:A3:0:42/127 [115/20]
     via FE80::24, Ethernet0/0
I2  FDE8:8A:FC:1:10:A3:0:44/127 [115/20]
     via FE80::25, Ethernet0/2
     
R26#sh isis database  

IS-IS Level-1 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R26.00-00           * 0x00000005   0x8AB3        643               1/0/0
IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x0000000E   0x40A4        490               0/0/0
R24.00-00             0x00000008   0x495A        647               0/0/0
R24.02-00             0x00000003   0x7522        1169              0/0/0
R25.00-00             0x0000000A   0xE257        649               0/0/0
R26.00-00           * 0x00000005   0x09EB        648               0/0/0
R26.01-00           * 0x00000002   0xCC22        646               0/0/0
R26.02-00           * 0x00000002   0xC704        647               0/0/0
```
