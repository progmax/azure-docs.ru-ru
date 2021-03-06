---
title: включение файла
description: включение файла
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: e5e015c1e12e68f54ae7fd32082faed1abfebe74
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2018
ms.locfileid: "51626404"
---
1. Войдите на [портале Azure](http://portal.azure.com).

1. В области слева выберите **Создать ресурс**. Выполните поиск по фразе **digital twins** и выберите **Digital Twins (предварительная версия)**. Нажмите кнопку **Создать**, чтобы начать развертывание.

   ![Выбранные элементы для создания экземпляра Digital Twins](./media/create-digital-twins-portal/create-digital-twins.png)

1. В области **Digital Twins** введите следующие сведения:
   * **Имя ресурса**. Задайте уникальное имя для экземпляра Digital Twins.
   * **Подписка**. Выберите подписку, в которой будет создан экземпляр Digital Twins. 
   * **Группа ресурсов**. Выберите или создайте [группу ресурсов](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) для экземпляра Digital Twins.
   * **Расположение**. Выберите ближайшее расположение для своих устройств.

    ![Область Digital Twins с введенными сведениями](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Проверьте сведения, указанные для Digital Twins, и щелкните **Создать**. Создание экземпляра Digital Twins может занять несколько минут. Ход создания можно отслеживать на панели **уведомлений**.

1. Откройте область **Общие сведения** для созданного экземпляра Digital Twins. Обратите внимание на ссылку, которая отображается под элементом **API управления**.

   URL-адрес **API управления** имеет следующий вид: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_. По этому URL-адресу находится документация по REST API для Azure Digital Twins, которая относится к созданному экземпляру. См. сведения об [использовании Azure Digital Twins ](../articles/digital-twins/how-to-use-swagger.md), чтобы получить представление о том, как изучать и применять эту документацию по API.

    Измените URL-адрес **API управления** следующим образом: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_. Этот измененный URL-адрес станет базовым адресом для доступа вашего приложения к экземпляру Digital Twins. Скопируйте измененный URL-адрес во временный файл. Этот URL-адрес понадобится вам в следующих разделах.

    ![API управления](./media/create-digital-twins-portal/digital-twins-management-api.png)