it
# DZ
<img width="1077" height="813" alt="image" src="https://github.com/user-attachments/assets/04a3ff61-8089-4724-ad01-03bb78fd6323" />
<img width="995" height="777" alt="image" src="https://github.com/user-attachments/assets/3b5d70c4-27a6-4963-b157-20bcac315191" />

начальная настройка
Router>enable
Router#conf t
Router(config)#hostname R1         - назвать
R1(config)#no ip domain-lookup     - отключить поиск dns чтобы не пингавал вместо любой непонятной команды
R1(config)#enable secret class     - пароль на привелигированнный режим
R1(config)#line console 0          
R1(config-line)#password cisco     - пароль на консольное подключение
R1(config-line)#exec-timeout 99 0  - сколько минут не разлогинивать
R1(config-line)#logging synchronous  - логи не будут мешать вводить команды
R1(config-line)#exit
R1(config)#line vty 0 4
R1(config-line)#password cisco     - пароль на линии удаленного подключения
R1(config-line)#exec-timeout 99 0
R1(config-line)#logging synchronous
R1(config-line)#exit
ipv6 unicast-routing              - 
R1(config)#service password-encryption    - засекретить не засекреченные пароли
R1(config)#banner motd #Unauthorized access prohibited! Uhodi#    - банер
R1(config)#exit
R1#
*Apr 17 14:56:25.052: %SYS-5-CONFIG_I: Configured from console by console
R1#copy running-config startup-config    - сохранить конфиг
Destination filename [startup-config]?
Building configuration...
[OK]
R1#clock set 18:00:00 17 apr 2025       - указать время

swit trunk encapsulation dot1q

маршрутизатор
R17(config)#int et 0/0.30
encapsulation dot1q 30
ip address 192.168.30.17 255.255.255.0  - ip yt gjdnjhz.obtcz yf rf;lsq vfhihenbpfnjh cdjq d 'njq ctnb
R17(config-subif)#exit
R17(config)#



SW9#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW9(config)#int Vlan45
SW9(config-if)#
*Jul  2 14:48:13.439: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan45, changed state to down
SW9(config-if)#ip addr
SW9(config-if)#ip address 192.168.45.9 255.255.255.0
SW9(config-if)#no ah
                   ^
% Invalid input detected at '^' marker.

SW9(config-if)#no sh
SW9(config-if)#
*Jul  2 14:49:15.163: %LINK-3-UPDOWN: Interface Vlan45, changed state to up
*Jul  2 14:49:16.167: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan45, changed state to up
SW9(config-if)#exit
SW9(config)#interface loopback 0
SW9(config-if)#
*Jul  2 14:50:22.351: %LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up
SW9(config-if)#ip address 10.0.0.9 255.255.255.0
SW9(config-if)#exit
SW9(config)#ip default-
SW9(config)#ip default-g
SW9(config)#ip default-gateway 192.168.45.1
SW9(config)#exit
SW9#
*Jul  2 14:51:56.681: %SYS-5-CONFIG_I: Configured from console by console
SW9#copy running-config startup-config





Сменить имя и можно вставлять



en
conf t
hostname R17
no ip domain-lookup
line console 0          
exec-timeout 99 0
logging synchronous
exit
line vty 0 4
exec-timeout 99 0
logging synchronous  
exit
ipv6 unicast-routing !!!! Это только для роутеров
service password-encryption
banner motd #Unauthorized access prohibited! Uhodi#
exit
copy running-config startup-config

Проектирование сети

Цель:
В данной самостоятельной работе необходимо распланировать адресное пространство
Настроить IP на всех активных портах для дальнейшей работы над проектом
Адресное пространство должно быть задокументировано


Описание/Пошаговая инструкция выполнения домашнего задания:
В этой самостоятельной работе мы ожидаем, что вы самостоятельно:

Разработаете и задокументируете адресное пространство для лабораторного стенда.
Настроите ip адреса на каждом активном порту
Настроите каждый VPC в каждом офисе в своем VLAN.
Настроите VLAN/Loopbackup interface управления для сетевых устройств
Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано
Используете IPv4. IPv6 по желанию
