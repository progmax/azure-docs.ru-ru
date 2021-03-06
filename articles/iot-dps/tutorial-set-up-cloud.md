---
title: Настройка облака для службы подготовки устройств для Центра Интернета вещей Azure на портале | Документация Майкрософт
description: Автоматическая подготовка устройств Центра Интернета вещей на портале Azure
author: sethmanheim
ms.author: sethm
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 971b00f54d59782d5aa7ca752fc06e490d372760
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514848"
---
# <a name="configure-cloud-resources-for-device-provisioning-with-the-iot-hub-device-provisioning-service"></a>Настройка облачных ресурсов для подготовки устройств с помощью службы подготовки устройств для Центра Интернета вещей

В этом руководстве показано, как настроить облако для автоматической подготовки устройств с помощью службы подготовки устройств для Центра Интернета вещей. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * использовать портал Azure для создания службы подготовки устройств для Центра Интернета вещей и получить область идентификаторов;
> * Создание Центра Интернета вещей
> * связать Центр Интернета вещей со службой подготовки устройств;
> * Задание политики распределения для службы подготовки устройств

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="create-a-device-provisioning-service-instance-and-get-the-id-scope"></a>Создание экземпляра службы подготовки устройства и получение области идентификаторов

Выполните следующие действия для создания экземпляра службы подготовки устройства.

1. В верхнем левом углу окна портала Azure щелкните **Создать ресурс**.

2. В поле поиска введите **подготовка устройства**. 

3. Щелкните **IoT Hub Device Provisioning Service** (Служба подготовки устройств для Центра Интернета вещей).

4. Заполните форму **IoT Hub Device Provisioning Service** (Служба подготовки устройств для Центра Интернета вещей) следующими сведениями:
    
   | Параметр       | Рекомендуемое значение | Описание | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Имя** | Любое уникальное имя. | -- | 
   | **Подписка** | Ваша подписка  | Дополнительные сведения о подписках см. [здесь](https://account.windowsazure.com/Subscriptions). |
   | **Группа ресурсов** | myResourceGroup | Допустимые имена групп ресурсов см. в статье о [правилах и ограничениях именования](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Местоположение.** | Любое допустимое расположение | Дополнительные сведения о регионах Azure см. [здесь](https://azure.microsoft.com/regions/). |   

   ![Ввод основных сведений о службе подготовки устройств на портале](./media/tutorial-set-up-cloud/create-iot-dps-portal.png)

5. Нажмите кнопку **Создать**. Через несколько секунд создастся экземпляр Службы подготовки устройств и отобразится страница **Обзор**.

6. На странице **Обзор** нового экземпляра службы скопируйте значение **Область идентификатора**. Оно понадобится позднее. Это значение используется для определения идентификаторов регистрации и гарантирует, что идентификатор регистрации является уникальным.

7. Кроме того, скопируйте значение **Конечная точка службы**. Оно также потребуется позднее. 

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Получение строки подключения для центра Интернета вещей

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

Теперь у вас есть Центр Интернета вещей, а также имя узла и строка подключения Центра Интернета вещей, необходимые для завершения работы с этим руководством.

## <a name="link-the-device-provisioning-service-to-an-iot-hub"></a>Связывание службы подготовки устройств с Центром Интернета вещей

Следующим шагом является связывание службы подготовки устройств и Центра Интернета вещей, чтобы служба подготовки устройств для Центра Интернета вещей могла регистрировать устройства в центре. Служба подготовки устройств может подготавливать для Центров Интернета вещей только связанные с ней устройства. Выполните следующие действия.

1. На странице **Все ресурсы** щелкните экземпляр службы подготовки устройства, созданный ранее.

2. На странице службы подготовки устройств щелкните **Linked IoT hubs** (Связанные Центры Интернета вещей).

3. Щелкните **Добавить**.

4. На странице **Добавление ссылки на Центр Интернета вещей** укажите следующие сведения и нажмите кнопку **Сохранить**:

    * **Подписка**. Обязательно выберите подписку, содержащую Центр Интернета вещей. Можно указать ссылку на Центр Интернета вещей, который находится в другой подписке.

    * **Центр Интернета вещей**. Выберите имя Центра Интернета вещей, который требуется связать с этим экземпляром Службы подготовки устройств.

    * **Политика доступа**. Выберите **iothubowner** в качестве учетных данных для установления связи с Центром Интернета вещей.

   ![Связывание имени центра со службой подготовки устройств на портале](./media/tutorial-set-up-cloud/link-iot-hub-to-dps-portal.png)

## <a name="set-the-allocation-policy-on-the-device-provisioning-service"></a>Задание политики распределения для службы подготовки устройств

Политика распределения является параметром службы подготовки устройств для Центра Интернета вещей, который определяет, как устройства назначаются Центру Интернета вещей. Поддерживаются три политики распределения: 

1. **Lowest latency** (Минимальная задержка). Устройства подготавливаются в Центре Интернета вещей на основе центра с минимальной задержкой доступа к устройству.

2. **Evenly weighted distribution** (Равномерное распределение) (по умолчанию). В связанных Центрах Интернета вещей в равной степени могут быть подготовлены устройства. Это значение по умолчанию. При подготовке устройств только в одном Центре Интернета вещей можно оставить это значение. 

3. **Static configuration via the enrollment list** (Статическая конфигурация через список регистрации). Спецификация нужного Центра Интернета вещей в списке регистрации имеет приоритет над политикой распределения на уровне службы подготовки устройств.

Чтобы задать политику распределения, на странице службы подготовки устройств щелкните **Manage allocation policy** (Управление политиками распределения). Убедитесь, что для политики распределения установлено значение **Evenly weighted distribution** (Равномерное распределение) (по умолчанию). Если вы внесете здесь изменения, щелкните **Сохранить**.

![Управление политикой распределения](./media/tutorial-set-up-cloud/iot-dps-manage-allocation.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Другие руководства в этой коллекции созданы на основе этого руководства. Если вы планируете продолжать работу с этими руководствами по быстрому запуску или обычными руководствами, не удаляйте созданные ресурсы. Если вы не планируете продолжать работу, удалите все созданные ресурсы, выполнив на портале Azure следующие действия.

1. В меню слева на портале Azure нажмите кнопку **Все ресурсы** и откройте экземпляр службы подготовки устройств для Центра интернета вещей. В верхней части страницы **Все ресурсы** щелкните **Удалить**.  

2. В меню слева на портале Azure нажмите кнопку **Все ресурсы** и выберите свой Центр Интернета вещей. В верхней части страницы **Все ресурсы** щелкните **Удалить**.
 
## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * использовать портал Azure для создания службы подготовки устройств для Центра Интернета вещей и получить область идентификаторов;
> * Создание Центра Интернета вещей
> * связать Центр Интернета вещей со службой подготовки устройств;
> * Задание политики распределения для службы подготовки устройств

Перейдите к следующему руководству, чтобы узнать, как настроить устройство для подготовки.

> [!div class="nextstepaction"]
> [Set up a device to provision using the Azure IoT Hub Device Provisioning Service](tutorial-set-up-device.md) (Настройка устройства для подготовки с помощью службы подготовки устройств для Центра Интернета вещей)
