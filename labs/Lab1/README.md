# Лабораторная работа №1. Настройка маршрутизации между VLAN с использованием конфигурации router-on-a-stick
### Цели:
1. Создание сети и настройка основных параметров устройства
2. Создание сетей VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами.
4. Настройка маршрутизации между сетями VLAN
5. Проверка работы маршрутизации между VLAN

### Решение:
### 1. Создание сети и настройка основных параметров устройства
#### Шаги
1. Создать сеть согласно топологии
2. Настроить базовые параметры для маршрутизатора
3. Настроить базовые параметры каждого коммутатора
4. Настроить узлы ПК

Создаём сеть согласно топологии:  
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab2/Topology.JPG)

### Таблица адресации
| Устройство | Интерфейс |  IP-адрес    | Маска подсети | Шлюз по умолчанию |
|------------|-----------| ------------ |---------------|-------------------|
| R1         | G0/0/1.3  | 192.168.3.1  | 255.255.255.0 | —                 |
|            | G0/0/1.4  | 192.168.4.1  | 255.255.255.0 |                   |
|            | G0/0/1.8  | —            | —             |                   |
| S1         | VLAN3     | 192.168.3.11 | 255.255.255.0 | 192.168.3.1       |
| S2         | VLAN3     | 192.168.3.12 | 255.255.255.0 | 192.168.3.1       |
| PC-A       | NIC       | 192.168.3.3  | 255.255.255.0 | 192.168.3.1       |
| PC-B       | NIC       | 192.168.4.3  | 255.255.255.0 | 192.168.4.1       |

### Таблица VLAN
| VLAN       | Имя        | Назначенный интерфейс                                           |
|------------|------------| ----------------------------------------------------------------|
| 3          | Management | S1: VLAN <br/> S2: VLAN 3 <br/> S1: F0/6                        |
| 4          | Operations | S2: F0/18                                                       |
| 7          | ParkingLot | S1: F0/2-4, F0/7-24, G0/1-2 <br/> S2: F0/2-17, F0/19-24, G0/1-2 |
| 8          | Native     | —                                                               |


Настроим базовые параметры маршрутизатора.
#### Маршрутизатор R1:
```
en
conf t
hostname R1
no ip domain-lookup
enable secret class
line vty 0 15
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited" 
copy running-config startup-config
clock set hh:mm:ss 31 may 2022
```

Настроим базовые параметры каждого коммутатора.
#### Rоммутатор S1:
```
en
conf t
hostname S1
no ip domain-lookup
enable secret class
line vty 0 15
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited"
clock set hh:mm:ss 31 may 2022
copy running-config startup-config
```

#### Rоммутатор S2:
```
en
conf t
hostname S2
no ip domain-lookup
enable secret class
line vty 0 15
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited"
clock set hh:mm:ss 31 may 2022
copy running-config startup-config
```

Настраиваем узлы ПК в соответствии с таблицей адресации.

### 2. Создание сетей VLAN и назначение портов коммутатора
#### Шаги
1. Создать сети VLAN на коммутаторах
2. Назначить сети VLAN соответствующим интерфейсам коммутатора












