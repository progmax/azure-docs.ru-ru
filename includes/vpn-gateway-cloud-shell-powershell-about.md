---
title: включение файла
description: включение файла
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b878d54f0f52768459dbfc810e47d294b9c8d996
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50097772"
---
В этой статье используются командлеты PowerShell. Для запуска командлетов можно использовать бесплатную интерактивную оболочку Azure Cloud Shell. Она включает предварительно установленные общие инструменты Azure и настроена для использования с вашей учетной записью. Нажмите кнопку **Копировать**, чтобы скопировать код. Вставьте его в Cloud Shell и нажмите клавишу ВВОД, чтобы запустить код. Cloud Shell можно запустить разными способами:

|  |   |
|-----------------------------------------------|---|
| Нажмите кнопку **Попробовать** в правом верхнем углу блока с кодом. | ![Cloud Shell в этой статье](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Откройте Cloud Shell в браузере. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Нажмите кнопку меню **Cloud Shell** в правом верхнем углу окна портала Azure. | [![Cloud Shell на портале](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Если вы не хотите использовать Azure Cloud Shell, можно установить PowerShell локально. Чтобы установить и использовать PowerShell локально, обязательно установите последнюю версию командлетов PowerShell для Azure Resource Manager. Командлеты PowerShell обновляются часто, и вам, как правило, необходимо обновить командлеты PowerShell, чтобы получить новые функциональные возможности. Если вы не обновите командлеты PowerShell, при указании значений может произойти сбой. Чтобы узнать, какая версия PowerShell выполняется локально, используйте командлет Get-Module -ListAvailable AzureRM. Если необходимо выполнить обновление, см. статью об [установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps). Подробнее: [Установка и настройка Azure PowerShell](/powershell/azure/overview).