# SNMP
---

**Simple Network Management Protocol** - простой протокол сетевого управления. Интернет протокол для управления устройствами в IP-сетях на основе TCP/UDP.

---

## Компоненты

- Менеджер
- Управляемое устройство
- Агент
- Система сетевого управления

![snmpcommunication](/SNMP_communication_principles_diagram.png)
___


### Менеджер
Обрабатывают данные о конфигурации и конфигурировании управляемых систем.
Доступные через SNMP переменные организованы в иерархии. Эти иерархии описываются базами управляющей информации (MIB - Managment information base)

___

### Управляемое устройство

Элемент сети (оборудование или ПО) реализующий интерфейст управления. 

Ведет обмен информацией с *Менеджером*

___

### Агент

Программны мойдуль распологающийся на управляемом устройстве. "Преобразует" информацию в формат SNMP и обратно.

___

### Система сетевого управления (NMS)
Приложение отслеживающие и контроллирующее управляемые устройства. Обеспечивает основную часть обработки данны (история значений, оповещение об изменении состояния)

---

## Базы управляющей информации

Так как адреса объетов имеют цифровой формат для удобного запоминания существует базы управляющей информации. Суть базы в преобразовании числового адреса объекта в тестовый понятный человеку.

---

## Версии протокола
- SNMPv1
- SNMPv2
- SNMPv3
___

### SNMPv1
Изначальная реализация протокола SNMP, разрабатывался в 80х годах.
___
### SNMPv2
Расширяет SNMPv1 добавляя 2 новых PDU. Все изменения нацелены на повышения производительности.
___
### SNMPv3
- шифрование
- контроль целостности
- аутентификация
---

## Детали протокола

SNMP работает на прикладном уровне TCP/IP. Агент получается запросы по 161/UDP порту. 

Менеджер может получать уведомления (трапы) по порту 162/UDP

При исопльзовании TLS запросы получаются по порту 10161 а трапы на порт 10162.

___

### Протокольные единицы

В SNMPv1 существует 5 протокольных единиц обмена (protocol data units - PDU)

В SNMPv2 и SNMPv3 добавлены 2 дополнительные единицы.

Формат PDU:
![snmpmessage](/SNMP_Message.png)

Ниже будут перечислины все 7 протокольных единиц
___

### GetRequest

Запрос от менеджера к объекту для получения значения переменной или списка переменных.
___
### SetRequest

Запрос от менеджера к объекту для изменения переменной или списка переменных.

___

### GetNextRequest
Запрос от менеджера к объекту для обнаружения доступных переменных и их значений.
___

### GetBulkRequest
Улучшенная версия GetNextRequest. Запрос от менеджера к объекту для многочисленных итераций GetNextRequest.
___

### Response 
Возвращает связанные переменные и значения от агента менеджеру для GetRequest, SetRequest, GetNextRequest, GetBulkRequest и InformRequest. 
___

### Trap 
Асинхронное уведомление от агента — менеджеру. Включает в себя текущее значение sysUpTime, OID, определяющий тип trap (ловушки), и необязательные связанные переменные. 
___

### InformRequest
Асинхронное уведомление от менеджера — менеджеру или от агента — менеджеру. Уведомления от менеджера — менеджеру были возможны уже в SNMPv1 (с помощью Trap), но SNMP обычно работает на протоколе UDP,

---

## Настрока SNMP агентов
Для примера настроим SNMP на Linux сервере и коммутаторе
___
Dlink

```bash
enable snmp
config snmp system_contact admin@example.com
delete snmp community public
delete snmp community private
create snmp group public v1 read_view CommunityView notify_view CommunityView 
create snmp group public v2c read_view CommunityView notify_view CommunityView 
create snmp community public view CommunityView read_only
create snmp group private v1 read_view CommunityView write_view CommunityView notify_view CommunityView 
create snmp group private v2c read_view CommunityView write_view CommunityView notify_view CommunityView 
create snmp community private view CommunityView read_write
disable community_encryption

```
___

Linux server snmpd

```bash
apt-get install net-snmp snmp snmpd
services snmpd start
```

Конфигурационный файл
```bash
/etc/snmp/snmpd.conf
```
---

## Примеры использования SNMP
Получение имени хоста
```bash
snmpget -v 2c -c public 192.168.1.1 sysName.0
```
Измененение hostname
```bash
snmpset -v 2c -c private 192.168.1.1 sysName.0 s testhost
```
---
# Всем бобра! )
Контакты:

Сергей (ёж) Ларченко

EMail: [hdhog@hdhog.ru](mailto:hdhog@hdhog.ru)

Telegram: [telegram.me/hdhog](telegram.me/hdhog)

VK: [vk.com/hdhog](https://vk.com/hdhog)
