# Лабораторная работа №3. Реализация DHCPv4
### Цели:
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка двух серверов DHCPv4 на R1
3. Настройка и проверка DHCP-ретрансляции на R2

### Решение:
### 1. Создание сети и настройка основных параметров устройства
#### Шаги
1. Создать схему адресации
2. Создать сеть согласно топологии
3. Настроить базовые параметры каждого маршрутизатора
4. Настроить маршрутизацию между сетями VLAN на маршрутизаторе R1
5. Настроить G0/0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих
маршрутизаторов
6. Настроить базовые параметры каждого коммутатора
7. Создать сети VLAN на коммутаторе S1
8. Назначить сети VLAN соответствующим интерфейсам коммутатора
9. Вручную настроить интерфейс S1 F0/5 в качестве транка 802.1Q

Создаём сеть согласно топологии:  
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab3/Lab3.1/Topology_DHCP.JPG)

### Таблица адресации
Сеть настроена в соответствии с таблицей адресации:
| Устройство | Интерфейс    |  IP-адрес    | Маска подсети  | Шлюз по умолчанию |
|------------|--------------| ------------ |--------------- |-------------------|
| R1         | G0/0/0/0     | 10.0.0.1     | 255.255.255.252| N/A               |
|            | G0/0/0/1     | N/A          | N/A            |                   |
|            | G0/0/0/1.100 | —            | —              |                   |
|            | G0/0/0/1.200 | —            | —              |                   |
|            | G0/0/0/1.1000| N/A          | N/A            |                   |
| R2         | G0/0/0/0     | 10.0.0.2     | 255.255.255.252| N/A               |
|            | G0/0/0/1     |              |                |                   |
| S1         | VLAN200      |              |                |                   |
| S2         | VLAN1        |              |                |                   |
| PC-A       | NIC          | DHCP         | DHCP           | DHCP              |
| PC-B       | NIC          | DHCP         | DHCP           | DHCP              |

### Таблица VLAN
| VLAN       | Имя        | Назначенный интерфейс                                                        |
|------------|------------| -----------------------------------------------------------------------------|
| 1          | N/A        | S2: F0/18                                                                    |
| 100        | Clients    | S1: F0/6                                                                     |
| 200        | Management | S1: VLAN 200                                                                 |
| 999        | ParkingLot | S1: F0/1-4, F0/7-24, G0/1-2 <br/> S2: F0/1-4, F0/6-17, F0/19-24,<br/> G0/1-2 |
| 1000       | Native     | N/A                                                                          |


Настроим базовые параметры маршрутизаторов. 

#### Маршрутизатор R1:
```
en
conf t
no ip domain-lookup
hostname S1
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
  logging synchronous
banner motd % Unathorized access is prohibited % 
interface vlan 1
  ip address 192.168.1.1 255.255.255.0
  no shutdown
copy running-config startup-config
```

#### Маршрутизатор R2:
```
en
conf t
no ip domain-lookup
hostname S2
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
  logging synchronous
banner motd % Unathorized access is prohibited % 
interface vlan 1
  ip address 192.168.1.2 255.255.255.0
  no shutdown
copy running-config startup-config
```



