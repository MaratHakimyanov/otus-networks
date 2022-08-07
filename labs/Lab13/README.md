# Лабораторная работа №13. VPN. GRE. DMVPN
### Цели:
1. Настроить GRE между офисами "Москва" и "С.-Петербург"
2. Настроить DMVPN между офисами "Москва" и "Чокурдах", "Лабытнанги"

### Описание:
1. Настроить GRE между офисами "Москва" и "С.-Петербург"
2. Настроить DMVPN между "Москва" и "Чокурдах", "Лабытнанги"
3. Все узлы в офисах в лабораторной работе должны иметь IP связность

![alt-текст](https://github.com/MaratHakimyanov/otus-networks/blob/main/labs/Lab7/Lab7_Topology.JPG)

### Решение:
### 1. Настройка GRE между офисами "Москва" и "С.-Петербург"

Настроим туннлеи GRE на маршрутизаторах R14, R15, R18.
#### Маршрутизатор R14:
```
int tunnel 1
  ip address 172.16.13.1 255.255.255.0
  tunnel mode gre ip
  tunnel source e0/2
  tunnel destination 46.12.1.37
```

#### Маршрутизатор R15:
```
int tunnel 0
  ip address 172.16.12.2 255.255.255.0
  tunnel mode gre ip
  tunnel source e0/2
  tunnel destination 46.12.1.33
```

#### Маршрутизатор R18:
```
int tunnel 0
  ip address 172.16.12.4 255.255.255.0
  tunnel mode gre ip
  tunnel source e0/2
  tunnel destination 46.12.1.5
int tunnel 1
  ip address 172.16.13.2 255.255.255.0
  tunnel mode gre ip
  tunnel source e0/3
  tunnel destination 46.12.1.1
```

Проверим правильность настройки с помощью команд **traceroute** и **sh interfaces tunnel 0**

#### Маршрутизатор R15:
```
R15#ping 172.16.12.4
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.12.4, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/5 ms
R15#traceroute 172.16.12.4
Type escape sequence to abort.
Tracing the route to 172.16.12.4
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.12.4 1 msec *  5 msec
R15#sh interfaces tunnel 0
Tunnel0 is up, line protocol is up 
  Hardware is Tunnel
  Internet address is 172.16.12.2/24
  MTU 17916 bytes, BW 100 Kbit/sec, DLY 50000 usec, 
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation TUNNEL, loopback not set
  Keepalive not set
  Tunnel linestate evaluation up
  Tunnel source 46.12.1.5 (Ethernet0/2), destination 46.12.1.33
   Tunnel Subblocks:
      src-track:
         Tunnel0 source tracking subblock associated with Ethernet0/2
          Set of tunnels with source Ethernet0/2, 1 member (includes iterators), on interface <OK>
  Tunnel protocol/transport GRE/IP
    Key disabled, sequencing disabled
    Checksumming of packets disabled
  Tunnel TTL 255, Fast tunneling enabled
  Tunnel transport MTU 1476 bytes
  Tunnel transmit bandwidth 8000 (kbps)
  Tunnel receive bandwidth 8000 (kbps)
  Last input 00:00:13, output 00:00:13, output hang never
  Last clearing of "show interface" counters 01:53:30
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/0 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     15 packets input, 1556 bytes, 0 no buffer
     Received 0 broadcasts (0 IP multicasts)
     0 runts, 0 giants, 0 throttles 
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     25 packets output, 2796 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 unknown protocol drops
     0 output buffer failures, 0 output buffers swapped out
```

### 2. Настройка DMVPN между "Москва" и "Чокурдах", "Лабытнанги"



