# Просмотр таблицы MAC-адресов коммутатора
## Толопогия
<img width="501" height="295" alt="image" src="https://github.com/user-attachments/assets/b34281ad-42a9-4218-a2fb-bd7891f47366" />

## Таблица адресации  
| Устройство  | Интерфейс  | IP-адрес   | Маска подсети  |
| :------------ | :------------ | :------------ | :------------ |
| S1  | VLAN1   | 192.168.1.11  | 255.255.255.0  |
| S2  | VLAN1  | 192.168.1.12  | 255.255.255.0  |
| PC-A  | NIC  | 192.168.1.1  | 255.255.255.0  |
| PC-B  | NIC  | 192.168.1.2  | 255.255.255.0  |

## Цели    
1. Создание и настройка сети  
2. Изучение таблицы МАС-адресов коммутатора  
## Ход работы  
### Создание и настройка сети  
Создаем сеть, согласно топологии: добавляем в CiscoPT два коммутатора 2960 S1 и  S2, и два ПК PC-A и PC-B.  
Задаем ip-адреса нашим ПК. Подключаемся к коммутаторам с помощью консольного порта для базовой настройки с помощью терминала:  
Switch>enable  
Switch#configure terminal  
Enter configuration commands, one per line.  End with CNTL/Z.  
Switch(config)#no ip domain-lookup  
Switch(config)#hostname S1  
S1(config)#enable secret class  
S1(config)#line console 0  
S1(config-line)#password cisco  
S1(config-line)#login  
S1(config-line)#logging synchronous   
S1(config-line)#exec-timeout 5 0  
S1(config-line)#exit  
S1(config)#line vty 0 15  
S1(config-line)#password cisco  
S1(config-line)#login  
S1(config-line)#exec-timeout 5 0  
S1(config-line)#exit  
S1(config)#interface vlan1   
S1(config-if)#ip address 192.168.1.11 255.255.255.0  
S1(config-if)#no shutdown  
S1(config-if)#  
%LINK-5-CHANGED: Interface Vlan1, changed state to up  
S1(config-if)#exit  
S1(config)#service password-encryption  
S1#  
%SYS-5-CONFIG_I: Configured from console by console  
#### Подключаемся витой парой к f 0/6 порту S1  
%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to up  
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to up  
%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up  
Переходим в командную строку ПК для проверки доступности коммутатора:  
C:\>ping 192.168.1.11  
Pinging 192.168.1.11 with 32 bytes of data:  
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255  
Reply from 192.168.1.11: bytes=32 time=2ms TTL=255  
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255  
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255  
Ping statistics for 192.168.1.11:  
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),  
Approximate round trip times in milli-seconds:  
    Minimum = 0ms, Maximum = 2ms, Average = 0ms  
#### Проверяем доступность SVI через Telnet:
Trying 192.168.1.11 ...Open  
User Access Verification  
Password:  
S1>enable  
Password: 
S1#show run   
Building configuration...  
Current configuration : 1289 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
service password-encryption  
!  
hostname S1  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
no ip domain-lookup  
!    
spanning-tree mode pvst   
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
!  
interface FastEthernet0/2  
!  
interface FastEthernet0/3  
!  
interface FastEthernet0/4  
!  
interface FastEthernet0/5  
!  
interface FastEthernet0/6  
!  
interface FastEthernet0/7  
!  
interface FastEthernet0/8  
!  
interface FastEthernet0/9  
!  
interface FastEthernet0/10   
!  
interface FastEthernet0/11  
!  
interface FastEthernet0/12  
!  
interface FastEthernet0/13  
!  
interface FastEthernet0/14  
!  
interface FastEthernet0/15  
!  
interface FastEthernet0/16  
!  
interface FastEthernet0/17  
!  
interface FastEthernet0/18  
!  
interface FastEthernet0/19  
!  
interface FastEthernet0/20  
!  
interface FastEthernet0/21  
!  
interface FastEthernet0/22  
!  
interface FastEthernet0/23  
!   
interface FastEthernet0/24  
!  
interface GigabitEthernet0/1  
!  
interface GigabitEthernet0/2  
!  
interface Vlan1  
 ip address 192.168.1.11 255.255.255.0  
