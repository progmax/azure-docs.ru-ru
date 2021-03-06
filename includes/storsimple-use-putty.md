---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 50a5c8d515e27db7c2c65b484cdecad8ff00baf8
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164408"
---
<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a>Подключение через последовательную консоль
1. Подключите последовательный кабель к устройству (непосредственно или через USB-адаптер).
2. Откройте **Панель управления**, а затем откройте **Диспетчер устройств**.
3. Определите COM-порт, как показано на следующем рисунке.
   
     ![Подключение через последовательную консоль](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Запустите PuTTY. 
5. В правой панели измените **Тип соединения** на **Последовательный**.
6. В правой панели введите соответствующий COM-порт. Убедитесь, что параметры последовательной конфигурации заданы следующим образом:
   
   * Скорость: 115200.
   * Биты данных: 8.
   * Стоповые биты: 1.
   * Четность: нет.
   * Управление потоком: нет.
     
     Эти параметры показаны на следующем рисунке.
     
     ![Параметры PuTTY](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Если параметр управления потоком по умолчанию не работает, попробуйте задать для управления потоком значение "XON/XOFF".
     > 
     > 
7. Щелкните **Открыть** , чтобы запустить последовательный сеанс.

