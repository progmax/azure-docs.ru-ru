---
title: Изменение сведений о группе в Azure Active Directory | Документы Майкрософт
description: Узнайте, как изменить сведения о группе с помощью Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731348"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Практическое руководство. Изменение сведений о группе с помощью Azure Active Directory

С помощью Azure Active Directory можно изменить параметры группы, в том числе имя, описание или тип членства.

## <a name="to-edit-your-group-settings"></a>Изменение параметров группы
1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью глобального администратора каталога.

2. Выберите **Azure Active Directory**, а затем щелкните **Группы**.

    Откроется страница **Группы — Все группы** со списком активных групп.

3. На странице **Группы — Все группы** введите максимальную часть имени группы в поле **Поиск**. В этой статье мы будем искать группу **Политика управления мобильными устройствами — Запад**.

    Результаты поиска будут отображаться под полем **поиска** по мере ввода.

    ![Страница "Все группы" с заполненным полем поиска](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Выберите группу **Политика управления мобильными устройствами — Запад**, а затем выберите **Свойства** в разделе **Управление**.

    ![Страница общих сведений о группе с числом участников и выделенным параметром "Участник"](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Измените **Общие параметры** по необходимости, например:

    ![Параметры свойств для группы](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Имя группы.** Измените существующее имя группы.
    
    - **Описание группы.** Измените существующее описание группы.

    - **Тип группы.** Тип группы нельзя изменить после создания. Чтобы изменить **тип группы**, необходимо удалить группу и создать новую.
    
    - **Тип членства.** Измените тип членства. Дополнительные сведения о различных типах членства см. в разделе [Как создать простую группу и добавлять участников на портале Azure Active Directory](active-directory-groups-create-azure-portal.md)
    
    - **Идентификатор объекта.** Невозможно изменить идентификатор объекта, но можно скопировать его и использовать в командах PowerShell для группы. Дополнительные сведения об использовании командлетов PowerShell см. в статье [Настройка параметров групп с помощью командлетов Azure Active Directory](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Дополнительная информация
В следующих статьях содержатся дополнительные сведения об Azure Active Directory.

- [Просмотр групп и участников](active-directory-groups-view-azure-portal.md)

- [Создание простой группы и добавление участников](active-directory-groups-create-azure-portal.md)

- [Добавление или удаление участников из группы](active-directory-groups-members-azure-portal.md)

- [Управление динамическими правилами для пользователей в группе](../users-groups-roles/groups-create-rule.md)

- [Управление членством в группе](active-directory-groups-membership-azure-portal.md)

- [Управление доступом к ресурсам с помощью групп](active-directory-manage-groups.md)

- [Связывание подписки Azure с Azure Active Directory или добавление ее в службу](active-directory-how-subscriptions-associated-directory.md)
