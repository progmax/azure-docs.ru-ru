---
title: Написание кода для отслеживания запросов с помощью Azure Application Insights | Документация Майкрософт
description: Написание кода для отслеживания запросов с помощью Application Insights, что позволяет получить профили для ваших запросов
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 20f408d9dd32c3fd7a0e319e4051483e3aa54dd9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2018
ms.locfileid: "52722128"
---
# <a name="write-code-to-track-requests-with-application-insights"></a>Написание кода для отслеживания запросов с помощью Azure Application Insights

Чтобы просмотреть профили для приложения на странице "Производительность", Application Insights нужно отслеживать запросы для вашего приложения. Application Insights может автоматически отслеживать запросы для приложений, построенных на уже инструментированных платформах например ASP.net и ASP.Net Core. Но для других приложений, например рабочих ролей в облачной службе Azure и интерфейсов API без отслеживания состояния в Service Fabric, нужно добавить новый код, который сообщит Application Insights о начале и завершении запроса. Когда этот код будет готов, телеметрия запросов будут направляться в Application Insights, вы увидите эти данные на странице "Производительность" и для соответствующих запросов будут собираться профили. 

Ниже приведены шаги, которые позволяют вручную настроить отслеживание запросов.


  1. Добавьте следующий код в раннюю точку во времени существования приложения:  

        ```csharp
        using Microsoft.ApplicationInsights.Extensibility;
        ...
        // Replace with your own Application Insights instrumentation key.
        TelemetryConfiguration.Active.InstrumentationKey = "00000000-0000-0000-0000-000000000000";
        ```
      Дополнительные сведения об этой глобальной конфигурации ключа инструментирования см. в статье [Using Service Fabric with Application Insights](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/blob/dev/appinsights/ApplicationInsights.md) (Использование Service Fabric с Application Insights).  

  1. Для всех фрагментов кода, которые необходимо инструментировать, добавьте оператор `StartOperation<RequestTelemetry>` **USING**, как показано в следующем примере:

        ```csharp
        using Microsoft.ApplicationInsights;
        using Microsoft.ApplicationInsights.DataContracts;
        ...
        var client = new TelemetryClient();
        ...
        using (var operation = client.StartOperation<RequestTelemetry>("Insert_Your_Custom_Event_Unique_Name"))
        {
          // ... Code I want to profile.
        }
        ```

        Вызов `StartOperation<RequestTelemetry>` в другой области `StartOperation<RequestTelemetry>` не поддерживается. Вместо этого можно использовать `StartOperation<DependencyTelemetry>` во вложенной области. Например:   
        
        ```csharp
        using (var getDetailsOperation = client.StartOperation<RequestTelemetry>("GetProductDetails"))
        {
        try
        {
          ProductDetail details = new ProductDetail() { Id = productId };
          getDetailsOperation.Telemetry.Properties["ProductId"] = productId.ToString();
        
          // By using DependencyTelemetry, 'GetProductPrice' is correctly linked as part of the 'GetProductDetails' request.
          using (var getPriceOperation = client.StartOperation<DependencyTelemetry>("GetProductPrice"))
          {
              double price = await _priceDataBase.GetAsync(productId);
              if (IsTooCheap(price))
              {
                  throw new PriceTooLowException(productId);
              }
              details.Price = price;
          }
        
          // Similarly, note how 'GetProductReviews' doesn't establish another RequestTelemetry.
          using (var getReviewsOperation = client.StartOperation<DependencyTelemetry>("GetProductReviews"))
          {
              details.Reviews = await _reviewDataBase.GetAsync(productId);
          }
        
          getDetailsOperation.Telemetry.Success = true;
          return details;
        }
        catch(Exception ex)
        {
          getDetailsOperation.Telemetry.Success = false;
        
          // This exception gets linked to the 'GetProductDetails' request telemetry.
          client.TrackException(ex);
          throw;
        }
        }
        ```
