---
title: Краткое руководство по преобразованию текста в речь на Python (служба "Речь")
titleSuffix: Azure Cognitive Services
description: В этом кратком руководстве описано, как преобразовать текст в речь с помощью Python и REST API преобразования текста в речь. В это руководство включен пример текста, оформленный по стандарту разметки SSML (Speech Synthesis Markup Language). Этот стандарт позволяет выбрать голос и язык для речевого ответа. REST API также поддерживает обычный текст (ASCII или UTF-8), но для запросов в формате обычного текста ответ всегда возвращается с голосом и языком по умолчанию, настроенным для службы "Речь".
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 11/16/2018
ms.author: erhopf
ms.openlocfilehash: 38bcd67dfb6d9b1a1955658872fee7b8b7a14a0c
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447471"
---
# <a name="quickstart-convert-text-to-speech-using-python"></a>Краткое руководство по преобразованию текста в речь с помощью Python

В этом кратком руководстве описано, как преобразовать текст в речь с помощью Python и REST API преобразования текста в речь. Текст запроса в этом руководстве оформлен по стандарту разметки [SSML (Speech Synthesis Markup Language)](speech-synthesis-markup.md), который позволяет выбрать голос и язык для ответа.

Для этого краткого руководства требуется [учетная запись Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) с ресурсом службы "Речь". Если у вас нет учетной записи, можно использовать [бесплатную пробную версию](get-started.md), чтобы получить ключ подписки.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

* Python 2.7.x или 3.x;
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) или любой другой редактор кода;
* ключ подписки Azure для службы "Речь".

## <a name="create-a-project-and-import-required-modules"></a>Создание проекта и импорт обязательных модулей

Создайте проект Python, используя любую IDE или любой текстовый редактор. Затем скопируйте в файл проекта с именем `tts.py` этот фрагмент кода.

```python
import os, requests, time
```

> [!NOTE]
> Если вы ранее не использовали эти модули, вам потребуется установить их перед запуском программы. Чтобы установить эти пакеты, выполните команду: `pip install requests`.

Эти модули используются для записи речи в файл с меткой времени, создания запроса HTTP и вызова API преобразования текста в речь.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Настройка ключа подписки и создание запроса для преобразования текста в речь

В следующих разделах описано, как создать методы для обработки авторизации, вызова API преобразования текста в речь и проверки ответа. Для начала мы добавим фрагмент, который проверяет совместимость кода с Python версии 2.7.x и 3.x.

```python
try: input = raw_input
except NameError: pass
```

Теперь давайте создадим класс. Именно в него мы поместим все методы для обмена маркерами и вызова API преобразования текста в речь.

```python
class TextToSpeech(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.tts = input("What would you like to convert to speech: ")
        self.timestr = time.strftime("%Y%m%d-%H%M")
        self.access_token = None
```

Здесь `subscription_key` обозначает уникальный ключ, полученный на портале Azure. `tts` предлагает пользователю ввести текст, который нужно преобразовать в речь. Входные данные обрабатываются как строковый литерал, поэтому символы не нужно экранировать. Наконец, `timestr` получает текущее время, которое нам потребуется для создания имени файла.

## <a name="get-an-access-token"></a>Получение маркера доступа

REST API преобразования текста в речь требует предоставить маркер доступа для аутентификации. Чтобы получить маркер доступа, нужно выполнить обмен. Этот пример меняет ключ подписки службы "Речь" на маркер доступа через конечную точку `issueToken`.

В этом примере предполагается, что подписка службы "Речь" размещена в регионе "Западная часть США". Если вы используете другой регион, измените значение `fetch_token_url`. См. [полный список регионов](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Скопируйте следующий код в класс `TextToSpeech`:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Дополнительные сведения см. в руководстве по [получению маркера доступа](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#how-to-get-an-access-token).

## <a name="make-a-request-and-save-the-response"></a>Выполнение запроса и сохранение ответа

Здесь вы создадите запрос и сохраните ответ службы "Речь". Прежде всего задайте значения `base_url` и `path`. В этом примере предполагается, что вы используете конечную точку в регионе "Западная часть США". Если ресурс зарегистрирован в другом регионе, обновите значение `base_url`. Дополнительные сведения см. в [списке регионов службы "Речь"](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Теперь следует добавить в запрос обязательные заголовки. Обязательно сохраните в `User-Agent` имя ресурса (размещенного на портале Azure), а в `X-Microsoft-OutputFormat` укажите нужный аудиовыход. См. [полный список форматов аудиовыхода](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#audio-outputs).

Затем создайте текст запроса с соблюдением синтаксиса SSML. Этот пример определяет структуру и применяет входные данные `tts`, которые вы создали ранее.

>[!NOTE]
> В этом примере используется голос `ZiraRUS`. См. [полный список предоставляемых корпорацией Майкрософт голосов и языков](language-support.md).
> Если вы хотите создать для своего бренда уникальный и узнаваемый голос, см. руководство по [созданию пользовательского голоса](how-to-customize-voice-font.md).

И наконец, направьте запрос в службу. Если запрос выполнен успешно, возвращается код состояния 200 и речевой ответ сохраняется в файл с меткой времени.

Скопируйте следующий код в класс `TextToSpeech`:

```python
def save_audio(self):
    base_url = 'https://westus.tts.speech.microsoft.com/'
    path = 'cognitiveservices/v1'
    constructed_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token,
        'Content-Type': 'application/ssml+xml',
        'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
        'User-Agent': 'YOUR_RESOURCE_NAME',
        'cache-control': 'no-cache'
    }
    body = "<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>" + self.tts + "</voice></speak>"

    response = requests.post(constructed_url, headers=headers, data=body)
    if response.status_code == 200:
        with open('sample-' + self.timestr + '.wav', 'wb') as audio:
            audio.write(response.content)
            print("\nStatus code: " + str(response.status_code) + "\nYour TTS is ready for playback.\n")
    else:
        print("\nStatus code: " + str(response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")

```

## <a name="put-it-all-together"></a>Сборка

Осталось совсем немного. Последним шагом будет создание экземпляра класса и вызов функций.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = TextToSpeech(subscription_key)
    app.get_token()
    app.save_audio()
```

## <a name="run-the-sample-app"></a>Запуск примера приложения

Итак, все готово к запуску примера приложения преобразования текста в речь. В командной строке или сеансе терминала перейдите к каталогу проекта и выполните следующую команду:

```console
python tts.py
```

Когда появится запрос, введите любой текст для преобразования в речь. Если все пройдет успешно, готовый речевой файл будет размещен в папке проекта. Воспроизведите его в любом проигрывателе мультимедиа.

## <a name="clean-up-resources"></a>Очистка ресурсов

Удалите из исходного кода примера приложения все конфиденциальные сведения, например ключи подписки.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Справочник по API преобразования текста в речь](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)

## <a name="see-also"></a>См. также

* [Создание настраиваемого голоса](how-to-customize-voice-font.md)
* [Запись примеров голоса для создания пользовательских голосовых моделей](record-custom-voice-samples.md)
