---
title: Требования к системе для Azure Data Box Edge | Документация Майкрософт
description: Дополнительные сведения о требованиях к программному обеспечению и сети для Azure Data Box Edge
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 7f03953739eebc63c57b30750e15329f24a00537
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263449"
---
# <a name="azure-data-box-edge-system-requirements-preview"></a>Системные требования для Azure Data Box Edge (предварительная версия)

В этой статье описаны важные требования к системе и рекомендации для решения на основе шлюза Azure Data Box Edge и для клиентов, подключающихся к нему. Перед началом развертывания Data Box Edge рекомендуется ознакомиться со следующими сведениями. Данные сведения можно просматривать во время развертывания и при последующих операциях.

Требования к системе Data Box Edge.

- **Требования к программному обеспечению для узлов**. Описание поддерживаемых платформ, браузеров для локальной конфигурации пользовательского интерфейса, клиентов SMB и любых дополнительных требований для клиентов, которые получают доступ к устройству.
- **Требования к сети для устройства**. Предоставление информации о любых сетевых требованиях к работе физическое устройства.

> [!IMPORTANT]
> В данный момент решение Data Box Edge находится в состоянии предварительной версии. Изучите [условия использования для предварительной версии](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), прежде чем развертывать это решение.

## <a name="supported-os-for-clients-connected-to-device"></a>Поддерживаемая ОС для клиентов, подключенных к устройству

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Поддерживаемые протоколы для клиентов, обращающихся к устройству

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Учетные записи хранилища BLOB-объектов

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Поддерживаемые типы хранилищ

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Поддерживаемые браузеры для локального пользовательского веб-интерфейса

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="port-configuration-for-data-box-edge"></a>Конфигурация порта для Data Box Edge

В таблице ниже приведены порты, которые должны быть открытыми в брандмауэре для разрешения использования SMB, облака или управления трафиком. В этой таблице значение *в* или *входящий* относится к направлению, из которого клиент запрашивает доступ к вашему устройству. Значение *исходящий* указывает на *направление*, в котором устройство Data Box Edge отправляет данные за пределами развертывания, например, в Интернет.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="port-configuration-for-iot-edge"></a>Конфигурация порта для IoT Edge

Azure IoT Edge обеспечивает исходящее подключение локального пограничного устройства с облаком Azure с помощью поддерживаемых протоколов Центра Интернета вещей. Входящий трафик требуется только для конкретных сценариев, в которых Центр Интернета вещей отправляет сообщения на устройство Azure IoT Edge (например, обмен сообщениями между облаком и устройством).

Чтобы получить конфигурацию порта для серверов, на которых размещается среда выполнения Azure IoT Edge, используйте следующую таблицу.

| Порт № | Входящий или исходящий | Область порта | Обязательно | Руководство |
|----------|-----------|------------|----------|----------|
| TCP 5671 (AMQP)| Исходящий       | WAN        | Yes      | Протокол связи по умолчанию для IoT Edge. Должен быть открыт, если Azure IoT Edge не настроен для других поддерживаемых протоколов или если AMQP является требуемым протоколом связи. <br>IoT Edge не поддерживает порт 5672 для AMQP. <br>Заблокируйте этот порт, если Azure IoT Edge использует другой протокол, поддерживаемый Центром Интернета вещей. |
| TCP 443 (HTTPS)| Исходящий       | WAN        | Yes      | Исходящие подключение открыто для службы IoT Edge (при подготовке). Если у вас есть прозрачный шлюз с конечными устройствами, которые могут отправлять запросы метода. В этом случае порт 443 не должен быть открыт во внешних сетях для подключения к Центру Интернета вещей или предоставления служб Центра Интернета вещей через Azure IoT Edge. Таким образом, правило для входящего трафика может разрешать только открытие входящего подключения из внутренней сети. |
| TCP 5671 (AMQP) | В        |            | Нет        | Входящие подключения должны быть заблокированы.|
| TCP 443 (HTTPS) | В        |            | В некоторых случаях см. комментарии | Входящие подключения должны быть открытыми только для указанных сценариев. Если невозможно настроить протоколы, отличные от HTTP (например, AMQP, MQTT), сообщения могут отправляться через протоколы WebSocket, использующие порт 443. |

Дополнительные сведения см. в статье [Распространенные проблемы и их решения для Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>Шаблоны URL-адресов для правил брандмауэра

Нередко сетевые администраторы могут настраивать правила брандмауэра на основе шаблонов URL-адресов, чтобы фильтровать входящий и исходящий трафик. Устройство и служба Data Box Edge зависят от других приложений Майкрософт, в том числе от служебной шины Microsoft Azure, контроля доступа Azure Active Directory, учетных записей хранения и серверов центра обновления Майкрософт. Шаблоны URL-адресов, связанных с этими приложениями, можно использовать для настройки правил брандмауэра. Важно понимать, что шаблоны URL-адресов, связанных с этими приложениями, могут меняться. Данные изменения требуют от сетевого администратора отслеживать правила брандмауэра для Data Box Edge и обновлять их по мере необходимости.

В большинстве случаев мы рекомендуем настраивать правила брандмауэра для исходящего трафика на основе фиксированных IP-адресов Data Box Edge. Тем не менее, чтобы задать расширенные правила брандмауэра, необходимые для создания безопасных сред, можно использовать приведенные ниже сведения.

> [!NOTE]
> - В качестве IP-адресов устройств (источников) всегда должны устанавливаться все сетевые интерфейсы с поддержкой облака.
> - В качестве IP-адресов назначения должны быть заданы [диапазоны IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     Шаблон URL-адреса                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Компонент или функциональность                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Служба Azure Data Box Edge<br>Azure Service Bus<br>Служба проверки подлинности    |
|    http://*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Активация устройства                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Отзыв сертификатов                                                                               |
|    https://*.core.windows.net/* https://*.data.microsoft.com http://*.msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Учетные записи хранения Azure и мониторинг                                                                |
|    http://windowsupdate.microsoft.com<br>http://*.windowsupdate.microsoft.com<br>https://*.windowsupdate.microsoft.com<br>http://*.update.microsoft.com<br>https://*.update.microsoft.com<br>http://*.windowsupdate.com<br>http://download.microsoft.com<br>http://*.download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*.ws.microsoft.com<br>https://*.ws.microsoft.com<br>http://*.mp.microsoft.com        |    Серверы Центра обновления Майкрософт                                                                             |
|    http://*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    CDN Akamai                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Вспомогательный пакет                                                                                      |
|    http://*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    Подробнее о службе телеметрии в Windows см. на странице описания обновления для улучшения качества программного обеспечения и диагностики телеметрии      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |

## <a name="internet-bandwidth"></a>Пропускная способность Интернета

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Дальнейшие действия

* [Руководство. Подготовка к развертыванию Azure Data Box Edge (предварительная версия)](data-box-Edge-deploy-prep.md)
