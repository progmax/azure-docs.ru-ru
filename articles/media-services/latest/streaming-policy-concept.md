---
title: Политики потоковой передачи в Службах мультимедиа Azure | Документация Майкрософт
description: В этой статье приведены общие сведения о политиках потоковой передачи и их использовании в Службах мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: c5f441fef95989e5c82586d96fc6c10e00a9627c
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085501"
---
# <a name="streaming-policies"></a>Политики потоковой передачи

Политики потоковой передачи в Службах мультимедиа Azure версии 3 позволяют вам определять потоковые протоколы и параметры шифрования для StreamingLocators. Вы можете указать имя созданной вами политики потоковой передачи или использовать одну из предопределенных политик. В настоящее время доступны следующие предопределенные политики потоковой передачи: Predefined_DownloadOnly, Predefined_ClearStreamingOnly, Predefined_DownloadAndClearStreaming, Predefined_ClearKey, Predefined_MultiDrmCencStreaming и Predefined_MultiDrmStreaming.

> [!IMPORTANT]
> При использовании пользовательской политики [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) следует разработать ограниченный набор таких политик для учетной записи Служб мультимедиа и повторно использовать их для StreamingLocators всякий раз, когда требуются те же параметры шифрования и протоколы. У вашей учетной записи Служб мультимедиа есть квота на количество записей StreamingPolicy. Вы не должны создавать новый StreamingPolicy для каждого StreamingLocator.

## <a name="streamingpolicy-definition"></a>Определение политики StreamingPolicy

В следующей таблице представлены свойства StreamingPolicy и их определения.

|ИМЯ|ОПИСАНИЕ|
|---|---|
|id|Полный идентификатор ресурса.|
|name|Имя ресурса.|
|properties.commonEncryptionCbcs|Конфигурация CommonEncryptionCbcs|
|properties.commonEncryptionCenc|Конфигурация CommonEncryptionCenc|
|properties.created |Время создания политики потоковой передачи|
|properties.defaultContentKeyPolicyName |ContentKey, который по умолчанию используется текущей потоковой политикой|
|properties.envelopeEncryption  |Конфигурация EnvelopeEncryption|
|properties.noEncryption|Конфигурации NoEncryption|
|Тип|Тип ресурса.|

Полное определение см. на странице [политик потоковой передачи](https://docs.microsoft.com/rest/api/media/streamingpolicies).

## <a name="filtering-ordering-paging"></a>Фильтрации, упорядочивание, разбиение по страницам

Службы мультимедиа поддерживают следующие параметры запроса OData для политик потоковой передачи: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Описание оператора:

* Eq = равно.
* Ne = не равно.
* Ge = больше или равно.
* Le = меньше или равно.
* Gt = больше чем.
* Lt = меньше чем.

### <a name="filteringordering"></a>Фильтрация и упорядочение

В следующей таблице показано, как можно применить эти параметры к свойствам StreamingPolicy: 

|ИМЯ|Фильтр|Порядок|
|---|---|---|
|id|||
|name|Eq, ne, ge, le, gt, lt|По возрастанию и по убыванию|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|properties.created |Eq, ne, ge, le, gt, lt|По возрастанию и по убыванию|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|Тип|||

### <a name="pagination"></a>Разбиение на страницы

Для каждого из четырех порядков сортировки поддерживается разбиение на страницы. В настоящее время размер страницы составляет 10.

> [!TIP]
> Для перечисления коллекции всегда нужно использовать следующую ссылку, которая не зависит от конкретного размера страницы.

Если ответ на запрос содержит большое количество элементов, служба возвращает свойство \@odata.nextLink, чтобы получить следующую страницу результатов. Это можно использовать для просмотра всего результирующего набора. Вы не можете настроить размер страницы. 

Если StreamingPolicy создается или удаляется во время разбивки коллекции на страницы, изменения отражаются в возвращаемых результатах (если эти изменения находятся в той части коллекции, которая не была загружена). 

В следующем примере C# показано перечисление всех StreamingPolicy в учетной записи.

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

Примеры использования REST см. в статье о [перечислении политик потоковой передачи](https://docs.microsoft.com/rest/api/media/streamingpolicies/list).

## <a name="next-steps"></a>Дополнительная информация

[Краткое руководство по потоковой передаче видеофайлов — .NET](stream-files-dotnet-quickstart.md)
