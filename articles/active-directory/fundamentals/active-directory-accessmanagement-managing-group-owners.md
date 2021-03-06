---
title: Как добавить или удалить владельца группы Azure Active Directory | Документы Майкрософт
description: Узнайте, как добавить или удалить владельцев группы в Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: lizross
ms.custom: it-pro
ms.openlocfilehash: fae68bccbeaa54ca1bab9d77510fe6baecd11fcc
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139726"
---
# <a name="how-to-add-or-remove-group-owners-in-azure-active-directory"></a>Практическое руководство. Добавление или удаление владельцев группы в Azure Active Directory
Группы Azure Active Directory (Azure AD) управляются владельцами группы. Владельцы группы назначаются для управления группой и ее участниками владельцем ресурса (администратором). Владельцы группы не должны быть участниками группы. После назначения владельца группы только владелец ресурса можно добавить или удалить владельца.

В некоторых случаях администратор может не назначать владельца группы. В этом случае вы становитесь владельцем группы. Кроме того, владелец может назначать других владельцев группы, если это не запрещено в параметрах группы.

## <a name="add-an-owner-to-a-group"></a>Добавление владельца группы
Добавьте дополнительных владельцев группы в Azure AD.

### <a name="to-add-a-group-owner"></a>Добавление владельца группы
1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью глобального администратора каталога.

2. Выберите **Azure Active Directory**, затем **Группы** и выберите группу, для которой вы хотите добавить владельца (в этом примере — *Политика управления мобильными устройствами — Запад*).

3. На странице обзора группы **Политика управления мобильными устройствами — Запад** выберите **Владельцы**.

    ![Страница обзора группы "Политика управления мобильными устройствами — Запад" с выделенным параметром "Владельцы"](media/active-directory-accessmanagement-managing-group-owners/add-owners-option-overview-blade.png)

4. На странице **Политика управления мобильными устройствами — Запад — Владельцы** выберите **Добавить владельцев**, а затем найдите и выберите пользователя, который станет новым владельцем группы, и нажмите **Выбрать**.

    ![Страница "Политика управления мобильными устройствами — Запад — Владельцы" с выделенным параметром "Добавить владельцев"](media/active-directory-accessmanagement-managing-group-owners/add-owners-owners-blade.png)

    После выбора нового владельца обновите страницу **Владельцы** — в списке владельцев появится новое имя.

## <a name="remove-an-owner-from-a-group"></a>Удаление владельца группы
Удаление владельца группы в Azure AD.

### <a name="to-remove-an-owner"></a>Удаление владельца
1. Войдите на [портал Azure](https://portal.azure.com) с учетной записью глобального администратора каталога.

2. Выберите **Azure Active Directory**, затем **Группы** и выберите группу, для которой вы хотите удалить владельца (в этом примере — *Политика управления мобильными устройствами — Запад*).

3. На странице обзора группы **Политика управления мобильными устройствами — Запад** выберите **Владельцы**.

    ![Страница обзора группы "Политика управления мобильными устройствами — Запад" с выделенным параметром "Владельцы"](media/active-directory-accessmanagement-managing-group-owners/remove-owners-option-overview-blade.png)

4. На странице **Политика управления мобильными устройствами — Запад — Владельцы** выберите пользователя, которого необходимо удалить из владельцев группы. Нажмите **Удалить** на странице сведений о пользователе и выберите **Да** для подтверждения.

    ![Страница сведений о пользователе с выделенным параметром "Удалить"](media/active-directory-accessmanagement-managing-group-owners/remove-owner-info-blade.png)

    После удаления владельца вернитесь на страницу **Владельцы** — имя будет удалено из списка владельцев.

## <a name="next-steps"></a>Дополнительная информация
- [Управление доступом к ресурсам с помощью групп Azure Active Directory](active-directory-manage-groups.md)

- [Настройка параметров групп с помощью командлетов Azure Active Directory](../users-groups-roles/groups-settings-cmdlets.md)

- [Использование групп для назначения доступа к интегрированному приложению SaaS](../users-groups-roles/groups-saasapps.md)

- [Интеграция локальных удостоверений с Azure Active Directory](../hybrid/whatis-hybrid-identity.md)

- [Настройка параметров групп с помощью командлетов Azure Active Directory](../users-groups-roles/groups-settings-v2-cmdlets.md)
