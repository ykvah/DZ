# DZ

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

Сменить имя и можно вставлять

en
conf t
hostname S1
no ip domain-lookup
enable secret class
line console 0          
password cisco
exec-timeout 99 0
logging synchronous
exit
line vty 0 4
password cisco
exec-timeout 99 0
logging synchronous  
exit
ipv6 unicast-routing !!!! Это только для роутеров
service password-encryption
banner motd #Unauthorized access prohibited! Uhodi#
exit
copy running-config startup-config


Device	Interface	IPv6 Address
R1	G0/0/0	2001:db8:acad:2::1 /64
R1	G0/0/0	fe80::1
R1	G0/0/1	2001:db8:acad:1::1/64
R1	G0/0/1	fe80::1
R2	G0/0/0	2001:db8:acad:2::2/64
R2	G0/0/0	fe80::2
R2	G0/0/1	2001:db8:acad:3::1 /64
R2	G0/0/1	fe80::1
PC-A	NIC	DHCP
PC-B	NIC	DHCP
Objectives

