# Лабораторная работа №4. IPv4/6
### Цели:
1. Разработать и задокументировать адресное пространство для лабораторного стенда
2. Настроить IP-адреса на каждом активном порту
3. Настроить каждый VPC в каждом офисе в своем VLAN
4. Настроить VLAN управления для сетевых устройств


### Решение:
### 1. Разработать адресное пространство

В ходе выполнения лабораторной работы было разработано адресное пространство.

### Таблица адресации
|   Устройство   | Интерфейс    |Тип IP-адреса        |           IP-адрес          |       Маска подсети/префикс IPv6        |     Описание      |
|----------------|--------------|-------------------- | --------------------------- |-----------------------------------------|-------------------|
|**Москва**      |              |                     |                             |                                         |                   |
| R14            | e0/0         | IPv4                | 192.168.1.6                 | 255.255.255.252                         | R14 - R12         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:23/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R14 - R12         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R14 - R12         |
|                | e0/1         | IPv4                | 192.168.1.14                | 255.255.255.252                         | R14 - R13         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:27/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R14 - R13         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R14 - R13         |
|                | e0/2         | IPv4                | 46.12.1.1                   | 255.255.255.252                         | R14 - R22         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:20/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R14 - R22         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R14 - R22         |
|                | e0/3         | IPv4                | 192.168.1.1                 | 255.255.255.252                         | R14 - R19         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:20/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R14 - R19         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R14 - R19         |
| R15            | e0/0         | IPv4                | 192.168.1.22                | 255.255.255.252                         | R15 - R13         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:31/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R15 - R13         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R15 - R13         |
|                | e0/1         | IPv4                | 192.168.1.10                | 255.255.255.252                         | R15 - R12         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:25/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R15 - R12         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R15 - R12         |
|                | e0/2         | IPv4                | 46.12.1.5                   | 255.255.255.252                         | R15 - R21         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:22/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R15 - R21         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R15 - R21         |
|                | e0/3         | IPv4                | 192.168.1.17                | 255.255.255.252                         | R15 - R20         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:28/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R15 - R20         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R15 - R20         |
| R19            | e0/0         | IPv4                | 192.168.1.2                 | 255.255.255.252                         | R19 - R14         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:21/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R19 - R14         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R19 - R14         |
| R20            | e0/0         | IPv4                | 192.168.1.18                | 255.255.255.252                         | R20 - R15         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:29/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R20 - R15         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R20 - R15         |
| R12            | e0/0         | IPv4                | 192.168.1.25                | 255.255.255.252                         | R12 - SW4         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:32/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R12 - SW4         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R12 - SW4         |
|                | e0/1         | IPv4                | 192.168.1.29                | 255.255.255.252                         | R12 - SW5         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:34/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R12 - SW5         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R12 - SW5         |
|                | e0/2         | IPv4                | 192.168.1.5                 | 255.255.255.252                         | R12 - R14         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:22/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R12 - R14         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R12 - R14         |
|                | e0/3         | IPv4                | 192.168.1.9                 | 255.255.255.252                         | R12 - R15         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:24/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R12 - R15         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R12 - R15         |
| R13            | e0/0         | IPv4                | 192.168.1.37                | 255.255.255.252                         | R13 - SW5         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:38/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R13 - SW5         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R13 - SW5         |
|                | e0/1         | IPv4                | 192.168.1.33                | 255.255.255.252                         | R13 - SW4         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:36/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R13 - SW4         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R13 - SW4         |
|                | e0/2         | IPv4                | 192.168.1.21                | 255.255.255.252                         | R13 - R15         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:30/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R13 - R15         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R13 - R15         |
|                | e0/3         | IPv4                | 192.168.1.13                | 255.255.255.252                         | R13 - R14         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:26/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R13 - R14         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R13 - R14         |
| SW4            | e0.100       | IPv4                | 192.168.1.129               | 255.255.255.192                         | VLAN 100 e0/1     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:40/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 100 e0/1     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 100 e0/1     |
|                | e0.401       | IPv4                | 42.26.1.195                 | 255.255.255.192                         | VLAN 401 e0/1     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:83/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 401 e0/1     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 401 e0/1     |
|                | e0.101       | IPv4                | 192.168.1.193               | 255.255.255.192                         | VLAN 101 e0/0     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:2/122  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 101 e0/0     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 101 e0/0     |
|                | e1/0         | IPv4                | 192.168.1.26                | 255.255.255.252                         | SW4 - R12         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:33/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW4 - R12         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW4 - R12         |
|                | e1/1         | IPv4                | 192.168.1.34                | 255.255.255.252                         | SW4 - R13         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:37/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW4 - R13         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW4 - R13         |
| SW5            | e0.100       | IPv4                | 192.168.1.130               | 255.255.255.192                         | VLAN 100 e0/0     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:41/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 100 e0/0     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 100 e0/0     |
|                | e0.401       | IPv4                | 42.26.1.196                 | 255.255.255.192                         | VLAN 401 e0/1     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:84/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 401 e0/1     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 401 e0/1     |
|                | e0.101       | IPv4                | 192.168.1.194               | 255.255.255.192                         | VLAN 101 e0/1     |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:1/122  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 101 e0/1     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 101 e0/1     |
|                | e1/0         | IPv4                | 192.168.1.38                | 255.255.255.252                         | SW5 - R13         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:39/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW5 - R13         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW5 - R13         |
|                | e1/1         | IPv4                | 192.168.1.30                | 255.255.255.252                         | SW5 - R12         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:35/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW5 - R12         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW5 - R12         |
| SW3            | VLAN 401     | IPv4                | 42.26.1.193                 | 255.255.255.192                         | VLAN 401          |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:82/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 401          |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 401          |
| SW2            | VLAN 401     | IPv4                | 42.26.1.194                 | 255.255.255.192                         | VLAN 401          |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:81/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 401          |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 401          |
| VPC1           | eth0         | IPv4                | 192.168.1.195               | 255.255.255.192                         |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:42/122 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |
| VPC7           | eth0         | IPv4                | 192.168.1.131               | 255.255.255.192                         |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a4:0:3/122  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |
|**Киторн**      |              |                     |                             |                                         |                   |
| R22            | e0/0         | IPv4                | 46.12.1.2                   | 255.255.255.252                         | R22 - R14         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:21/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R22 - R14         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R22 - R14         |
|                | e0/1         | IPv4                | 46.12.1.10                  | 255.255.255.252                         | R22 - R21         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:25/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R22 - R21         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R22 - R21         |
|                | e0/2         | IPv4                | 46.12.1.13                  | 255.255.255.252                         | R22 - R23         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:26/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R22 - R23         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R22 - R23         |
|**Ламас**       |              |                     |                             |                                         |                   |
| R21            | e0/0         | IPv4                | 46.12.1.2                   | 255.255.255.252                         | R21 - R15         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:23/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R21 - R15         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R21 - R15         |
|                | e0/1         | IPv4                | 46.12.1.10                  | 255.255.255.252                         | R21 - R22         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:24/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R21 - R22         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R21 - R22         |
|                | e0/2         | IPv4                | 46.12.1.13                  | 255.255.255.252                         | R21 - R24         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:28/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R21 - R24         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R21 - R24         |
|**Триада**      |              |                     |                             |                                         |                   |
| R23            | e0/0         | IPv4                | 46.12.1.14                  | 255.255.255.252                         | R23 - R22         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:27/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R23 - R22         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R23 - R22         |
|                | e0/1         | IPv4                | 192.168.2.5                 | 255.255.255.252                         | R23 - R25         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:44/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R23 - R25         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R23 - R25         |
|                | e0/2         | IPv4                | 192.168.2.1                 | 255.255.255.252                         | R23 - R24         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:42/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R23 - R24         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R23 - R24         |
| R24            | e0/0         | IPv4                | 46.12.1.18                  | 255.255.255.252                         | R24 - R21         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:29/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R24 - R21         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R24 - R21         |
|                | e0/1         | IPv4                | 192.168.2.9                 | 255.255.255.252                         | R24 - R26         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:46/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R24 - R26         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R24 - R26         |
|                | e0/2         | IPv4                | 192.168.2.2                 | 255.255.255.252                         | R24 - R23         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:43/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R24 - R23         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R24 - R23         |
|                | e0/3         | IPv4                | 46.12.1.34                  | 255.255.255.252                         | R24 - R18         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:37/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R24 - R18         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R24 - R18         |
| R25            | e0/0         | IPv4                | 192.168.2.6                 | 255.255.255.252                         | R25 - R23         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:45/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R25 - R23         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R25 - R23         |
|                | e0/1         | IPv4                | 46.12.1.21                  | 255.255.255.252                         | R25 - R27         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:30/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R25 - R27         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R25 - R27         |
|                | e0/2         | IPv4                | 192.168.2.13                | 255.255.255.252                         | R25 - R26         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:48/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R25 - R26         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R25 - R26         |
|                | e0/3         | IPv4                | 46.12.1.25                  | 255.255.255.252                         | R25 - R28         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:32/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R25 - R28         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R25 - R28         |
| R26            | e0/0         | IPv4                | 192.168.2.10                | 255.255.255.252                         | R26 - R24         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:47/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R26 - R24         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R26 - R24         |
|                | e0/1         | IPv4                | 46.12.1.29                  | 255.255.255.252                         | R26 - R28         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:34/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R26 - R28         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R26 - R28         |
|                | e0/2         | IPv4                | 192.168.2.14                | 255.255.255.252                         | R26 - R25         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:49/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R26 - R25         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R26 - R25         |
|                | e0/3         | IPv4                | 46.12.1.38                  | 255.255.255.252                         | R26 - R18         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:39/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R26 - R18         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R26 - R18         |
|**Лабытнанги**  |              |                     |                             |                                         |                   |
| R27            | e0/0         | IPv4                | 46.12.1.22                  | 255.255.255.252                         | R27 - R25         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:31/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R27 - R25         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R27 - R25         |
|**Чокурдах**    |              |                     |                             |                                         |                   |
| R28            | e0/0         | IPv4                | 46.12.1.14                  | 255.255.255.252                         | R28 - R26         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:35/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R28 - R26         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R28 - R26         |
|                | e0/1         | IPv4                | 192.168.2.5                 | 255.255.255.252                         | R28 - R25         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:33/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R28 - R25         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R28 - R25         |
|                | e0.402       | IPv4                | 42.26.2.193                 | 255.255.255.192                         | VLAN 402 e0/2     |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:50/124 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | VLAN 402 e0/2     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 402 e0/2     |
|                | e0.102       | IPv4                | 192.168.3.1                 | 255.255.255.0                           | VLAN 102 e0/2     |
|                |              | IPv6                | fde8:8a:fc:1:10:a5:0:1/120  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ff00 | VLAN 102 e0/2     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 102 e0/2     |
| SW29           | VLAN 402     | IPv4                | 42.26.2.194                 | 255.255.255.192                         | VLAN 402          |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:51/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | VLAN 402          |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 402          |
| VPC30          | eth0         | IPv4                | 192.168.3.2                 | 255.255.255.0                           |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a5:0:2/120  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ff00 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |
| VPC31          | eth0         | IPv4                | 192.168.3.3                 | 255.255.255.0                           |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a5:0:3/120  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ff00 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |
|**С.-Петербург**|              |                     |                             |                                         |                   |
| R18            | e0/0         | IPv4                | 192.168.4.6                 | 255.255.255.252                         | R18 - R16         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:55/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R18 - R16         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R18 - R16         |
|                | e0/1         | IPv4                | 192.168.4.2                 | 255.255.255.252                         | R18 - R17         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:53/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R18 - R17         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R18 - R17         |
|                | e0/2         | IPv4                | 46.12.1.33                  | 255.255.255.252                         | R18 - R24         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:36/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R18 - R24         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R18 - R24         |
|                | e0/3         | IPv4                | 192.168.1.37                | 255.255.255.252                         | R18 - R26         |
|                |              | IPv6                | 2de8:8a:fc:1:10:a3:0:38/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R18 - R26         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R18 - R26         |
| R17            | e0/0         | IPv4                | 192.168.4.13                | 255.255.255.252                         | R17 - SW9         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:58/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R17 - SW9         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R17 - SW9         |
|                | e0/1         | IPv4                | 192.168.4.1                 | 255.255.255.252                         | R17 - R18         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:52/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R17 - R18         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R17 - R18         |
|                | e0/2         | IPv4                | 192.168.4.17                | 255.255.255.252                         | R17 - SW10        |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:60/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R17 - SW10        |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R17 - SW10        |
| R16            | e0/0         | IPv4                | 192.168.4.25                | 255.255.255.252                         | R16 - SW10        |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:64/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R16 - SW10        |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R16 - SW10        |
|                | e0/1         | IPv4                | 192.168.4.5                 | 255.255.255.252                         | R16 - R18         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:54/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R16 - R18         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R16 - R18         |
|                | e0/2         | IPv4                | 192.168.4.21                | 255.255.255.252                         | R16 - SW9         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:62/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R16 - SW9         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R16 - SW9         |
|                | e0/3         | IPv4                | 192.168.4.9                 | 255.255.255.252                         | R16 - R32         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:56/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R16 - R32         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R16 - R32         |
| R32            | e0/0         | IPv4                | 192.168.4.10                | 255.255.255.252                         | R32 - R16         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:57/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | R32 - R16         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | R32 - R16         |
| SW9            | e0.103       | IPv4                | 192.168.4.1                 | 255.255.255.128                         | VLAN 103 e0/2     |
|                |              | IPv6                | fde8:8a:fc:1:10:a6:0:1/121  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ff80 | VLAN 103 e0/2     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 103 e0/2     |
|                | e0/3         | IPv4                | 192.168.4.14                | 255.255.255.252                         | SW9 - R17         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:59/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW9 - R17         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW9 - R17         |
|                | e1/0         | IPv4                | 192.168.4.22                | 255.255.255.252                         | SW9 - R16         |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:63/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW9 - R16         |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW9 - R16         |
| SW10           | e0.403       | IPv4                | 46.26.3.193                 | 255.255.255.192                         | VLAN 403 e0/2     |
|                |              | IPv6                | fde8:8a:fc:1:10:a7:0:1/122  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 | VLAN 403 e0/2     |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | VLAN 403 e0/2     |
|                | e0/3         | IPv4                | 192.168.4.26                | 255.255.255.252                         | SW10 - R16        |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:65/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW10 - R16        |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW10 - R16        |
|                | e1/0         | IPv4                | 192.168.4.18                | 255.255.255.252                         | SW10 - R17        |
|                |              | IPv6                | fde8:8a:fc:1:10:a3:0:61/127 | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe | SW10 - R17        |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         | SW10 - R17        |
| VPC8           | eth0         | IPv4                | 192.168.4.2                 | 255.255.255.128                         |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a6:0:2/121  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ff80 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |
| VPC            | eth0         | IPv4                | 46.26.3.194                 | 255.255.255.192                         |                   |
|                |              | IPv6                | fde8:8a:fc:1:10:a7:0:2/122  | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffc0 |                   |1
|                |              | IPv6 Link-<br/>Local| fe80::14                    |                                         |                   |


