# Лабораторная работа №10. iBGP
### Цели:
1. Настроить iBGP в офисе "Москва"
2. Настроить iBGP в сети провайдера "Триада"

### Описание:
1. Настроить iBGP в офисом "Москва" между маршрутизаторами R14 и R15.
2. Настроить iBGP в провайдере "Триада".
3. Настроить офис "Москва" так, чтобы приоритетным провайдером стал "Ламас".
4. Настроить офис "С.-Петербург" так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
5. Все сети в лабораторной работе должны иметь IP связность.

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab10/Lab10_Topology.JPG)

### Решение:
### 1. Настройка iBGP в офисом "Москва" между маршрутизаторами R14 и R15

Настроим на R14 и R15 loopback-интерфейсы, настроим на них OSPF, и настроим между ними соседство iBGP.  
#### Маршрутизатор R14:
```
int lo0
  ip address 14.14.14.14 255.255.255.255
  ip ospf 1 area 0
router bgp 1001
  neighbor 15.15.15.15 remote-as 1001
  neighbor 15.15.15.15 update-source lo0
  address-family ipv4
    neighbor 15.15.15.15 activate
    neighbor 15.15.15.15 next-hop-self
  exit-address-family
```

#### Маршрутизатор R15:
```
int lo0
  ip address 15.15.15.15 255.255.255.255
  ip ospf 1 area 0
router bgp 1001
  neighbor 14.14.14.14 remote-as 1001
  neighbor 14.14.14.14 update-source lo0
  address-family ipv4
    neighbor 14.14.14.14 activate
    neighbor 14.14.14.14 next-hop-self
  exit-address-family
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.
#### Маршрутизатор R14:
```
R14#sh ip bgp summary 
BGP router identifier 14.14.14.14, local AS number 1001
BGP table version is 10, main routing table version 10
4 network entries using 560 bytes of memory
10 path entries using 800 bytes of memory
5/3 BGP path/bestpath attribute entries using 720 bytes of memory
5 BGP AS-PATH entries using 120 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 2200 total bytes of memory
BGP activity 4/0 prefixes, 12/2 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
15.15.15.15     4         1001      20      20       10    0    0 00:09:26        2
2DE8:8A:FC:1:10:A3:0:21
                4          101      45      48       10    0    0 00:35:38        4
46.12.1.2       4          101      44      48       10    0    0 00:35:37        4
```

#### Маршрутизатор R15:
```
R15#sh ip bgp summary 
BGP router identifier 15.15.15.15, local AS number 1001
BGP table version is 7, main routing table version 7
4 network entries using 560 bytes of memory
10 path entries using 800 bytes of memory
4/3 BGP path/bestpath attribute entries using 576 bytes of memory
4 BGP AS-PATH entries using 96 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 2032 total bytes of memory
BGP activity 4/0 prefixes, 13/3 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
14.14.14.14     4         1001      13      13        7    0    0 00:03:00        2
2DE8:8A:FC:1:10:A3:0:23
                4          301      37      38        7    0    0 00:29:11        4
46.12.1.6       4          301      37      38        7    0    0 00:29:04        4
```

### 2. Настройка iBGP в провайдере "Триада"

Настроим на R23,R24,R25,R26 loopback-интерфейсы, настроим на них IS-IS, и настроим между ними iBGP. Настроим R23 в качестве  Route-Reflector.

#### Маршрутизатор R23:
```
int lo0
  ip address 23.23.23.23 255.255.255.255
  ip router isis
router bgp 520
  neighbor 24.24.24.24 remote-as 520
  neighbor 24.24.24.24 update-source Loopback0
  neighbor 24.24.24.24 route-reflector-client
  neighbor 25.25.25.25 remote-as 520
  neighbor 25.25.25.25 update-source Loopback0
  neighbor 25.25.25.25 route-reflector-client
  neighbor 26.26.26.26 remote-as 520
  neighbor 26.26.26.26 update-source Loopback0
  neighbor 26.26.26.26 route-reflector-client
  exit-address-family
```

#### Маршрутизатор R24:
```
int lo0
  ip address 24.24.24.24 255.255.255.255
  ip router isis
router bgp 520
  neighbor 23.23.23.23 remote-as 520
  neighbor 23.23.23.23 update-source Loopback0
```

#### Маршрутизатор R25:
```
int lo0
  ip address 25.25.25.25 255.255.255.255
  ip router isis
router bgp 520
  neighbor 23.23.23.23 remote-as 520
  neighbor 23.23.23.23 update-source Loopback0
```

#### Маршрутизатор R26:
```
int lo0
  ip address 26.26.26.26 255.255.255.255
  ip router isis
router bgp 520
  neighbor 23.23.23.23 remote-as 520
  neighbor 23.23.23.23 update-source Loopback0
