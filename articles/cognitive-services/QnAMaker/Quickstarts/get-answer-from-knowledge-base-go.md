---
title: Краткое руководство. Получение ответа из базы знаний в QnA Maker с помощью REST (Go)
titlesuffix: Azure Cognitive Services
description: В этом кратком руководстве по Go для REST описывается, как программным способом получить ответ из базы знаний.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: diberry
ms.openlocfilehash: 54c2133e35de09cae411f3bcec9f4421e4586f18
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977824"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-go"></a>Получение ответов на вопрос из базы знаний с помощью Go

В этом кратком руководстве описано, как программным способом получить ответ из базы знаний QnA Maker. Служба QnA Maker автоматически извлекает вопросы и ответы из частично структурированного содержимого, например со страниц с вопросами и ответами, [источников данных](../Concepts/data-sources-supported.md). Вопрос в формате JSON отправляется в тексте запроса API. 

## <a name="prerequisites"></a>Предварительные требования

* [Go 1.10.1](https://golang.org/dl/).
* [Visual Studio Code](https://code.visualstudio.com/)
* У вас должна быть [служба QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Чтобы получить ключ для ресурса QnA Maker, выберите **Ключи** в разделе **Управление ресурсами** на панели мониторинга Azure. 
* Параметры страницы **Публикация**. Если у вас нет опубликованной базы знаний, создайте новую, импортируйте ее на странице **Параметры**, а затем опубликуйте ее. Можно скачать и использовать [этот пример базы знаний](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv). 

    На странице параметров публикации представлены значения Host, EndpointKey и маршрута POST. 

    ![Параметры публикации](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Код для этого краткого руководства доступен в репозитории [https://github.com/Azure-Samples/cognitive-services-qnamaker-Go](https://github.com/Azure-Samples/cognitive-services-qnamaker-Go/tree/master/documentation-samples/quickstarts/get-answer). 

## <a name="create-a-go-file"></a>Создание файла Go

Создайте в VSCode файл с именем `get-answer.go` и добавьте следующий класс:

```Go
package main

func main() {

}
```

## <a name="add-the-required-dependencies"></a>Добавление необходимых зависимостей

Над функцией `main` в верхней части файла `get-answer.go` добавьте необходимые зависимости в проект:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=3-9 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Добавление необходимых констант

В верхней части функции `main` добавьте необходимые константы для обеспечения доступа к QnA Maker. Эти значения будут доступны на странице **Публикация** после публикации базы знаний. 

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=17-33 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Добавление запроса POST для отправки вопроса и получения ответа

Следующий код выполняет HTTPS-запрос к API службы QnA Maker, чтобы отправить вопрос в базу знаний и получить ответ:

[!code-go[Add a POST request to send question to knowledge base](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to send question to knowledge base")]

Значение заголовка `Authorization` содержит строку `EndpointKey `. 

## <a name="build-and-run-the-program"></a>Сборка и запуск проекта

Выполните сборку и запуск программы из командной строки. Запрос к API службы QnA Maker будет отправлен автоматически, а полученный ответ отобразится в окне консоли.

1. Выполните сборку файла:

    ```bash
    go build get-answer.go
    ```

1. Выполните запуск файла:

    ```bash
    ./get-answer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [QnA Maker (V4) REST API Reference](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) (Справочник по API REST QnA Maker (V4))