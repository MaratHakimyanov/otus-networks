# Лабораторная работа №9. BGP. Основы
### Цели:
1. Настроить BGP между автономными системами
2. Организовать доступность между офисами "Москва" и "С.-Петербург"

### Задачи:
1. Настроить eBGP между офисом "Москва" и двумя провайдерами - "Киторн" и "Ламас".
2. Настроить eBGP между провайдерами "Киторн" и "Ламас".
3. Настроить eBGP между "Ламас"/"Киторн" и "Триада".
4. Настроить eBGP между офисом "С.-Петербург" и провайдером "Триада".
5. Организовать IP доступность между пограничным роутерами офисов "Москва" и "С.-Петербург".

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab7/Lab7_Topology.JPG)

### Решение:
### 1. Настройка eBGP между офисом "Москва" и двумя провайдерами - "Киторн" и "Ламас"

Настроим eBGP на маршрутизаторах R14, R22, R15, R21.
#### Маршрутизатор R14:
```
router bgp 1001
  bgp router-id 14.14.14.14
  neighbor 46.12.1.2 remote-as 101
  neighbor 2DE8:8A:FC:1:10:A3:0:21 remote-as 101
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:21 activate
    neighbor 46.12.1.2 activate
  exit-address-family
```

#### Маршрутизатор R22:
```
router bgp 101
  bgp router-id 22.22.22.22
  neighbor 46.12.1.1 remote-as 1001
  neighbor 2DE8:8A:FC:1:10:A3:0:20 remote-as 1001
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:20 activate
    neighbor 46.12.1.1 activate
  exit-address-family
```

#### Маршрутизатор R15:
```
router bgp 1001
  bgp router-id 15.15.15.15
  neighbor 46.12.1.6 remote-as 301
  neighbor 2DE8:8A:FC:1:10:A3:0:23 remote-as 301
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:23 activate
    neighbor 46.12.1.6 activate
  exit-address-family
```

#### Маршрутизатор R21:
```
router bgp 301
  bgp router-id 21.21.21.21
  neighbor 46.12.1.5 remote-as 1001
  neighbor 2DE8:8A:FC:1:10:A3:0:22 remote-as 1001
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:22 activate
    neighbor 46.12.1.5 activate
  exit-address-family
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.

#### Маршрутизатор R14:
```
R14#sh ip bgp summary 
BGP router identifier 14.14.14.14, local AS number 1001
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:21
                4          101      15      15        1    0    0 00:12:11        0
46.12.1.2       4          101      60      61        1    0    0 00:51:33        0
```

#### Маршрутизатор R22:
```
R22#sh ip bgp summary 
BGP router identifier 22.22.22.22, local AS number 101
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:20
                4         1001      18      17        1    0    0 00:14:17        0
46.12.1.1       4         1001      63      62        1    0    0 00:53:40        0
```

#### Маршрутизатор R15:
```
R15#sh ip bgp summary 
BGP router identifier 15.15.15.15, local AS number 1001
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:23
                4          301       9       9        1    0    0 00:06:42        0
46.12.1.6       4          301      13      12        1    0    0 00:07:52        0
```

#### Маршрутизатор R21:
```
R21#sh ip bgp summary 
BGP router identifier 21.21.21.21, local AS number 301
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:22
                4         1001       8       8        1    0    0 00:05:53        0
46.12.1.5       4         1001      11      12        1    0    0 00:07:03        0
```

### 2. Настройка eBGP между провайдерами "Киторн" и "Ламас"

Настроим на маршрутизаторах R22 и R21 соседство для ipv4 и ipv6.

#### Маршрутизатор R22:
```
router bgp 101
  neighbor 46.12.1.9 remote-as 301
  neighbor 2DE8:8A:FC:1:10:A3:0:24 remote-as 301
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:24 activate
    neighbor 46.12.1.9 activate
  exit-address-family
```

#### Маршрутизатор R21:
```
router bgp 301
  neighbor 46.12.1.10 remote-as 101
  neighbor 2DE8:8A:FC:1:10:A3:0:25 remote-as 101
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:25 activate
    neighbor 46.12.1.10 activate
  exit-address-family
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.

#### Маршрутизатор R22:
```
R22#sh ip bgp summary 
BGP router identifier 22.22.22.22, local AS number 101
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:20
                4         1001      35      35        1    0    0 00:30:16        0
2DE8:8A:FC:1:10:A3:0:24
                4          301       3       3        1    0    0 00:01:36        0
46.12.1.1       4         1001      81      80        1    0    0 01:09:38        0
46.12.1.9       4          301       5       4        1    0    0 00:02:40        0
```

