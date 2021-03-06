---
title: Системные переменные в фабрике данных Azure | Документация Майкрософт
description: В этой статье описаны системные переменные, поддерживаемые фабрикой данных Azure. Вы можете использовать эти переменные в выражениях при определении сущностей фабрики данных.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 43ea8703bdbfc23511c831a5f91c9461948cc254
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/27/2018
ms.locfileid: "37055670"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Системные переменные, поддерживаемые в фабрике данных Azure
В этой статье описаны системные переменные, поддерживаемые фабрикой данных Azure. Вы можете использовать эти переменные в выражениях при определении сущностей фабрики данных.

## <a name="pipeline-scope"></a>Область конвейера
Ссылки на эти системные переменные можно добавлять в любой части JSON конвейера.

| Имя переменной | ОПИСАНИЕ |
| --- | --- |
| @pipeline().DataFactory |Имя фабрики данных, в которой выполняется конвейер. |
| @pipeline().Pipeline |Имя конвейера. |
| @pipeline().RunId | Идентификатор запуска определенного конвейера. |
| @pipeline().TriggerType | Тип триггера, вызвавшего конвейер (ручной, планировщик). |
| @pipeline().TriggerId| Идентификатор триггера, который вызывает конвейер. |
| @pipeline().TriggerName| Имя триггера, который вызывает конвейер. |
| @pipeline().TriggerTime| Время, когда триггер вызвал конвейер. Время триггера — это фактическое время срабатывания, а не запланированное время. Например, `13:20:08.0149599Z` возвращается вместо `13:20:00.00Z`. |

## <a name="schedule-trigger-scope"></a>Область триггера расписания
Ссылки на эти системные переменные можно добавлять в любой части JSON триггера, если это триггер типа ScheduleTrigger.

| Имя переменной | ОПИСАНИЕ |
| --- | --- |
| @trigger().scheduledTime |Время, на которое запланирован вызов запуска конвейера. Например, для триггера, срабатывающего каждые 5 минут, эта переменная будет возвращать `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z` и `2017-06-01T22:29:00Z` соответственно.|
| @trigger().startTime |Время, когда триггер **фактически** вызывает запуск конвейера. Например, для триггера, срабатывающего каждые 5 минут, эта переменная может возвращать примерно `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z` и `2017-06-01T22:29:00.9935483Z` соответственно.|

## <a name="tumbling-window-trigger-scope"></a>Область триггера "переворачивающегося" окна
Ссылки на эти системные переменные можно добавлять в любой части JSON триггера, если это триггер типа TumblingWindowTrigger.

| Имя переменной | ОПИСАНИЕ |
| --- | --- |
| @trigger().outputs.windowStartTime |Начало временного интервала, на который запланирован вызов запуска конвейера. Если частота срабатывания триггера "переворачивающегося" окна — каждый час, это будет время в начале часа.|
| @trigger().outputs.windowEndTime |Окончание временного интервала, на который запланирован вызов запуска конвейера. Если частота срабатывания триггера "переворачивающегося" окна — каждый час, это будет время в конце часа.|
## <a name="next-steps"></a>Дополнительная информация
Сведения о том, как эти переменные используются в выражениях, см. в статье [Выражения и функции в фабрике данных Azure](control-flow-expression-language-functions.md).
