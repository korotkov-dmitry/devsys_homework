# Домашнее задание к занятию "3.7. Компьютерные сети, лекция 2"

## 1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?
Linux:

        `vagrant@vagrant:~$ ifconfig
        eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
                inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
                inet6 fe80::a00:27ff:fe73:60cf  prefixlen 64  scopeid 0x20<link>
                ether 08:00:27:73:60:cf  txqueuelen 1000  (Ethernet)
                RX packets 95585  bytes 16772279 (16.7 MB)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 110084  bytes 9622242 (9.6 MB)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
                
        lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
                inet 127.0.0.1  netmask 255.0.0.0
                inet6 ::1  prefixlen 128  scopeid 0x10<host>
                loop  txqueuelen 1000  (Local Loopback)
                RX packets 987  bytes 91273 (91.2 KB)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 987  bytes 91273 (91.2 KB)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0`

Windows:

        `PS C:\Users\fulla> ipconfig
        Настройка протокола IP для Windows

        Адаптер Ethernet VirtualBox Host-Only Network:
            DNS-суффикс подключения . . . . . :
            Локальный IPv6-адрес канала . . . : fe80::934:4ef6:ee0e:295f%5
            IPv4-адрес. . . . . . . . . . . . : 192.168.56.1
            Маска подсети . . . . . . . . . . : 255.255.255.0
            Основной шлюз. . . . . . . . . :

        Адаптер беспроводной локальной сети Подключение по локальной сети* 1:
            Состояние среды. . . . . . . . : Среда передачи недоступна.
            DNS-суффикс подключения . . . . . :

        Адаптер беспроводной локальной сети Подключение по локальной сети* 10:
            Состояние среды. . . . . . . . : Среда передачи недоступна.
            DNS-суффикс подключения . . . . . :

        Адаптер беспроводной локальной сети Беспроводная сеть:
            DNS-суффикс подключения . . . . . :
            Локальный IPv6-адрес канала . . . : fe80::c878:fd68:417d:1b60%7
            IPv4-адрес. . . . . . . . . . . . : 192.168.1.105
            Маска подсети . . . . . . . . . . : 255.255.255.0
            Основной шлюз. . . . . . . . . : 192.168.1.1

        Адаптер Ethernet Сетевое подключение Bluetooth:
            Состояние среды. . . . . . . . : Среда передачи недоступна.
            DNS-суффикс подключения . . . . . :`
## 2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?
LLDP – протокол для обмена информацией между соседними устройствами.

Пакет `lldpd`

        `vagrant@vagrant:~$ lldpctl
        -------------------------------------------------------------------------------
        LLDP neighbors:
        -------------------------------------------------------------------------------`
## 3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.
VLAN (Virtual Local Area Network)

Пакет `vlan`. Команды `vconfig ` и `ip`.

        `vagrant@vagrant:~$ vim /etc/network/interfaces
            # interfaces(5) file used by ifup(8) and ifdown(8)
            # Include files from /etc/network/interfaces.d:
            source-directory /etc/network/interfaces.d
            ##vlan с ID-100 для интерфейса eth0 with ID - 100
            auto eth0.100
            iface eth0.100 inet static
            address 192.168.1.200
            netmask 255.255.255.0
            vlan-raw-device eth0`

        `vagrant@vagrant:~$ ifconfig
            eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
                    inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
                    inet6 fe80::a00:27ff:fe73:60cf  prefixlen 64  scopeid 0x20<link>
                    ether 08:00:27:73:60:cf  txqueuelen 1000  (Ethernet)
                    RX packets 598  bytes 60113 (60.1 KB)
                    RX errors 0  dropped 0  overruns 0  frame 0
                    TX packets 412  bytes 63177 (63.1 KB)
                    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

            eth0.100: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
                    inet 192.168.1.200  netmask 255.255.255.0  broadcast 192.168.1.255
                    inet6 fe80::a00:27ff:fe73:60cf  prefixlen 64  scopeid 0x20<link>
                    ether 08:00:27:73:60:cf  txqueuelen 1000  (Ethernet)
                    RX packets 0  bytes 0 (0.0 B)
                    RX errors 0  dropped 0  overruns 0  frame 0
                    TX packets 12  bytes 936 (936.0 B)
                    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

            lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            ...`
