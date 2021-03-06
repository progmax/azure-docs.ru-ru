---
title: Предварительно созданные домены для интеллектуальной службы распознавания речи (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS включает набор предварительно созданных доменов для быстрого добавления общих разговорных сценариев.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: b1c7ced4a934ea5d094e0c54a295870986f09933
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2018
ms.locfileid: "49651825"
---
# <a name="add-prebuilt-domains-for-common-usage-scenarios"></a>Добавление предварительно созданных доменов для распространенных сценариев использования 

Служба LUIS включает ряд готовых намерений из предварительно созданных предметных областей для быстрого добавления общих намерений и фраз. Это быстрый и простой способ добавления возможностей в клиентское приложение общения без необходимости разрабатывать модели для этих возможностей. 

## <a name="add-a-prebuilt-domain"></a>Добавление готовой предметной области

1. На странице **Мои приложения** выберите свое приложение. Откроется приложение на разделе **Сборка**. 

1. На странице **Намерения** выберите **Добавить предварительно созданные домены** из нижней панели слева 

1. Выберите намерение **Календарь** и нажмите на кнопку **Добавить домен**.

    ![Добавление предварительно созданного домена календаря](./media/luis-prebuilt-domains/add-prebuilt-domain.png)

1. Выберите **Намерения** на левой панели навигации, чтобы просмотреть намерения календаря. Каждое намерение в этом домене будет начинаться с префикса `Calendar.`. Вместе с фразами в приложение добавляются две сущности для этого домена: `Calendar.Location` и `Calendar.Subject`. 

## <a name="train-and-publish"></a>Обучение и публикация

1. После добавления домена обучите приложение, выбрав **Обучить** на панели в правом верхнем углу. 

1. В верхней части панели инструментов щелкните **Опубликовать**. Выполните публикацию в **рабочей среде**. 

1. При появлении уведомления об успешной публикации щелкните ссылку **Перейти к списку конечных точек**, чтобы просмотреть конечные точки.

1. Выберите конечную точку. Откроется новая вкладка браузера для этой конечной точки. Не закрывайте вкладку браузера и перейдите в раздел **Тест**.

## <a name="test"></a>Тест

Выполните тест нового намерения в конечной точке, добавив значение для параметра **q**: `Schedule a meeting with John Smith in Seattle next week`.

LUIS возвращает правильное намерение и тему встречи:

```JSON
{
  "query": "Schedule a meeting with John Smith in Seattle next week",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.824783146
  },
  "entities": [
    {
      "entity": "a meeting with john smith",
      "type": "Calendar.Subject",
      "startIndex": 9,
      "endIndex": 33,
      "score": 0.484055847
    }
  ]
}
```

## <a name="next-steps"></a>Дополнительная информация
> [!div class="nextstepaction"]
> [Справочник по предварительно созданным доменам](./luis-reference-prebuilt-domains.md)
