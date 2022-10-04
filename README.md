# Дипломный проекта по итогам обучения на курсе "Сетевой инженер"

* [Цель проекта](#цель-курсового-проекта)
* [Чек-лист готовности к выполнению проекта](#Чек-лист-готовности-к-выполнению-проекта)
* [Задание](#задание)
* [Этапы выполнения проекта](#этапы-выполнения-проекта) 
* [Тестирование](#тестирование) 
* [Правила приема проекта](#правила-приема-проекта)
* [Критерии оценки проекта](#критерии-оценки-проекта)


### Цель курсового проекта
 

В результате выполнения этого задания вы научитесь настраивать хорошо масштабируемую отказоустойчивую и защищённую сеть, выделяя для каждого пользовательского сервиса уникальные права и пропускную полосу. Также вы научитесь объединять разросненные филиальные сети компании в одну единую. Для удобства администрирования вы научитесь интегрировать инструменты централизованной аутентификации и логирования в настреонную вами сеть. 


1. Настроить отказоустойчивую связку RSTP+HSRP на уровне доступ-ядро.
2. Настроить безопасное централизованное назначение сетевых настроек для оконечных устройств.
3. Организовать централизованный беспроводной гостевой доступ к сети интернет.
4. Организовывать отказустойчивую маршрутизацию в сети компании.
5. Организовывать отказоустойчивый доступ к сети интернет по протоколу BGP.
6. Разворачивать сервис VoIP внутри компании.
7. Настраивать сервисы централизованной аутентификации, синхронизации времени и логирования сетевых устройств.
8. Настраивать сеть компании полностью "под ключ".


### Чек-лист готовности к выполнению проекта

Для выполнения работ потребуется:

1. Понимание протоколов маршрутизации eBGP, OSPF, route redistribute.
2. Понимание предсказуемного поведения протоколов RSTP, HSRP, LAG.
3. Знание протоколов безопасного подключенияк уровню доступа(vlan, trunk, portfast, bpduguard, dhcp-snooping)
4. Умение настраивать туннельный протокол GRE.
5. Умение настраивать Qos для голосового траффика внутри GRE.
6. Знание aaa, tacacs+, NTP, syslog.
7. Умение настраивать устрофства беспроводной сети: AP, WLC.
8. Умение настраивать внутреннюю voip-телефонию.
9. Знание особенностей настройки межсетевого экранирования на Cisco ASA.
10. Умение настраивать static NAT, PAT overload.

------

### Задание

Нужно настроить отказоустойчивую, безопасную, масштабируемую сеть и запустить на ней пользовательские сервисы. 

------

### Этапы выполнения проекта
1) Соберите топологию сети в Cisco Packet tracer. Модели сетевого оборудования: border-router - 1941, fw - ASA 5506-x, ядро - 3560-24, доступ - 2960-24, wlan - WLC-2504, 3702i AP, internet - switch 3650, VOIP-server - router 2811, ip-phone 7960. Используйте за основу шаблон топологии, в котором уже есть оборудование, настройки которого изменять нельзя.

![image](https://user-images.githubusercontent.com/5977962/192236499-941224d0-4694-4c74-8cb1-95a068e99fda.png)

2) Заполните таблицу распределения подсетей и адресов(по примеру коммутатора "internet"). Нужно учёсть возможность добавления пяти новых коммутаторов доступа в ЦО, и открытия трёх новых филиалов:
![image](https://user-images.githubusercontent.com/5977962/193801499-a7d236bf-4e61-482e-961c-fd6190ec1d6c.png)


3) Настройте на коммутаторах доступа порты для подключения пользовательских устройств и аплинки. Также на пользовательских портах коммутатора следует настроить port-security на 2 адреса, dhcp-snooping, portfast, RSTP.
  ![image](https://user-images.githubusercontent.com/5977962/192258720-b8f713d3-e42a-4af4-87ed-36efc8629c71.png)

4) На коммутаторах ядра требуется настроить vlan, LAG, RSTP, SVI,dhcp и HSRP для всех vlan. На этом шаге проверяем работу stp, lag. 
![image](https://user-images.githubusercontent.com/5977962/193780747-d0c64050-94db-499c-8d9e-9f3b7a06f5de.png)

![image](https://user-images.githubusercontent.com/5977962/192258850-47edbb83-3118-4f1b-b735-dd280b103df6.png)

5) Нужно настроить сервисы для распределения сетевых настроек для пользовательских устройств. Так как серве находится в отдельной сети, на SVI настраивается helper. По окончанию настроек dhcp-сервер должен раздать настройки PC, телефонам и принтерам, с любого хоста ЦО должен быть доступен любой другой хост.
![image](https://user-images.githubusercontent.com/5977962/192258899-0278a6e5-815b-458d-bed0-bbab3d21603f.png)

6) Настроить сервис БЛВС. Точки доступа должны подключаться к контроллеру, который сообщит им настройки по capwap. Нужно подключить ноутбук к ТД, проверить связность сети.
![image](https://user-images.githubusercontent.com/5977962/192258971-afec76f9-8e71-45e5-af8a-7a6debcf767c.png)

7) На коммутаторах ядра нужно запустить протокол маршрутизации ospf. Он должен анонсировать все внутренние сети в зоне 1.
![image](https://user-images.githubusercontent.com/5977962/192259147-f4fbe92b-440f-41ba-a54e-036e14eaf0d8.png)

8) На каждом межсетевом экране необходимо настроить адресацию и три зоны: inside, outside, DMZ. Правила фильтрации:
   - из inside доступ свободный во все зоны.
   - из DMZ в inside доуступ разрешен для траффика от приватных адресов. Разрешен траффик со всех адресов в зону DMZ.
   - из DMZ разрешен доступ только в outside на публичные адреса.
![image](https://user-images.githubusercontent.com/5977962/192261102-8b1924bf-0a03-44a7-96b8-8fb0e4e3e2e1.png)

9) На Cisco ASA настроить протокол ospf, принимать и анонсировать все сети в зоне 1. Настроить web-сервер, подключенный в DMZ зону одной из ASA. На этом шаге нужно проверить, все ли сети получены, все ли сети анонсируются на коммутаторы ядра и проверить доступ к web-серверу и с него.
10) Настройте пограничные маршрутизаторы. Нужно настроить адресацию и проверить сетевую связность внутри ЛВС и доступность шлюза провайдера. 
11) Нужно настроить маршрутизацию ospf: интерфейсы в сторону ASA в зоне 1, между собой в зоне 0. Настроить анонс маршрута 0.0.0.0/0 во внутреннюю сеть с разными метриками для резервирования подключения. Другие маршруты с бордеров во внутреннюю сеть не должны анонсироваться. Проверить получение и анонс маршрутов.
![image](https://user-images.githubusercontent.com/5977962/193406159-cd3286ac-3eaa-4ea1-90b2-1530c8de7ce5.png)

12) Настроить ebgp-сессии с оборудованием провайдера. Проверить получение и анонс маршрутов. 
![image](https://user-images.githubusercontent.com/5977962/193406083-3f31ed36-3f14-4406-8b73-9d7d3d9c8c65.png)

13) Настроить правила NAT,PAT на пограничных маршрутизаторах.
14) Настроить маршрутизатор филиала: адресация, статический маршрут до роутеров ЦО через провайдера. Проверить связность с внешними интерфейсами бордеров ЦО.
![image](https://user-images.githubusercontent.com/5977962/193351322-6637e59e-8010-462e-be4b-54ac23b34d2c.png)


15) На маршрутизаторе филиала настроить Tunnel-интерфейсы gre до бордеров ЦО. Настроить протокол ospf для получения и анонса внутренних сетей. Туннельные интерфейсы  в зоне 2.

![image](https://user-images.githubusercontent.com/5977962/193405773-e0dc77ea-b506-4fad-9700-11e270a8b418.png)

16) Настроить коммутатор доступа филиала для подключения к сети ip-телефона, ПК и точки доступа. На маршрутизаторе настроить helper для централизованного получения сетевых настроек оконечными устройствами. 
![image](https://user-images.githubusercontent.com/5977962/192262489-921445f7-54eb-4537-910b-71215db8eb81.png)

17) Настроить БЛВС ТД филиала, подключить к ней ноутбук. Проверить сетевую связность.
18) Настроить на АСО интерфейсы для управления. Настроить на них аутентификацию по tacacs+ и отправку логов по syslog. 
19) Настроить ip-телефоны, проверить дозвон.

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

По результатам выполнения работы необходимо предоставить файлы, содержащие следующую информацию.

Пояснительную записку, которая содержит таблицы и описания к решениям, указанным на всех этапах выполнения работ. Обоснование предполагает короткое описание (1-3 предложения) выбранного технического решения, которое должно ответить на вопрос, почему вы выбрали именно это решение, а не иное. 
Собранную топологию в PacketTracer в виде. pkt файла.
Конфигурационные файлы с оборудования.
Результаты тестирования, проведенного на шаге "Тестирование", в виде отдельного текстового файла с вложенными в него скриншотами и трассировками.

### Критерии оценки проекта

1. Зачёт:
   -Топология построена корректно.
   
   -Адреса и подсети распределены с учётом размера сети и возможного масштабирования. 
   
   -Все тесты пройдены успешно.
   
   Допускается не более 3 ошибок в тестах и двух ошибок выделении подстетей.
   
2. Незачёт:

   -Ошибки в выделении адресов, подсетей и масок, более трёх тестов не пройдено.

