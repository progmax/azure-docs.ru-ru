---
title: 'Краткое руководство: создание запроса для визуального поиска, Node.js — Визуальный поиск Bing'
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как отправить изображение в API визуального поиска Bing и получить аналитические сведения об этом изображении.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 553d068d70f7e722f3c8e4de3978f3583b941963
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442539"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Краткое руководство: ваш первый запрос в API визуального поиска Bing на JavaScript

API Bing для наглядного поиска возвращает сведения об изображении, которое вы предоставляете. Изображение можно предоставить с помощью URL-адреса изображения, токена аналитики или через отправку изображения. Сведения об этих параметрах см. в статье [What is Bing Visual Search API?](../overview.md) (Что такое API Bing для наглядного поиска?) В этой статье показано, как отправлять изображения. Отправка изображения может быть полезна в сценариях с использованием мобильных устройств, когда вы фотографируете какую-то достопримечательность и получаете сведения о ней. Например, аналитические сведения могут содержать любопытные факты об этой достопримечательности. 

При отправке локального изображения отображаются данные формы, которые необходимо включить в текст запроса POST. Эти данные формы должны содержать заголовок Content-Disposition. Его параметру `name` необходимо присвоить значение "image", а для параметра `filename` можно задать любую строку. Содержимым формы является двоичный файл изображения. Максимально допустимый размер отправляемого изображения — 1 МБ. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

В этой статье описано простое консольное приложение, которое отправляет запрос к API Bing для визуального поиска и отображает результаты поиска в формате JSON. Хотя это приложение написано на JavaScript, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать JSON. 

## <a name="prerequisites"></a>Предварительные требования
Для выполнения действий, описанных в этом кратком руководстве, нужно создать подписку в ценовой категории S9, как указано на странице [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Чтобы создать подписку на портале Azure:
1. Введите запрос BingSearchV7 в строке поиска с текстом `Search resources, services, and docs` в верхней части страницы на портале Azure.  
2. В меню Marketplace в раскрывающемся списке выберите `Bing Search v7`.
3. В поле `Name` (Имя) введите имя нового ресурса.
4. Выберите подписку `Pay-As-You-Go` (Оплата по мере использования).
5. Выберите ценовую категорию `S9`.
6. Щелкните `Enable` (Активировать), чтобы создать подписку.

Для выполнения этого кода требуется [Node.js 6](https://nodejs.org/en/download/).

## <a name="running-the-application"></a>Запуск приложения

Ниже показано, как отправить сообщение с использованием FormData в Node.js.

Чтобы запустить это приложение, сделайте следующее:

1. Создайте папку для проекта (либо воспользуйтесь выбранной интегрированной средой разработки или редактором).
2. Из командной строки или терминала перейдите в эту папку.
3. Установите модули request:  
  ```  
  npm install request  
  ```  
3. Установите модули form-data:  
  ```  
  npm install form-data  
  ```  
4. Создайте файл с именем GetVisualInsights.js и добавьте в него следующий код.
5. Замените значение `subscriptionKey` своим ключом подписки.
6. Замените значение `imagePath` путем к отправляемому изображению.
7. Запустите программу.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Дополнительная информация

[Получение полезных сведений об изображении с помощью токена аналитики](../use-insights-token.md)  
[Руководство по передаче изображения в Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Руководство по одностраничным веб-приложениям для Bing Visual Search](../tutorial-bing-visual-search-single-page-app.md)  
[Общие сведения об API Bing для наглядного поиска](../overview.md)  
[Попробовать](https://aka.ms/bingvisualsearchtryforfree)  
[Получить ключ доступа бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Справочник по API Bing для наглядного поиска](https://aka.ms/bingvisualsearchreferencedoc)