```

Проверим правильность настройки с помощью команды **show ip bgp summary**.
#### Маршрутизатор R23:
```
R23#sh ip bgp summary 
BGP router identifier 23.23.23.23, local AS number 520
BGP table version is 11, main routing table version 11
4 network entries using 560 bytes of memory
10 path entries using 800 bytes of memory
5/3 BGP path/bestpath attribute entries using 720 bytes of memory
4 BGP AS-PATH entries using 96 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 2176 total bytes of memory
BGP activity 6/2 prefixes, 13/3 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
24.24.24.24     4          520      23      25       11    0    0 00:15:26        4
25.25.25.25     4          520      44      52       11    0    0 00:36:42        0
26.26.26.26     4          520      44      47       11    0    0 00:36:00        0
2DE8:8A:FC:1:10:A3:0:26
                4          101      15      15       11    0    0 00:08:24        3
46.12.1.13      4          101      15      18       11    0    0 00:08:14        3
```

#### Маршрутизатор R24:
```
R24#sh ip bgp summary 
BGP router identifier 24.24.24.24, local AS number 520
BGP table version is 5, main routing table version 5
4 network entries using 560 bytes of memory
10 path entries using 800 bytes of memory
5/3 BGP path/bestpath attribute entries using 720 bytes of memory
4 BGP AS-PATH entries using 96 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 2176 total bytes of memory
BGP activity 5/0 prefixes, 11/0 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
23.23.23.23     4          520      24      22        5    0    0 00:14:55        3
2DE8:8A:FC:1:10:A3:0:28
                4          301      14      17        5    0    0 00:07:53        3
2DE8:8A:FC:1:10:A3:0:36
                4         2042      13      14        5    0    0 00:08:03        0
46.12.1.17      4          301      13      16        5    0    0 00:07:55        3
46.12.1.33      4         2042      14      16        5    0    0 00:08:02        0
```

#### Маршрутизатор R25:
```
R25#sh ip bgp summary 
BGP router identifier 25.25.25.25, local AS number 520
BGP table version is 4, main routing table version 4
4 network entries using 560 bytes of memory
4 path entries using 320 bytes of memory
3/1 BGP path/bestpath attribute entries using 432 bytes of memory
1 BGP rrinfo entries using 24 bytes of memory
2 BGP AS-PATH entries using 48 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1384 total bytes of memory
BGP activity 4/0 prefixes, 7/3 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
23.23.23.23     4          520      51      43        4    0    0 00:35:43        4
```

#### Маршрутизатор R26:
```
R26#sh ip bgp summary 
BGP router identifier 26.26.26.26, local AS number 520
BGP table version is 4, main routing table version 4
4 network entries using 560 bytes of memory
4 path entries using 320 bytes of memory
3/1 BGP path/bestpath attribute entries using 432 bytes of memory
1 BGP rrinfo entries using 24 bytes of memory
2 BGP AS-PATH entries using 48 bytes of memory
0 BGP route-map cache entries using 0 bytes of memory
0 BGP filter-list cache entries using 0 bytes of memory
BGP using 1384 total bytes of memory
BGP activity 4/0 prefixes, 7/3 paths, scan interval 60 secs

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
23.23.23.23     4          520      46      43        4    0    0 00:34:33        4
2DE8:8A:FC:1:10:A3:0:38
                4         2042      12      14        4    0    0 00:06:58        0
46.12.1.37      4         2042      12      12        4    0    0 00:07:01        0
```

### 3. Настройка офиса "Москва"

Необходимо настроить офис "Москва" так, чтобы приоритетным провайдером стал "Ламас". Для этого настроим на R15 route-map для R21, меняющий local-pregerence на 250. Настроить route-map на BGP соседа R21.

#### Маршрутизатор R15:
```
route-map AS301 permit 10
  set local-preference 250
router bgp 1001
  address-family ipv4
    neighbor 46.12.1.6 route-map AS301 in
  address-family ipv6
    neighbor 2de8:8a:fc:1:10:a3:0:23 route-map AS301 in
```

Проверим правильность настройки с помощью команды **show ip route bgp**.
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

      46.0.0.0/8 is variably subnetted, 6 subnets, 3 masks
B        46.12.1.0/30 [200/0] via 14.14.14.14, 01:41:07
B        46.12.1.8/30 [200/0] via 14.14.14.14, 01:41:07
B        46.12.1.32/30 [20/0] via 46.12.1.6, 02:06:16
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

      46.0.0.0/8 is variably subnetted, 6 subnets, 3 masks
B        46.12.1.4/30 [200/0] via 15.15.15.15, 01:42:10
B        46.12.1.8/30 [20/0] via 46.12.1.2, 02:08:54
B        46.12.1.32/30 [20/0] via 46.12.1.2, 00:45:41
```

### 4. Настройка офиса "С.-Петербург"

Необходимо настроить офис "С.-Петербург" так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.

#### Маршрутизатор R18:
```
router bgp 2042
  address-family ipv4
    maximum-path 2
```

Проверим правильность настройки с помощью команды **show ip route bgp**.

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
B        46.12.1.0/30 [20/0] via 46.12.1.38, 00:06:53
                      [20/0] via 46.12.1.34, 00:06:53
B        46.12.1.4/30 [20/0] via 46.12.1.38, 00:06:53
                      [20/0] via 46.12.1.34, 00:06:53
B        46.12.1.8/30 [20/0] via 46.12.1.38, 00:06:53
                      [20/0] via 46.12.1.34, 00:06:53
```
