---
title: Часто задаваемые вопросы об акселераторах решений для Интернета вещей Azure | Документация Майкрософт
description: Часто задаваемые вопросы об акселераторах решений для Интернета вещей
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 294eae1dd5e591d12fdb82f62ea013ba54489686
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253352"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Часто задаваемые вопросы об акселераторах решений для Интернета вещей

Дополнительные сведения см. также в ответах на часто задаваемые вопросы по [подключенной фабрике](iot-accelerators-faq-cf.md) и [удаленному мониторингу](iot-accelerators-faq-rm-v2.md).

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Где можно найти исходный код акселераторов решений?

Исходный код хранится в следующих репозиториях GitHub:

* [акселератор решений для удаленного мониторинга (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet);
* [акселератор решений для удаленного мониторинга (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java);
* [акселератор решений для прогнозного обслуживания](https://github.com/Azure/azure-iot-predictive-maintenance);
* [акселератор решения "Подключенная фабрика"](https://github.com/Azure/azure-iot-connected-factory).

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Какие пакеты SDK можно использовать для разработки клиентов устройств для акселераторов решений?

Ссылки на пакеты SDK для устройств Интернета вещей для других языков (C, .NET, Java, Node.js, Python) можно найти в репозиториях GitHub для [пакетов SDK Microsoft Azure для Интернета вещей](https://github.com/Azure/azure-iot-sdks).

Если вы используете устройство DevKit, ресурсы и примеры можно найти в репозитории GitHub для [пакета SDK для DevKit](https://github.com/Microsoft/devkit-sdk).

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Доступна ли новая архитектура микрослужб для всех трех типов акселераторов решений?

Сейчас архитектура микрослужб используется только в решении для удаленного мониторинга, так как в нем охватывается самый широкий сценарий.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Каковы преимущества новой архитектуры на базе микрослужб с открытым кодом в новом обновлении?

За последние два года облачная архитектура значительно улучшилась. Микрослужбы стали оптимальным вариантом, который обеспечивает масштабируемость и гибкость без снижения скорости разработки. В некоторых службах Майкрософт по умолчанию используется шаблон архитектуры, который обеспечивает высокую надежность и масштабируемость. Мы реализуем полученные знания на практике, чтобы предоставить своим клиентам преимущества.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Где можно найти сведения о предыдущей версии решения удаленного мониторинга?

Предыдущая версия акселератора решений для удаленного мониторинга была известна как предварительно сконфигурированное решение удаленного мониторинга IoT Suite. Можно найти в заархивированную документацию на сайте [https://docs.microsoft.com/previous-versions/azure/iot-suite/](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Доступен ли новый акселератор решений в том же географическом регионе, что и существующее решение?

Да, новое решение для удаленного мониторинга доступно в том же географическом регионе.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>В чем разница между удалением группы ресурсов на портале Azure и нажатием кнопки "Удалить" в акселераторе решений на сайте azureiotsuite.com?

* Если вы удаляете акселератор решений на сайте [azureiotsuite.com](https://www.azureiotsolutions.com/), вместе с ним будут удалены все ресурсы, подготовленные при его создании. Если вы добавляли в группу ресурсов дополнительные ресурсы, они также будут удалены.
* Если удалить группу ресурсов на [портале Azure](https://portal.azure.com), будут удалены ресурсы в этой группе. Вам также придется удалить приложение Azure Active Directory, связанное с акселератором решений.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Можно ли и дальше использовать существующие системы на базе акселераторов решений для Интернета вещей Azure?

Да. Любое имеющееся решение продолжает работать в подписке Azure, а исходный код остается доступным на GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Сколько экземпляров центров Интернета вещей можно подготовить в рамках одной подписки?

По умолчанию [в рамках одной подписки можно подготовить 10 экземпляров Центра Интернета вещей](../azure-subscription-service-limits.md#iot-hub-limits). Вы можете отправить [запрос в службу поддержки Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), чтобы увеличить этот лимит. Так как каждый акселератор решений подготавливает новый Центр Интернета вещей, в рамках одной подписки можно подготовить не более 10 акселераторов решений.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Сколько экземпляров Azure Cosmos DB можно подготовить в рамках одной подписки?

Пятьдесят. Вы можете отправить [запрос в службу поддержки Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), чтобы увеличить это количество, но по умолчанию в рамках одной подписки можно подготовить только 50 экземпляров Cosmos DB.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Сколько бесплатных API-интерфейсов Карт Bing можно подготовить в рамках одной подписки?

Два. В рамках подписки Azure можно создать только две карты Bing уровня 1 с внутренними транзакциями для корпоративных планов. Решение для удаленного мониторинга по умолчанию подготавливается с помощью плана уровня 1 с внутренними транзакциями. Иными словами, используя одну подписку, в которую не вносились изменения, вы можете подготовить не более двух решений для удаленного мониторинга.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Можно ли создать акселератор решений, если я использую Microsoft Azure для DreamSpark?

> [!NOTE]
> Microsoft Azure для DreamSpark теперь называется Microsoft Imagine для учащихся.

Сейчас вы не можете создать акселератор решений с использованием учетной записи [Microsoft Azure для DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/). Но вы можете всего за пару минут создать [бесплатную пробную учетную запись Azure](https://azure.microsoft.com/free/), которая позволит создать акселератор решений.

### <a name="how-do-i-delete-an-aad-tenant"></a>Как удалить клиент AAD?

Ознакомьтесь с записью блога Эрика Голпа (Eric Golpe) [Walkthrough of Deleting an Azure AD Tenant](https://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx) (Пошаговое руководство по удалению клиента Azure AD).

### <a name="next-steps"></a>Дополнительная информация

Вы также можете ознакомиться с другими функциями и возможностями акселераторов решений для Интернета вещей:

* [Обзор возможностей акселератора решений для удаленного мониторинга](quickstart-remote-monitoring-deploy.md)
* [Общие сведения об акселераторе решений для диагностического обслуживания](iot-accelerators-predictive-overview.md)
* [Развертывание акселератора решения "Подключенная фабрика"](quickstart-connected-factory-deploy.md)
* [Комплексная защита в Интернете вещей](/azure/iot-fundamentals/iot-security-ground-up)
