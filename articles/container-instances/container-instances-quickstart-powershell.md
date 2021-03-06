---
title: Краткое руководство. Запуск приложения в службе "Экземпляры контейнеров Azure" с помощью PowerShell
description: В этом кратком руководстве вы развернете приложение, выполняющееся в контейнере Docker, в службе "Экземпляры контейнеров Azure" с помощью Azure PowerShell.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b17cca7f0c00aba260b97b29345ff33156a50138
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183953"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>Краткое руководство. Запуск контейнерного приложения в службе "Экземпляры контейнеров Azure" с помощью Azure PowerShell

Служба "Экземпляры контейнеров Azure" позволяет легко и быстро запускать контейнеры Docker в Azure. В этом случае не нужно развертывать виртуальные машины или использовать единую платформу оркестрации контейнеров, такую как Kubernetes. В этом кратком руководстве вы с помощью портала Azure создадите в Azure контейнер Windows и сделаете его приложение доступным по полному доменному имени (FQDN). Через несколько секунд после выполнения одной команды развертывания можно просмотреть выполняющееся приложение:

![Приложение, развернутое в службе "Экземпляры контейнеров Azure" (просмотр в браузере)][qs-powershell-01]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Чтобы установить и использовать PowerShell локально для работы с этим руководством, вам понадобится модуль Azure PowerShell 5.5 или более поздней версии. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить обновление, ознакомьтесь со статьей, посвященной [установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps). Если модуль PowerShell запущен локально, необходимо также выполнить командлет `Connect-AzureRmAccount`, чтобы создать подключение к Azure.

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Экземпляры контейнеров Azure, как и все ресурсы Azure, должны быть развернуты в группе ресурсов. Группы ресурсов позволяют организовать соответствующие ресурсы Azure и управлять ими.

Для начала создайте группу ресурсов с именем *myResourceGroup* в регионе *eastus* с помощью следующей команды [New-AzureRmResourceGroup][New-AzureRmResourceGroup]:

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Создание контейнера

Теперь, когда вы создали группу ресурсов, можно запустить контейнер в Azure. Чтобы создать экземпляр контейнера с помощью Azure PowerShell, укажите имя группы ресурсов, имя экземпляра контейнера и образ контейнера Docker для команды [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Вы можете предоставить контейнеры в Интернете, указав один или несколько портов для открытия, метку имени DNS или и то и другое. В этом кратком руководстве вы развернете контейнер с меткой имени DNS, в котором размещены службы IIS, запущенные на Nano Server.

Выполните следующую команду, чтобы запустить экземпляр контейнера. Значение `-DnsNameLabel` должно быть уникальным в пределах региона Azure, в котором создается экземпляр. Если появится сообщение об ошибке "Метка имени DNS недоступна", попробуйте другую метку имени DNS.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

В течение нескольких секунд вы должны получить ответ от Azure. Состояние `ProvisioningState` контейнера изначально **Создается**, но через минуту или две оно должно измениться на **Успешно**. Вы можете проверить состояние развертывания с помощью командлета [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Состояние подготовки, полное доменное имя (FQDN) и IP-адрес контейнера отображаются в выходных данных командлета:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Когда состояние `ProvisioningState` контейнера **Успешно**, перейдите в `Fqdn` в браузере. Если появится примерно такая веб-страница, поздравляем! Вы успешно развернули приложение, выполняющееся в контейнере Docker в Azure.

![Службы IIS, развернутые с помощью службы "Экземпляры контейнеров Azure" (просмотр в браузере)][qs-powershell-01]

## <a name="clean-up-resources"></a>Очистка ресурсов

По завершении работы с контейнером его можно удалить с помощью командлета [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Дополнительная информация

С помощью этого краткого руководства вы создали экземпляр контейнера Azure из образа, размещенного в общедоступном реестре Docker Hub. Если вы хотите создать образ контейнера и развернуть его через частный реестр контейнеров Azure, перейдите к руководству по использованию службы "Экземпляры контейнеров Azure".

> [!div class="nextstepaction"]
> [Руководство по использованию службы "Экземпляры контейнеров Azure"](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
