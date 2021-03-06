---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 304637422c0b8fd4dfa99e2e434e13d12f1fb342
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164672"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Двойники устройств — это документы JSON, хранящие сведения о состоянии устройства (метаданные, конфигурации и условия). Центр Интернета вещей сохраняет двойник устройства для каждого устройства, подключаемого к нему.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Двойники устройства используются для выполнения следующих действий:

* хранение метаданных устройства из серверной части вашего решения;

* сообщение сведений о текущем состоянии приложения устройства, таких как доступные возможности и условия (например, используемый метод подключения);

* синхронизация состояния длительных рабочих процессов между приложением устройства и серверной частью (например, при обновлении встроенного ПО и конфигурации);

* выполнение запроса метаданных, конфигурации или состояния устройства.

Двойники устройств используются для синхронизации и выполнения запроса конфигураций или условий устройства. Дополнительные сведения об использовании двойников устройства см. в статье [Общие сведения о двойниках устройств и их использование в Центре Интернета вещей](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Двойники устройств хранятся в Центре Интернета вещей. Они содержат следующие компоненты:

* *теги* — метаданные устройства, доступные только в серверной части решения;

* *требуемые свойства* — объекты JSON, задаваемые только в серверной части решения и наблюдаемые в приложении устройства; а также

* *сообщаемые свойства* — объекты JSON, задаваемые только в приложении устройства и считываемые в серверной части решения (теги и свойства не могут содержать массивы, но объекты могут быть вложенными).

![Изображение двойника устройства, демонстрирующее функциональность](./media/iot-hub-selector-twin-get-started/twin.png)

Кроме того, из серверной части решения можно запросить двойники устройств на основе всех вышеизложенных данных.
Дополнительные сведения о двойниках устройств см. в статье [Общие сведения о двойниках устройств и их использование в Центре Интернета вещей](../articles/iot-hub/iot-hub-devguide-device-twins.md). Справочные материалы по запросам см. в статье [Справочник по языку запросов Центр Интернета вещей для двойников устройств и заданий](../articles/iot-hub/iot-hub-devguide-query-language.md).


В этом учебнике описаны следующие процедуры.

* создание серверного приложения, добавляющего *теги* в двойник устройства и приложение, имитирующее устройство, которое сообщает о своем канале подключения в виде *сообщаемого свойства* в двойнике устройства;

* запрос устройств из серверного приложения с использованием фильтров в созданных ранее тегах и свойствах.
