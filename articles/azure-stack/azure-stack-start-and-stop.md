---
title: Запуск и остановка Azure Stack | Документация Майкрософт
description: Сведения о том, как запустить и завершить работу Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 43BF9DCF-F1B7-49B5-ADC5-1DA3AF9668CA
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 6da81ab90a87285f9e1874e3f10eff4570124192
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344019"
---
# <a name="start-and-stop-azure-stack"></a>Запуск и остановка Azure Stack
Следуйте инструкциям по корректному завершению работы и перезапуску служб Azure Stack, приведенным в этой статье. Операция завершения работы физически отключает питание всей среды Azure Stack. Запуск включает все роли инфраструктуры и возвращает ресурсы клиента в состояние, в котором они находились до завершения работы.

## <a name="stop-azure-stack"></a>Остановка Azure Stack 

Чтобы завершить работу Azure Stack, сделайте следующее:

1. Подготовьте всех рабочие нагрузки в ресурсах клиента в среде Azure Stack к завершению работы. 

2. Откройте сеанс привилегированной конечной точки (PEP) на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack. См. инструкции по [использованию привилегированной конечной точки в Azure Stack](azure-stack-privileged-endpoint.md).

3. Из сеанса PEP выполните следующую команду:

    ```powershell
      Stop-AzureStack
    ```

4. Дождитесь отключения питания на всех физических узлах Azure Stack.

> [!Note]  
> Чтобы проверить состояние питания физического узла, следуйте инструкциям от изготовителя оборудования (OEM), поставившего оборудование Azure Stack. 

## <a name="start-azure-stack"></a>Запуск Azure Stack 

Чтобы запустить Azure Stack, сделайте следующее. Выполните следующие действия, независимо от того, как остановлена работа Azure Stack.

1. Включите питание на всех физических узлах в вашей среде Azure Stack. Убедитесь, что питание для физических узлов включено по инструкциям изготовителя оборудования, поставившего оборудование для Azure Stack.

2. Дождитесь запуска служб инфраструктуры Azure Stack. Для завершения запуска служб инфраструктуры Azure Stack может потребоваться два часа. Вы можете проверить состояние запуска Azure Stack с помощью командлета [**Get-ActionStatus**](#get-the-startup-status-for-azure-stack).

3. Убедитесь, что все ресурсы клиента вернулись в состояние, в котором они находились до завершения работы. Рабочие нагрузки, выполняющиеся на ресурсах клиента, могут потребовать повторной настройки в диспетчере рабочих нагрузок после запуска.

## <a name="get-the-startup-status-for-azure-stack"></a>Получение сведений о состоянии запуска для Azure Stack

Чтобы получить сведения о запуске для подпрограммы запуска Azure Stack, сделайте следующее:

1. Откройте сеанс PEP на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack.

2. Из сеанса PEP выполните следующую команду:

    ```powershell
      Get-ActionStatus Start-AzureStack
    ```

## <a name="troubleshoot-startup-and-shutdown-of-azure-stack"></a>Устранение неполадок при запуске и завершении работы Azure Stack

Если через два часа после включения питания в среде Azure Stack не удалось запустить службы инфраструктуры и клиентские службы, выполните следующие действия: 

1. Откройте сеанс PEP на компьютере с сетевым доступом к виртуальным машинам ERCS Azure Stack.

2. Выполните команду: 

    ```powershell
      Test-AzureStack
      ```

3. Просмотрите выходные данные и устраните ошибки работоспособности. Дополнительные сведения см. в статье [Запуск проверочного теста в Azure Stack](azure-stack-diagnostic-test.md).

4. Выполните команду:

    ```powershell
      Start-AzureStack
    ```

5. Если команда **Start-AzureStack** завершится ошибкой, обратитесь в службу поддержки пользователей Майкрософт. 

## <a name="next-steps"></a>Дополнительная информация 

Дополнительные сведения о [средствах диагностики Azure Stack](azure-stack-diagnostics.md)
