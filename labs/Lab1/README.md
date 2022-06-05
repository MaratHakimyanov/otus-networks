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
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab1/Topology_stick.JPG)

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
#### Коммутатор S1:
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

#### Коммутатор S2:
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

Результаты шагов 1-2 приведены ниже.

#### Коммутатор S1:
```
vlan 3
  name Management
vlan 4
  name Operations
vlan 7
  name ParkingLot
vlan 8
  name Native
int vlan 3
  ip address 192.168.3.11 255.255.255.0
ip default-gateway 192.168.3.1
int range Fa0/2-4, Fa0/7-24, G0/1-2
  switchport mode access
  switchport access vlan 7 
  shutdown
int Fa0/6
  switchport mode access
  switchport access vlan 3
```

#### Коммутатор S2:
```
vlan 3
  name Management
vlan 4
  name Operations
vlan 7
  name ParkingLot
vlan 8
  name Native
int vlan 3
  ip address 192.168.3.12 255.255.255.0
ip default-gateway 192.168.3.1
int range Fa0/2-17, Fa0/19-24, G0/1-2
  switchport mode access
  switchport access vlan 7 
  shutdown
int Fa0/18
  switchport mode access
  switchport access vlan 4
```

Вывод команды **show vlan brief**.

#### Коммутатор S1:
```
S1#sh vlan brief


VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/5
3    Management                       active    Fa0/6
4    Operations                       active    
7    ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
8    Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active
```

#### Коммутатор S2:
```
S2#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
3    Management                       active    
4    Operations                       active    Fa0/18
7    ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
8    Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active
```

### 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

#### Шаги
1. Вручную настроить магистральный интерфейс F0/1
2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1

Настраиваем интерфейс F0/1 на обоих коммутаторах.
#### Коммутатор S1:
```
int Fa0/1
  switchport mode trunk
  switchport trunk native vlan 8
  switchport trunk allowed vlan 3,4,8
```

#### Коммутатор S2:
```
int Fa0/1
  switchport mode trunk
  switchport trunk native vlan 8
  switchport trunk allowed vlan 3,4,8
```

Вывод команды **show interfaces trunk**
#### Коммутатор S1:
```
S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4,8

Port        Vlans allowed and active in management domain
Fa0/1       3,4,8

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4,8
```

#### Коммутатор S2:
```
S2#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4,8

Port        Vlans allowed and active in management domain
Fa0/1       3,4,8

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4,8
```

Настраиваем интерфейс F0/5 на коммутаторе S1.
#### Коммутатор S1:
```
int Fa0/5
  switchport mode trunk
  switchport trunk native vlan 8
  switchport trunk allowed vlan 3,4,8
  copy running-config startup-config
```

Сохраняем конфигурацию коммутаторов.
#### Коммутатор S1:
```
copy running-config startup-config
```

#### Коммутатор S2:
```
copy running-config startup-config
```

Вывод команды **show interfaces trunk**
#### Коммутатор S1:
```
S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      8

Port        Vlans allowed on trunk
Fa0/1       3-4,8

Port        Vlans allowed and active in management domain
Fa0/1       3,4,8

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       3,4,8
```

Fa0/5 не отображается, потому что он находится в состоянии Down.

### 4. VLAN на маршрутизаторе

#### Шаги
1. Активировать интерфейс G0/0/1 на маршрутизаторе
2. Настроить подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации
3. С помощью команды show ip interface brief проверьте конфигурацию подинтерфейса

Результаты шагов 1-2 приведены ниже.

#### Маршрутизатор R1:
```
int G0/0/1
  no shutdown
int G0/0/1.3
  description Management
  encapsulation dot1q 3
  ip address 192.168.3.1 255.255.255.0
int G0/0/1.4
  description Operations
  encapsulation dot1q 4
  ip address 192.168.4.1 255.255.255.0
int G0/0/1.8
  description Native
  encapsulation dot1q 8 native
```

Проверяем конфигурацию подинтерфейсов командой **show ip interface brief**.
#### Маршрутизатор R1:
```
R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.3 192.168.3.1     YES manual up                    up 
GigabitEthernet0/0/1.4 192.168.4.1     YES manual up                    up 
GigabitEthernet0/0/1.8 unassigned      YES unset  up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```

### 4. Проверка маршрутизации между VLAN

Отправляем эхо-запрос с PC-A на шлюз по умолчанию.
#### PC-A:
```
C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

Отправляем эхо-запрос с PC-A на PC-B.
#### PC-A:
```
C:\>ping 192.168.4.3

Pinging 192.168.4.3 with 32 bytes of data:

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time=1ms TTL=127

Ping statistics for 192.168.4.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```

Отправляем эхо-запрос с PC-A на коммутатор S2.
#### PC-A:
```
C:\>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:

Reply from 192.168.3.12: bytes=32 time=9ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 9ms, Average = 2ms
```

Выполняем команду **tracert** с PC-B на PC-A.
#### PC-B:
```
C:\>tracert 192.168.3.3

Tracing route to 192.168.3.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3

Trace complete.
```

Отображается промежуточный IP-адрес 192.168.4.1, принадлежащий подинтерфейсу G0/0/1.4 на маршрутизаторе R1.
