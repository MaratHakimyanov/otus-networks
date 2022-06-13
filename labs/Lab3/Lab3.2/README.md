# Лабораторная работа №3.2. Конфигурирование DHCPv6
### Цели:
1. Создание сети и настройка основных параметров устройства
2. Проверка назначения адреса SLAAC от R1
3. Настройка и проверка Stateless DHCPv6 сервера на R1
4. Настройка и проверка Stateful DHCPv6 сервера на R1
5. Настройка и проверка DHCPv6-ретрансляции на R2

### Решение:
### 1. Создание сети и настройка основных параметров устройства
#### Шаги
1. Создать сеть согласно топологии
2. Настроить базовые параметры каждого коммутатора (опционально)
3. Настроить базовые параметры каждого маршрутизатора
4. Настроить интерфейсы и маршрутизацию для обоих маршрутизаторов

### Таблица адресации
| Устройство | Интерфейс    |  IPv6-адрес          |
|------------|--------------| ---------------------|
| R1         | G0/0/0       | 2001:db8:acad:2::1/64| 
|            |              | fe80::1              | 
|            | G0/0/1       | 2001:db8:acad:1::1/64| 
|            |              | fe80::1              | 
| R2         | G0/0/0       | 2001:db8:acad:2::2/64|
|            |              | fe80::2              |
|            | G0/0/1       | 2001:db8:acad:3::1/64| 
|            |              | fe80::1              |
| PC-A       | NIC          | DHCP                 | 
| PC-B       | NIC          | DHCP                 | 

Создаём сеть согласно топологии:  
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab3/Lab3.1/Topology_DHCP.JPG)

Настроим базовые параметры каждого коммутатора.
#### Коммутатор S1:
```
en
conf t
hostname S1
no ip domain-lookup
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited"
int range F0/1-4,F0/7-24,G0/1-2
  shutdown
copy running-config startup-config
```
#### Коммутатор S2:
```
en
conf t
hostname S2
no ip domain-lookup
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited"
int range Fa0/1-4,Fa0/6-17,Fa0/19-24,G0/1-2
  shutdown
copy running-config startup-config
```

Настроим базовые параметры и интерфейсы маршрутизаторов. 
#### Маршрутизатор R1:
```
en
conf t
no ip domain-lookup
hostname R1
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited" 
ipv6 unicast-routing
int G0/0/0
  ipv6 address 2001:db8:acad:2::1/64
  ipv6 address fe80::1 link-local
  no shutdown
int G0/0/1
  ipv6 address 2001:db8:acad:1::1/64
  ipv6 address fe80::1 link-local
  no shutdown
copy running-config startup-config
```
#### Маршрутизатор R2:
```
en
conf t
no ip domain-lookup
hostname R2
enable secret class
line vty 0 4
  password cisco
  login
line console 0
  password cisco
  login
service password-encryption
banner motd "Unathorized access is prohibited" 
ipv6 unicast-routing
int G0/0/0
  ipv6 address 2001:db8:acad:2::2/64
  ipv6 address fe80::2 link-local
  no shutdown
int G0/0/1
  ipv6 address 2001:db8:acad:3::1/64
  ipv6 address fe80::1 link-local
  no shutdown
copy running-config startup-config
```

Настроим статические маршруты на маршрутизаторах.
#### Маршрутизатор R1:
```
ipv6 route ::/0 2001:db8:acad:2::2
```
#### Маршрутизатор R2:
```
ipv6 route ::/0 2001:db8:acad:2::1
```

Проверяем доступность и сохраняем конфигурацию.
#### Маршрутизатор R1:
```
R1#ping 2001:db8:acad:3::1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:db8:acad:2::2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```

### 2. Проверка назначения адреса SLAAC от R1

Включим PC-A и настроим его на IPv6.
#### PC-A
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::260:3EFF:FE06:4737
   IPv6 Address....................: 2001:DB8:ACAD:1:260:3EFF:FE06:4737
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
```

### 3. Настройка и проверка Stateless DHCPv6 сервера на R1
#### Шаги
1. Изучить конфигурацию PC-A
2. Настроить Stateless DHCPv6 на R1

Смотрим конфигурацию PC-A.
#### PC-A
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0060.3E06.4737
   Link-local IPv6 Address.........: FE80::260:3EFF:FE06:4737
   IPv6 Address....................: 2001:DB8:ACAD:1:260:3EFF:FE06:4737
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-2B-CB-5E-93-00-60-3E-06-47-37
   DNS Servers.....................: ::
                                     0.0.0.0
```

