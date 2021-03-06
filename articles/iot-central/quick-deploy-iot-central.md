---
title: Создание приложения Azure IoT Central | Документация Майкрософт
description: Создание приложения Azure IoT Central. Создание пробной версии приложения с оплатой по мере использования с помощью шаблона приложения.
author: viv-liu
ms.author: viviali
ms.date: 10/31/2018
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 3ffc361421f57b405c284742b662a833b178f9da
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52260329"
---
# <a name="create-an-azure-iot-central-application"></a>Создание приложения Azure IoT Central

С помощью пользовательского интерфейса Azure IoT Central _конструктор_ может определить свое приложение Microsoft Azure IoT Central. В этом кратком руководстве объясняется, как создать приложение Azure IoT Central, которое содержит пример _шаблона устройства_ и _имитированные устройства_.

Перейдите на страницу [диспетчера приложений](https://aka.ms/iotcentral) в Azure IoT Central. Вам нужно будет войти в систему с помощью личной, рабочей или учебной учетной записи Microsoft.

Чтобы начать создание приложения Azure IoT Central, выберите **Новое приложение**. Вы перейдете на страницу **Создать приложение**.

![Страница создания приложения Azure IoT Central](media/quick-deploy-iot-central/iotcentralcreate.png)

Чтобы создать приложение Azure IoT Central, сделайте следующее:

1. Выберите план оплаты:
    - **Пробные версии** приложений предоставляются бесплатно в течение 7 дней, после чего срок их действия истекает. Их можно преобразовать для оплаты по мере использования в любое время до истечения срока действия.
    - За приложения **с оплатой по мере использования** плата взимается за каждое устройство, а первые 5 устройств предоставляются бесплатно.

    Узнайте больше о ценах на службу Azure IoT Central на [этой странице](https://azure.microsoft.com/pricing/details/iot-central/).

1. Выберите понятное имя для приложения, например **Contoso IoT**. Azure IoT Central создаст уникальный префикс URL-адреса. Вы можете изменить этот префикс на что-то более запоминающееся.

1. Выберите шаблон приложений. Шаблон приложения может содержать стандартные элементы (например, шаблоны устройств и панели мониторинга), которые позволяют вам приступить к работе.
    | Шаблон приложения | ОПИСАНИЕ |
    | -------------------- | ----------- |
    | Sample Contoso (Образец Contoso)       | Создает приложение, содержащее шаблон устройства, созданный для охлаждаемого торгового автомата. Используйте этот шаблон, чтобы приступить к работе в Azure IoT Central. |
    | Custom application (Образец Devkits)       | Создает приложение с готовыми шаблонами устройств, чтобы вы могли подключить устройство MXChip или Raspberry Pi. Используйте этот шаблон, если вы являетесь разработчиком устройства, работающим с одним из этих устройств. |
    | Custom application (Пользовательское приложение)   | Создает пустое приложение, в которое необходимо добавить собственные шаблоны устройств и сами устройства. |

1. При создании приложения **с оплатой по мере использования** необходимо выбрать *каталог*, *подписку Azure* и *регион*. 
    - *Каталог* — Azure Active Directory для создания приложения. Он содержит удостоверения пользователей, учетные данные и другие сведения об организации. Если у вас нет клиента Azure Active Directory, он будет создан автоматически при создании подписки Azure.

    - *Подписка Azure* позволяет создавать экземпляры служб Azure. IoT Central подготовит ресурсы в вашей подписке. Если у вас еще нет подписки, создайте ее на [странице входа в Azure](https://aka.ms/createazuresubscription). После создания подписки Azure перейдите на страницу **Создание приложения**. В раскрывающемся списке **Подписка Azure** отобразится новая подписка.

    - *Регион* — физическое расположение, в котором вы хотите создать приложение. Как правило, для получения оптимальной производительности следует выбирать регион, ближайший к устройствам. Регионы, в которых доступна служба Azure IoT Central, можно просмотреть на странице [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/).

    > [!Note]
    > Выбрав регион, вы не можете переместить приложение в другой регион.

1. Нажмите кнопку **Создать**.

## <a name="next-steps"></a>Дополнительная информация

В этом кратком руководстве вы создали приложение IoT Central. Ниже приведено предлагаемое дальнейшее действие:

> [!div class="nextstepaction"]
> [Узнайте о возможностях IoT Central](https://docs.microsoft.com/azure/iot-central/overview-iot-central-tour)
