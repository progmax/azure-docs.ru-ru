---
title: Автоматизация проверки Azure Stack с помощью PowerShell | Документация Майкрософт
description: Проверку Azure Stack можно автоматизировать с помощью PowerShell.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 3c3e064ea229db97c59cc5b49107b568a2fdaa98
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334454"
---
# <a name="automate-azure-stack-validation-with-powershell"></a>Автоматическая проверка Azure Stack с помощью PowerShell

Проверка как услуга (VaaS) предоставляет возможность автоматизировать запуск тестов с помощью скрипта **LaunchVaaSTests.ps1**.

PowerShell можно использовать для выполнения следующего рабочего процесса:

- Прохождение теста.

Из этого руководства вы узнаете, как создать скрипт, который выполняет следующие действия:

> [!div class="checklist"]
> * установка необходимых компонентов;
> * установка и запуск локального агента;
> * запуск категорий тестов, таких как интеграция, функциональность, надежность;
> * вывод результатов теста.

## <a name="launch-the-test-pass-workflow"></a>Запуск процесса прохождения теста

1. Откройте командную строку PowerShell с повышенными привилегиями.

2. Запустите следующий скрипт, чтобы скачать скрипт автоматизации.

    ```PowerShell
    New-Item -ItemType Directory -Path <VaaSLaunchDirectory>
    Set-Location <VaaSLaunchDirectory>
    Invoke-WebRequest -Uri https://storage.azurestackvalidation.com/packages/Microsoft.VaaS.Scripts.latest.nupkg -OutFile "LaunchVaaS.zip"
    Expand-Archive -Path ".\LaunchVaaS.zip" -DestinationPath .\ -Force
    ```

3. Выполните следующий скрипт, используя соответствующие значения параметров:

    ```PowerShell
    $VaaSAccountCreds = New-Object System.Management.Automation.PSCredential "<VaaSUserId>", (ConvertTo-SecureString "<VaaSUserPassword>" -AsPlainText -Force)
    .\LaunchVaaSTests.ps1 -VaaSAccountCreds $VaaSAccountCreds `
                          -VaaSAccountTenantId <VaaSAccountTenantId> `
                          -VaaSSolutionName <VaaSSolutionName> `
                          -VaaSTestPassName <VaaSTestPassName> `
                          -VaaSTestCategories Integration,Functional `
                          -MaxScriptWaitTimeInHours 12 `
                          -ServiceAdminUserName <AzSServiceAdminUser> `
                          -ServiceAdminUserPassword <AzSServiceAdminPassword> `
                          -TenantAdminUserName <AzSTenantAdminUser> `
                          -TenantAdminUserPassword <AzSTenantAdminPassword> `
                          -CloudAdminUserName <AzSCloudAdminUser> `
                          -CloudAdminUserPassword <AzSCloudAdminPassword>
    ```

    **Параметры**

    | Параметр | ОПИСАНИЕ |
    | --- | --- |
    | VaaSUserld | Идентификатор пользователя VaaS. |
    | VaaSUserPassword | Пароль VaaS. |
    | VaaSAccountTenantId | GUID клиента VaaS. |
    | VaaSSolutionName | Имя решения VaaS, в котором будет выполняться прохождение теста. |
    | VaaSTestPassName | Имя рабочего процесса прохождения теста VaaS, который необходимо создать. |
    | VaaSTestCategories | `Integration`, `Functional` или `Reliability`. Если вы используете несколько значений, разделите их запятой.  |
    | ServiceAdminUserName | Учетная запись администратора службы Azure Stack.  |
    | ServiceAdminPassword | Пароль службы Azure Stack.  |
    | TenantAdminUserName | Администратор основного клиента.  |
    | TenantAdminPassword | Пароль основного клиента.  |
    | CloudAdminUserName | Имя пользователя администратора облака.  |
    | CloudAdminPassword | Пароль администратора облака.  |

4. Просмотрите результаты теста. Другие параметры см. в статье [Мониторинг теста с помощью проверки как услуги Azure Stack](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Дополнительная информация

Чтобы узнать больше о PowerShell в Azure Stack, ознакомьтесь с последними версиями модулей.

> [!div class="nextstepaction"]
> [Модуль Azure Stack](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.5.0)
