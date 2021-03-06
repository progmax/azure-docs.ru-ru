---
ms.assetid: ''
title: Обратимое удаление в Azure Key Vault | Документация Майкрософт
ms.service: key-vault
ms.topic: conceptual
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 09/25/2017
ms.openlocfilehash: 12b14b87a02619b21e80436c80a284c4011f8b33
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300325"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Общие сведения об обратимом удалении в Azure Key Vault

Функция обратимого удаления в Key Vault позволяет восстанавливать удаленные хранилища и объекты хранилищ. В частности, рассматриваются следующие сценарии:

- Поддержка восстанавливаемого удаления хранилища ключей
- Поддержка восстанавливаемого удаления объектов Key Vault (например, ключей, секретов, сертификатов).

## <a name="supporting-interfaces"></a>Поддержка интерфейсов

Функция обратимого удаления изначально доступна через интерфейсы REST, .NET/C#, PowerShell и CLI.

Дополнительные сведения см. в [документации по Key Vault](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Сценарии

Azure Key Vault — это отслеживаемые ресурсы, управляемые Azure Resource Manager. Azure Resource Manager также задает четко определенное поведение для удаления, которое требует, чтобы после успешной операции удаления этот ресурс больше не был доступен. Функция обратимого восстановления восстанавливает удаленный объект независимо от того, было ли удаление случайным или преднамеренным.

1. В типичном сценарии пользователь может случайно удалить Key Vault или его объект. Если они будут доступны для восстановления в течение предопределенного периода, пользователь сможет отменить удаление и восстановить свои данные.

2. В другом сценарии несанкционированный пользователь может попытаться удалить Key Vault или его объект, например ключ в хранилище, чтобы нарушить работу компании. Разделение удаления Key Vault или его объектов и удаления базовых данных может использоваться как мера предосторожности, к примеру, для ограничения разрешений на удаление данных другой доверенной ролью. По существу, при таком подходе требуется кворум для операции, так как в противном случае может произойти немедленная потеря данных.

### <a name="soft-delete-behavior"></a>Поведение обратимого удаления

При использовании этой функции операция DELETE для хранилища ключей или его объекта является обратимым удалением. Это позволяет в течение определенного периода (90 дней) сохранять ресурсы, которые при этом как будто удалены. Затем эта служба предоставляет механизм восстановления удаленного объекта, отменяющий удаление. 

Обратимое удаление — это дополнительная функция Key Vault, которая **не включена по умолчанию** в этом выпуске. 

### <a name="purge-protection--flag"></a>Параметр защиты от удаления
Параметр защиты от удаления (**--enable-purge-protection** в Azure CLI) отключен по умолчанию. Если этот параметр включен, хранилище или его объект в удаленном состоянии не могут быть удалены до истечения срока в 90 дней. Такое хранилище или его объект все еще подлежат восстановлению. Этот параметр обеспечивает для клиентов более высокий уровень защиты, гарантируя, что хранилище или его объект не могут быть необратимо удалены до истечения срока хранения. Вы можете включить параметр защиты от удаления только в том случае, если включен параметр обратимого удаления. Кроме того, во время создания хранилища вы можете включить и обратимое удаление, и защиту от удаления.

> [!NOTE] 
   Обязательное предварительное условие для включения защиты от удаления — включенный параметр обратимого удаления.
В Azure CLI 2 его можно включить с помощью команды:

```
az keyvault create --name "VaultName" --resource-group "ResourceGroupName" --location westus --enable-soft-delete true --enable-purge-protection true
```

### <a name="permitted-purge"></a>Разрешенная очистка

Окончательное удаление и очистку хранилища ключей можно выполнить с помощью операции POST на прокси-ресурсе. Это требует специальных привилегий. Как правило, только владелец подписки может очистить хранилище ключей. Операция POST активирует немедленное и необратимое удаление этого хранилища. 

Исключения составляют:
- Случай когда подписка Azure была помечена как *неудаляемая*. В этом случае только служба может выполнить фактическое удаление в рамках запланированной процедуры. 
- Когда параметр --enable-purge-protection включен в самом хранилище. В этом случае Key Vault будет ожидать 90 дней с момента, когда исходный объект секрета был помечен для удаления, прежде чем окончательно удалить объект.

### <a name="key-vault-recovery"></a>Восстановление Key Vault

Когда хранилище ключей будет удалено, служба создаст прокси-ресурс в рамках подписки, добавив достаточное количество метаданных для восстановления. Прокси-ресурс — это хранимый объект, доступный в том же месте, что и удаленное Key Vault. 

### <a name="key-vault-object-recovery"></a>Восстановление объектов Key Vault

После удаления объекта Key Vault, например ключа, служба переведет его в удаленное состояние, делая его недоступным для любой операции извлечения. В этом состоянии объект хранилища ключей можно только внести в список, восстановить, а также принудительно или окончательно удалить. 

В то же время Key Vault запланирует удаление базовых данных удаленного Key Vault или его объекта, чтобы удалить их по истечении предопределенного интервала хранения. Запись DNS, соответствующая хранилищу, также сохраняется в течение этого интервала.

### <a name="soft-delete-retention-period"></a>Период хранения при обратимом удалении

Ресурсы обратимого удаления сохраняются в течение заданного периода времени — 90 дней. В течение интервала хранения применяются следующие условия:

- Вы можете вести список всех Key Vault и их объектов в состоянии обратимого удаления для вашей подписки, а также просматривать сведения об их удалении и восстановлении.
    - Только пользователи со специальными разрешениями могут вести список удаленных хранилищ. Мы советуем нашим пользователям создать настраиваемую роль с этими разрешениями для управления удаленными хранилищами.
- Хранилище ключей с тем же именем не может быть создано в одном и том же месте. Соответственно, объект хранилища ключей не может быть создан в данном хранилище, если оно содержит объект с тем же именем и находится в удаленном состоянии. 
- Только пользователь со специальными привилегиями может восстановить хранилище ключей или его объект, выполнив команду восстановления на соответствующем прокси-ресурсе.
    - Пользователь, участник настраиваемой роли, у которого есть привилегии создавать хранилище ключей в группе ресурсов, может восстановить хранилище.
- Только пользователь со специальными привилегиями может принудительно удалить хранилище ключей или его объект, выполнив команду удаления на соответствующем прокси-ресурсе.

Если хранилище ключей или его объект не будут восстановлены, служба выполнит очистку обратимо удаленного хранилища ключей или его объекта вместе с содержимым в конце периода хранения. Удаление ресурса нельзя запланировать повторно.

### <a name="billing-implications"></a>Процесс выставления счетов

В общем случае, когда объект (хранилище ключей, ключ или секрета) находится в состоянии удаления, возможны только две операции: очистка и восстановление. Все остальные операции завершатся ошибкой. Таким образом, хотя объект существует, операции выполнять нельзя, следовательно счета за использование не выставляются. Но есть и следующие исключения.

- Действия очистки и удаления будут считаться обычными операциями хранилища ключей, следовательно, они будут тарифицироваться.
- Если объект является ключом HSM, ежемесячная плата за версию каждого ключа с защитой HSM будет взиматься, если версия ключа использовалась в течение последних 30 дней. После этого, так как объект находится в состоянии удаления, операции с ним невозможны, плата не будет взиматься.

## <a name="next-steps"></a>Дополнительная информация

Следующие два руководства содержат основные сценарии использования обратимого удаления.

- [Как использовать обратимое удаление в Key Vault с помощью PowerShell](key-vault-soft-delete-powershell.md) 
- [Как использовать обратимое удаление в Key Vault с помощью CLI](key-vault-soft-delete-cli.md)

