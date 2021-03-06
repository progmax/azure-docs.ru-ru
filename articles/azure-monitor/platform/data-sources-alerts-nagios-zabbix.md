---
title: Сбор оповещений Nagios и Zabbix в Log Analytics | Документация Майкрософт
description: Nagios и Zabbix — средства мониторинга с открытым исходным кодом. Оповещения от этих средств мониторинга можно собирать в Log Analytics для анализа вместе с оповещениями из других источников.  В этой статье описано, как настроить агент Log Analytics для Linux для сбора оповещений из этих систем.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 3331ed7775cd3027f1262b195c6230fbea742497
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2018
ms.locfileid: "52336601"
---
# <a name="collect-alerts-from-nagios-and-zabbix-in-log-analytics-from-log-analytics-agent-for-linux"></a>Сбор оповещений Nagios и Zabbix с помощью агента Log Analytics для Linux в службу Log Analytics 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
[Nagios](https://www.nagios.org/) и [Zabbix](http://www.zabbix.com/) — средства мониторинга с открытым исходным кодом. Оповещения от этих средств мониторинга можно собирать в Log Analytics для анализа вместе с [оповещениями из других источников](../../monitoring-and-diagnostics/monitoring-overview-alerts.md).  В этой статье описано, как настроить агент Log Analytics для Linux для сбора оповещений из этих систем.
 
## <a name="prerequisites"></a>Предварительные требования
Агент Log Analytics для Linux поддерживает сбор оповещений из Nagios до версии 4.2.x и Zabbix до версии 2.x.

## <a name="configure-alert-collection"></a>Настройка сбора оповещений

### <a name="configuring-nagios-alert-collection"></a>Настройка сбора оповещений Nagios
Для сбора оповещений выполните следующие действия на сервере Nagios.

1. Предоставьте пользователю **omsagent** разрешение на чтение для файла журнала Nagios `/var/log/nagios/nagios.log`. Если файл nagios.log принадлежит группе `nagios`, можно добавить пользователя **omsagent** в группу **nagios**. 

    sudo usermod -a -G nagios omsagent

2.  Измените файл конфигурации на `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Для этого введите следующие записи в незакомментированном виде:  

        <source>  
          type tail  
          #Update path to point to your nagios.log  
          path /var/log/nagios/nagios.log  
          format none  
          tag oms.nagios  
        </source>  
      
        <filter oms.nagios>  
          type filter_nagios_log  
        </filter>  

3. Перезапустите управляющую программу omsagent

    ```
    sudo sh /opt/microsoft/omsagent/bin/service_control restart
    ```

### <a name="configuring-zabbix-alert-collection"></a>Настройка сбора оповещений Zabbix
Для сбора оповещений сервера Zabbix необходимо указать имя пользователя и пароль в формате *открытого текста*.  Хотя это и не оптимально, рекомендуется создать пользователя Zabbix с разрешениями только для чтения для перехвата соответствующих предупреждений.

Для сбора оповещений выполните следующие действия на сервере Nagios.

1. Измените файл конфигурации на `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`. Убедитесь, что в файле есть следующие параметры и что они не закомментированы.  Измените имя пользователя и пароль на соответствующие значения для вашей среды Zabbix.

        <source>
         type zabbix_alerts
         run_interval 1m
         tag oms.zabbix
         zabbix_url http://localhost/zabbix/api_jsonrpc.php
         zabbix_username Admin
         zabbix_password zabbix
        </source>

2. Перезапустите управляющую программу omsagent

    `sudo sh /opt/microsoft/omsagent/bin/service_control restart`


## <a name="alert-records"></a>Записи оповещений
Записи оповещений из Nagios и Zabbix можно получить с помощью [поиска по журналам](../../log-analytics/log-analytics-queries.md) в Log Analytics.

### <a name="nagios-alert-records"></a>Записи оповещений Nagios

Для записей оповещений Nagios параметр **Type** (тип) имеет значение **Alert** (оповещение), а параметр **SourceSystem** (источник) — значение **Nagios**.  У них есть свойства, приведенные в таблице ниже.

| Свойство | ОПИСАНИЕ |
|:--- |:--- |
| type |*Предупреждение* |
| SourceSystem |*Nagios* |
| AlertName |Имя оповещения. |
| AlertDescription | Описание оповещения. |
| AlertState | Состояние службы или узла.<br><br>ОК<br>ПРЕДУПРЕЖДЕНИЕ<br>РАБОТАЕТ<br>СБОЙ |
| HostName | Имя узла, который создал оповещение. |
| PriorityNumber | Приоритет оповещения. |
| StateType | Тип состояния оповещения.<br><br>SOFT — проблема, которая не проверялась повторно.<br>HARD — проблема, которая проверялась повторно заданное число раз.  |
| TimeGenerated |Дата и время создания оповещения. |


### <a name="zabbix-alert-records"></a>Записи оповещений Zabbix
Для записей оповещений Nagios параметр **Type** (тип) имеет значение **Alert** (оповещение), а параметр **SourceSystem** (источник) — значение **Zabbix**.  У них есть свойства, приведенные в таблице ниже.

| Свойство | ОПИСАНИЕ |
|:--- |:--- |
| type |*Предупреждение* |
| SourceSystem |*Zabbix* |
| AlertName | Имя оповещения. |
| AlertPriority | Серьезность оповещения.<br><br>не определен<br>информационный.<br>Предупреждение<br>average<br>высокий<br>очень высокий  |
| AlertState | Состояние оповещения.<br><br>0 — актуальное состояние.<br>1 — состояние неизвестно.  |
| AlertTypeNumber | Указывает, может ли оповещение создавать несколько событий, связанных с проблемой.<br><br>0 — актуальное состояние.<br>1 — состояние неизвестно.    |
| Комментарии | Дополнительные комментарии для оповещения. |
| HostName | Имя узла, который создал оповещение. |
| PriorityNumber | Значение, указывающее уровень серьезности оповещения.<br><br>0 — не определен<br>1 — информация<br>2 — предупреждение<br>3 — средний<br>4 — высокий<br>5 — очень высокий |
| TimeGenerated |Дата и время создания оповещения. |
| TimeLastModified |Дата и время последнего изменения состояния оповещения. |


## <a name="next-steps"></a>Дополнительная информация
* Ознакомьтесь с дополнительными сведениями об [оповещениях](../../monitoring-and-diagnostics/monitoring-overview-alerts.md) в Log Analytics.
* Узнайте больше об [операциях поиска по журналу](../../log-analytics/log-analytics-queries.md) , которые можно применять для анализа данных, собираемых из источников данных и решений. 
