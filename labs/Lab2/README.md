# Лабораторная работа №2. Развертывание коммутируемой сети с резервными каналами
### Цели:
1. Создание сети и настройка основных параметров устройства
2. Выбор корневого моста
3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

### Решение:
### 1. Создание сети и настройка основных параметров устройства
#### Шаги
1. Создать сеть согласно топологии
2. Выполнить инициализацию и перезагрузку коммутаторов
3. Настроить базовые параметры каждого коммутатора
4. Проверить связь

Создаём сеть согласно топологии:  
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab2/Topology.JPG)

### Таблица адресации
Сеть настроена в соответствии с таблицей адресации:
| Устройство | Интерфейс |  IP-адрес   | Маска подсети |
|:----------:|:---------:| :--------:  |:-------------:|
| S1         | VLAN1     | 192.168.1.1 |255.255.255.0  |
| S2         | VLAN1     | 192.168.1.2 |255.255.255.0  |
| S3         | VLAN1     | 192.168.1.3 |255.255.255.0  |


Настроим базовые параметры коммутаторов. 

#### Коммутатор S1:
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

#### Коммутатор S2:
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

#### Коммутатор S3:
```
en
conf t
no ip domain-lookup
hostname S3
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
  ip address 192.168.1.3 255.255.255.0
  no shutdown
copy running-config startup-config
```

Проверяем способность коммутаторов обмениваться эхо-запросами. Проводим следующие проверки:

1. S1 - S2
2. S1 - S3
3. S2 - S3

#### S1 - S2:
```
S1#ping 192.168.1.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

#### S1 - S3:
```
S1#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

#### S2 - S3:
```
S2#ping 192.168.1.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

### 2. Выбор корневого моста
#### Шаги
1. Отключить все порты на коммутаторах
2. Настроить подключенные порты в качестве транковых
3. Включить порты Fa0/2 и Fa0/4 на всех коммутаторах
4. Отобразить данные протокола spanning-tree

Отключаем все порты на коммутаторах.
#### Коммутатор S1:
```
interface range Fa0/1-4
  shutdown
```

#### Коммутатор S2:
```
interface range Fa0/1-4
  shutdown
```

#### Коммутатор S3:
```
interface range Fa0/1-4
  shutdown
```

Настраиваем подключенные порты в качестве транковых.

#### Коммутатор S1:
```
interface range Fa0/1-4
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```
#### Коммутатор S2:
```
interface range Fa0/1-4
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```
#### Коммутатор S3:
```
interface range Fa0/1-4
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```

Включаем порты Fa0/2 и Fa0/4 на всех коммутаторах.

#### Коммутатор S1:
```
int range Fa0/2,Fa0/4
  no shutdown
```

#### Коммутатор S2:
```
int range Fa0/2,Fa0/4
  no shutdown
```

#### Коммутатор S3:
```
int range Fa0/2,Fa0/4
  no shutdown
```

Отображаем данные протокола spanning-tree.
#### Коммутатор S1:
```
S1#show span 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.96C1.5854
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     000C.85BC.E817
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Root FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```
#### Коммутатор S2:
```
S2#show span
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.96C1.5854
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.96C1.5854
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```
#### Коммутатор S3:
```
S3#show span
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.96C1.5854
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0060.476A.A71A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Root FWD 19        128.2    P2p
Fa0/4            Altn BLK 19        128.4    P2p
```
В результате получаем схему:
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab2/STP.JPG)

С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы:
1. Какой коммутатор является корневым мостом? **S2**
2. Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста? **Из-за наименьшего MAC адреса**
3. Какие порты на коммутаторе являются корневыми портами? **На коммутаторе S1 Fa0/2, на коммутаторе S3 Fa0/2**
4. Какие порты на коммутаторе являются назначенными портами? **На коммутаторе S2 Fa0/2 и Fa0/4, на коммутаторе S1 Fa0/4**
5. Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? **На коммутаторе S3 Fa0/4**
6. Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта? **Из-за наибольшего идентификатора порта отправителя (порт Fa0/2 на коммутаторе S2)**

