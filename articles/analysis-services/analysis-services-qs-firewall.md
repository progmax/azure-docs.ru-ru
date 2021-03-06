---
title: Краткое руководство. Настройка брандмауэра для сервера Analysis Services в Azure | Документация Майкрософт
description: Узнайте, как настроить брандмауэр для экземпляра сервера Analysis Services в Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0351ab3213ac75559cc025f2ef71e2f098a7504c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993200"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Краткое руководство. Настройка брандмауэра сервера с помощью портала

Это краткое руководство поможет настроить брандмауэр для сервера Azure Analysis Services. Включение брандмауэра и настройка диапазонов IP-адресов только для тех компьютеров, которые имеют доступ к серверу, является важной частью процесса защиты сервера и данных.

## <a name="prerequisites"></a>Предварительные требования

- Подписка на сервер служб Analysis Services. Дополнительные сведения см. в статье [ Краткое руководство по созданию сервера с помощью портала](analysis-services-create-server.md) или [ Краткое руководство по созданию сервера с помощью PowerShell](analysis-services-create-powershell.md).
- Один или несколько диапазонов IP-адресов для клиентских компьютеров (при необходимости).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure 

[Войдите на портал](https://portal.azure.com).

## <a name="configure-a-firewall"></a>Настройка брандмауэра

1. Чтобы открыть страницу обзора, щелкните сервер. 
2. Во вкладках **ПАРАМЕТРЫ** > **Брандмауэр** > **Включить брандмауэр** щелкните кнопку **Включить**.
3. Чтобы разрешить доступ к DirectQuery из службы Power BI для поля **Разрешить доступ из Power BI**, нажмите кнопку **Включить**.  
4. (Необязательно) Укажите один или несколько диапазонов IP-адресов. Введите имя, начальный и конечный IP-адрес для каждого из диапазонов. 
5. Выберите команду **Сохранить**.

     ![Параметры брандмауэра](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите диапазоны IP-адресов или отключите брандмауэр, когда они больше не нужны.

## <a name="next-steps"></a>Дополнительная информация
Из этого краткого руководства вы узнали, как настроить брандмауэр для сервера. Теперь, когда сервер защищен брандмауэром, можно добавить к нему базовый образец модели данных с портала. Наличие образца модели полезно, чтобы изучить настройку ролей шаблонов базы данных и тестирование клиентских подключений. Для получения дополнительных сведений перейдите к руководству по добавлению образца модели.

> [!div class="nextstepaction"]
> [Руководство. Добавление образца модели на сервер](analysis-services-create-sample-model.md)