## 4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.
Типы агрегации:
* Mode-0(balance-rr);
* Mode-1(active-backup);
* Mode-2(balance-xor);
* Mode-3(broadcast);
* Mode-4(802.3ad);
* Mode-5(balance-tlb);
* Mode-6(balance-alb).

Опции:
* downdelay - время ожидания в миллисекундах перед отключением устройства после обнаружения сбоя связи;
* miimon - частота мониторинга канала в миллисекундах;
* updelay - время ожидания в миллисекундах перед включением устройства после восстановления связи.
        
        `vagrant@vagrant:~$ sudo vim /etc/network/interfaces
        # interfaces(5) file used by ifup(8) and ifdown(8)
        # Include files from /etc/network/interfaces.d:
        source-directory /etc/network/interfaces.d
        ##vlan с ID-100 для интерфейса eth0 with ID - 100
        auto eth0.100
        iface eth0.100 inet static
        address 192.168.1.200
        netmask 255.255.255.0
        vlan-raw-device eth0

        auto eth0.200
        iface eth0.200 inet static
        address 192.168.1.250
        netmask 255.255.255.0
        vlan-raw-device eth0

        # The bond0 network interface
        auto bond0
        allow-hotplug bond0
        iface bond0 inet static
        address 192.168.1.150
        netmask 255.255.255.0
        gateway 192.168.1.1
        dns-nameservers  192.168.1.1 8.8.8.8
        dns-search domain.local
                slaves eth0.100 eth0.200
                bond_mode 4`
                
        `vagrant@vagrant:~$ ifenslave -a
        1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
            link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
            link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
        4: eth0.100@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
            link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
        5: eth0.200@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
            link/ether 08:00:27:73:60:cf brd ff:ff:ff:ff:ff:ff
        6: bond0: <NO-CARRIER,BROADCAST,MULTICAST,MASTER,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
            link/ether 12:75:c6:af:30:30 brd ff:ff:ff:ff:ff:ff`
## 5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.
1. 8 всего, 2 зарезервировано, 6 доступно
        
        `vagrant@vagrant:~$ ipcalc -b 10.10.10.0/29
        Address:   10.10.10.0
        Netmask:   255.255.255.248 = 29
        Wildcard:  0.0.0.7
        =>
        Network:   10.10.10.0/29
        HostMin:   10.10.10.1
        HostMax:   10.10.10.6
        Broadcast: 10.10.10.7
        Hosts/Net: 6                     Class A, Private Internet`
        
2. 32

Расчет:

/29 - префикс сети
        В формате двоичных чисел 11111111 11111111 11111111 11111000 
        В формате десятичных чисел 255.255.255.248

В четвертом поле (последний октет) 11111000 первые 5 бит определяют число подсетей - 2 в степени 5 = 32.
        В четвертом поле (последний октет) 11111000 последие 3 бита определяют число хостов подсети, в нашем примере 2 в степени 3 = 8.

Диапазон IP первой подсети 0~7 (8 хостов), 0 - это подсеть, 8 - это Broadcast. Таким образом, максимальное число хостов данной подсети - 6.

Первая подсеть: 10.10.10.0
        Broadcast первой подсети: 10.10.10.7

Вторая подсеть: 10.10.10.8
        Broadcast второй подсети: 10.10.10.15

        `vagrant@vagrant:~$ ipcalc -b 10.10.10.0/24
        Address:   10.10.10.0
        Netmask:   255.255.255.0 = 24
        Wildcard:  0.0.0.255
        =>
        Network:   10.10.10.0/24
        HostMin:   10.10.10.1
        HostMax:   10.10.10.254
        Broadcast: 10.10.10.255
        Hosts/Net: 254                   Class A, Private Internet`
        