!  
line con 0  
 password 7 0822455D0A16  
 logging synchronous  
 login  
 exec-timeout 5 0   
!  
line vty 0 4  
 exec-timeout 5 0  
 password 7 0822455D0A16  
 login
line vty 5 15  
 exec-timeout 5 0  
 password 7 0822455D0A16  
 login   
!  
!  
end  
S1#  

#### Аналогичную операцию проделываем с коммутатором S-2 с помощью ПК PC-B.  
Для начальной настройки через console, а потом и через f0/18 порт с помощью витой пары. Задаем S2 адрес 192.168.1.12.  

После этого я соединяю S1 и S2 витой парой через их порты f 0/1.  

### Изучение таблицы МАС-адресов коммутатора.  
Проверяем mac-адреса  PC-A и PC-B с помощью командной строки и ipconfig /all.  
#### PC-A:  
FastEthernet0 Connection:(default port)  
 Connection-specific DNS Suffix..:  
  #### Physical Address................: 00D0.97E0.D4AC  
   Link-local IPv6 Address.........: FE80::2D0:97FF:FEE0:D4AC  
   IPv6 Address....................: ::  
   IPv4 Address....................: 192.168.1.1  
   Subnet Mask.....................: 255.255.255.0  
   Default Gateway.................: ::  
                                     0.0.0.0  
   DHCP Servers....................: 0.0.0.0  
   DHCPv6 IAID.....................:    
   DHCPv6 Client DUID..............: 00-01-00-01-8C-DE-79-C5-00-D0-97-E0-D4-AC  
   DNS Servers.....................: ::  
                                     0.0.0.0  
 #### PC-B:  
  FastEthernet0 Connection:(default port)  
   Connection-specific DNS Suffix..:   
  #### Physical Address................: 000A.41A4.34C1  
   Link-local IPv6 Address.........: FE80::20A:41FF:FEA4:34C1  
   IPv6 Address....................: ::  
   IPv4 Address....................: 192.168.1.2  
   Subnet Mask.....................: 255.255.255.0  
   Default Gateway.................: ::  
                                     0.0.0.0  
   DHCP Servers....................: 0.0.0.0  
   DHCPv6 IAID.....................:   
   DHCPv6 Client DUID..............: 00-01-00-01-A2-87-A3-CA-00-0A-41-A4-34-C1  
   DNS Servers.....................: ::  
                                     0.0.0.0  

  #### Подключаемся через console к S1 и S2 для проверки интерфейсов f0/1.  
  ##### S1:  
S2#show interface f0/1  
FastEthernet0/1 is up, line protocol is up (connected)  
 ####  Hardware is Lance, address is 0004.9a28.8701 (bia 0004.9a28.8701) - MAC-адрес коммутатора  
 BW 100000 Kbit, DLY 1000 usec,  
     reliability 255/255, txload 1/255, rxload 1/255  
  Encapsulation ARPA, loopback not set  
  Keepalive set (10 sec)   
  Full-duplex, 100Mb/s  
  input flow-control is off, output flow-control is off  
  ARP type: ARPA, ARP Timeout 04:00:00  
  Last input 00:00:08, output 00:00:05, output hang never  
  Last clearing of "show interface" counters never  
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0  
  Queueing strategy: fifo  
  Output queue :0/40 (size/max)  
  5 minute input rate 0 bits/sec, 0 packets/sec  
  5 minute output rate 0 bits/sec, 0 packets/sec  
     956 packets input, 193351 bytes, 0 no buffer  
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles  
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort  
     0 watchdog, 0 multicast, 0 pause input  
     0 input packets with dribble condition detected  
     2357 packets output, 263570 bytes, 0 underruns  
     0 output errors, 0 collisions, 10 interface resets  
     0 babbles, 0 late collision, 0 deferred  
     0 lost carrier, 0 no carrier  
     0 output buffer failures, 0 output buffers swapped out  
     
  ##### S2:  
  S1#show interface f 0/1  
