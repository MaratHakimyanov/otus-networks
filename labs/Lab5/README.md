# Лабораторная работа №5. Policy-based Routing
### Цели:
1. Настроить политику маршрутизации для сетей офиса "Чокурдах". Распределить трафик между двумя линками с провайдером. Настроить отслеживание линка через технологию IP SLA.(только для IPv4)
2. Настроить для офиса "Лабытнанги" маршрут по-умолчанию.

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab5/Lab5_Topology.JPG)

### Решение:
### 1. Настройка политики маршрутизации
#### Шаги
1. Настроить политику маршрутизации для сетей офиса "Чокурдах"
2. Распределить трафик между двумя линками с провайдером
3. Настроить отслеживание линка через технологию IP SLA.(только для IPv4)

Распределим трафик между двумя линками провайдера так, чтобы трафик из VLAN 102 отправлялся через маршрутизатор R26, а VLAN 402 через маршрутизатор R25. Для этого необходимо настроить R28: создать два списка доступа для обоих подсетей, создать route-map с двумя пунктами для каждого линка и с назначением в качестве next-hop необходимого маршрутизатора, назначить route-map на интерфейсы в сторону SW29.

#### Маршрутизатор R28:
```
ip access-list standard ACL102
  permit 192.168.3.0 0.0.0.255
  deny any
ip access-list standard ACL402
 permit 46.26.1.192 0.0.0.63
 deny any
route-map LAB5 permit 10
  match ip address ACL102
  set ip next-hop 46.12.1.29
route-map LAB5 permit 20
  match ip address ACL402
  set ip next-hop 46.12.1.25
int range e0/0-1
  ip policy route-map LAB5
```

IP-связности между "Триада" и "Чокурды" нет. Настроим R28 вы качестве шлюв по умолчанию для R25 R26, а также редистрибуцию статических маршрутов.
#### Маршрутизатор R25:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.26
router bgp 520
  redistribute static
```

#### Маршрутизатор R26:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.30
router bgp 520
  redistribute static
```
 
Теперь проверим правильность настройки на коммутаторе SW29, VPC30, VPC31 c помощью команды **traceroute**.

#### Коммутатор SW29:
```
SW29#traceroute 192.168.52.2
Type escape sequence to abort.
Tracing the route to 192.168.52.2
VRF info: (vrf in name/id, vrf out name/id)
  1 80.80.1.129 2 msec 0 msec 1 msec
  2  * 
    89.110.29.221 0 msec 1 msec
  3 192.168.52.5 0 msec 1 msec 1 msec
  4 192.168.52.2 1 msec *  1 msec
```

#### VPC30:
```
VPC30> traceroute 192.168.52.2
Bad command: "traceroute 192.168.52.2". Use ? for help.

VPC30> trace 192.168.2.5     
trace to 192.168.2.5, 8 hops max, press Ctrl+C to stop
 1   192.168.3.1   0.636 ms  0.304 ms  0.278 ms
 2     *46.12.1.25   0.851 ms  0.913 ms
 3   *192.168.2.5   1.118 ms (ICMP type:3, code:3, Destination port unreachable)  *
```

#### VPC31:
```
VPC31> trace 192.168.2.5
trace to 192.168.2.5, 8 hops max, press Ctrl+C to stop
 1   192.168.3.1   0.549 ms  0.353 ms  0.309 ms
 2   46.12.1.25   0.579 ms  0.588 ms  0.659 ms
 3   *192.168.2.5   0.912 ms (ICMP type:3, code:3, Destination port unreachable)  *
```

Настроим IP SLA на маршрутизаторе R28 для проверки доступности маршрутизаторов провайдера R25 и R26.

#### Маршрутизатор R28:
```
ip sla 1
 icmp-echo 46.12.1.25
 frequency 20
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-echo 46.12.1.29
 frequency 20
ip sla schedule 2 life forever start-time now
```

Проверим работоспособность IP SLA на маршрутизаторе R28 c помощью команды **sh ip sla statistics**.
#### Маршрутизатор R28:
```
R28#sh ip sla statistics 
IPSLAs Latest Operation Statistics

IPSLA operation id: 1
Latest RTT: 1 milliseconds
Latest operation start time: 23:52:51 msk Mon Aug 1 2022
Latest operation return code: OK
Number of successes: 25
Number of failures: 0
Operation time to live: Forever



IPSLA operation id: 2
Latest RTT: 1 milliseconds
Latest operation start time: 21:40:59 msk Mon Aug 1 2022
Latest operation return code: OK
Number of successes: 24
Number of failures: 0
Operation time to live: Forever
```

### 2. Настройка маршрута по-умолчанию для офиса "Лабытнанги" 

Необходимо настроить на маршрутизаторе R27 маршрут по умолчанию для IPv4 и IPv6.
#### Маршрутизатор R27:
```
ip route 0.0.0.0 0.0.0.0 46.12.1.21
ipv6 route ::/0 2de8:8a:fc:1:10:a3:0:30
```

