
# Расширение сети небольшого оператора связи

### Содержание
[Задачи](#задачи)

[Схема](#Схема)

[Протоколы](#Протоколы)

[Адресация](#Адресация)

[Схема](#Схема)

<a name="задачи"></a>
### Цели и задачи
Мы расширяем сеть небольшого оператора и продлеваем ее в новый район выстраивая новую инфраструктуру в будущем соединив ее с имеющейся.
Необходимо проработать подключение к BRAS и спроектировать разветвленную  сеть оборудования которое будет резервироваться с минимальными затратами и предоставлять обширный доступ для подключения "последний мили" в сторону клиентов


### Схема
<a name="Схема"></a>

<details>
<img width="970" height="650" alt="1" src="https://github.com/user-attachments/assets/a1aae947-aa29-45c6-a292-bec08febcb2d" />
</details>

#### Пояснения

Брас или новый или уже имеющийся

("победить" EVE-ng не удалось и вместо образа браса у меня выступает аналог Циски 7200 )

core_1 и core_2 выполняют функции ядра. Могут соединятся с другими маршрутизаторами ядра.

Agr1,Agr2,Agr3 - маршрутизаторы агрегации

ACC - коммутаторы доступа. В их порты можно подключать клиента на прямую или достраивать сеть в сторону клиента например радиомостом или связкой shdsl модемов если уже проложена линия.

core_1 и core_2 имеют резервный линк между собой на случай падения связи брасом 

Маршрутизаторы агрегации cоединены каждый с каждым а также Agr1 и Agr3 имеют резервные линки к ядрам.

Коммутаторы доступа соединены в кольца с Agr



### Что настраиваем
<a name="Протоколы"></a>
На L2 кольцах настраиваем STP или аналоги делая линки между ACC не приоритетными

Все маршрутизаторы будут в Area0 OSPF и подключаются к RR по ibgp. +LDP

Все данные от клиентов "кладем" в L2PVN (xconnect) и передаем их по MPLS до выходного сабинтерфейса на брасе


### Адресация
<a name="Адресация"></a>







































































<img width="328" height="832" alt="image" src="https://github.com/user-attachments/assets/d40fc2f1-ff39-4920-8ebc-d2e9f1b994ce" />

<img width="501" height="854" alt="image" src="https://github.com/user-attachments/assets/c33ff7b7-7c3a-4a33-92d9-fa4aadfc2c3c" />

<img width="315" height="253" alt="image" src="https://github.com/user-attachments/assets/2a2d9a7b-ee39-4b3f-beb9-001efd753cac" />



Сеть управления сейчас завернута в сторону браса но это ограничение обрезанной схемы. В идеале если эта часть внедрена в сеть оператора то все управление будет уходить на общий сервер доступов и мониторинга.







Пример конфига ACC_1_1 c PSTP 
<img width="1570" height="657" alt="image" src="https://github.com/user-attachments/assets/a90d6776-8f42-4ed4-92ef-376e3f084965" />

interface e0/1

ip address 10.0.2.21 255.255.255.252

mpls ip

interface e1/1.111

description Client_VLAN111

encapsulation dot1q 111

no ip address

xconnect 10.42.0.30 111 encapsulation mpls


it
# DZ

<img width="607" height="806" alt="image" src="https://github.com/user-attachments/assets/05daef55-2a4e-481c-b1fa-1aeb237c78d4" />


<img width="1108" height="635" alt="image" src="https://github.com/user-attachments/assets/bdd149bc-cd5b-4db4-b017-6dbf9cb451ea" />


Пример конфигурации коммутаторов 
настройка стп на кольце позже еще увеличил веса линков между ACC_1_1 и ACC_2_2 // ACC_3_1 и ACC_2_1

например на ACC_1_1 (порт к ACC_2_2)

interface e0/1

spanning-tree vlan 1-4094 cost 200


<img width="937" height="1028" alt="image" src="https://github.com/user-attachments/assets/1d408725-9d5f-467e-aefe-de3075b67d05" />

уже исправленный ACC_3_1

<img width="668" height="984" alt="image" src="https://github.com/user-attachments/assets/9362267d-b436-402a-9bec-9185b989ee58" />





<img width="989" height="730" alt="image" src="https://github.com/user-attachments/assets/1608de35-7ef7-4b3c-9c5a-2fdf1686dd1e" />


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
