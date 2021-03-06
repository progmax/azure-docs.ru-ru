---
title: Создание и запуск заданий в приложении Azure IoT Central | Документация Майкрософт
description: Задания Azure IoT Central позволяют использовать массовые функции управления устройствами, такие как обновление свойства устройства, настройка или выполнение команды.
ms.service: iot-central
services: iot-central
author: sarahhubbard
ms.author: sahubbar
ms.date: 09/15/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 35db7bf87c7b72fc31d820c9058b1df8415bd553
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031311"
---
# <a name="create-and-run-a-job-in-your-azure-iot-central-application"></a>Создание и запуск заданий в приложении Azure IoT Central

Вы можете использовать Microsoft Azure IoT Central для управления подключенными устройствами в масштабе с использованием заданий. Задания позволяют выполнять массовые обновления свойств устройства, параметров и команд. В этой статье рассказывается, как начать работу с заданиями в вашем приложении.

## <a name="create-and-run-a-job"></a>Создание и запуск заданий

В этом разделе показано, как создать и запустить задание. Каждый шаг сопровождается примером, демонстрирующим, как выполнять задание для холодильных автоматов, для которых нужно повысить скорость вращения вентилятора.

1. В области навигации перейдите к заданиям.

1. Щелкните **+ Создать**, чтобы создать новое задание.

    ![Создание задания](./media/howto-run-a-job/createnewjob.png)

1. Введите имя и описание, помогающее определить задание, которое вы создаете.

1. Выберите набор устройств, к которым будет применяться задание. После выбора набора устройств правая часть заполнится устройствами из выбранного набора. Если вы выберете поврежденный набор устройств, устройства не будут отображаться и вы увидите сообщение, объясняющее, что ваш набор поврежден.

1. Затем выберите тип задания, которое будет определено (параметр, свойство или команда). Щелкните **+** рядом с типом выбранного задания и добавьте нужные операции.

    ![Настройка задания](./media/howto-run-a-job/configurejob.png)

1. С правой стороны выберите устройства, на которых вы хотите запустить задание. Установите верхний флажок, чтобы выбрать все устройства в наборе. Установите флажок рядом с именем, чтобы выбрать все устройства на текущей странице.

1. Выбрав нужные устройства, щелкните **Запуск**. Задание появится на вашей главной странице **Задания**. В этом представлении вы можете увидеть текущее задание и историю любых ранее запущенных заданий. Ваше текущее задание всегда будет отображаться в верхней части списка.

    ![Запуск задания](./media/howto-run-a-job/runjob.png)

    ![Просмотр задания](./media/howto-run-a-job/viewjob.png)

    > [!NOTE]
    > В журнале отображается история выполненных заданий за 30 дней.

1. Чтобы получить общие сведения о задании, щелкните в списке имя задания, которое вы хотите просмотреть. Вы увидите сведения о задании, устройствах и состояниях устройств.

    ![Просмотр состояния устройства](./media/howto-run-a-job/viewdevicestatus.png)

### <a name="stop-a-running-job"></a>Остановка выполнения задания

Если вы хотите остановить задание, которое выполняется в данный момент, щелкните его имя. Нажмите кнопку **Остановить** на панели. Вы увидите, что состояние задания изменилось.

> [!NOTE]
> Остановленное задание нельзя перезапустить. Вы должны создать другое задание с требуемыми операциями и устройствами.

## <a name="view-the-job-status"></a>Просмотр состояния задания

После создания задания в столбце **Состояние** обновится последнее сообщение о состоянии задания. Сообщения о состоянии означают следующее:

| Сообщение о состоянии       | Значение состояния                                          |
| -------------------- | ------------------------------------------------------- |
| Завершено            | Это задание было выполнено на всех устройствах.              |
| Сбой               | Это задание завершилось сбоем и выполнено не полностью на устройствах.  |
| Ожидает              | Это задание еще не начало выполняться на устройствах.        |
| Выполнение              | Это задание в данный момент выполняется на устройствах.             |
| Остановлено              | Это задание вручную остановлено пользователем.           |

За сообщением состояния следуют общие сведения об устройствах в задании. Эти состояния устройств обозначают следующее.

| Сообщение о состоянии       | Значение состояния                                                     |
| -------------------- | ------------------------------------------------------------------ |
| Succeeded            | Количество устройств, на которых успешно выполнено задание.  |
| Сбой               | Количество устройств, на которых выполнение задания завершилось сбоем.      |

### <a name="view-the-device-status"></a>Просмотр состояния устройства

Чтобы просмотреть состояние каждого устройства в задании, щелкните имя задания. Здесь вы увидите подробные сведения о задании и всех устройствах, которые были частью этого конкретного задания. Рядом с именем каждого устройства вы увидите одно из следующих сообщений о состоянии:

| Сообщение о состоянии       | Значение состояния                                                                |
| -------------------- | ----------------------------------------------------------------------------- |
| Завершено            | Это задание было выполнено на этом устройстве.                                     |
| Сбой               | Это задание завершилось сбоем на этом устройстве. В сопроводительном сообщении об ошибке будут содержаться дополнительные сведения.  |
| Ожидает              | Задание еще не выполнено на этом устройстве.                                  |

> [!NOTE]
> Если устройство было удалено, вы не сможете его выбрать, и оно будет отображаться как удаленное с идентификатором устройства.

## <a name="next-steps"></a>Дополнительная информация

Вы узнали, как создавать задания в приложении Azure IoT Central, а значит вы готовы к следующим шагам:

- [использовать наборы устройств](howto-use-device-sets.md);
- [Управление устройствами](howto-manage-devices.md)
- [Создание версии шаблона нового устройства](howto-version-devicetemplate.md)
