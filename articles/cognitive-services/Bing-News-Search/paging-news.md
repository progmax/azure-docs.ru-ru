---
title: Как разбить на страницы список доступных новостных статей — Поиск новостей Bing
titlesuffix: Azure Cognitive Services
description: Показано, как разбить на страницы список всех новостных статей, которые может вернуть API Bing для поиска новостей.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 0507f2cfb1d75025d1b6aadccc442326a52ceebc
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50739810"
---
# <a name="paging-news"></a>Разбиение новостей по страницам

Когда вы вызываете API Bing для поиска новостей, Bing возвращает список результатов. Список — это подмножество общего количества результатов, относящихся к запросу. Чтобы получить предполагаемое общее количество доступных результатов, необходимо обратиться к полю [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) объекта ответа.  
  
В следующем примере показано поле `totalEstimatedMatches`, содержащееся в результатах поиска новостей.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Чтобы разбить на страницы список доступных статей, используйте параметры запроса [count](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) и [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset).  
  
Параметр `count` задает количество результатов, возвращаемых в ответе. Максимальное количество результатов, которые можно запросить в ответе, равно 100. По умолчанию задано значение 10. Фактическое число полученных результатов может быть меньше запрошенного.

Параметр `offset` задает количество пропускаемых результатов. Значение `offset` начинается с нуля и должно быть меньше (`totalEstimatedMatches` - `count`).  

Если вы хотите отображать 20 статей на странице, необходимо задать для параметра `count` значение 20, а для параметра `offset` — значение 0, чтобы получить первую страницу результатов. Для каждой последующей страницы значение параметра `offset` увеличивается на 20 (например, 20, 40).  
  
В следующем примере показано, как запрашиваются 20 новостных статей с начальным смещением в 40 результатов.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Если значение по умолчанию `count` подходит для вашей реализации, необходимо указать только параметр запроса `offset`, как показано в следующем примере:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Разбиение на страницы доступно только для поиска новостей (/news/search) и не применимо к темам, набирающим популярность (/news/trendingtopics), или категориям новостей (/news).

> [!NOTE]
> Поле `TotalEstimatedAnswers` содержит приблизительное общее число результатов поиска, которые вы можете получить для текущего запроса.  При задании параметров `count` и `offset` число `TotalEstimatedAnswers` может измениться. 