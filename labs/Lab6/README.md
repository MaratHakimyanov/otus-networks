# Лабораторная работа №6. OSPF. Фильтрация
### Цели:
1. Настроить OSPF офисе "Москва"
2. Разделить сеть на зоны
3. Настроить фильтрацию между зонами


![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab1/Topology_stick.JPG)

### Описание:
1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
5. Настройка для IPv6 повторяет логику IPv4.

### Решение:
### 1. Настройка OSPF офисе "Москва"

Настроим на маршрутизаторах процесс OSPF и router-id: 
```
router ospf 1
  router-id <id>
ipv6 router ospf 1
```

Настроим OSPF на конкретных интерфейсах маршрутизаторов:
```
int <тип и номер интерфейса>
  ip ospf 1 area <номер зоны>
  ipv6 ospf 1 area <номер зоны>
```

В выводе **running-config** маршрутизаторов появятся настройки:

#### Маршрутизатор R14:
```
interface Ethernet0/0
 ip address 192.168.1.6 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:23/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/1
 ip address 192.168.1.14 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:27/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/2
 ip address 46.12.1.1 255.255.255.252
 ipv6 address FE80::14 link-local
 ipv6 address 2DE8:8A:FC:1:10:A3:0:20/127
!         
interface Ethernet0/3
 ip address 192.168.1.1 255.255.255.252
 ip ospf 1 area 101
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:20/127
 ipv6 ospf 1 area 101
!         
router ospf 1
 router-id 14.14.14.14
 default-information originate
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 46.12.1.3
!         
ipv6 router ospf 1
```

#### Маршрутизатор R15:
```
interface Ethernet0/0
 ip address 192.168.1.22 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:31/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/1
 ip address 192.168.1.10 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:25/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/2
 ip address 46.12.1.5 255.255.255.252
 ipv6 address FE80::14 link-local
 ipv6 address 2DE8:8A:FC:1:10:A3:0:22/127
!         
interface Ethernet0/3
 ip address 192.168.1.17 255.255.255.252
 ip ospf 1 area 102
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:28/127
 ipv6 ospf 1 area 102
!         
router ospf 1
 router-id 15.15.15.15
 default-information originate
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 46.12.1.7
!         
ipv6 router ospf 1
```

#### Маршрутизатор R12:
```
interface Ethernet0/0
 ip address 192.168.1.25 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:32/127
 ipv6 ospf 1 area 10
!         
interface Ethernet0/1
 ip address 192.168.1.29 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:34/127
 ipv6 ospf 1 area 10
!         
interface Ethernet0/2
 ip address 192.168.1.5 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:22/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/3
 ip address 192.168.1.9 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:24/127
 ipv6 ospf 1 area 0
!                
router ospf 1
 router-id 12.12.12.12
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

#### Маршрутизатор R13:
```
interface Ethernet0/0
 ip address 192.168.1.37 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:38/127
 ipv6 ospf 1 area 10
!         
interface Ethernet0/1
 ip address 192.168.1.33 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:36/127
 ipv6 ospf 1 area 10
!         
interface Ethernet0/2
 ip address 192.168.1.21 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:30/127
 ipv6 ospf 1 area 0
!         
interface Ethernet0/3
 ip address 192.168.1.13 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:26/127
 ipv6 ospf 1 area 0
!                
router ospf 1
 router-id 13.13.13.13
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

#### Маршрутизатор R19:
```
interface Ethernet0/0
 ip address 192.168.1.2 255.255.255.252
 ip ospf 1 area 101
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:21/127
 ipv6 ospf 1 area 101
!                
router ospf 1
 router-id 19.19.19.19
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

#### Маршрутизатор R20:
```
interface Ethernet0/0
 ip address 192.168.1.18 255.255.255.252
 ip ospf 1 area 102
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:29/127
 ipv6 ospf 1 area 102
!                
router ospf 1
 router-id 20.20.20.20
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

#### L3-коммутатор SW4:
```
interface Ethernet0/0
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet0/0.101
 description MSC_Client2
 encapsulation dot1Q 101
 ip address 192.168.1.193 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:40/122
 ipv6 ospf 1 area 10
!         
interface Ethernet0/0.401
 description MGMT
 encapsulation dot1Q 401
 ip address 46.26.1.195 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:83/122
 ipv6 ospf 1 area 10
!         
interface Ethernet0/1
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet0/1.100
 description MSC_Client1
 encapsulation dot1Q 100
 ip address 192.168.1.129 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:2/122
 ipv6 ospf 1 area 10
!         
interface Ethernet0/2
 ip address 192.168.1.41 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:40/127
!         
interface Ethernet0/3
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet1/0
 ip address 192.168.1.26 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:33/127
!         
interface Ethernet1/1
 ip address 192.168.1.34 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:37/127
!                
router ospf 1
 router-id 4.4.4.4
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

#### L3-коммутатор SW5:
```
interface Ethernet0/0
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet0/0.100
 description MSC_Client1
 encapsulation dot1Q 100
 ip address 192.168.1.130 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:1/122
 ipv6 ospf 1 area 10
