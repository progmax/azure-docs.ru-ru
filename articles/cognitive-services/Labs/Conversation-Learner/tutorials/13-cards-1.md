---
title: Использование карт в модели обучения приложения Conversation Learner в Microsoft Cognitive Services. Часть 1 | Документация Майкрософт
titleSuffix: Azure
description: Сведения об использовании карт в приложении Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: da261beeec4f02dfa7c7cf9071e51dc17cf5c7cd
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254389"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Использование карт (часть 1 из 2)

В этом руководстве показано, как добавить и использовать простую карту в боте.

> [!NOTE]
> В настоящее время, чтобы использовать приложение Conversation Learner, файлы с определениями карт должны размещаться в каталоге с именем cards, созданном в каталоге, из которого запущен бот. В будущем мы сделаем это настраиваемым.

## <a name="video"></a>Видео

[![Предварительная версия руководства 13](https://aka.ms/cl-tutorial-13-preview)](https://aka.ms/blis-tutorial-13)

## <a name="requirements"></a>Требования
Для работы с этим руководством требуется запущенный бот обучения общего назначения.

    npm run tutorial-general

## <a name="details"></a>Сведения

Карты — это элементы пользовательского интерфейса, которые позволяют пользователю выбрать вариант ответа в диалоге. 

### <a name="open-the-demo"></a>Запуск демонстрационного примера

В списке моделей веб-интерфейса щелкните Tutorial-13-Cards-1. 

### <a name="the-card"></a>Карта

Определение карты находится по следующему адресу: C:\<каталог_установки\>\src\cards\prompt.json.

Система ищет определения карт в указанном каталоге cards.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Обратите внимание на тип текста `TextBlock` и заполнитель `{{question}}` в текстовом поле.
> Доступны две кнопки отправки и текст, который передается каждой из них.

### <a name="actions"></a>Действия

Мы создали три действия. Как показано ниже, первое действие представляет собой карту.

![](../media/tutorial13_actions.PNG)

Давайте посмотрим, как создается этот тип действия:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> Поле ввода вопроса и кнопки 1 и 2. Это ссылки на шаблон в карте, куда вы вводите вопрос и соответствующие ответы. Также можно ссылаться на сущности или сочетание текста и сущностей и использовать их.

Значок в виде глаза показывает, как выглядит карта.

### <a name="train-dialog"></a>Обучение диалогу

Давайте подробнее рассмотрим процесс обучения диалогу.

1. Щелкните Train Dialogs (Обучение диалогам), а затем New Train Dialog (Обучение новому диалогу).
1. Введите строку hi (Привет).
2. Щелкните Score Action (Оценить действие).
3. Щелкните Prompt go left or right (Запрос на выбор: влево или вправо).
    - Выбор left (Влево) или right (Вправо) эквивалентен вводу пользователем соответствующих слов. 
4. Щелкните Score Actions (Оценить действия).
4. Щелкните left (Влево). Для этого действия ожидание не предусмотрено.
6. Щелкните Prompt go left or right (Запрос на выбор: влево или вправо).
4. Щелкните right (Вправо).
5. Щелкните Score Actions (Оценить действия).
3. Щелкните Right (Вправо).
6. Щелкните Prompt go left or right (Запрос на выбор: влево или вправо).
4. Щелкните Done Testing (Завершить тестирование).

Теперь вы увидели, как работают карты. Они определены в каталоге cards в виде шаблонов JSON. Шаблоны отображаются в пользовательском интерфейсе, где можно их заполнить, используя строки, сущности или их сочетание.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Карты. Часть 2](./14-cards-2.md)