Настроим Stateless DHCPv6 на R1.
#### Маршрутизатор R1:
```
ipv6 dhcp pool R1-STATELESS
  dns-server 2001:db8:acad::254
  domain-name STATELESS.com
int G0/0/1
  ipv6 nd other-config-flag
  ipv6 dhcp server R1-STATELESS
```

Перезагрузим PC-A и посмотрим измененную конфигурацию.
#### PC-A
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: STATELESS.com 
   Physical Address................: 0060.3E06.4737
   Link-local IPv6 Address.........: FE80::260:3EFF:FE06:4737
   IPv6 Address....................: 2001:DB8:ACAD:1:260:3EFF:FE06:4737
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 395773329
   DHCPv6 Client DUID..............: 00-01-00-01-2B-CB-5E-93-00-60-3E-06-47-37
   DNS Servers.....................: 2001:DB8:ACAD::254
                                     0.0.0.0
```

Проверяем подключение до интерфейса G0/0/1 маршрутизатора R2.
#### PC-A
```
C:\>ping 2001:db8:acad:3::1

Pinging 2001:db8:acad:3::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254

Ping statistics for 2001:DB8:ACAD:3::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

### 4. Настройка и проверка Stateful DHCPv6 сервера на R1

Настроим Stateful DHCPv6 на R1.
#### Маршрутизатор R1:
```
ipv6 dhcp pool R2-STATEFUL
  address prefix 2001:db8:acad:3:aaa::/80
  dns-server 2001:db8:acad::254
  domain-name STATEFUL.com
int G0/0/0
  ipv6 dhcp server R2-STATEFUL
```

### 5. Настройка и проверка DHCPv6-ретрансляции на R2
#### Шаги
1. Включить PC-B и проверить адрес SLAAC, который он генерирует
2. Настроить R2 в качестве агента ретрансляции DHCP для локальной сети на G0/0/1
3. Попытаться получить IPv6-адрес от DHCPv6 на PC-B

Включим PC-B и проверим адрес SLAAC, который он генерирует.
#### PC-В
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 00D0.D388.3107
   Link-local IPv6 Address.........: FE80::2D0:D3FF:FE88:3107
   IPv6 Address....................: 2001:DB8:ACAD:3:2D0:D3FF:FE88:3107
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-85-53-93-35-00-D0-D3-88-31-07
   DNS Servers.....................: ::
                                     0.0.0.0
```

Настроим R2 в качестве агента ретрансляции DHCP для локальной сети на G0/0/1.
#### Маршрутизатор R2:
```
int G0/0/1
  ipv6 nd managed-config-flag
  ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
  % Invalid input detected at '^' marker.
ipv6 dhcp pool R2-STATEFUL
  address prefix 2001:db8:acad:3:aaa::/80
  dns-server 2001:db8:acad::254
  domain-name STATEFUL.com
int G0/0/1
  ipv6 nd managed-config-flag
  ipv6 dhcp server R2-STATEFUL
copy running-config startup-config
```
Cisco Packet Tracer не поддерживает команду **ipv6 dhcp relay**, настроим на R2 Stateful DHCPv6.

Перезагрузим PC-B и посмотрим измененную конфигурацию.
#### PC-В
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: STATEFUL.com 
   Physical Address................: 00D0.D388.3107
   Link-local IPv6 Address.........: FE80::2D0:D3FF:FE88:3107
   IPv6 Address....................: 2001:DB8:ACAD:3:AAA:A4B2:9610:7BFB
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 1041285535
   DHCPv6 Client DUID..............: 00-01-00-01-85-53-93-35-00-D0-D3-88-31-07
   DNS Servers.....................: 2001:DB8:ACAD::254
                                     0.0.0.0
```

Проверяем подключение до интерфейса G0/0/1 маршрутизатора R1.
#### PC-B
```
C:\>ping 2001:db8:acad:1::1

Pinging 2001:db8:acad:1::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:1::1: bytes=32 time=4ms TTL=254
Reply from 2001:DB8:ACAD:1::1: bytes=32 time<1ms TTL=254

Ping statistics for 2001:DB8:ACAD:1::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 4ms, Average = 1ms
```
