---
title: Проверка удостоверений Azure для Azure Stack | Документация Майкрософт
description: Проверки удостоверений Azure с помощью средства Azure Stack для проверки готовности к работе.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/04/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 61562450d484f34385b4e6e111bf62326eaca159
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888381"
---
# <a name="validate-azure-identity"></a>Проверка удостоверения Azure 
Средство проверки готовности Azure Stack (AzsReadinessChecker) позволяет убедиться, что ваша служба Azure Active Directory (Azure AD) готова к работе с Azure Stack. Прежде чем развертывать Azure Stack, проверьте решение для работы с удостоверениями Azure.  

Средство проверки готовности позволяет определить:
 - является ли Azure Active Directory (Azure AD) поставщиком удостоверений для Azure Stack;
 - предоставляет ли учетная запись Azure AD, которую вы намерены использовать, права глобального администратора в службе Azure Active Directory. 

Проверка гарантирует, что настроенное окружение обеспечивает все необходимое для того, чтобы инфраструктура Azure Stack сохраняла в Azure AD сведения о пользователях, приложениях, группах и субъектах-службах.

## <a name="get-the-readiness-checker-tool"></a>Получение средства проверки готовности
Скачайте последнюю версию средства проверки готовности Azure Stack (AzsReadinessChecker) из коллекции [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Предварительные требования
Убедитесь, что выполнены указанные ниже предварительные требования.

**На компьютере, где запускается это средство:**
 - Необходимо установить Windows 10 или Windows Server 2016 и обеспечить подключение к Интернету.
 - Необходимо установить PowerShell 5.1 или более поздней версии. Чтобы проверить используемую версию, запустите следующий командлет PowerShell и проверьте значения *Major* (основной номер версии) и *Minor* (дополнительный номер версии):  

   > `$PSVersionTable.PSVersion`
 - Необходимо настроить [PowerShell для использования с Azure Stack](azure-stack-powershell-install.md). 
 - Необходимо установить последнюю версию средства [ проверки готовности Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker).

**В среде Azure Active Directory:**
 - Определите учетную запись Azure AD, которую вы намерены использовать для Azure Stack и убедитесь, что она предоставляет права глобального администратора Azure Active Directory.
 - Определите имя клиента Azure AD. Это имя должно совпадать с *основным* доменным именем в Azure Active Directory. Например, *contoso.onmicrosoft.com*. 
 - Задайте параметр AzureEnvironement, который будет использоваться. Поддерживаемые значения имени параметра среды: AzureCloud, AzureChinaCloud или AzureUSGovernment в зависимости от используемой подписки Azure.

## <a name="validate-azure-identity"></a>Проверка удостоверения Azure 
1. На компьютере, который соответствует всем предварительным требованиям, откройте командную строку PowerShell с правами администратора и выполните следующую команду, чтобы установить AzsReadinessChecker:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. В командной строке PowerShell выполните приведенную ниже команду, чтобы назначить *$serviceAdminCredential* администратором службы для клиента Azure AD.  Замените *serviceadmin@contoso.onmicrosoft.com* именами учетной записи и клиента, соответственно. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. В командной строке PowerShell выполните приведенную ниже команду, чтобы начать проверку Azure AD. 
   - Укажите значение имени среды для AzureEnvironment. Поддерживаемые значения имени параметра среды: AzureCloud, AzureChinaCloud или AzureUSGovernment в зависимости от используемой подписки Azure.  
   - Задайте правильное имя клиента Azure Active Directory вместо *contoso.onmicrosoft.com*. 

   > `Invoke-AzsAzureIdentityValidation -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment <environment name> -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Когда средство завершит работу, просмотрите выходные данные. Убедитесь, что система соответствует требованиям для установки (**OK**). При успешном завершении проверки отобразится следующий результат: 
 
````PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: OK

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
````


## <a name="report-and-log-file"></a>Файлы отчета и журнала
При каждом запуске проверки все результаты сохраняются в файлах **AzsReadinessChecker.log** и **AzsReadinessCheckerReport.json**. Расположение этих файлов указывается в PowerShell вместе с результатами проверки.

Эти файлы помогут передать сведения о состоянии проверки другим заинтересованным лицам перед развертыванием Azure Stack или для исследования проблем, обнаруженных при проверке.  В обоих файлах сохраняются результаты каждой очередной проверки. В отчете содержатся подтверждения команды развертывания по конфигурации удостоверений. Файл журнала поможет командам развертывания или поддержки диагностировать проблемы с проверкой. 

По умолчанию оба файла сохраняются в расположении *C:\Users\<имя_пользователя>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Чтобы задать другое расположение отчетов, при запуске проверки можно указать в конце командной строки параметр **-OutputPath** ***&lt;путь&gt;***.   
 - Укажите параметр **-CleanReport** в конце команды, чтобы удалить из файла *AzsReadinessCheckerReport.json* сведения.  о предыдущих запусках средства. 

Дополнительные сведения об отчетах проверки Azure Stack можно найти [здесь](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Ошибки при проверке
Если проверка завершается ошибкой, сведения о сбое отображаются в окне PowerShell. Кроме того, сведения записываются в файл AzsReadinessChecker.log.

В следующих примерах приведены некоторые рекомендации по устранению распространенных ошибок проверки.

### <a name="expired-or-temporary-password"></a>Пароль с истекшим сроком действия или временный пароль 
 
````PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The password for account  has expired or is a temporary password that needs to be reset before continuing. Run Login-AzureRMAccount, login with  credentials and follow the prompts to reset.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
````
**Причина** — вход с помощью данной учетной записи невозможен, так как истек срок действия пароля или используется временный пароль.     

**Решение** — выполните в PowerShell приведенную ниже команду и следуйте инструкциям на экране, чтобы сбросить пароль.  
> `Login-AzureRMAccount`

Также вы можете войти с этой учетной записью в https://portal.azure.com. В таком случае пользователь должен будет сменить пароль.
### <a name="unknown-user-type"></a>Неизвестный тип пользователя 
 
````PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
Unknown user type detected. Check the account  is valid for AzureChinaCloud
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
````
**Причина** — вход в указанный каталог Azure Active Directory (AADDirectoryTenantName) с помощью данной учетной записи невозможен. В нашем примере параметр *AzureChinaCloud* имеет значение *AzureEnvironment*.

**Решение** — убедитесь, что учетная запись существует в указанном окружении Azure. Выполните в PowerShell следующую команду, чтобы проверить допустимость учетной записи для конкретного окружения: Login-AzureRmAccount – EnvironmentName AzureChinaCloud. 
### <a name="account-is-not-an-administrator"></a>Учетная запись не предоставляет права администратора 
 
````PowerShell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail 
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The Service Admin account you entered 'admin@contoso.onmicrosoft.com' is not an administrator of the Azure Active Directory tenant 'contoso.onmicrosoft.com'.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
````

**Причина** — учетная запись позволяет успешно войти в систему, но не предоставляет права администратора в Azure Active Directory (AADDirectoryTenantName).  

**Решение** — войдите с этой учетной записью в https://portal.azure.com, поочередно откройте элементы **Azure Active Directory** > **Пользователи** > *Выбор пользователя* > **Роль каталога**. Пользователь здесь должен быть обозначен как **Глобальный администратор**.  Если учетная запись предоставляет права пользователя, последовательно выберите **Azure Active Directory** > **Пользовательский домен** и убедитесь, что указанное в качестве *AADDirectoryTenantName* имя домена здесь отмечено как основное имя домена для каталога.  В нашем примере это *contoso.onmicrosoft.com*. 

В Azure Stack требуется, чтобы доменное имя являлось основным.

## <a name="next-steps"></a>Дальнейшие действия
[Validate Azure registration](azure-stack-validate-registration.md) (Проверка регистрации в Azure)  
[Просмотр отчета о готовности](azure-stack-validation-report.md)  
[Общие рекомендации по интеграции Azure Stack](azure-stack-datacenter-integration.md)  

