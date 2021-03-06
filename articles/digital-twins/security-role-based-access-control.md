---
title: Сведения об управлении доступом к Azure Digital Twins на основе ролей | Документация Майкрософт
description: Сведения об аутентификации в службе Digital Twins с управлением доступом на основе ролей.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014794"
---
# <a name="role-based-access-control"></a>Контроль доступа на основе ролей

Служба Azure Digital Twins предоставляет детализированное управление доступом для определенных данных, ресурсов и действий в пространственном графе. Эти функции предоставляются с помощью детализированных возможностей управления ролями и доступом, которые называются управление доступом на основе ролей (RBAC). RBAC состоит с _ролей_ и _назначений ролей_. Роли определяют уровень разрешений. Назначения ролей сопоставляет роль с пользователем или устройством.

Используя RBAC, разрешения может предоставляться:

- пользователю;
- устройству;
- субъект-службе;
- определяемой пользователем функции; 
- всем пользователям, которые являются частью домена; 
- клиенту.
 
Уровень доступности может быть точно настроенным.

RBAC — уникальное, так как позволяет наследовать разрешения по пространственному графу.

## <a name="what-can-i-do-with-rbac"></a>Что можно сделать с помощью RBAC?

RBAC может использоваться разработчиком для целей, приведенных ниже.

* Предоставление пользователю возможности управления устройствами во всем здании, или только в определенной комнате, или на указанном этаже.
* Предоставление администратору глобального доступа ко всем узлам пространственного графа (всего графа или только определенного его раздела).
* Предоставление специалисту службы поддержки доступа на чтение к графу, за исключением ключей доступа.
* Предоставление всем участникам домена доступа на чтение для всех объектов графа.

## <a name="rbac-best-practices"></a>Рекомендации по использованию RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Роли

### <a name="role-definitions"></a>Определения ролей

Определение роли представляет собой коллекцию разрешений (это иногда называется ролью). Определение роли содержит список разрешенных операций, среди них: создание, чтение, обновление и удаление. Это определение также указывает, к каким типам объектов применяются данные разрешения.

В службе Azure Digital Twins доступны следующие роли:

* **Администратор пространства.** Разрешение на создание, чтение, обновление и удаление для указанного пространства и всех его узлов. Глобальное разрешение.
* **Администратор пользователей.** Разрешение на создание, чтение, обновление и удаление для пользователей и связанных с ними объектов. Разрешение на чтение для пространств.
* **Администратор устройств.** Разрешение на создание, чтение, обновление и удаление для устройств и связанных с ними объектов. Разрешение на чтение для пространств.
* **Администратор ключей.** Разрешение на создание, чтение, обновление и удаление для ключей доступа. Разрешение на чтение для пространств.
* **Администратор маркеров.** Разрешение на чтение и обновление для ключей доступа. Разрешение на чтение для пространств.
* **Пользователь.** Разрешение на чтение для пространств, датчиков и пользователей, которое включает соответствующие связанные объекты.
* **Специалист службы поддержки.** Разрешение на чтение для всех элементов, кроме ключей доступа.
* **Установщик устройств.** Разрешение на чтение и обновление для устройств и датчиков, которое включает соответствующие связанные объекты. Разрешение на чтение для пространств.
* **Устройство шлюза.** Разрешение на создание для датчиков. Разрешение на чтение для устройств и датчиков, которое включает соответствующие связанные объекты.

>[!NOTE]
> Чтобы получить полные определения для вышеупомянутых ролей, запросите системы или роли API.

### <a name="object-types"></a>Типы объектов

Элемент `ObjectIdType` указывает на тип идентификатора, предоставленного для роли. Помимо типов `DeviceId` и `UserDefinedFunctionId`, типы соответствуют свойству объекта Azure Active Directory (Azure AD):
  
* Тип `UserId` назначает роли для пользователя.
* Тип `DeviceId` назначает роли для устройства.
* Тип `DomainName` назначает роли для доменного имени. Каждому пользователю с указанным именем домена предоставлены права на доступ к соответствующей роли.
* Тип `TenantId` назначает роли для клиента. Каждому пользователю, принадлежащему к указанному идентификатору клиента Azure Active Directory, будут предоставлены права на доступ к соответствующей роли.
* Тип `ServicePrincipalId` назначает роль для идентификатора объекта субъекта-службы.
* Тип `UserDefinedFunctionId` назначает роль для определяемой пользователем функции (UDF).

> [!div class="nextstepaction"]
> [Get-AzureADUser](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Get-AzureRmADServicePrincipal](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Краткое руководство: настройка среды разработки](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Назначения ролей

Чтобы предоставить разрешения для получателя, создайте назначения ролей. Чтобы отменить разрешения, удалите назначение ролей. Назначение ролей Digital Twins связывает объект (пользователя, клиента Azure Active Directory и т. д.) с ролью и пространством. Разрешения предоставляются всем объектам, которые принадлежат этой области. Пространство включает весь пространственный граф.

Например, пользователь получает назначение ролей с ролью `DeviceInstaller` для корневого узла пространственного графа, представляющего здание. После этого пользователь сможет считывать и обновлять устройства для этого узла и для всех других дочерних пространств в здании.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о безопасности Azure Digital Twins см. в статье [Создание назначений ролей и управление ими](./security-create-manage-role-assignments.md).
