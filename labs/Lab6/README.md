# Лабораторная работа №6. OSPF. Фильтрация
### Цели:
1. Настроить OSPF офисе "Москва"
2. Разделить сеть на зоны
3. Настроить фильтрацию между зонами

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
```

Настроим OSPF на конкретных интерфейсах маршрутизаторов:
```
int <тип и номер интерфейса>
  ip ospf 1 area <номер зоны>
```

В выводе **running-config** маршрутизаторов появятся настройки:

#### Маршрутизатор R14:
```
interface Ethernet0/0
 ip address 192.168.1.6 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:23/127
!         
interface Ethernet0/1
 ip address 192.168.1.14 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:27/127
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
 ipv6 address FDE8:8A:FC:1:10:A3:0:21/127
!         
router ospf 1
 router-id 14.14.14.14
```

#### Маршрутизатор R15:
```
interface Ethernet0/0
 ip address 192.168.1.22 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:31/127
!         
interface Ethernet0/1
 ip address 192.168.1.10 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:25/127
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
!         
router ospf 1
 router-id 15.15.15.15
```

#### Маршрутизатор R12:
```
interface Ethernet0/0
 ip address 192.168.1.25 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:32/127
!         
interface Ethernet0/1
 ip address 192.168.1.29 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:34/127
!         
interface Ethernet0/2
 ip address 192.168.1.5 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:22/127
!         
interface Ethernet0/3
 ip address 192.168.1.9 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:24/127 
!         
router ospf 1
 router-id 12.12.12.12
```

#### Маршрутизатор R13:
```
interface Ethernet0/0
 ip address 192.168.1.37 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:38/127
!         
interface Ethernet0/1
 ip address 192.168.1.33 255.255.255.252
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:36/127
!         
interface Ethernet0/2
 ip address 192.168.1.21 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:30/127
!         
interface Ethernet0/3
 ip address 192.168.1.13 255.255.255.252
 ip ospf 1 area 0
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:26/127
!                 
router ospf 1
 router-id 13.13.13.13
```

#### Маршрутизатор R19:
```
interface Ethernet0/0
 ip address 192.168.1.2 255.255.255.252
 ip ospf 1 area 101
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:21/127
!                
router ospf 1
 router-id 19.19.19.19
```

#### Маршрутизатор R20:
```
interface Ethernet0/0
 ip address 192.168.1.18 255.255.255.252
 ip ospf 1 area 102
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A3:0:29/127
!              
router ospf 1
 router-id 20.20.20.20
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
!         
interface Ethernet0/0.401
 description MGMT
 encapsulation dot1Q 401
 ip address 46.26.1.195 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:83/122
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
!         
interface Ethernet0/0.401
 description MGMT
 encapsulation dot1Q 401
 ip address 46.26.1.196 255.255.255.192
 ip ospf 1 area 10
 ipv6 address FE80::14 link-local
 ipv6 address FDE8:8A:FC:1:10:A4:0:84/122
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
```



На маршрутизаторах R14 и R15 также настроим распространение маршрута по умолчанию:

#### Маршрутизатор R14:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.3
router ospf 1
  default-information originate
```

#### Маршрутизатор R15:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.7
router ospf 1
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
```


### 2. Разделение сетей на зоны





### 3. Настройка фильтрации между зонами






