---
title: Создание первой функции в Azure с помощью Java и Maven | Документация Майкрософт
description: Создание и публикация в Azure с Java и Maven простой функции, активируемой с помощью HTTP.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: функции azure, функции, обработка событий, вычисления, независимая архитектура
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fdd29bbfaf36619fd823220e5d32a48a1619679b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842073"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Создание первой функции с помощью Java и Maven (предварительная версия)

> [!NOTE] 
> Сейчас доступна предварительная версия Java для Функций Azure.

В этом кратком руководстве содержится информация о том, как создать проект функции [без сервера](https://azure.microsoft.com/solutions/serverless/) с помощью Maven, локально протестировать и развернуть его в Azure. По завершении проекта код функции на Java будет выполняться в облаке и может запускаться из HTTP-запроса.

![Доступ к функции Hello World из командной строки с помощью cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования
Для разработки функций приложения с помощью Java, должны быть установлены следующие компоненты:

-  [Java Developer Kit (JDK)](https://www.azul.com/downloads/zulu/) версии 8.
-  [Apache Maven](https://maven.apache.org) 3.0 или более поздней версии.
-  [Интерфейс командной строки Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Переменной среде JAVA_HOME необходимо присвоить расположение установки JDK, чтобы завершить выполнение заданий этого краткого руководства.

## <a name="install-the-azure-functions-core-tools"></a>Установка основных инструментов Функций Azure

[Основные средства Функций Azure версии 2.0](https://www.npmjs.com/package/azure-functions-core-tools) предоставляют локальной среде разработки возможность записи, выполнения и отладки Функций Azure. 

Для установки посетите раздел об [установке](https://github.com/azure/azure-functions-core-tools#installing) в проекте "Основные инструменты службы «Функции Azure»", чтобы найти конкретные инструкции для вашей операционной системы.

Кроме того, можно установить средства вручную с помощью [npm](https://www.npmjs.com/) в составе [Node.js](https://nodejs.org/), установив следующие требуемые компоненты:

-  [.NET Core](https://www.microsoft.com/net/core) последней версии.
-  [Node.js ](https://nodejs.org/download/)версии 8.6 или выше.

Чтобы продолжить установку при помощи npm, выполните следующую команду:

```
npm install -g azure-functions-core-tools@core
```

> [!NOTE]
> Если возникли проблемы при установке основных средств Функций Azure версии 2.0, см. раздел [Среда выполнения версии 2.x](/azure/azure-functions/functions-run-local#version-2x-runtime).

## <a name="generate-a-new-functions-project"></a>Создание нового проекта функций

В пустой папке выполните следующую команду, чтобы создать проект функций из [архетипа Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven запросит значения, необходимые для завершения создания проекта. Дополнительную информацию о значениях _groupId_, _artifactId_ и о _версии_ см. по ссылке [соглашения об именовании Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html). Значение _appName_ должно быть уникальным в Azure, поэтому Maven создает имя приложения на основе ранее заданного по умолчанию _artifactId_. Значение _ackageName_ определяет пакет Java для создаваемого кода функции.

Идентификаторы `com.fabrikam.functions` и `fabrikam-functions` ниже используются в качестве примера и для понимания дальнейших шагов в этом кратком руководстве. Рекомендуется ввести собственные значения для Maven на этом шаге.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven создает файлы проекта в новой папке с именем _artifactId_, в этом примере — `fabrikam-functions`. Созданный в проекте и готовый для запуска код — это простая функция [активации HTTP](/azure/azure-functions/functions-bindings-http-webhook), возвращающая текст запроса.

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>Локальное выполнение функции

Измените каталог на только что созданную папку проекта, создайте и запустите функцию с помощью Maven.

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> При возникновении исключения `javax.xml.bind.JAXBException` с Java 9 инструкции по его устранению см. на [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Вы увидите эти выходные данные, если функция выполняется локально на компьютере и готова отвечать на запросы HTTP:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Вызовите функцию из командной строки, используя curl в новом окне терминала:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

Используйте `Ctrl-C` в терминале, чтобы остановить код функции.

## <a name="deploy-the-function-to-azure"></a>Развертывание функции для Azure

В процессе развертывания для Функций Azure используются данные учетной записи из Azure CLI. Прежде чем продолжить, [войдите с помощью Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

```azurecli
az login
```

Разверните свой код в новом приложении-функции, используя целевой объект Maven `azure-functions:deploy`.

```
mvn azure-functions:deploy
```

После завершения развертывания появится URL-адрес, который можно использовать для доступа к приложению-функции Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Проверьте приложение-функцию, работающее в Azure, используя `cURL`. Необходимо изменить URL-адрес из примера ниже, чтобы он соответствовал развернутому URL-адресу приложения-функции из предыдущего шага.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Внесение изменений и повторное развертывание

Измените исходный файл `src/main.../Function.java` в созданном проекте, чтобы изменить текст, возвращаемый приложением-функцией. Измените эту строку:

```java
return request.createResponse(200, "Hello, " + name);
```

Вместо нее укажите следующую:

```java
return request.createResponse(200, "Hi, " + name);
```

Сохраните изменения и выполните повторное развертывание, запустив `azure-functions:deploy` в окне терминала, как и в прошлый раз. Приложение-функция и этот запрос обновятся:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

Появятся обновленные выходные данные:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Дополнительная информация

Вы создали функцию-приложение Java с помощью простого триггера HTTP и развернули его в Функциях Azure.

- Дополнительные сведения о разработке функции Java см. в статье [Azure Functions Java developer guide](functions-reference-java.md) (Руководство разработчика Java для Функций Azure).
- Добавьте в проект дополнительные функции с помощью различных триггеров целевого объекта Maven`azure-functions:add`.
- Создавайте и отлаживайте функции локально с помощью [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) и [Eclipse](functions-create-maven-eclipse.md). 
- Отлаживайте функции, развернутые в Azure, с помощью Visual Studio Code. Инструкции см. в документации Visual Studio Code по [бессерверным приложениям Java](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud).