3.      10.10.10.1 255.255.255.248
        10.10.10.2 255.255.255.248
## 6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.
Используем диапазон `100.64.0.0 — 100.127.255.255`

Вариант 1

        `vagrant@vagrant:~$ ipcalc -b --split 40 50 100.64.0.0/26
        Address:   100.64.0.0
        Netmask:   255.255.255.192 = 26
        Wildcard:  0.0.0.63
        =>
        Network:   100.64.0.0/26
        HostMin:   100.64.0.1
        HostMax:   100.64.0.62
        Broadcast: 100.64.0.63
        Hosts/Net: 62                    Class A

        1. Requested size: 40 hosts
        Netmask:   255.255.255.192 = 26
        Network:   100.64.0.0/26
        HostMin:   100.64.0.1
        HostMax:   100.64.0.62
        Broadcast: 100.64.0.63
        Hosts/Net: 62                    Class A

        2. Requested size: 50 hosts
        Netmask:   255.255.255.192 = 26
        Network:   100.64.0.64/26
        HostMin:   100.64.0.65
        HostMax:   100.64.0.126
        Broadcast: 100.64.0.127
        Hosts/Net: 62                    Class A

        Network is too small
        Needed size:  128 addresses.
        Used network: 100.64.0.0/25
        Unused:`
        
Вариант 2

        `vagrant@vagrant:~$ ipcalc -b --split 40 50 100.64.0.0/25
        Address:   100.64.0.0
        Netmask:   255.255.255.128 = 25
        Wildcard:  0.0.0.127
        =>
        Network:   100.64.0.0/25
        HostMin:   100.64.0.1
        HostMax:   100.64.0.126
        Broadcast: 100.64.0.127
        Hosts/Net: 126                   Class A

        1. Requested size: 40 hosts
        Netmask:   255.255.255.192 = 26
        Network:   100.64.0.0/26
        HostMin:   100.64.0.1
        HostMax:   100.64.0.62
        Broadcast: 100.64.0.63
        Hosts/Net: 62                    Class A

        2. Requested size: 50 hosts
        Netmask:   255.255.255.192 = 26
        Network:   100.64.0.64/26
        HostMin:   100.64.0.65
        HostMax:   100.64.0.126
        Broadcast: 100.64.0.127
        Hosts/Net: 62                    Class A

        Needed size:  128 addresses.
        Used network: 100.64.0.0/25
        Unused:`
## 7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?
Windows:
        
        `PS C:\Users\fulla> arp -a

        Интерфейс: 192.168.56.1 --- 0x5
          адрес в Интернете      Физический адрес      Тип
          192.168.56.255        ff-ff-ff-ff-ff-ff     статический
          ...
        Интерфейс: 192.168.1.105 --- 0x7
          адрес в Интернете      Физический адрес      Тип
          192.168.1.1           84-16-f9-db-b9-95     динамический
          192.168.1.102         f4-6d-04-e9-96-d5     динамический
        ...`

Для удаления используется опция `-d` - `arp -d ip`.

Для удаления всех записей используется опция `-d *`.

Для очистки кеша `netsh interface ip delete arpcache`
        
Linux:

        `vagrant@vagrant:~$ arp -v
        Address                  HWtype  HWaddress           Flags Mask            Iface
        _gateway                 ether   52:54:00:12:35:02   C                     eth0
        10.0.2.3                 ether   52:54:00:12:35:03   C                     eth0
        Entries: 2      Skipped: 0      Found: 2`

или

        `vagrant@vagrant:~$ ip neighbour
        10.0.2.2 dev eth0 lladdr 52:54:00:12:35:02 REACHABLE
        10.0.2.3 dev eth0 lladdr 52:54:00:12:35:03 STALE`
        
Для удаления используется опция `-d` - `arp -d 10.0.2.3`.

Очистка кеша: 
        `ip link set arp off dev eth0`

Заполнение таблицы ARP:
        `ip link set arp on dev eth0`
