---
title: Добавление или изменение ролей подписки администратора Azure | Документация Майкрософт
description: Инструкции по добавлению или изменению соадминистратора Azure, администратора службы или администратора учетной записи
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: cwatson
ms.openlocfilehash: 2380cd3712c47ca08e9b9b3597f09f4119238af3
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581619"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Добавление или изменение администраторов подписки Azure

Чтобы управлять доступом к ресурсам Azure, требуется соответствующая роль администратора. В этой статье описано, как добавлять или изменять роли администратора для пользователя на уровне подписки.

> [!div class="nextstepaction"]
> [Помогите нам улучшить документацию по управлению счетами и подписками в Azure](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>Какую роль администратора следует использовать?

Azure предлагает несколько разных ролей. Для управления доступом к ресурсам, можно использовать классические роли администратора подписки, такие как администратор службы и соадминистратор, или новую систему авторизации, которая называется управлением доступом на основе ролей (RBAC). Для улучшения контроля и упрощения управления доступом для всех задач по управлению доступом мы рекомендуем использовать RBAC. Если это возможно, рекомендуется перенастроить существующие политики доступа для использования RBAC. См. дополнительные сведения об [управлении доступом на основе ролей (RBAC)](../role-based-access-control/overview.md) и [различии между ролями в Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Добавление владельца RBAC для подписки на портале Azure 

Чтобы добавить пользователя как администратора для подписки Azure, назначьте ему роль RBAC [Владелец](../role-based-access-control/built-in-roles.md#owner) в этой подписке. Роль владельца может управлять ресурсами назначенной подписки, но не имеет доступа к другим подпискам.

1. Откройте раздел [**Подписки** на портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Выберите подписку, которой нужно предоставить доступ.
3. В списке выберите **Управление доступом (IAM)**.
4. Выберите **Добавить**.
   (Если кнопка "Добавить" отсутствует, это значит, что у вас нет прав на добавление разрешений.)
5. В поле **Роль** выберите значение **Владелец**. 
6. В поле **Назначение доступа** выберите значение **Пользователь, группа или приложение Azure AD**. 
7. В поле **Выбрать** укажите адрес электронной почты пользователя, для которого хотите добавить роль "Владелец". Выберите пользователя, а затем щелкните **Сохранить**.

    ![Снимок экрана, на котором показана выбранная роль "Владелец"](./media/billing-add-change-azure-subscription-administrator/add-role.png)

После этого пользователь получит полный доступ ко всем ресурсам, включая право делегировать доступ другим пользователям. Чтобы изменить область доступа, например группу ресурсов, откройте колонку **Управление доступом (IAM)** для этой области.

## <a name="add-or-change-co-administrator"></a>Добавление или изменение соадминистратора

В качестве соадминистратора можно добавить только [владельца](../role-based-access-control/built-in-roles.md#owner). Пользователей с такими ролями, как [Участник](../role-based-access-control/built-in-roles.md#contributor) и [Читатель](../role-based-access-control/built-in-roles.md#reader), добавить в качестве соадминистратора нельзя.

> [!TIP]
> Если пользователю нужно управлять классическими развертываниями Azure, просто добавьте учетную запись владельца в качестве соадминистратора. Для всех других целей мы рекомендуем использовать RBAC.

1. Если владелец еще не назначен, сделайте это, следуя инструкциям выше.
2. **Щелкните правой кнопкой мыши** только что добавленного владельца, а затем выберите **Добавить соадминистратора**. Если вы не видите параметр **Добавить соадминистратора**, обновите страницу или попробуйте использовать другой браузер. 

    ![Снимок экрана, на котором показаны элементы для добавления соадминистратора](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Чтобы удалить разрешения соадминистратора, **щелкните правой кнопкой мыши** соадминистратора, а затем выберите **Удалить соадминистратора**.

    ![Снимок экрана, на котором показаны элементы для удаления соадминистратора](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Добавление гостевого пользователя с правами соадминистратора

Гостевые пользователи, которым была назначена роль соадминистратора, могут видеть некоторые различия, в отличие от пользователей-участников с той же ролью. Рассмотрим следующие сценарии.

- Пользователь A с рабочей или учебной учетной записью Azure AD является администратором службы для подписки Azure.
- Пользователь B имеет учетную запись Майкрософт.
- Пользователь А присваивает роль соадминистратора пользователю B.
- Пользователь B может выполнять практически любые действия, кроме регистрации приложения или поиска пользователей в каталоге Azure AD.

Можно ожидать, что пользователь В будет управлять всем. Это различие связано с тем, что учетная запись Майкрософт добавляется к подписке в качестве гостевого пользователя вместо пользователя-участника. По сравнению с пользователями-участниками, гостевые пользователи имеют разные разрешения в Azure AD по умолчанию. Например, пользователи-участники могут считывать других пользователей в Azure AD, а гостевые пользователи — не могут. Пользователи-участники могут регистрировать новые субъекты-службы в Azure AD, а гостевые пользователи — не могут. Если гостевому пользователю необходимо выполнить эти задачи, возможным решением будет назначить требуемые конкретные роли администратора Azure AD. Например, в приведенном выше сценарии можно было назначить роли [Читатели каталогов](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) считывать других пользователей, а роли [Разработчик приложения](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) — создавать субъекты-службы. Дополнительные сведения о гостевых пользователях и пользователях-участниках, а также об их разрешениях см. в статье [Разрешения пользователя по умолчанию в Azure Active Directory](../active-directory/fundamentals/users-default-permissions.md).

Обратите внимание, что [встроенные роли ресурсов Azure](../role-based-access-control/built-in-roles.md) отличаются от [ролей администратора Azure AD](../active-directory/users-groups-roles/directory-assign-admin-roles.md). Встроенные роли не предоставляют доступ к Azure AD. Дополнительные сведения см. в статье [Роли классического администратора подписки, роли RBAC Azure и роли администратора Azure AD](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Изменение администратора служб для подписки Azure

Только администратор учетной записи может изменить администратора службы для подписки. По умолчанию для новой подписки администратор учетных записей является также администратором служб. Если роль администратора службы назначить другому пользователю, администратор учетных записей потеряет доступ к порталу Azure. Тем не менее администратор учетных записей всегда может использовать Центр управления учетной записью, чтобы назначить себя администратором служб.

1. Убедитесь, что сценарий поддерживается, просмотрев [ограничения на изменение роли администратора служб](#limits).
1. Войдите в [Центр управления учетной записью](https://account.windowsazure.com/subscriptions) в качестве администратора учетной записи.
1. Выберите подписку.
1. В правой части выберите **Изменить сведения о подписке**.

    ![Снимок экрана, отображающий кнопку "Изменить подписку" в Центре управления учетной записью](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. В поле **Администратор службы** введите адрес электронной почты нового администратора службы.

    ![Снимок экрана, отображающий поле для изменения электронного адреса администратора служб](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Ограничения на изменение роли администратора служб

* Каждая подписка связана с каталогом Azure AD. Чтобы найти каталог, с которым связана подписка, перейдите в раздел [**Подписки**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), а затем выберите подписку и просмотрите каталог.
* Если вы выполняете вход с использованием рабочей или учебной учетной записи, вы можете добавлять другие учетные записи в своей организации в качестве администратора служб. Например, abby@contoso.com может добавить bob@contoso.com в качестве администратора служб, но он не может добавить john@notcontoso.com, если john@notcontoso.com не находится в каталоге contoso.com. Пользователи, выполнившие вход с использованием рабочей или учебной учетной записи, могут и дальше добавлять пользователей учетной записи Майкрософт в качестве администратора служб.

  | Метод входа | Возможность добавления пользователя учетной записи Майкрософт в качестве администратора служб | Возможность добавления рабочей или учебной учетной записи в той же организации, к которой относится администратор служб | Возможность добавления рабочей или учебной учетной записи в организации, к которой не относится администратор служб |
  | --- | --- | --- | --- |
  |  Учетная запись Майкрософт |Yes |Нет  |Нет  |
  |  Рабочая или учебная учетная запись |Yes |Да |Нет  |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Изменение администратора учетной записи для подписки Azure

Администратор учетной записи — это пользователь, который изначально зарегистрирован для подписки Azure как владелец подписки, которому выставляются счета. Сведения о том, как изменить администратора учетной записи для подписки, см. в статье [Передача прав владения подпиской Azure другой учетной записи](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Не знаете, кто является администратором учетной записи?** Выполните следующие действия.

1. Откройте раздел [**Подписки** на портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Выберите подписку, которую требуется проверить, а затем просмотрите раздел **Параметры**.
1. Выберите **Свойства**. Администратор учетной записи подписки отобразится в поле **Администратор учетной записи**.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Дополнительные сведения о контроле доступа к ресурсам и Active Directory

* См. дополнительные сведения об [управлении доступом на основе ролей (RBAC) на портале Azure](../role-based-access-control/overview.md).
* См. дополнительные сведения о [разных ролях в Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Дополнительные сведения об Azure Active Directory см. в статьях [Связь между подписками Azure и службой Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) и [Назначение ролей администратора в Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создайте запрос в службу поддержки](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).