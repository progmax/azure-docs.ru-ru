---
title: включение файла
description: включение файла
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 10/29/2018
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 900d75f826830ea7336044a892506d3bec546e30
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283936"
---
## <a name="download-the-source-code"></a>Скачивание исходного кода

Репозиторий исходного кода для удаленного мониторинга включает в себе файлы конфигурации Docker, необходимые для запуска образов Docker микрослужб.

Чтобы клонировать и создать локальную версию репозитория, используйте среду командной строки для перехода в подходящую папку на локальном компьютере. Затем запустите один из следующих наборов, чтобы клонировать репозиторий .NET или Java.

Чтобы загрузить последнюю версию реализаций микрослужбы .NET, выполните:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

Чтобы загрузить последнюю версию реализаций микрослужбы Java, выполните:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Эти команды загружают исходный код для всех микрослужб в дополнение к сценариям, которые используются для локального запуска микрослужб. Хотя для запуска микрослужб в Docker не требуется исходный код, он полезен на случай, если в дальнейшем планируется изменить акселератор решений и протестировать изменения локально.

## <a name="deploy-the-azure-services"></a>Развертывание служб Azure

Несмотря на то что в этой статье показано, как запускать микрослужбы локально, они зависят от служб Azure, выполняемых в облаке. Воспользуйтесь приведенным ниже скриптом для развертывания служб Azure. В следующих примерах сценария предполагается, что вы используете репозиторий .NET на компьютере Windows. Если вы работаете в другой среде, измените пути, расширения файлов и разделители путей соответствующим образом.

### <a name="create-new-azure-resources"></a>Создание ресурсов Azure

Если вы еще не создали необходимые ресурсы Azure, выполните следующие действия:

1. В среде командной строки перейдите в папку**\services\scripts\local\launch** в копии репозитория.

1. Выполните следующие команды для установки инструмента CLI **pcs** и войдите в вашу учетную запись Azure.

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Запустите скрипт **start.cmd**. Скрипт запрашивает следующие сведения:
    * имя решения;
    * Подписка Azure, которую нужно использовать.
    * расположение центра обработки данных Azure, который будет использоваться.

    Сценарий создает в Azure группу ресурсов с именем решения. Эта группа содержит ресурсы Azure, которые использует акселератор решений. Если соответствующие ресурсы больше не нужны, группу ресурсов можно удалить.

    Скрипт также добавляет набор переменных среды с префиксом **PCS** на ваш локальный компьютер. При локальном запуске контейнеры Docker считывают значения конфигурации с этих переменных среды.

> [!TIP]
> Когда выполнение сценария будет завершено, появится список переменных сред. Если эти значения сохранить в файле **services\\scripts\\local\\.env**, то их можно использовать в будущем для развертываний акселератора решений. Обратите внимание, что при запуске **docker-compose** набор переменных среды на вашем компьютере переопределяет значения в файле **services\\scripts\\local\\.env**.

### <a name="use-existing-azure-resources"></a>Использование существующих ресурсов Azure

Если необходимые ресурсы Azure уже существуют, создайте соответствующие переменные среды на вашем компьютере. Возможно, вы сохранили эти значения в файле **services\\scripts\\local\\.env** во время последнего развертывания. Обратите внимание, что при запуске **docker-compose** набор переменных среды на вашем компьютере переопределяет значения в файле **services\\scripts\\local\\.env**.