### Таблица VLAN
|      VLAN      | IPv4 подсеть |    Адрес подсети    |         IPv6 подсеть        |       Имя VLAN       |              Устройство:Интерфейс              |
|----------------|--------------|-------------------- | --------------------------- |----------------------|------------------------------------------------|
| 100            | e0/0         | IPv4                | 192.168.1.6                 | MSC_Client1          | SW4:e0/1.100; SW5:e0/0.100; VPC7:eth0          |
| 101            | 192.168.1.192| 255.255.255.192     | fde8:8a:fc:1:10:a4:0:0/122  | MSC_Client2          | SW4:e0/0.101; SW5:e0/1.101; VPC1:eth0          |
| 102            | 192.168.3.0  | 255.255.255.0       | fde8:8a:fc:1:10:a5:0:1/120  | SPB_Client           | R28:e0/2.102                                   |
| 103            | 192.168.4.0  | 255.255.255.128     | fde8:8a:fc:1:10:a6:0:0/121  | CHO_Client           | SW9:e0/2; VPC8:eth0                            |
| 401            | 42.26.1.192  | 255.255.255.192     | fde8:8a:fc:1:10:a4:0:80/122 | MGMT                 | SW4:e0/1.401; SW5:e0/1.401; SW2:e0/0; SW3:e0/0 |
| 402            | 42.26.2.192  | 255.255.255.192     | fde8:8a:fc:1:10:a3:0:50/127 | MGMT                 | SW29:e0/1; R28:e0/2.402                        |
| 403            | 46.26.3.192  | 255.255.255.192     | fde8:8a:fc:1:10:a7:0:0/122  | MGMT                 | SW10:e0/2; VPC:eth0                            |

На основе топологии сети и таблицы адресации была разработана L3-схема сети.
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4%20_Topology.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology1.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology2.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology3.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology4.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology5.JPG)
![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab4/Lab4_Topology6.JPG)

### 2. Настройка IP-адресов на всех активных портах

В ходе выполнения лабораторной работы было разработано адресное пространство.


### 3. Настройка каждого VPC в каждом офисе в своем VLAN


### 4. Настройка VLAN управления для сетевых устройств








