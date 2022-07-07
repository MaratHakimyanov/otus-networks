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

Настроим Area 0 на маршрутизаторах R14, R15, R12, R13.


В выводе running-config маршрутизаторов появятся настройки:

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






### 2. Разделение сетей на зоны





### 3. Настройка фильтрации между зонами






