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
  exit-address-family
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int

address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 18.18.18.18
  no shutdown
  exit-address-family
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int
```


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
  exit-address-family
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int

address-family ipv6 unicast autonomous-system 1
  topology base
    exit-af-topology  
  eigrp router-id 18.18.18.18
  no shutdown
  exit-address-family
  af-int e0/2
    passive-int
  af-int e0/3
    passive-int
```





