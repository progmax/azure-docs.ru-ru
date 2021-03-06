---
title: Руководство. Получение данных, используя API отчетов Azure AD с сертификатами | Документы Майкрософт
description: В этом руководстве описывается использования API отчетов Azure AD с учетными данными сертификатов для получения данных из каталогов без вмешательства пользователя.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 0ee756828a50cdf62471923614afbe88e238b9ef
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624563"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Руководство. Получение данных с помощью API отчетов Azure Active Directory с сертификатами

[API-интерфейсы отчетов Azure Active Directory (Azure AD)](concept-reporting-api.md) предоставляют программный доступ к данным с помощью набора API-интерфейсов на базе REST. Эти интерфейсы API можно вызвать, используя различные языки и инструменты программирования. Чтобы получить доступ к API отчетов Azure AD без вмешательства пользователя, необходимо настроить доступ с помощью сертификатов.

Из этого руководства вы узнаете, как создать тестовый сертификат и использовать его для доступа к API Microsoft Graph для создания отчетов. Мы не рекомендуем использовать тестовые сертификаты в рабочей среде. 

## <a name="prerequisites"></a>Предварительные требования

1. Сначала выполните [Предварительные требования для доступа к API отчетов Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md). 

2. Скачайте и установите [Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

3. Установите [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Этот модуль предоставляет несколько служебных командлетов, позволяющих получить:
    - Библиотеки ADAL, необходимые для проверки подлинности
    - маркеры доступа пользователя, ключи приложений и сертификаты с использованием ADAL;
    - обработку результатов с разбивкой на страницы с помощью API Graph.

4. Если вы используете модуль впервые, выполните командлет **Install-MSCloudIdUtilsModule** или же импортируйте его с помощью команды PowerShell **Import-Module**. Сеанс должен выглядеть так, как показано ниже.

        ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
5. Используйте командлет Powershell **New-SelfSignedCertificate** для создания тестового сертификата.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

6. Используйте командлет **Export-Certificate**, чтобы экспортировать его в файл сертификата.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Получение данных с помощью API отчетов Azure Active Directory с сертификатами

1. Перейдите на [портал Azure](https://portal.azure.com), выберите **Azure Active Directory**, затем выберите **Регистрация приложений** и ваше приложение из списка. 

2. Выберите **Параметры** > **Ключи**, а затем **Отправить открытый ключ**.

3. Выберите файл сертификата из предыдущего шага и нажмите **Сохранить**. 

4. Запишите идентификатор приложения и отпечаток сертификата, который вы только что зарегистрировали для приложения. Чтобы найти отпечаток, на странице приложения на портале щелкните **Параметры** и выберите пункт **Ключи**. Отпечаток будет указан в списке **Открытые ключи**.

5. Откройте манифест приложения во встроенном редакторе манифеста и замените свойство *keyCredentials* данными нового сертификата, используя следующую схему. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Сохраните манифест. 
  
7. Теперь вы можете получить маркер доступа для API Microsoft Graph, используя этот сертификат. Используйте командлет **Get MSCloudIdMSGraphAccessTokenFromCert** из PowerShell-модуля MSCloudIdUtils, передав идентификатор приложения и отпечаток, полученный на предыдущем шаге. 

 ![Портал Azure](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

8. Используйте маркер доступа в скрипте PowerShell для отправки запроса в API Graph. Используйте командлет **Invoke-MSCloudIdMSGraphQuery** из модуля MSCloudIDUtils для перечисления операций входа и запроса конечной точки diectoryAudits. Этот командлет обрабатывает результаты с разбивкой на несколько страниц и отправляет их в конвейер PowerShell.

9. Запросите конечную точку directoryAudits для получения журналов аудита. 
 ![портал Azure](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

10. Запросите конечную точку signins для получения журналов входа в систему.
 ![портал Azure](./media/tutorial-access-api-with-certificates/query-signins.png)

11. Теперь можно экспортировать эти данные в CSV-файл и сохранить его в системе SIEM. Также можно перенести скрипт в запланированную задачу, чтобы периодически получать данные Azure AD из клиента без необходимости сохранять ключи приложений в исходном коде. 

## <a name="next-steps"></a>Дополнительная информация

* [Ознакомление с API отчетов](concept-reporting-api.md)
* [Справочник по API аудита](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Справочник по API отчетов о действиях при входе](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
