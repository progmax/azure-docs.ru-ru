---
title: Создание службы SignalR с помощью примера скрипта в интерфейсе командной строки Azure
description: Пример скрипта Azure CLI. Создание службы SignalR
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: 364a8b6574b06aa2403ea028fecd0676ba0342a7
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256219"
---
# <a name="create-a-signalr-service"></a>Создание службы SignalR 

В этом примере скрипта создается ресурс службы Azure SignalR со случайным именем в новой группе ресурсов.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этой статьей вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Пример скрипта

В скрипте используется расширение *signalr* для Azure CLI. Выполните следующую команду, чтобы установить расширение *signalr* для Azure CLI, прежде чем использовать скрипт из этого примера:

```azurecli-interactive
az extension add -n signalr
```

С помощью этого скрипта создается ресурс службы SignalR и группа ресурсов. 

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-service-and-group/create-signalr-service-and-group.sh "Creates a new Azure SignalR Service resource and resource group")]

Запишите фактическое имя, созданное для новой группы ресурсов. Это имя группы ресурсов вам понадобится для удаления групп ресурсов.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

Для каждой команды в таблице приведены ссылки на соответствующую документацию. Этот сценарий использует следующие команды:

| Get-Help | Примечания |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az signalr create](/cli/azure/group#az-group-create) | Создание ресурса службы Azure SignalR. |
| [az signalr key list](/cli/azure/ext/signalr/signalr/key#ext-signalr-az-signalr-key-list) | Выводит список ключей, которые будут использоваться приложением для принудительной отправки обновлений через SignalR в режиме реального времени. |


## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](/cli/azure).

Дополнительные примеры скриптов CLI для службы Azure SignalR см. в [документации по службе Azure SignalR](../signalr-cli-samples.md).
