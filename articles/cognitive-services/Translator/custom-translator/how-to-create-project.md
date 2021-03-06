---
title: Как создать проект Custom Translator
titleSuffix: Azure Cognitive Services
description: Как создать проект в Custom Translator
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: 4e5ac4386af55855c5240f89557feafd4a93adfb
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626905"
---
# <a name="create-a-project"></a>Создание проекта

Проект — это контейнер для моделей, документов и тестов. Каждый проект автоматически включает все документы с правильный языковой парой, переданные в эту рабочую область. 

Создание проекта — это первый этап создания модели. 

## <a name="create-a-project"></a>Создание проекта

1.  На портале [Custom Translator](https://portal.customtranslator.azure.ai) выберите "Создать проект".

    ![Создание проекта](media/how-to/how-to-create-project.png)

2.  В диалоговом окне введите сведения о проекте в указанные ниже поля:

    a.  "Имя проекта" (обязательно). Присвойте проекту понятное уникальное имя. В названии не нужно упоминать языки.

    b.  "Описание". Краткие сведения о проекте. Это описание не влияет на поведение Custom Translator или конечной пользовательской системы, но может помочь отличать этот проект от других.

    c.  Language pair (Языковая пара) (обязательно). Выберите исходный и целевой языки для перевода.

    d.  "Категория" (обязательно). Выберите категорию, которая лучше всего подходит для вашего проекта. Категория описывает терминологию и стиль документов, которые будут переводиться.

    д.  Category descriptor (Описание категории). Используйте это поле, чтобы точнее описать определенную сферу или отрасль, в которой вы работаете. Например, если категория — медицина, вы можете добавить определенный документ, связанный, к примеру, с хирургией или педиатрией. Описание не влияет на поведение Custom Translator или конечной пользовательской системы.

    Е.  Project label (Метра проекта). [Метка проекта](workspace-and-project.md#project-labels) позволяет различать проекты с одинаковыми парой языка и категорией. Метку рекомендуется использовать *только* в том случае, если вы планируете создать несколько проектов для одной языковой пары и категории и хотите получать доступ к этим проектам с помощью разных идентификаторов категории. Не используйте это поле, если вы создаете системы только для одной категории. Метка проекта не является обязательной и не помогает различать языковые пары. Одну метку можно использовать для нескольких проектов.

    ![Диалоговое окно создания проекта](media/how-to/how-to-create-project-dialog.png)

3.  Щелкните Создать. 

## <a name="view-project-details"></a>Просмотр сведений о проекте

На целевой странице Custom Translator отображаются первые 10 проектов в рабочей области. Здесь показаны имя проекта, языковая пара, категория, состояние и оценка BLEU.

Когда вы выберете проект, на странице проекта отобразятся следующее элементы:

- CategoryID. Идентификатор создается путем объединения идентификатора рабочей области, метки проекта и кода категории. Свойство CategoryID используется с API перевода текстов для получения пользовательских переводов.

- Кнопка Train (Обучение). Это кнопка для запуска [обучения модели](how-to-train-model.md).

- Кнопка Add documents (Добавить документы). Это кнопка для [отправки документов](how-to-upload-document.md).

- Кнопка Filter documents (Отфильтровать документы). Это кнопка для фильтрации и поиска конкретных документов.

    ![Просмотр сведений о проекте](media/how-to/how-to-view-project.png)

## <a name="next-steps"></a>Дополнительная информация

- Узнайте, [как выполнять поиск проекта, изменять и удалять его](how-to-search-edit-delete-projects.md).
- Узнайте, [как отправлять документ](how-to-upload-document.md) для создания моделей перевода.
