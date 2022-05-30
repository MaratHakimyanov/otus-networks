# Настройка STP
### Задание:
1. Создание сети и настройка основных параметров устройства
2. Выбор корневого моста
3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

### Решение:
#### 1. Создание сети и настройка основных параметров устройства
Создаём сеть согласно топологии:


Таблица адресации
| Устройство | Интерфейс |  IP-адрес   | Маска подсети |
|:----------:|:---------:| :--------:  |:-------------:|
| S1         | VLAN1     | 192.168.1.1 |255.255.255.0  |
| S2         | VLAN1     | 192.168.1.2 |255.255.255.0  |
| S3         | VLAN1     | 192.168.1.3 |255.255.255.0  |


Настроим базовые параметры коммутатора S1:
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

S2:
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

S3:
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

Проверяем способность коммутаторов обмениваться эхо-запросами. Эхо-запросы успешно выполняются.

#### 2. Выбор корневого моста
Отключаем все порты на коммутаторах:
```
S1,S2,S3:
interface range E0/0-3
shutdown
```

Настраиваем подключенные порты в качестве транковых:
```
S1,S2,S3:
interface range E0/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
```

Включаем порты E0/1 и E0/3 на всех коммутаторах:
```
S1,S2,S3:
int range E0/1,E0/3
no shutdown
```