FastEthernet0/1 is up, line protocol is up (connected)  
####  Hardware is Lance, address is 00e0.b031.d101 (bia 00e0.b031.d101) - MAC-адрес коммутатора  
 BW 100000 Kbit, DLY 1000 usec,  
     reliability 255/255, txload 1/255, rxload 1/255  
  Encapsulation ARPA, loopback not set  
  Keepalive set (10 sec)  
  Full-duplex, 100Mb/s  
  input flow-control is off, output flow-control is off  
  ARP type: ARPA, ARP Timeout 04:00:00  
  Last input 00:00:08, output 00:00:05, output hang never  
  Last clearing of "show interface" counters never  
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0  
  Queueing strategy: fifo  
  Output queue :0/40 (size/max)  
  5 minute input rate 0 bits/sec, 0 packets/sec  
  5 minute output rate 0 bits/sec, 0 packets/sec  
     956 packets input, 193351 bytes, 0 no buffer  
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles  
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort  
     0 watchdog, 0 multicast, 0 pause input  
     0 input packets with dribble condition detected  
     2357 packets output, 263570 bytes, 0 underruns  
     0 output errors, 0 collisions, 10 interface resets  
     0 babbles, 0 late collision, 0 deferred  
     0 lost carrier, 0 no carrier  
     0 output buffer failures, 0 output buffers swapped out  
#### Проверяем таблицу адресов S1 и S2:  
Запускаем терминал на PC и проверяем, что в таблице MAC-адресов: 


S1#show mac-address-table  
Mac Address Table  
| Vlan  |Mac Address   | Type    |  Ports  |
| :------------ | :------------ | :------------ | :------------ |
| 1  | 00e0.b031.d101  |DYNAMIC    | Fa0/1   | 

S2# show mac address-table  
Mac Address Table  
| Vlan  |Mac Address   | Type    |  Ports  |
| :------------ | :------------ | :------------ | :------------ |
| 1  |0004.9a28.8701  |DYNAMIC    | Fa0/1   |

Из выведенных мне данных понятно, что на обоих коммутаторах для VLAN1 на интерфейсах f 0/1 сопоставлены MAС-адреса коммутаторов, которые стоят за ними.  
Вводим в обоих терминалах команду clear mac address-table dynamic для очистки таблицы.  
Сразу после вводим вновь show mac-address-table  
Таблицы выводятся пустые на обоих коммутаторах.  
После 10 секунд ожидания и повторения команды выводится таблица, идектичная первой проверке: коммутаторы видят друг друга за интерфейсаи f 0/1.  
Открываю командную строку PC-B:   
C:\>arp -a  
|Internet Address   | Physical Address  | Type  |
| :------------ | :------------ | :------------ |
|192.168.1.1    | 00d0.97e0.d4ac  | dynamic  |
|192.168.1.11   | 0001.9732.61a3   | dynamic  |
| 192.168.1.12  | 0001.c921.3804    | dynamic  |

  Поскольку я проверял доступность устройств в процессе её построения, то в таблице отобразились ip адреса с сопоставлеными им mac-адресами устройств.  
C помощью команды ping отправляю запросы S1, S1, PC-A.
Подключаюсь через терминал к 
Во всех случаях пришёл ответ о удачной передаче пакетов:  
 #### Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)  
#### Открываю терминал на PC-B для подключения к S2:  

S2#show mac address-table  
          Mac Address Table  
| VLAN  | Mac Address  | Type  | Ports  |
| :------------ | :------------ | :------------ | :------------ |
| 1  | 0001.9732.61a3 | DYNAMIC  | Fa0/1  |
| 1  | 0004.9a28.8701  | DYNAMIC  | Fa0/1  |
| 1   | 000a.41a4.34c1  | DYNAMIC  | Fa0/18  |
| 1  | 00d0.97e0.d4ac  | DYNAMIC  | Fa0/1  |

Таблица mac-адресов наполнилась записями, т.к. мы посылали эхо-запросы с хоста PC-B.
Всё, кроме  PC-B, которому соответствует адрес на интерфейсе f 0/18, находится за f 0/1.

