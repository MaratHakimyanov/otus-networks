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
ip route 0.0.0.0 0.0.0.0 46.12.1.17
ipv6 route ::/0 2DE8:8A:FC:1:10:A3:0:28
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




