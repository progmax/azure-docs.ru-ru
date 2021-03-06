---
title: Краткое руководство о политике срока действия групп Office 365 в Azure Active Directory | Документация Майкрософт
description: Срок действия групп Office 365 в Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: quickstart
ms.date: 08/07/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 7008943e9077cbad3c58de43f64b105f35931bf3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2018
ms.locfileid: "40208933"
---
# <a name="quickstart-set-office-365-groups-to-expire-in-azure-active-directory"></a>Краткое руководство. Задание срока действия групп Office 365 в Azure Active Directory

В этом кратком руководстве описано, как задать политику срока действия групп Office 365. Если у пользователя есть возможность настройки собственных групп, количество неиспользуемых групп увеличивается. Первый способ управления неиспользуемыми группами — задание срока действия этих групп, чтобы сократить обслуживание при их удалении вручную.

Политика срока действия простая:

* Владельцы группы получают уведомления, чтобы обновить группу с истекающим сроком действия
* Группа, которая не была обновлена, удаляется.
* Владелец группы или администратор Azure AD может восстановить удаленную группу Office 365 в течение 30 дней

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisite"></a>Предварительные требования

Настроить срок действия группы может только глобальный администратор или администратор учетных записей пользователей в клиенте.

## <a name="turn-on-user-creation-for-groups"></a>Включение создания пользователя для групп

1. Войдите на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора или администратора учетных записей пользователей каталога.

2. Нажмите кнопку **Группы**, а затем выберите раздел **Общие**.
  
  ![Самостоятельные настройки групп](./media/groups-quickstart-expiration/self-service-settings.png)

3. Задайте для параметра **Пользователи могут создавать группы Office 365** значение **Да**.

4. Когда все будет готово, нажмите кнопку **Сохранить** для сохранения параметров групп.

## <a name="set-group-expiration"></a>Настройка срока действия группы

1. Чтобы открыть параметры срока действия, перейдите на [портал Azure](https://portal.azure.com) и щелкните следующие элементы: **Azure Active Directory** > **Группы** > **Окончание срока действия**.
  
  ![Параметры срока действия](./media/groups-quickstart-expiration/expiration-settings.png)

2. Установите интервал срока действия. Выберите заранее установленное значение или введите пользовательское значение больше 31 дня. 

3. Если у группы нет владельца, укажите адрес электронной почты, на который необходимо отправлять уведомления об истечении срока действия.

4. При работе с этим кратким руководством для параметра **Включить срок действия для этих групп Office 365** нужно нажать кнопку **Все**.

5. Когда все будет готово, нажмите кнопку **Сохранить** для сохранения срока действия.

Вот и все! При работе с этим кратким руководством вы успешно задали политику срока действия выбранных групп Office 365.

## <a name="clean-up-resources"></a>Очистка ресурсов

**Удаление политики срока действия**

1. Убедитесь, что вы вошли на [портал Azure](https://portal.azure.com) с помощью учетной записи глобального администратора клиента.
2. Выберите **Azure Active Directory** > **Группы** > **Окончание срока действия**.
3. Нажмите кнопку **Отсутствует** для функции **Включить срок действия для этих групп Office 365**.

**Выключение создания групп пользователями**

1. Выберите **Azure Active Directory** > **Группы** > **Общие**. 
2. Нажмите кнопку **Нет** для параметра **Пользователи могут создавать группы Office 365 на порталах Azure**.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о сроке действия, включая технические ограничения, добавление списка пользовательских запрещенных слов и взаимодействие с пользователем в приложениях Office 365, см. в следующей статье, которая содержит эти сведения о политике срока действия:

> [!div class="nextstepaction"]
> [Настройка политики срока действия для групп Office 365](groups-lifecycle.md)