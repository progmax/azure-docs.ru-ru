---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 5e4921be3116754f146ed0845513010f2642c97b
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164787"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Установка исправлений режима обслуживания с помощью Windows PowerShell для StorSimple
> [!IMPORTANT]
> В режиме обслуживания необходимо сначала применить исправление на одном контроллере, а затем на другом.
> 
> 

1. Переведите устройство в режим обслуживания. Инструкции по переходу в режим обслуживания см. в разделе [Шаг 2. Вход в режим обслуживания](../articles/storsimple/storsimple-update-device.md#step2).
2. Чтобы применить исправление, введите:
   
     `Start-HcsHotfix` 
3. При появлении запроса укажите путь к общей сетевой папке, содержащей файлы исправления.
4. После этого введите подтверждение для применения этих исправлений. Нажмите клавишу **Y** для продолжения установки исправления.
5. После применения исправления на одном контроллере войдите на другой контроллер. Установите исправление, как для предыдущего контроллера.
6. После применения исправления выйдите из режима обслуживания. Инструкции см. в разделе [Шаг 4. Выход из режима обслуживания](../articles/storsimple/storsimple-update-device.md#step4).