!         
interface Ethernet0/0.401
 description MGMT
 encapsulation dot1Q 401
 ip address 46.26.1.196 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:84/122
 ipv6 ospf 1 area 10
!         
interface Ethernet0/1
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet0/1.101
 description MSC_Client2
 encapsulation dot1Q 101
 ip address 192.168.1.194 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:41/122
!         
interface Ethernet0/2
 ip address 192.168.1.42 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:41/127
!         
interface Ethernet0/3
 no ip address
 ip ospf 1 area 10
!         
interface Ethernet1/0
 ip address 192.168.1.38 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:39/127
!         
interface Ethernet1/1
 ip address 192.168.1.30 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:35/127
!         
router ospf 1
 router-id 5.5.5.5
!         
ip forward-protocol nd
!         
!         
no ip http server
no ip http secure-server
!         
ipv6 router ospf 1
```

### 2. Настройка распространения маршрута по умолчанию

На маршрутизаторах R14 и R15 также настроим распространение маршрута по умолчанию:

#### Маршрутизатор R14:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.3
router ospf 1
  default-information originate

ipv6 route ::/0 2de8:8a:fc:1:10:a3:0:21
ipv6 router ospf 1
  default-information originate
```

#### Маршрутизатор R15:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.7
router ospf 1
  default-information originate

ipv6 route ::/0 2de8:8a:fc:1:10:a3:0:23
ipv6 router ospf 1
  default-information originate  
```

Проверим, получают ли R12 и R13 маршруты по умолчанию:

#### Маршрутизатор R12:
```
R12#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.10 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 192.168.1.10, 00:01:12, Ethernet0/3
                [110/1] via 192.168.1.6, 00:02:30, Ethernet0/2
      46.0.0.0/26 is subnetted, 1 subnets
O        46.26.1.192 [110/20] via 192.168.1.30, 00:47:58, Ethernet0/1
                     [110/20] via 192.168.1.26, 01:14:07, Ethernet0/0
      192.168.1.0/24 is variably subnetted, 17 subnets, 3 masks
O IA     192.168.1.0/30 [110/20] via 192.168.1.6, 01:23:37, Ethernet0/2
O        192.168.1.12/30 [110/20] via 192.168.1.6, 01:23:37, Ethernet0/2
O IA     192.168.1.16/30 [110/20] via 192.168.1.10, 01:23:37, Ethernet0/3
O        192.168.1.20/30 [110/20] via 192.168.1.10, 01:23:37, Ethernet0/3
O        192.168.1.32/30 [110/20] via 192.168.1.26, 01:19:35, Ethernet0/0
O        192.168.1.36/30 [110/20] via 192.168.1.30, 00:49:11, Ethernet0/1
O        192.168.1.40/30 [110/20] via 192.168.1.30, 00:49:11, Ethernet0/1
                         [110/20] via 192.168.1.26, 01:19:45, Ethernet0/0
O        192.168.1.128/26 [110/20] via 192.168.1.30, 00:48:19, Ethernet0/1
                          [110/20] via 192.168.1.26, 00:49:36, Ethernet0/0
O        192.168.1.192/26 [110/20] via 192.168.1.30, 00:47:35, Ethernet0/1
                          [110/20] via 192.168.1.26, 01:14:18, Ethernet0/0
R12#sh ipv6 route ospf
IPv6 Routing Table - default - 5 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
```

#### Маршрутизатор R13:
```
R13#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.22 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 192.168.1.22, 00:03:27, Ethernet0/2
                [110/1] via 192.168.1.14, 00:04:46, Ethernet0/3
      46.0.0.0/26 is subnetted, 1 subnets
O        46.26.1.192 [110/20] via 192.168.1.38, 00:50:13, Ethernet0/0
                     [110/20] via 192.168.1.34, 01:16:22, Ethernet0/1
      192.168.1.0/24 is variably subnetted, 17 subnets, 3 masks
O IA     192.168.1.0/30 [110/20] via 192.168.1.14, 01:24:58, Ethernet0/3
O        192.168.1.4/30 [110/20] via 192.168.1.14, 01:24:58, Ethernet0/3
O        192.168.1.8/30 [110/20] via 192.168.1.22, 01:24:58, Ethernet0/2
O IA     192.168.1.16/30 [110/20] via 192.168.1.22, 01:24:58, Ethernet0/2
O        192.168.1.24/30 [110/20] via 192.168.1.34, 01:21:53, Ethernet0/1
O        192.168.1.28/30 [110/20] via 192.168.1.38, 00:51:26, Ethernet0/0
O        192.168.1.40/30 [110/20] via 192.168.1.38, 00:51:26, Ethernet0/0
                         [110/20] via 192.168.1.34, 01:21:53, Ethernet0/1