#### Маршрутизатор R21:
```
R21#sh ip bgp summary 
BGP router identifier 21.21.21.21, local AS number 301
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:22
                4         1001      28      28        1    0    0 00:24:01        0
2DE8:8A:FC:1:10:A3:0:25
                4          101       4       4        1    0    0 00:02:02        0
46.12.1.5       4         1001      32      32        1    0    0 00:25:11        0
46.12.1.10      4          101       5       5        1    0    0 00:03:06        0
```

### 3. Настройка eBGP между "Ламас"/"Киторн" и "Триада"

Настроим на маршрутизаторах R22-R23 и R21-R24 соседство для IPv4 и IPv6.

#### Маршрутизатор R22:
```
router bgp 101
  neighbor 46.12.1.14 remote-as 520
  neighbor 2DE8:8A:FC:1:10:A3:0:27 remote-as 520
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:27 activate
    neighbor 46.12.1.14 activate
  exit-address-family
```

#### Маршрутизатор R23:
```
router bgp 520
  bgp router-id 23.23.23.23
  neighbor 46.12.1.13 remote-as 101
  neighbor 2DE8:8A:FC:1:10:A3:0:26 remote-as 101
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:26 activate
    neighbor 46.12.1.13 activate
  exit-address-family
```

#### Маршрутизатор R21:
```
router bgp 301
  neighbor 46.12.1.18 remote-as 520
  neighbor 2DE8:8A:FC:1:10:A3:0:29 remote-as 520
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:29 activate
    neighbor 46.12.1.18 activate
  exit-address-family
```

#### Маршрутизатор R24:
```
router bgp 520
  bgp router-id 24.24.24.24
  neighbor 46.12.1.17 remote-as 301
  neighbor 2DE8:8A:FC:1:10:A3:0:28 remote-as 301
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:28 activate
    neighbor 46.12.1.17 activate
  exit-address-family
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.

#### Маршрутизатор R22:
```
R22#sh ip bgp summary 
BGP router identifier 22.22.22.22, local AS number 101
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:20
                4         1001      59      58        1    0    0 00:51:18        0
2DE8:8A:FC:1:10:A3:0:24
                4          301      26      26        1    0    0 00:22:38        0
2DE8:8A:FC:1:10:A3:0:27
                4          520       9       9        1    0    0 00:06:55        0
46.12.1.1       4         1001     104     103        1    0    0 01:30:40        0
46.12.1.9       4          301      28      27        1    0    0 00:23:42        0
46.12.1.14      4          520      12      10        1    0    0 00:08:00        0
```

#### Маршрутизатор R23:
```
R23#sh ip bgp summary 
BGP router identifier 192.168.2.5, local AS number 520
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:26
                4          101      10       9        1    0    0 00:07:19        0
46.12.1.13      4          101      11      13        1    0    0 00:08:24        0
```

#### Маршрутизатор R21:
```
R21#sh ip bgp summary 
BGP router identifier 21.21.21.21, local AS number 301
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:22
                4         1001      50      50        1    0    0 00:44:02        0
2DE8:8A:FC:1:10:A3:0:25
                4          101      25      26        1    0    0 00:22:03        0
2DE8:8A:FC:1:10:A3:0:29
                4          520       3       4        1    0    0 00:01:48        0
46.12.1.5       4         1001      54      54        1    0    0 00:45:12        0
46.12.1.10      4          101      27      27        1    0    0 00:23:07        0
46.12.1.18      4          520       7       5        1    0    0 00:02:58        0
```

#### Маршрутизатор R24:
```
R24#sh ip bgp summary 
BGP router identifier 192.168.2.9, local AS number 520
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:28
                4          301       2       2        1    0    0 00:00:17        0
46.12.1.17      4          301       3       5        1    0    0 00:01:27        0
```

### 4. Настройка eBGP между офисом "С.-Петербург" и провайдером "Триада"

Настроим на маршрутизаторах R24-R18 и R26-R18 соседство для IPv4 и IPv6.
#### Маршрутизатор R24:
```
router bgp 520
  neighbor 46.12.1.33 remote-as 2042
  neighbor 2DE8:8A:FC:1:10:A3:0:36 remote-as 2042
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:36 activate
    neighbor 46.12.1.33 activate
  exit-address-family
```

#### Маршрутизатор R26:
```
router bgp 520
  bgp router-id 26.26.26.26
  neighbor 46.12.1.37 remote-as 2042
  neighbor 2DE8:8A:FC:1:10:A3:0:38 remote-as 2042
  address-family ipv4
    neighbor 2DE8:8A:FC:1:10:A3:0:38 activate
    neighbor 46.12.1.37 activate
  exit-address-family
