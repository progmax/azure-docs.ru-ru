---
title: Краткое руководство. Отправка запроса в API Bing Local Business Search с помощью Python | Документация Майкрософт
titleSuffix: Azure Cognitive Services
description: Из этой статьи вы узнаете, как использовать API Bing Local Business Search с помощью Python.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: d48f24048225f4a0571915c2e27b931f858e8f6e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316979"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Краткое руководство. Отправка запроса в API Bing Local Business Search с помощью Python

Используйте это краткое руководство, чтобы начать отправку запросов в API Bing Local Business Search в Cognitive Services. Хотя это простое приложение написано на Python, API является веб-службой RESTful, совместимой с любым языком программирования, который может выполнять HTTP-запросы и анализировать данные в формате JSON.

В этом примере приложения из API извлекаются сведения о местных компаниях по поисковому запросу `hotel in Bellevue`.

## <a name="prerequisites"></a>Предварительные требования

* [Python](https://www.python.org/) 2.x или 3.x.
 
Необходима [учетная запись API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с API Bing. Для этого краткого руководства достаточно [бесплатной пробной версии](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api). Используйте ключ доступа, предоставляемый в бесплатной пробной версии.  См. также [Цены на Cognitive Services. API-интерфейсы поиска Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Запуск готового приложения

Следующий код получает локализованные результаты. Он реализуется с помощью следующих действий:
1. Объявите переменные для указания конечной точки с помощью узла и пути.
2. Задайте параметр запроса. 
3. Определите функцию Поиска, которая создает запрос и добавляет заголовок Ocp-Apim-Subscription-Key.
4. Задайте заголовок Ocp-Apim-Subscription-Key. 
5. Установите подключение и отправьте запрос.
6. Выведите на экран результаты JSON.

Полный код этого примера выглядит так:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com/bing'
path = '/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````

## <a name="next-steps"></a>Дополнительная информация
- [Краткое руководство. Использование Local Business Search с помощью Java](local-search-java-quickstart.md)
- [Краткое руководство. Использование Local Business Search с помощью C#](local-quickstart.md)
- [Краткое руководство. Использование Local Business Search с помощью Node](local-search-node-quickstart.md)
