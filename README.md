# Дипломный проект по итогам обучения на курсе "Сетевой инженер"

* [Цель проекта](#цель-курсового-проекта)
* [Чек-лист готовности к выполнению проекта](#чек-лист-готовности-к-выполнению-проекта)
* [Задание](#задание)
* [Этапы выполнения проекта](#этапы-выполнения-проекта) 
* [Тестирование](#тестирование) 
* [Правила приема проекта](#правила-приема-проекта)
* [Критерии оценки проекта](#критерии-оценки-проекта)


### Цель курсового проекта
 

В результате выполнения этого задания вы научитесь настраивать хорошо масштабируемую отказоустойчивую и защищённую сеть, выделяя для каждого пользовательского сервиса уникальные права и пропускную полосу. Также вы научитесь объединять разрозненные филиальные сети компании в одну единую. Для удобства администрирования вы научитесь интегрировать инструменты централизованной аутентификации и логирования в настроенную вами сеть. 


1. Настроить отказоустойчивую связку RSTP+HSRP на уровне доступ-ядро.
2. Подготовить безопасное централизованное назначение сетевых настроек для оконечных устройств.
3. Организовать централизованный беспроводной гостевой доступ к сети интернет.
4. Организовать отказустойчивую маршрутизацию в сети компании.
5. Организовать отказоустойчивый доступ к сети интернет по протоколу BGP.
6. Развернуть сервис VoIP внутри компании.
7. Настроить сервисы централизованной аутентификации, синхронизации времени и логирования сетевых устройств.
8. Настроить сеть компании полностью "под ключ".


### Чек-лист готовности к выполнению проекта

Для выполнения работ потребуется:

1. Понимание протоколов маршрутизации eBGP, OSPF.
2. Понимание предсказуемного поведения протоколов RSTP, HSRP, LAG.
3. Знание протоколов безопасного подключения к уровню доступа (vlan, trunk, portfast, bpduguard, dhcp-snooping)
4. Умение настраивать туннельный протокол GRE.
5. Умение настраивать Qos для голосового траффика внутри GRE.
6. Знание aaa, tacacs+, NTP, syslog.
7. Умение настраивать устройства беспроводной сети: AP, WLC.
8. Умение настраивать внутреннюю voip-телефонию.
9. Знание особенностей настройки межсетевого экранирования на Cisco ASA.
10. Умение настраивать static NAT, PAT overload. 

------

### Инструменты/ дополнительные материалы, которые пригодятся для выполнения задания

1. Packet Tracer 8.1.1
2. [Шаблон топологии](https://github.com/netology-code/ntw-diplom/blob/main/ntw_topology.pkt)
3. [Таблица распределения подсетей и адресов](https://github.com/netology-code/ntw-diplom/blob/main/ip-address-table.xlsx)
------

## Задание

Нужно настроить отказоустойчивую, безопасную, масштабируемую сеть и запустить на ней пользовательские сервисы. 

### Этапы выполнения проекта

1) Соберите топологию сети в Cisco Packet tracer. 
      Модели сетевого оборудования: 
      * border-router - 1941, 
      * fw - ASA 5506-x, 
      * ядро - 3560-24, 
      * доступ - 2960-24, 
      * wlan - WLC-2504, 
      * 3702i AP, 
      * internet - switch 3650, 
      * VOIP-server - router 2811, 
      * ip-phone 7960. 

      Используйте за основу [шаблон топологии](https://github.com/netology-code/ntw-diplom/blob/main/ntw_topology.pkt), в котором уже есть оборудование, настройки которого изменять нельзя.

      ![image](https://user-images.githubusercontent.com/5977962/192236499-941224d0-4694-4c74-8cb1-95a068e99fda.png)

2) Заполните [таблицу распределения подсетей и адресов](https://github.com/netology-code/ntw-diplom/blob/main/ip-address-table.xlsx) (по примеру коммутатора "internet"). 

      Нужно учёсть возможность добавления пяти новых коммутаторов доступа в ЦО, и открытия трёх новых филиалов:

      ![image](https://user-images.githubusercontent.com/5977962/193801499-a7d236bf-4e61-482e-961c-fd6190ec1d6c.png)

3) Настройте на коммутаторах доступа порты для подключения пользовательских устройств и аплинки. Также на пользовательских портах коммутатора следует настроить: 
     * port-security на 2 адреса, 
     * dhcp-snooping, 
     * portfast, 
     * RSTP.
     
  ![image](https://user-images.githubusercontent.com/5977962/192258720-b8f713d3-e42a-4af4-87ed-36efc8629c71.png)

4) Настройте на коммутаторах ядра: 
     * vlan, 
     * LAG, 
     * RSTP, 
     * SVI,
     * dhcp 
     * HSRP для всех vlan.  
  
   На этом шаге проверьте работу stp, lag. 
       
  ![image](https://user-images.githubusercontent.com/5977962/193780747-d0c64050-94db-499c-8d9e-9f3b7a06f5de.png)

  ![192258850-47edbb83-3118-4f1b-b735-dd280b103df6](https://user-images.githubusercontent.com/85602495/194490766-5d5ef8e8-7fe6-4ed0-a8a5-643a8de66ce5.png)

5) Настройте сервисы для распределения сетевых настроек для пользовательских устройств. Так как сервер находится в отдельной сети, на SVI настраивается helper. По окончанию настроек dhcp-сервер должен раздать настройки PC, телефонам и принтерам, с любого хоста ЦО должен быть доступен любой другой хост.  
  ![image](https://user-images.githubusercontent.com/5977962/192258899-0278a6e5-815b-458d-bed0-bbab3d21603f.png)

6) Настройте сервис БЛВС. Точки доступа должны подключаться к контроллеру, который сообщит им настройки по capwap. Подключите ноутбук к ТД, проверьте связность сети.
![image](https://user-images.githubusercontent.com/5977962/192258971-afec76f9-8e71-45e5-af8a-7a6debcf767c.png)

7) На коммутаторах ядра запустите протокол маршрутизации ospf. Он должен анонсировать все внутренние сети в зоне 1.
![image](https://user-images.githubusercontent.com/5977962/192259147-f4fbe92b-440f-41ba-a54e-036e14eaf0d8.png)

8) На каждом межсетевом экране настройте адресацию и три зоны: inside, outside, DMZ.  
   
   Правила фильтрации:
   * из inside доступ свободный во все зоны
   * из DMZ в inside доуступ разрешен для траффика от приватных адресов. Разрешен траффик со всех адресов в зону DMZ
   * из DMZ разрешен доступ только в outside на публичные адреса

![image](https://user-images.githubusercontent.com/5977962/192261102-8b1924bf-0a03-44a7-96b8-8fb0e4e3e2e1.png)

9) На Cisco ASA настройте протокол ospf. МСЭ должен принимать и анонсировать все сети в зоне 1.  
   Настройте web-сервер, подключенный в DMZ зону одной из ASA.  

   На этом шаге проверьте:  
   * все ли сети получены
   * все ли сети анонсируются на коммутаторы ядра
   * есть ли доступ к web-серверу и с него
   
10) Настройте пограничные маршрутизаторы. Настройте адресацию и проверьте сетевую связность внутри ЛВС и доступность шлюза провайдера. 

11) Настройте маршрутизацию ospf: 
 * интерфейсы в сторону ASA в зоне 1 
 * между собой в зоне 0   
 
   Настройте анонс маршрута 0.0.0.0/0 во внутреннюю сеть с разными метриками для резервирования подключения. Другие маршруты с бордеров во внутреннюю сеть не должны анонсироваться. Проверьте получение и анонс маршрутов.  
  ![image](https://user-images.githubusercontent.com/5977962/193406159-cd3286ac-3eaa-4ea1-90b2-1530c8de7ce5.png)

12) Настройте ebgp-сессии с оборудованием провайдера. Проверьте получение и анонс маршрутов. 
![image](https://user-images.githubusercontent.com/5977962/193406083-3f31ed36-3f14-4406-8b73-9d7d3d9c8c65.png)

13) Настройте правила NAT,PAT на пограничных маршрутизаторах.

14) Настройте маршрутизатор филиала: адресацию, статический маршрут до роутеров ЦО через провайдера. Проверьте связность с внешними интерфейсами бордеров ЦО.  
![image](https://user-images.githubusercontent.com/85602495/194488837-7e68a8a5-be09-4937-8e9c-7f7c0810bac5.png)

15) На маршрутизаторе филиала настройте Tunnel-интерфейсы gre до бордеров ЦО. А так же протокол ospf для получения и анонса внутренних сетей. Туннельные интерфейсы  в зоне 2.  
  ![image](https://user-images.githubusercontent.com/5977962/193405773-e0dc77ea-b506-4fad-9700-11e270a8b418.png)

16) Настройте коммутатор доступа филиала для подключения к сети ip-телефона, ПК и точки доступа. На маршрутизаторе настройте helper для централизованного получения сетевых настроек оконечными устройствами. 
![image](https://user-images.githubusercontent.com/85602495/194488342-09e1a8db-8e12-4d8e-9939-57b045cc70ab.png)

17) Настройте БЛВС ТД филиала, подключить к ней ноутбук. Проверьте сетевую связность.

18) Настройте на АСО интерфейсы для управления. Настройте на них аутентификацию по tacacs+ и отправку логов по syslog. 

19) Настройте ip-телефоны, проверьте дозвон.

Кратко опишите, чем чреват выбор протокола GRE для объединения сети ЦО и филиала в 15 пункте. Какие более безопасные альтернативы можно предложить без потери функциональности?

### Тестирование

1) Проверка STP, HSRP. Роль Root bridge и HSRP-active на одном устройстве. Команды: show spanning-tree, show standby на этом устройстве.
2) Проверка маршрутизации на коммутаторах ядра. Show ip route. Должен присутствовать маршрут по-умолчанию и маршруты до интерфейсов ASA и бордеров.
3) Проверка LAG на бордерах show etherchannel summary.
4) Маршрутизация на бордерах sh ip route. В таблице маршрутизации должны присутствовать bgp-маршруты от провайдера, ospf-маршруты до внутренних подсетей ЦО и филиала.
5) Туннель CAPWAP на БЛВС ТД в статусе Connected, с ноутбуков есть связь с 8.8.8.8.
6) Телефонные аппараты зарегестрированы на VoIP сервере, прозвон с одного на другой работает.
7) На все сетевые устройства можно попасть по учётной записи tacacs+ сервера.
8) Время на устройствах синхронизировано. Show ntp status.
9) С 8.8.8.8 есть доступ к web-серверу в DMZ. Обратный доступ тоже есть. Проверять доступ необходимо браузером.
10) Отключение одного из каналов связи не приводит к потере доступа в интернет с пользовательских ПК(ping до сервера 8.8.8.8). 
11) Выход из строя одного из коммутаторов ядра, межсетевого экрана или бордер роутера не приводит к потере доступа в интернет с пользовательских ПК(ping до сервера 8.8.8.8). Потеря доступа к web-серверу извне доспускается.
12) Ноутбуки не имеют доступа к другим подсетям компании(ping svi users, mgmt, printer).

ПРИМЕЧАНИЕ. В связи с особенностью эмулятора Packet Tracer, часто бывает так, что для того, чтобы тест заработал, нужно перезагрузить устройства.

------

### Правила приема проекта

В качестве выполненной работы прикрепите в личном кабинете файлы, содержащие следующую информацию:
* Пояснительную записку, которая содержит таблицы и описания к решениям, указанным на всех этапах выполнения работ. Обоснование предполагает короткое описание (1-3 предложения) выбранного технического решения, которое должно ответить на вопрос, почему вы выбрали именно это решение, а не иное. 
* Собранную топологию в PacketTracer в виде. pkt файла.
* Конфигурационные файлы с оборудования.
* Результаты тестирования, проведенного на шаге "Тестирование", в виде отдельного текстового файла с вложенными в него скриншотами и трассировками.

### Критерии оценки проекта

1. Зачёт:
   - Топология построена корректно.
   - Адреса и подсети распределены с учётом размера сети и возможного масштабирования. 
   - Все тесты пройдены успешно.
   
   Допускается не более 3 ошибок в тестах и двух ошибок выделении подстетей.
   
2. Незачёт:

   - Ошибки в выделении адресов, подсетей и масок, более трёх тестов не пройдено.