```

#### Маршрутизатор R18:
```
router bgp 2042
  neighbor 46.12.1.34 remote-as 520
  neighbor 46.12.1.38 remote-as 520
  neighbor 2DE8:8A:FC:1:10:A3:0:37 remote-as 520
  neighbor 2DE8:8A:FC:1:10:A3:0:39 remote-as 520
  address-family ipv4
    neighbor 46.12.1.34 activate
    neighbor 46.12.1.38 activate
    neighbor 2DE8:8A:FC:1:10:A3:0:37 activate
    neighbor 2DE8:8A:FC:1:10:A3:0:39 activate
  exit-address-family
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.
#### Маршрутизатор R24:
```
R24#sh ip bgp summary 
BGP router identifier 24.24.24.24, local AS number 520
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:28
                4          301      20      21        1    0    0 00:17:04        0
2DE8:8A:FC:1:10:A3:0:36
                4         2042       5       5        1    0    0 00:03:25        0
46.12.1.17      4          301      20      21        1    0    0 00:17:04        0
46.12.1.33      4         2042       9       7        1    0    0 00:04:48        0
```

#### Маршрутизатор R26:
```
R26#sh ip bgp summary 
BGP router identifier 26.26.26.26, local AS number 520
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:38
                4         2042       5       5        1    0    0 00:03:32        0
46.12.1.37      4         2042       7       9        1    0    0 00:05:05        0
```

#### Маршрутизатор R18:
```
R18#sh ip bgp summary 
BGP router identifier 192.168.4.6, local AS number 2042
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
2DE8:8A:FC:1:10:A3:0:37
                4          520       6       6        1    0    0 00:04:10        0
2DE8:8A:FC:1:10:A3:0:39
                4          520       6       6        1    0    0 00:03:52        0
46.12.1.34      4          520       8      10        1    0    0 00:05:33        0
46.12.1.38      4          520       9       7        1    0    0 00:05:26        0
```
### 5. Организация IP доступности между пограничным роутерами офисов "Москва" и "С.-Петербург"

Настроим на R22 передачу маршрутов к подсети R14 и R21 для IPv4 и IPv6.
#### Маршрутизатор R22:
```
router bgp 101
  address-family ipv4
    network 46.12.1.0 mask 255.255.255.252
    network 46.12.1.8 mask 255.255.255.252  
    exit-address-family
  address-family ipv6
    network 2DE8:8A:FC:1:10:A3:0:20/127
    network 2DE8:8A:FC:1:10:A3:0:24/127
    exit-address-family
```
Настроим на R21 передачу маршрутов к подсети R15 для IPv4 и IPv6. 
#### Маршрутизатор R21:
```
router bgp 301
  address-family ipv4
    network 46.12.1.4 mask 255.255.255.252 
    exit-address-family
  address-family ipv6
    network 2DE8:8A:FC:1:10:A3:0:22/127
    exit-address-family
```
Настроим на R24 передачу маршрутов к подсети R18 для IPv4 и IPv6.
#### Маршрутизатор R24:
```
router bgp 520
  address-family ipv4
    network 46.12.1.32 mask 255.255.255.252 
    exit-address-family
  address-family ipv6
    network 2DE8:8A:FC:1:10:A3:0:36/127
    exit-address-family
```

Проверим доступность между пограничными роутерами офисами "Москва" и "С.-Петербург" с помощью команды **show ip route bgp** и запуская **ping**.
#### Маршрутизатор R18:
```
R18#sh ip route bgp
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

      46.0.0.0/8 is variably subnetted, 7 subnets, 2 masks
B        46.12.1.0/30 [20/0] via 46.12.1.34, 00:16:47
B        46.12.1.4/30 [20/0] via 46.12.1.34, 00:13:58
B        46.12.1.8/30 [20/0] via 46.12.1.34, 00:16:16
R18#ping 46.12.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 46.12.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R18#ping 46.12.1.5
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 46.12.1.5, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

#### Маршрутизатор R14:
```
R14#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 46.12.1.3 to network 0.0.0.0

      46.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
B        46.12.1.4/30 [20/0] via 46.12.1.2, 00:15:39
B        46.12.1.8/30 [20/0] via 46.12.1.2, 00:17:57
B        46.12.1.32/30 [20/0] via 46.12.1.2, 00:13:37
R14#ping 46.12.1.33
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 46.12.1.33, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

#### Маршрутизатор R15:
```
R15#sh ip route bgp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 46.12.1.7 to network 0.0.0.0

      46.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
B        46.12.1.0/30 [20/0] via 46.12.1.6, 00:20:19
B        46.12.1.8/30 [20/0] via 46.12.1.6, 00:19:47
B        46.12.1.32/30 [20/0] via 46.12.1.6, 00:15:27
R15#ping 46.12.1.33
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 46.12.1.33, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
