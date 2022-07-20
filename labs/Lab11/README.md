# Лабораторная работа №11. BGP. Фильтрация
### Цели:
1. Настроить фильтрацию для офиса "Москва"
2. Настроить фильтрацию для офиса "С.-Петербург"

### Описание:
1. Настроить фильтрацию в офисе "Москва" так, чтобы не появилось транзитного трафика(As-path).
2. Настроить фильтрацию в офисе "С.-Петербург" так, чтобы не появилось транзитного трафика(Prefix-list).
3. Настроить провайдер "Киторн" так, чтобы в офис "Москва" отдавался только маршрут по умолчанию.
4. Настроить провайдер "Ламас" так, чтобы в офис "Москва" отдавался только маршрут по умолчанию и префикс офиса "С.-Петербург".

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab11/Lab11_Topology.JPG)

### Решение:
### 1. Настройка фильтрации в офисе "Москва"

Необходимо настроить фильтрацию в офисе "Москва" так, чтобы не появилось транзитного трафика. Для этого настроим AS-path ACL на маршрутизаторах R14 и R15.

#### Маршрутизатор R14:
```
ip as-path access-list 100 permit ^$
ip as-path access-list 100 deny .*
router bgp 1001
  neighbor 46.12.1.2 filter-list 100 out
```

#### Маршрутизатор R15:
```
ip as-path access-list 100 permit ^$
ip as-path access-list 100 deny .*
router bgp 1001
  neighbor 46.12.1.6 filter-list 100 out
```

Проверим правильность настройки с помощью команд **sh ip bgp neighbors <адрес> advertised-routes**.
#### Маршрутизатор R14:
```
R14#sh ip bgp neighbors 46.12.1.2 advertised-routes 

Total number of prefixes 0 
```

#### Маршрутизатор R15:
```
R15#sh ip bgp neighbors 46.12.1.6 advertised-routes 

Total number of prefixes 0 
```
### 2. Настройка фильтрации в офисе "С.-Петербург"

Необходимо настроить фильтрацию в офисе "С.-Петербург" так, чтобы не появилось транзитного трафика. Для этого настроим prefix-list на маршрутизаторе R18.
#### Маршрутизатор R18:
```
ip prefix-list LIST_OUT1 seq 5 deny 46.12.1.0/30
ip prefix-list LIST_OUT1 seq 10 deny 46.12.1.4/30
ip prefix-list LIST_OUT1 seq 15 deny 46.12.1.8/30
ip prefix-list LIST_OUT1 seq 20 deny 46.12.1.32/30
ip prefix-list LIST_OUT1 seq 25 deny 46.12.1.20/30
router bgp 2042
  neighbor 46.12.1.34 prefix-list LIST_OUT1 out
  neighbor 46.12.1.38 prefix-list LIST_OUT1 out
```

Проверим правильность настройки с помощью команд **sh ip bgp neighbors <адрес> advertised-routes**.
#### Маршрутизатор R18:
```
R18#sh ip bgp neighbors 46.12.1.34 advertised-routes 

Total number of prefixes 0 

R18#sh ip bgp neighbors 46.12.1.38 advertised-routes 

Total number of prefixes 0 
```


### 3. Настройка провайдера "Киторн"

Необходимо настроить провайдер "Киторн" так, чтобы в офис "Москва" отдавался только маршрут по умолчанию. Для этого настроим prefix-list и route-map на маршрутизаторах R22 и R14.
#### Маршрутизатор R22:
```
ip prefix-list DG1001 seq 5 deny 46.12.1.4/30
ip prefix-list DG1001 seq 10 deny 46.12.1.8/30
ip prefix-list DG1001 seq 15 deny 46.12.1.32/30
route-map RM-DG1001 permit 10
  match ip address prefix-list DG1001
router bgp 101
  neighbor 46.12.1.1 route-map RM-DG1001 out
```

#### Маршрутизатор R14:
```
ip prefix-list DG1001 seq 5 deny 46.12.1.4/30
ip prefix-list DG1001 seq 10 deny 46.12.1.8/30
ip prefix-list DG1001 seq 15 deny 46.12.1.32/30
route-map RM-DG1001 permit 10
  match ip address prefix-list DG1001
router bgp 1001
  neighbor 46.12.1.2 route-map RM-DG1001 in
```

Проверим правильность настройки с помощью команд **show ip route bgp**.
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
```

### 4. Настройка провайдера "Ламас"
Необходимо настроить провайдер "Ламас" так, чтобы в офис "Москва" отдавался только маршрут по умолчанию и префикс офиса "С.-Петербург". Для этого настроим prefix-list и route-map на маршрутизаторах R21, R15 и R18.
#### Маршрутизатор R21:
```
ip prefix-list PR1001 seq 5 deny 46.12.1.0/30
ip prefix-list PR1001 seq 10 deny 46.12.1.8/30
ip prefix-list PR1001 seq 15 permit 46.12.1.32/30
route-map RM-PR1001 permit 10
match ip address prefix-list PR1001
router bgp 301
  neighbor 46.12.1.5 route-map RM-PR1001 out
```

#### Маршрутизатор R18:
```
ip prefix-list PR1001 seq 5 deny 46.12.1.0/30
ip prefix-list PR1001 seq 10 deny 46.12.1.8/30
ip prefix-list PR1001 seq 15 permit 46.12.1.32/30
route-map RM-PR1001 permit 10
match ip address prefix-list PR1001
router bgp 2042
  neighbor 46.12.1.33 route-map RM-PR1001 out
  neighbor 46.12.1.37 route-map RM-PR1001 out
```

#### Маршрутизатор R15:
```
ip prefix-list PR1001 seq 5 deny 46.12.1.0/30
ip prefix-list PR1001 seq 10 deny 46.12.1.8/30
ip prefix-list PR1001 seq 15 permit 46.12.1.32/30
route-map RM-PR1001 permit 10
match ip address prefix-list PR1001
router bgp 1001
  neighbor 46.12.1.5 route-map RM-PR1001 out
```

Проверим правильность настройки с помощью команд **show ip route bgp**.
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

      46.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
B        46.12.1.32/30 [20/0] via 46.12.1.6, 00:00:28
```
