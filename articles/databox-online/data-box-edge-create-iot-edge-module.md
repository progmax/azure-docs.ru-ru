---
title: Модуль Azure IoT Edge для Azure Data Box Edge на языке C# | Документация Майкрософт
description: Сведения о разработке на языке C# модуля IoT Edge, который можно развернуть в Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 10/16/2018
ms.author: alkohli
ms.openlocfilehash: 8d4a99ab9d8107f1b3fbe70f59299f427bc88bd5
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2018
ms.locfileid: "49465896"
---
# <a name="develop-a-c-iot-edge-module-to-move-files-on-data-box-edge-preview"></a>Разработка на C# модуля IoT Edge для перемещения файлов в Data Box Edge (предварительная версия)

В этой статье описано, как создать модуль IoT Edge, предназначенный для развертывания на устройство Data Box Edge. Azure Data Box Edge — это решение хранилища, с помощью которого можно обрабатывать данные и отправлять их по сети в Azure.

Модули Azure IoT Edge можно использовать вместе с Data Box Edge, чтобы преобразовывать данные при передаче в Azure. Используемый в этой статье модуль реализует логику копирования файла из общего локального ресурса в общий облачный ресурс на устройстве Data Box Edge.

В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Создание реестра контейнеров для хранения модулей (в виде образов Docker) и управление ими.
> * Создание модуля IoT Edge, предназначенного для развертывания на устройстве Data Box Edge.

> [!IMPORTANT]
> В данный момент решение Data Box Edge находится в состоянии предварительной версии. Изучите [Дополнительные условия использования Предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), прежде чем заказывать и развертывать это решение. 

## <a name="about-the-iot-edge-module"></a>Общая информация о модулях IoT Edge

На устройстве Data Box Edge можно развертывать и выполнять модули IoT Edge. Модули Edge — это по сути контейнеры Docker, предназначенные для выполнения конкретной задачи, например для приема сообщений от устройства, преобразования сообщений или отправки сообщений в Центр Интернета вещей. В этой статье описано, как создать модуль, который копирует файлы из общего локального ресурса в общий облачный ресурс на устройстве Data Box Edge.

1. Файлы сохраняются в общий локальный ресурс на устройстве Data Box Edge.
2. Генератор файловых событий создает файловое событие для каждого файла, сохраненного в общий локальный ресурс. Эти файловые события отправляются в концентратор IoT Edge, который расположен в среде выполнения IoT Edge.

   > [!IMPORTANT]
   > Файловые события создаются только для новых файлов. Изменение существующих файлов не создает файловые события.

3. Пользовательский модуль IoT Edge обрабатывает файловое событие и создает объект файлового события с относительным путем к соответствующему файлу. Модуль вычисляет абсолютный путь, используя этот относительный путь, и копирует файл из общего локального ресурса в общий облачный ресурс. Затем модуль удаляет файл из общего локального ресурса.

![Как работает модуль Azure IoT Edge на устройстве Data Box Edge](./media/data-box-edge-create-iot-edge-module/how-module-works.png)

Как только файл попадает в общий облачный ресурс, он автоматически загружается в учетную запись хранения Azure.

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы убедитесь, что у вас есть следующие ресурсы:

- Работающее устройство Data Box Edge.

    - Связанный с этим устройством ресурс Центра Интернета вещей. Дополнительные сведения см. в статье о [создании ресурса в Центре Интернета вещей](data-box-edge-deploy-configure-compute.md#create-an-iot-hub-resource) для Data Box Edge.
    - Настроенная на устройстве роль вычислений Edge. Дополнительные сведения см. в статье о [настройке роли вычислений](data-box-edge-deploy-configure-compute.md#set-up-compute-role) на устройстве Data Box Edge.

- Ресурсы для разработки:

    - [Visual Studio Code](https://code.visualstudio.com/).
    - [C# для расширения Visual Studio Code (на платформе OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).
    - [Расширение Azure IoT Edge для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).
    - [Пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/download).
    - [Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows). Вам может потребоваться учетная запись для скачивания и установки программного обеспечения.

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Реестр контейнеров Azure — это частный реестр Docker в Azure, где можно хранить частные образы контейнеров Docker и управлять ими. Две самые популярные облачные службы реестров Docker — Реестр контейнеров Azure и Центр Docker. В этой статье используется Реестр контейнеров.

1. Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).
2. Выберите **Создать ресурс > Контейнеры > Реестр контейнеров**. Нажмите кнопку **Создать**.
3. Сделайте следующее:

    1. Создайте уникальное в пределах Azure **имя реестра**, которое содержит от 5 до 50 буквенно-цифровых символов.
    2. Выберите **подписку**.
    3. Выберите существующую **группу ресурсов** или создайте новую.
    4. Выберите **расположение**. Мы рекомендуем выбрать то же расположение, которое связано с ресурсом Data Box Edge.
    5. **Включите** параметр **Пользователь-администратор**.
    6. Для номера SKU выберите вариант **Базовый**.

    ![Создание реестра контейнеров](./media/data-box-edge-create-iot-edge-module/create-container-registry-1.png)
 
4. Нажмите кнопку **Создать**.
5. После создания реестра контейнеров просмотрите его и выберите **Ключи доступа**.

    ![Получение ключей доступа](./media/data-box-edge-create-iot-edge-module/get-access-keys-1.png)
 
6. Скопируйте значения **Сервер входа**, **Имя пользователя** и **Пароль**. Эти значения вам потребуются позже при публикации образа Docker в реестре и добавлении учетных данных реестра в среду выполнения Azure IoT Edge.


## <a name="create-an-iot-edge-module-project"></a>Создание проекта модуля IoT Edge

На следующих шагах создается проект модуля IoT Edge на основе пакета SDK для .NET Core 2.1. В этом проекте используется Visual Studio Code с расширением Azure IoT Edge.

### <a name="create-a-new-solution"></a>Создание решения

Создайте шаблон решения C#, который можно настроить с помощью собственного кода.

1. В Visual Studio Code выберите **Представление > Палитра команд**, чтобы открыть палитру команд VS Code.
2. В палитре команд введите и выполните команду **Azure: Sign in** (Azure: Вход) и следуйте инструкциям, чтобы войти в свою учетную запись Azure. Если вход был выполнен, то этот шаг можно пропустить.
3. В палитре команд введите и выполните команду **Azure IoT Edge: New IoT Edge solution**. В палитре команд укажите следующие сведения для создания решения:

    1. Выберите папку, где требуется создать решение.
    2. Введите имя своего решения или примите имя по умолчанию **EdgeSolution**.
    
        ![Создание решения 1](./media/data-box-edge-create-iot-edge-module/create-new-solution-1.png)

    3. Выберите **C# Module** (Модуль C#) в качестве шаблона модуля.
    4. Замените имя модуля, предложенное по умолчанию, любым именем на ваш выбор. В нашем примере это **FileCopyModule**.
    
        ![Создание решения 2](./media/data-box-edge-create-iot-edge-module/create-new-solution-2.png)

    5. В качестве репозитория образов первого модуля укажите Реестр контейнеров Azure, который вы создали в предыдущем разделе. Замените **localhost:5000** скопированным значением имени входа на сервер.

        Строка должна выглядеть так: `<Login server name>/<Module name>`. В нашем примере это строка `mycontreg2.azurecr.io/filecopymodule`.

        ![Создание решения 3](./media/data-box-edge-create-iot-edge-module/create-new-solution-3.png)

4. Выберите **Файл > Открыть папку**.

    ![Создание решения 4](./media/data-box-edge-create-iot-edge-module/create-new-solution-4.png)

5. Найдите и выберите папку **EdgeSolution**, которую вы создали ранее. Окно VS Code загружает рабочую область решения IoT Edge, в которой размещены пять элементов верхнего уровня. В этой статье мы не будем изменять папку **.vscode**, а также файлы **.gitignore**, **.env** и **deployment.template.json**.
    
    Сейчас из этих компонентов нас интересует только папка modules. Эта папка содержит код C# для модуля и файлы Docker для сборки модуля в образ контейнера.

    ![Создание решения 5](./media/data-box-edge-create-iot-edge-module/create-new-solution-5.png)

### <a name="update-the-module-with-custom-code"></a>Обновление модуля с помощью пользовательского кода

1. В обозревателе VS Code выберите **modules > CSharpModule > Program.cs**.
2. В верхней части **пространства имен CSharpModule** добавьте следующие операторы using для типов, которые будут использоваться позже. **Microsoft.Azure.Devices.Client.Transport.Mqtt** — это протокол отправки сообщений в центр IoT Edge.

    ```
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;
    using Newtonsoft.Json;
    ```
3. Добавьте в класс Program переменные **InputFolderPath** и **OutputFolderPath**.

    ```
    class Program
        {
            static int counter;
            private const string InputFolderPath = "/home/LocalShare";
            private const string OutputFolderPath = "/home/CloudShare";
    ````

    > [!IMPORTANT]
    > Запишите значения `InputFolderPath` и `OutputFolderPath`. Эти пути вам нужно будет указать позднее при развертывании модуля.

4. Добавьте класс **MessageBody** в класс Program. Эти классы определяют ожидаемую схему текста входящего сообщения.

    ```
    /// <summary>
    /// The MessageBody class defines the expected schema for the body of incoming messages. 
    /// </summary>
    private class FileEvent
    {
        public string ChangeType { get; set; }

        public string ShareRelativeFilePath { get; set; }

        public string ShareName { get; set; }
    }
    ```

5. В методе **Init** код создает и настраивает объект **ModuleClient**. Этот объект позволяет модулю подключаться к локальной среде выполнения Azure IoT Edge по протоколу MQTT для отправки и получения сообщений. Строка подключения, используемая в методе Init, предоставляется модулю средой выполнения IoT Edge. Этот код регистрирует обратный вызов FileCopy для получения сообщений от центра IoT Edge через конечную точку **input1**.

    ```
    /// <summary>
    /// Initializes the ModuleClient and sets up the callback to receive
    /// messages containing file event information
    /// </summary>
    static async Task Init()
    {
        MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
        ITransportSettings[] settings = { mqttSetting };

        // Open a connection to the IoT Edge runtime
        ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
        await ioTHubModuleClient.OpenAsync();
        Console.WriteLine("IoT Hub module client initialized.");

        // Register callback to be called when a message is received by the module
        await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FileCopy, ioTHubModuleClient);
    }
    ```

6. Вставьте код метода **FileCopy**.

    ```
            /// <summary>
        /// This method is called whenever the module is sent a message from the IoT Edge Hub. 
        /// This method deserializes the file event, extracts the corresponding relative file path, and creates the absolute input file path using the relative file path and the InputFolderPath.
        /// This method also forms the absolute output file path using the relative file path and the OutputFolderPath. It then copies the input file to output file and deletes the input file after the copy is complete.
        /// </summary>
        static async Task<MessageResponse> FileCopy(Message message, object userContext)
        {
            int counterValue = Interlocked.Increment(ref counter);

            try
            {
                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                if (!string.IsNullOrEmpty(messageString))
                {
                    var fileEvent = JsonConvert.DeserializeObject<FileEvent>(messageString);

                    string relativeFileName = fileEvent.ShareRelativeFilePath.Replace("\\", "/");
                    string inputFilePath = InputFolderPath + relativeFileName;
                    string outputFilePath = OutputFolderPath + relativeFileName;

                    if (File.Exists(inputFilePath))                
                    {
                        Console.WriteLine($"Moving input file: {inputFilePath} to output file: {outputFilePath}");
                        var outputDir = Path.GetDirectoryName(outputFilePath);
                        if (!Directory.Exists(outputDir))
                        {
                            Directory.CreateDirectory(outputDir);
                        }

                        File.Copy(inputFilePath, outputFilePath, true);
                        Console.WriteLine($"Copied input file: {inputFilePath} to output file: {outputFilePath}");
                        File.Delete(inputFilePath);
                        Console.WriteLine($"Deleted input file: {inputFilePath}");
                    } 
                    else
                    {
                        Console.WriteLine($"Skipping this event as input file doesn't exist: {inputFilePath}");   
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Caught exception: {0}", ex.Message);
                Console.WriteLine(ex.StackTrace);
            }

            Console.WriteLine($"Processed event.");
            return MessageResponse.Completed;
        }

    }
    ```

7. Сохраните этот файл.

## <a name="build-your-iot-edge-solution"></a>Сборка решения IoT Edge

В предыдущем разделе описано, как создать решение IoT Edge и добавить в FileCopyModule код для копирования файлов из общего локального ресурса в общий облачный ресурс. Теперь необходимо создать решение в качестве образа контейнера и передать его в реестр контейнеров.

1. Войдите в Docker, введя следующую команду в окне интегрированного терминала Visual Studio Code.

    `docker login <ACR login server> -u <ACR username>`

    Укажите имя пользователя и сервер входа, которые вы скопировали из Реестра контейнеров Azure. 

    ![Сборка и отправка решения IoT Edge](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-1.png)

2. В ответ на запрос введите пароль. Сервер входа, имя пользователя и пароль можно также получить в разделе **Ключи доступа** Реестра контейнеров на портале Azure.
 
3. Предоставив учетные данные, вы сможете отправить образ модуля в Реестр контейнеров Azure. В обозревателе VS Code щелкните правой кнопкой мыши файл **module.json** и выберите действие **Build and Push IoT Edge solution** (Собрать и отправить решение IoT Edge).

    ![Сборка и отправка решения IoT Edge](./media/data-box-edge-create-iot-edge-module/build-iot-edge-solution-2.png)
 
    Получив команду на сборку решения, Visual Studio Code выполнит в интегрированном окне терминала две команды: docker build и docker push. Они создают код, упаковывают его в контейнер CSharpModule.dll и передают в реестр контейнера, который был указан при инициализации решения.

    Возможно, вы увидите следующее предупреждение, которое можно игнорировать:

    *Program.cs(77,44): warning CS1998: This async method lacks 'await' operators and will run synchronously. Consider using the 'await' operator to await non-blocking API calls, or 'await Task.Run(...)' to do CPU-bound work on a background thread. (Program.cs(77,44): предупреждение CS1998. В асинхронном методе отсутствуют операторы await; будет выполнен синхронный запуск. Воспользуйтесь оператором await для ожидания неблокирующих вызовов API или оператором await Task.Run(...) для выполнения связанных с ЦП заданий в фоновом потоке.)*

4. Полный адрес образа контейнера с тегом можно увидеть в окне интегрированного терминала VS Code. Адрес образа создается на основе информации, сохраненной в файле module.json в формате `<repository>:<version>-<platform>`. Для этой статьи адрес должен выглядеть так: `mycontreg2.azurecr.io/filecopymodule:0.0.1-amd64`.

## <a name="next-steps"></a>Дополнительная информация

Чтобы развернуть и запустить этот модуль в Data Box Edge, воспользуйтесь инструкциями из статьи [Добавление настраиваемого модуля](data-box-edge-deploy-configure-compute.md#add-a-custom-module).
