---
title: Краткое руководство. Транслитерация текста, C# — API перевода текстов
titleSuffix: Azure Cognitive Services
description: Из этого краткого руководства вы узнаете, как транслитерировать (преобразовывать) текст из одного набора символов в другой с помощью .NET Core и REST API перевода текстов. В этом примере японский текст транслитерируется в текст на латинице.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 11/21/2018
ms.author: erhopf
ms.openlocfilehash: d67fbdad32612f23b1de31f7d2a1d7b4dee3b68e
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2018
ms.locfileid: "52889146"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-c"></a>Краткое руководство. Транслитерация текста с помощью API перевода текстов и C#

Из этого краткого руководства вы узнаете, как транслитерировать (преобразовывать) текст из одного набора символов в другой с помощью .NET Core (C#) и REST API перевода текстов. В приведенном примере японский текст транслитерируется в текст на латинице.

Для этого краткого руководства требуется [учетная запись Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с ресурсом API перевода текстов. Если у вас нет учетной записи, можно использовать [бесплатную пробную версию](https://azure.microsoft.com/try/cognitive-services/), чтобы получить ключ подписки.

## <a name="prerequisites"></a>Предварительные требования

* [ПАКЕТ SDK .NET](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [пакет NuGet .NET для JSON](https://www.nuget.org/packages/Newtonsoft.Json/);
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) или любой другой редактор кода;
* ключ подписки Azure для API перевода текстов.

## <a name="create-a-net-core-project"></a>Создание проекта .NET Core

Откройте командную строку или сеанс терминала и выполните следующие команды:

```console
dotnet new console -o transliterate-sample
cd transliterate-sample
```

С помощью первой команды создается консольное приложение .NET и каталог с именем `transliterate-sample`. Вторая команда позволяет изменить каталог для проекта.

Вам нужно будет установить модуль вручную. В каталоге проекта выполните следующую команду:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Добавление обязательных пространств имен в проект

С помощью команды `dotnet new console`, которую вы выполнили ранее, был создан проект, включая `Program.cs`. Этот файл находится там, где будет размещаться код приложения. Откройте файл `Program.cs` и замените существующие инструкции using. Эти операторы обеспечивают доступ ко всем типам, требуемым для создания и запуска примера приложения.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-transliterate-text"></a>Создание функции для транслитерации текста

В классе `Program` создайте функцию `TransliterateText`. Этот класс позволяет инкапсулировать код, используемый для вызова ресурса Transliterate, и вывести результат в консоль.

```csharp
static void TransliterateText()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Выбор ключа подписки, имени узла и пути

Добавьте эти строки в функцию `TransliterateText`. Вы заметите, что кроме `api-version` в `route` добавлены два дополнительных параметра. Эти параметры позволяют указать язык ввода и наборы символов для транслитерации. В этом примере в качестве значений для параметров указан японский язык (`jpan`) и латиница (`latn`). Обязательно обновите значение ключа подписки.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/transliterate?api-version=3.0&language=ja&fromScript=jpan&toScript=latn";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Далее необходимо создать и сериализовать объект JSON, содержащий текст для транслитерации. Учитывайте, что в массив `body` можно передать несколько объектов.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"こんにちは" } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>Создание экземпляра клиента и выполнение запроса

С помощью этих строк создается экземпляр `HttpClient` и `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Создание запроса и вывод ответа

При использовании `HttpRequestMessage` выполняются следующие операции:

* объявление метода HTTP;
* создание URI запроса;
* вставка текста запроса (сериализованный объект JSON);
* добавление требуемых заголовков;
* выполнение асинхронного запроса;
* Вывод ответа

Добавьте следующий код в `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Сборка

Последний этап — вызов `TransliterateText()` в функции `Main`. Найдите `static void Main(string[] args)` и добавьте следующие строки:

```csharp
TransliterateText();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Запуск примера приложения

Теперь все готово к запуску примера приложения. В командной строке или сеансе терминала перейдите к каталогу проекта и выполните следующую команду:

```console
dotnet run
```

## <a name="sample-response"></a>Пример ответа

```json
[
    {
        "script": "latn",
        "text": "konnnichiha"
    }
]
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите все конфиденциальные сведения из исходного кода примера приложения, например ключи подписки.

## <a name="next-steps"></a>Дополнительная информация

Ознакомьтесь с примерами кода из этого и других кратких руководств, которые демонстрируют перевод и определение языка, а также с другими примерами проектов для API перевода текстов на сайте GitHub.

> [!div class="nextstepaction"]
> [Ознакомиться с примерами на C# на сайте GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>См. также

* [перевод текста](quickstart-csharp-translate.md);
* [определение языка по входным данным](quickstart-csharp-detect.md);
* [получение вариантов перевода](quickstart-csharp-dictionary.md);
* [получение списка поддерживаемых языков](quickstart-csharp-languages.md);
* [определение длины предложения на основе входных данных](quickstart-csharp-sentences.md).
