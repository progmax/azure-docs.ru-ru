---
title: Как добавить или удалить участников группы с помощью Azure Active Directory | Документы Майкрософт
description: Узнайте, как добавлять или удалять пользователей и устройства, которые входят в группу в Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 7c1a83d83dcbf247550c66602a6f53d4ef0d7930
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733370"
---
# <a name="how-to-add-or-remove-group-members-using-azure-active-directory"></a>Практическое руководство. Как добавить или удалить участников группы с помощью Azure Active Directory
С помощью Azure Active Directory можно добавлять и удалять участников группы.

## <a name="to-add-group-members"></a>Добавление участников группы

1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью глобального администратора каталога.

2. Выберите **Azure Active Directory**, а затем щелкните **Группы**.

3. На странице **Группы — Все группы** найдите и выберите группу, в которую вы хотите добавить участника. В этом случае используйте ранее созданную группу **Политика управления мобильными устройствами — Запад**.

    ![Страница "Группы — Все группы" с выделенным именем группы](media/active-directory-groups-members-azure-portal/group-all-groups-screen.png)

4. На странице **Обзор политики управления мобильными устройствами — Запад** выберите **Члены** из области **Управление**.

    ![Страница обзора группы "Политика управления мобильными устройствами — Запад" с выделенным параметром "Участники"](media/active-directory-groups-members-azure-portal/group-overview-blade.png)

5. Выберите **Добавить участников**, а затем найдите и выберите всех участников, которых вы хотите добавить в группу, и нажмите **Выбрать**.

    Вы получите сообщение о том, что участники успешно добавлены.

    ![Страница "Добавить участников" с поиском участников](media/active-directory-groups-members-azure-portal/update-members.png)

6. Обновите экран для просмотра имен всех добавленных в группу частников.

## <a name="to-remove-group-members"></a>Удаление участников группы

1. На странице **Группы — Все группы** найдите и выберите группу, из которой вы хотите удалить участника. Мы снова выберем группу **Политика управления мобильными устройствами — Запад**.

2. Выберите **Участники** в области **Управление**, найдите и выберите имя участника, которого требуется удалить, и нажмите **Удалить**.

    ![Страница сведений об участниках с параметром "Удалить"](media/active-directory-groups-members-azure-portal/remove-members-from-group.png)

## <a name="next-steps"></a>Дополнительная информация

- [Просмотр групп и участников](active-directory-groups-view-azure-portal.md)

- [Изменение параметров группы](active-directory-groups-settings-azure-portal.md)

- [Управление доступом к ресурсам с помощью групп](active-directory-manage-groups.md)

- [Управление динамическими правилами для пользователей в группе](../users-groups-roles/groups-create-rule.md)

- [Связывание подписки Azure с Azure Active Directory или добавление ее в службу](active-directory-how-subscriptions-associated-directory.md)
