---
title: Краткое руководство. Использование портала Azure для просмотра ролей, назначенных пользователю | Документация Майкрософт
description: Узнайте, как с помощью портала Azure просматривать управление доступом на основе ролей, назначенных пользователю, группе, субъект-службе или управляемому удостоверению.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: b755dd6223c21084cafea82a1c8857f9f54b03b5
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641871"
---
# <a name="quickstart-view-roles-assigned-to-a-user-using-the-azure-portal"></a>Краткое руководство. Использование портала Azure для просмотра ролей, назначенных пользователю

Колонку **Управление доступом (IAM)** в [Управление доступом на основе ролей (RBAC)](overview.md) можно использовать для просмотра назначений ролей для нескольких пользователей, групп, субъектов-служб и управляемых удостоверений, но иногда нужно лишь быстро просмотреть назначения ролей для одного пользователя, группы, субъекта-службы или управляемого удостоверения. Для этого лучше всего использовать функцию **Проверка доступа** на портале Azure.

## <a name="view-role-assignments"></a>Просмотр назначений ролей

Выполните следующие действия, чтобы просмотреть права доступа для одного пользователя, группы, субъекта-службы или управляемого удостоверения в области подписки.

1. На портале Azure щелкните **Все службы**, а затем **Подписки**.

1. Щелкните на подписку.

1. Щелкните **Управление доступом (IAM)**.

1. Щелкните вкладку **Проверить доступ**.

    ![Управление доступом — вкладка "Проверить доступ"](./media/check-access/access-control-check-access.png)

1. В списке **Найти** выберите тип субъекта безопасности, для которого нужно проверить доступ.

1. В поле поиска введите строку для поиска в каталоге по отображаемым именам, адресам электронной почты или идентификаторам объектов.

    ![Проверка доступа — выбор списка](./media/check-access/check-access-select.png)

1. Выберите субъект безопасности, чтобы открыть область **Назначения**.

    ![Область назначений](./media/check-access/check-access-assignments.png)

    В этой области можно увидеть роли, которые назначены выбранному субъекту безопасности для выбранной области. Здесь будут перечислены все запрещающие назначения, которые определены или наследуются в выбранной области.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Руководство. Предоставление доступа пользователю с помощью RBAC и портала Azure](quickstart-assign-role-user-portal.md)
