---
title: Добавление владельцев и пользователей в Azure DevTest Labs | Документация Майкрософт
description: Сведения о добавлении владельцев и пользователей в Azure DevTest Labs с помощью портала Azure или PowerShell
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: spelluru
ms.openlocfilehash: 558df3fa70989aaf9ba182df3a918994c7dc9db6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243715"
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Добавление владельцев и пользователей в Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Доступом к Azure DevTest Labs управляет [служба управления доступом на основе ролей Azure (RBAC)](../role-based-access-control/overview.md). RBAC позволяет распределить обязанности внутри команды c помощью *ролей* и предоставить пользователям доступ на уровне, который им необходим для выполнения поставленных задач. RBAC предусматривает такие три основные роли: *Владелец*, *Пользователь DevTest Labs* и *Участник*. В этой статье вы узнаете, какие действия могут выполнять пользователи с каждой из трех основных ролей RBAC. Здесь также приведены сведения о том, как добавлять пользователей в лабораторию (с использованием портала или сценария PowerShell) и как добавлять пользователей на уровне подписки.

## <a name="actions-that-can-be-performed-in-each-role"></a>Действия, которые можно выполнять в каждой роли
Пользователю можно назначить одну из трех основных ролей:

* Владелец.
* Пользователь DevTest Labs
* участник;

В следующей таблице представлены действия, которые могут выполнять пользователи с каждой из этих ролей.

| **Действия, которые могут выполнять пользователи с этой ролью** | **Пользователь DevTest Labs** | **Владелец** | **Участник** |
| --- | --- | --- | --- |
| **Задачи лаборатории** | | | |
| Добавление пользователей в лабораторию |Нет  |Yes |Нет  |
| Обновление параметров стоимости |Нет  |Yes |Yes |
| **Базовые задачи виртуальных машин** | | | |
| Добавление и удаление пользовательских образов |Нет  |Yes |Yes |
| Добавление, обновление и удаление формул |Yes |Да |Yes |
| Разрешенные образы Azure Marketplace |Нет  |Yes |Yes |
| **Задачи виртуальных машин** | | | |
| Создание виртуальных машин |Yes |Да |Yes |
| Запуск, остановка и удаление виртуальных машин |Только виртуальные машины, созданные пользователем |Yes |Yes |
| Обновление политик виртуальных машин |Нет  |Yes |Yes |
| Добавление и удаление дисков данных виртуальных машин |Только виртуальные машины, созданные пользователем |Yes |Yes |
| **Задачи артефактов** | | | |
| Добавление и удаление репозиториев артефактов |Нет  |Yes |Yes |
| Применение артефактов |Yes |Да |Yes |

> [!NOTE]
> Когда пользователь создает виртуальную машину, ему автоматически назначается роль **Владелец** для этой виртуальной машины.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Добавление владельца или пользователя на уровне лаборатории
Владельцев и пользователей можно добавлять на уровне лаборатории с помощью портала Azure. Можно добавить внешнего пользователя с допустимой [учетной записью Майкрософт (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
Ниже описана процедура добавления владельца или пользователя в лабораторию в Azure DevTest Labs.

1. Войдите на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Щелкните **Все службы** и выберите в списке **DevTest Labs**.
3. Из списка лабораторий выберите нужную лабораторию.
4. В колонке лаборатории выберите **Конфигурация и политики**. 
5. На странице **Configuration and Policies** (Конфигурация и политики) выберите **Управление доступом (IAM)** в меню слева. 
6. Щелкните **Добавить** на панели инструментов, чтобы добавить пользователя в роль.

    ![Добавление пользователя](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
1. На странице **Добавление разрешений** выполните следующие действия: 
    1. Выберите роль (например, пользователь DevTest Labs). В разделе [Действия, которые можно выполнять в каждой роли](#actions-that-can-be-performed-in-each-role) перечислены различные действия, которые могут выполнять пользователи с ролью владельца, пользователя DevTest или участника.
    2. Выберите пользователя для добавления в роль. 
    3. Щелкните **Сохранить**. 

        ![Добавление пользователя в роль](./media/devtest-lab-add-devtest-user/add-user.png) 
11. Вернувшись в колонку **Пользователи** , вы увидите, что пользователь добавлен.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>Добавление внешнего пользователя в лабораторию с помощью PowerShell
Помимо добавления пользователей на портале Azure, вы можете добавить внешнего пользователя в лабораторию с помощью сценария PowerShell. В следующем примере измените значения параметров под комментарием **Values to change** (Изменяемые значения).
Вы можете узнать значения `subscriptionId`, `labResourceGroup` и `labName` в колонке лаборатории на портале Azure.

> [!NOTE]
> В примере сценария предполагается, что указанный пользователь добавлен в Active Directory в качестве гостя. Если это не так, сценарий завершится ошибкой. Чтобы добавить в лабораторию пользователя, которого нет в Active Directory, используйте для назначения роли пользователю портал Azure, как показано в разделе [Добавление владельца или пользователя на уровне лаборатории](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Connect-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Добавление владельца или пользователя на уровне подписки
Разрешения Azure распространяются от родительской области к дочерней. Таким образом, владельцы подписки Azure, содержащей лаборатории, автоматически являются владельцами этих лабораторий. Они также являются владельцами виртуальных машин и других ресурсов, созданных пользователями лаборатории и службой Azure DevTest Labs. 

Дополнительных владельцев можно добавлять через колонку лаборатории на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040). Тем не менее область их администрирования будет более узкой, чем у владельцев подписки. У них, к примеру, не будет полного доступа к некоторым ресурсам, которые создаются в подписке службой DevTest Labs. 

Чтобы добавить владельца подписки Azure, сделайте следующее:

1. Войдите на [портале Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Щелкните **Все службы**, а затем в списке выберите **Подписки**.
3. Выберите нужную подписку.
4. Щелкните значок **Доступ** . 
   
    ![Доступ к пользователям](./media/devtest-lab-add-devtest-user/access-users.png)
5. Выберите **Добавить** в колонке **Пользователи**.
   
    ![Добавление пользователя](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. В колонке **Выберите роль** щелкните **Владелец**.
7. В колонке **Добавление пользователей** введите адрес электронной почты или имя пользователя, которому нужно назначить роль владельца. Если пользователь не найден, система выдаст соответствующее сообщение об ошибке. Если пользователь найден, он появится в текстовом поле **Пользователи** .
8. Выберите имя найденного пользователя.
9. Щелкните **Выбрать**.
10. Нажмите кнопку **ОК**, чтобы закрыть колонку **Добавить доступ**.
11. Вернувшись в колонку **Пользователи** , вы увидите, что пользователь добавлен как владелец. Теперь он является владельцем любых лабораторий, созданных в этой подписке, а значит может выполнять задачи пользователя. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