O        192.168.1.128/26 [110/20] via 192.168.1.38, 00:50:34, Ethernet0/0
                          [110/20] via 192.168.1.34, 00:51:51, Ethernet0/1
O        192.168.1.192/26 [110/20] via 192.168.1.38, 00:49:50, Ethernet0/0
                          [110/20] via 192.168.1.34, 01:16:33, Ethernet0/1
R13#sh ipv6 route ospf 
IPv6 Routing Table - default - 5 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application                          
```
По какой-то причине в IPv6 маршруты по умолчанию не передаются.

### 3. Настройка фильтрации маршрутизаторов для зоны 101

#### Маршрутизатор R19:
```
router ospf 1
  area 101 stub
ipv6 router ospf 1
  area 101 stub
```

#### Маршрутизатор R14:
```
router ospf 1
  area 101 stub no-summary
ipv6 router ospf 1
  area 101 stub no-summary
```

Теперь R19 должен получать только маршрут по умолчанию. Проверим правильность настройки с помощью команд **show ip route ospf**, **show ipv6 route ospf**.

#### Маршрутизатор R19:
```
R19#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.1 to network 0.0.0.0

O*IA  0.0.0.0/0 [110/11] via 192.168.1.1, 00:00:48, Ethernet0/0

R19#sh ipv6 route ospf
IPv6 Routing Table - default - 3 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2, la - LISP alt
       lr - LISP site-registrations, ld - LISP dyn-eid, a - Application
```

### 4. Настройка фильтрации маршрутизаторов для зоны 101


4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.

До настройки фильтрации R20 получает маршруты из Area 101.
#### Маршрутизатор R20:
```
R20#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.17 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 192.168.1.17, 00:58:06, Ethernet0/0
      46.0.0.0/26 is subnetted, 1 subnets
O IA     46.26.1.192 [110/40] via 192.168.1.17, 00:57:55, Ethernet0/0
      192.168.1.0/24 is variably subnetted, 14 subnets, 3 masks
O IA     192.168.1.0/30 [110/40] via 192.168.1.17, 00:57:56, Ethernet0/0
O IA     192.168.1.4/30 [110/30] via 192.168.1.17, 00:57:56, Ethernet0/0
O IA     192.168.1.8/30 [110/20] via 192.168.1.17, 00:58:06, Ethernet0/0
O IA     192.168.1.12/30 [110/30] via 192.168.1.17, 00:58:06, Ethernet0/0
O IA     192.168.1.20/30 [110/20] via 192.168.1.17, 00:58:06, Ethernet0/0
O IA     192.168.1.24/30 [110/30] via 192.168.1.17, 00:57:56, Ethernet0/0
O IA     192.168.1.28/30 [110/30] via 192.168.1.17, 00:57:56, Ethernet0/0
O IA     192.168.1.32/30 [110/30] via 192.168.1.17, 00:58:06, Ethernet0/0
O IA     192.168.1.36/30 [110/30] via 192.168.1.17, 00:58:06, Ethernet0/0
O IA     192.168.1.40/30 [110/40] via 192.168.1.17, 00:57:55, Ethernet0/0
O IA     192.168.1.128/26 [110/40] via 192.168.1.17, 00:57:55, Ethernet0/0
O IA     192.168.1.192/26 [110/40] via 192.168.1.17, 00:57:55, Ethernet0/0
```

Настроим фильтрацию на R15.
#### Маршрутизатор R15:
```
ip prefix-list BLOCK-101 seq 5 deny 192.168.1.0/30
ip prefix-list BLOCK-101 seq 10 permit 0.0.0.0/0 le 32
router ospf 1
  area 102 filter-list prefix BLOCK-101 in

ipv6 prefix-list BLOCK-101 seq 5 deny fde8:8a:fc:1:10:a3:0:20/127
ipv6 prefix-list BLOCK-101 seq 10 permit ::/0 le 128
ipv6 router ospf 1
  area 102 filter-list prefix BLOCK-101 in
```

Проверим правильность настройки фильтрации. Маршрутизатор R20 должен получать все маршруты, кроме маршрутов до сетей зоны 101.

#### Маршрутизатор R20:
```
R20#sh ip route ospf
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 192.168.1.17 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 192.168.1.17, 02:05:25, Ethernet0/0
      46.0.0.0/26 is subnetted, 1 subnets
O IA     46.26.1.192 [110/40] via 192.168.1.17, 00:05:51, Ethernet0/0
      192.168.1.0/24 is variably subnetted, 13 subnets, 3 masks
O IA     192.168.1.4/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.8/30 [110/20] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.12/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.20/30 [110/20] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.24/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.28/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.32/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.36/30 [110/30] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.40/30 [110/40] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.128/26 [110/40] via 192.168.1.17, 00:05:51, Ethernet0/0
O IA     192.168.1.192/26 [110/40] via 192.168.1.17, 00:05:51, Ethernet0/0
```
