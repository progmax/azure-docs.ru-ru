---
title: Использование обратного вызова обнаружения сущности в модели Conversation Learner Microsoft Cognitive Services | Документация Майкрософт
titleSuffix: Azure
description: Сведения о том, как использовать обратный вызов обнаружения сущности в модели Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e9a3b0a2be58313266b949b907e4eb49a318a6dc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260080"
---
# <a name="how-to-use-entity-detection-callback"></a>Как использовать обратный вызов обнаружения сущностей

В этом руководстве объясняются обратный вызов обнаружения сущностей и стандартный метод сопоставления сущностей.

## <a name="video"></a>Видео

[![Предварительная версия руководства 10](https://aka.ms/cl-tutorial-10-preview)](https://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Требования
Для работы с этим руководством требуется запущенный бот `tutorialEntityDetectionCallback`.

    npm run tutorial-entity-detection

## <a name="details"></a>Сведения
Обратный вызов обнаружения сущностей позволяет обрабатывать в пользовательском коде бизнес-правила, основанные на сущностях. В этом примере используются обратные вызовы и программные сущности для сопоставления вводимых пользователем названий городов с их официальными названиями (например, заменяя "северная столица" на "Санкт-Петербург").

### <a name="open-the-demo"></a>Запуск демонстрационного примера

В списке моделей щелкните Tutorial-10-EntityDetectionCallback. 

### <a name="entities"></a>Сущности

В модели определены три сущности.

![](../media/tutorial10_entities.PNG)

1. City. Обозначает пользовательскую сущность, которую пользователь вводит в текстовом виде.
2. CityUnknown — это программная сущность. Значения сущности заполняются системой. Если система не распознает город, в эту сущность подставятся введенные пользователем данные.
3. CityResolved — это город, который уже знаком системе. Эта сущность будет хранить официальное название города, например "the big apple" (северная столица) будет сопоставлено с "new york" (Санкт-Петербург).

### <a name="actions"></a>Действия

В модели определены три действия.

![](../media/tutorial10_actions.PNG)

1. Первое из них — "Which city do you want?" (Какой город вам нужен?).
2. Второе — "I don't know this city, $CityUknown. Which city do you want?" (Я не знаю такого города $CityUknown. Какой город вам нужен?).
3. И третье — "You said $City, and I resolved that to $CityResolved" (Ваш город $City сопоставлен с $CityResolved).

### <a name="callback-code"></a>Код обратного вызова

Давайте рассмотрим код примера. Метод EntityDetectionCallback можно найти в файле c:\<путь_установки>\src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial10_callbackcode.PNG)

Эта функция вызывается после разрешения сущностей.
 
- Первое, что она делает — очищает значение $CityUknown. $CityUknown сохраняется только на один шаг и всегда очищается в начале следующего.
- После этого будет получен список распознанных городов. Извлекаем первый из них и пытаемся сопоставить имя.
- Функция сопоставления (resolveCity) определена выше в том же коде. Она содержит список официальных названий городов. Функция возвращает название города, если находит его в списке. В противном случае она выполняет поиск через cityMap и возвращает сопоставленное название. Если же не удается найти город, возвращается значение null.
- Наконец, если для города обнаружено сопоставление, мы сохраняем его в сущности $CityKnown. В противном случае ответ пользователя очищается и заполняется сущность $CityUknown.

### <a name="train-dialogs"></a>Обучение диалогам

1. Щелкните "Train Dialogs" (Обучение диалогам), а затем — "New Train Dialog" (Обучение новому диалогу).
2. Введите "hello" (Привет).
3. Щелкните "Score Actions" (Оценить действия) и выберите "Which city do you want?" (Какой город вам нужен?)
2. Введите "new york" (Санкт-Петербург).
    - Текст распознается как сущность city.
5. Щелкните "Score Actions" (Оценить действия).
    - `City` и `CityResolved` были заполнены.
6. Выберите "You said $City, and I resolved that to $CityResolved" (Ваш город $City сопоставлен с $CityResolved).
7. Щелкните "Done Teaching" (Завершить обучение).

Добавьте другой пример диалога.

1. Щелкните "New Train Dialog" (Обучение новому диалогу).
2. Введите "hello" (Привет).
3. Щелкните "Score Actions" (Оценить действия) и выберите "Which city do you want?" (Какой город вам нужен?)
2. Введите "big apple".
    - Текст распознается как сущность city.
5. Щелкните "Score Actions" (Оценить действия).
    - `CityResolved` отображает результат выполнения кода.
6. Выберите "You said $City, and I resolved that to $CityResolved" (Ваш город $City сопоставлен с $CityResolved).
7. Щелкните "Done Teaching" (Завершить обучение).

Добавьте другой пример диалога.

1. Щелкните "New Train Dialog" (Обучение новому диалогу).
2. Введите "hello" (Привет).
3. Щелкните "Score Actions" (Оценить действия) и выберите "Which city do you want?" (Какой город вам нужен?)
2. Введите "foo".
    - Это пример текста, который не известен системе как название города. 
5. Щелкните "Score Actions" (Оценить действия).
6. Выберите "I don't know this city, $CityUknown". Which city do you want?" (Я не знаю такого города $CityUknown. Какой город вам нужен?)
7. Введите "new york" (Санкт-Петербург).
8. Щелкните Score Actions (Оценить действия).
    - `CityUknown` было очищено, а `CityResolved` было заполнено.
6. Выберите "You said $City, and I resolved that to $CityResolved" (Ваш город $City сопоставлен с $CityResolved).
7. Щелкните Done Teaching (Завершить обучение).

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Обратные вызовы сеансов](./11-session-callbacks.md)
