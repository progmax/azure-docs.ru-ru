---
title: Предоставление разрешения на доступ к хранилищу ключей Azure нескольким приложениям | Документация Майкрософт
description: Узнайте, как нескольким приложениям предоставить разрешение на доступ к хранилищу ключей
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: ambapat
ms.openlocfilehash: 4ad6a18f9937fcc7d24bebc3ac197e23990ff59e
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309252"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Предоставление нескольким приложениям доступа к хранилищу ключей

Для предоставления нескольким приложениям доступа к хранилищу ключей можно использовать политики управления доступом. Политика управления доступом может поддерживать до 1024 приложений и настроена следующим образом:

1. Создайте группу безопасности Azure Active Directory. 
2. Добавьте все приложения, связанные с субъекты-службами в группу безопасности.
3. Предоставьте доступ к группе безопасности к вашему хранилищу ключей.

Необходимые компоненты для установки:
* [Установка модуля PowerShell V2 для Azure Active Directory](https://www.powershellgallery.com/packages/AzureAD).
* [Установка Azure PowerShell](/powershell/azure/overview).
* Чтобы выполнить следующие команды, необходимо получить разрешение на создание и изменение групп в клиенте Azure Active Directory. Если у вас нет разрешений, обратитесь к администратору Azure Active Directory. Ознакомиться со сведениями о разрешениях политики доступа Key Vault можно в статье [Сведения о ключах, секретах и сертификатах](about-keys-secrets-and-certificates.md).

Теперь в PowerShell выполните следующие команды.

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Если группе приложений необходимо предоставить другой набор разрешений, создайте для таких приложений отдельную группу безопасности Azure Active Directory.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения см. в статье [Защита хранилища ключей](key-vault-secure-your-key-vault.md).
