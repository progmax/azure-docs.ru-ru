---
title: Обновление конфигурации кластера Azure Service Fabric | Документация Майкрософт
description: Узнайте, как обновить конфигурацию, которая работает в кластере Service Fabric в Azure, с помощью шаблона Resource Manager.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 9323b393edb808f3d2d069f868deb0b67cd0c871
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2018
ms.locfileid: "51857889"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Обновление конфигурации кластера в Azure 

В этой статье описывается, как настроить различные параметры структуры для кластера Service Fabric. Для кластеров, размещенных в Azure, можно настроить параметры на [портале Azure](https://portal.azure.com) или использовать шаблон Azure Resource Manager.

> [!NOTE]
> Не все параметры доступны на портале. Если один из параметров, перечисленных ниже, недоступен на портале, настройте его с помощью шаблона Azure Resource Manager.> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Настройка параметров кластера с помощью шаблонов Resource Manager
Кластеры Azure можно настроить с помощью JSON-шаблона Resource Manager. Дополнительные сведения см. в статье о [параметрах конфигурации для кластеров](service-fabric-cluster-fabric-settings.md). В качестве примера ниже приведены инструкции, с помощью которых можно добавить новый параметр *MaxDiskQuotaInMB* в раздел *Diagnostics*, используя обозреватель ресурсов Azure.

1. Перейдите на сайт https://resources.azure.com.
2. Перейдите к подписке, развернув **subscriptions** -> **\<Ваша подписка>** -> **resourceGroups** -> **\<Ваша группа ресурсов >** -> **providers** -> **Microsoft.ServiceFabric** -> **clusters** -> **\<Имя вашего кластера>**
3. В правом верхнем углу выберите пункт **Чтение и запись**.
4. Выберите **Изменить** и обновите элемент JSON `fabricSettings`, а затем добавьте новый элемент.

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Вы также можете настроить параметры кластера с помощью Azure Resource Manager одним из следующих способов:

- Экспортируйте и обновите шаблон Resource Manager с помощью [портала Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template).
- Экспортируйте и обновите шаблон Resource Manager с помощью [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell).
- Экспортируйте и обновите шаблон Resource Manager с помощью [Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli).
- Чтобы изменить параметр напрямую, используйте команды Azure RM PowerShell [​​Set-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Set-AzureRmServiceFabricSetting) и [Remove-AzureRmServiceFabricSetting](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/Remove-AzureRmServiceFabricSetting).
- чтобы изменить параметр напрямую, используйте команды Azure CLI [az sf cluster setting](https://docs.microsoft.com/cli/azure/sf/cluster/setting).

## <a name="next-steps"></a>Дополнительная информация
* Изучите сведения о [параметрах кластера Service Fabric](service-fabric-cluster-fabric-settings.md).
* Ознакомьтесь с концепцией [масштабирования кластера](service-fabric-cluster-scale-up-down.md).
