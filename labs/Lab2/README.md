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
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

#### S1 - S3:
```
S1#ping 192.168.1.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 1/1/1 ms
```

#### S2 - S3:
```
S2#ping 192.168.1.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

### 2. Выбор корневого моста
#### Шаги
1. Отключить все порты на коммутаторах
2. Настроить подключенные порты в качестве транковых
3. Включить порты e0/1 и e0/3 на всех коммутаторах
4. Отобразить данные протокола spanning-tree

Отключаем все порты на коммутаторах.
#### Коммутатор S1:
```
interface range E0/0-3
  shutdown
```

#### Коммутатор S2:
```
interface range E0/0-3
  shutdown
```

#### Коммутатор S3:
```
interface range E0/0-3
  shutdown
```

Настраиваем подключенные порты в качестве транковых.

#### Коммутатор S1:
```
interface range E0/0-3
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```
#### Коммутатор S2:
```
interface range E0/0-3
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```
#### Коммутатор S3:
```
interface range E0/0-3
  switchport trunk allowed vlan 1
  switchport trunk encapsulation dot1q
  switchport mode trunk
```

Включаем порты E0/1 и E0/3 на всех коммутаторах.

#### Коммутатор S1:
```
int range E0/1,E0/3
  no shutdown
```

#### Коммутатор S2:
```
int range E0/1,E0/3
  no shutdown
```

#### Коммутатор S3:
```
int range E0/1,E0/3
  no shutdown
```

Отображаем данные протокола spanning-tree.
#### Коммутатор S1:

#### Коммутатор S2:

#### Коммутатор S3:

В результате получаем схему:


С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы:
1. Какой коммутатор является корневым мостом? S1
2. Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста? Из-за наименьшего MAC адреса
3. Какие порты на коммутаторе являются корневыми портами? На коммутаторе S2 e0/1, на коммутаторе S3 e0/3
4. Какие порты на коммутаторе являются назначенными портами? Порты корневого моста и порт e0/3 S2
5. Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? Порт e0/1 S3
6. Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта? Из-за наибольшего идентификатора порта отправителя (порт e0/3 коммутатора S1)

