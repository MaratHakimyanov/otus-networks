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

