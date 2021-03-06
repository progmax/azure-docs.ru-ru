---
title: Краткое руководство по управлению устройством из Центра Интернета вещей (Python) | Документация Майкрософт
description: 'В этом кратком руководстве описано, как запустить два примера приложений Python: внутреннее приложение, которое может удаленно управлять подключенными к центру устройствами, и приложение, которое имитирует подключенное к центру устройство, которым можно управлять удаленно.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 08b2018ec1f1d34291778df0fa217b874cc3ffab
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515103"
---
# <a name="quickstart-control-a-device-connected-to-an-iot-hub-python"></a>Краткое руководство по управлению подключенным к Центру Интернета вещей устройством (Python)

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

Центр Интернета вещей — это служба Azure, которая позволяет получать большие объемы данных телеметрии с устройств Центра Интернета вещей в облаке и управлять устройствами из облака. В этом кратком руководстве управление имитированным устройством, подключенным к Центру Интернета вещей, осуществляется с помощью *прямого метода*. Этот метод позволяет удаленно изменить поведение подключенного к Центру Интернета вещей устройства.

В этом кратком руководстве используется два предварительно созданных приложения Python:

* Приложение имитированного устройства, реагирующее на прямые методы, вызванные из внутреннего приложения. Чтобы получать вызовы прямого метода, это приложение подключается к конечной точке конкретного устройства в Центре Интернета вещей.

* Внутреннее приложение, вызывающее прямые методы в имитированном устройстве. Чтобы вызвать прямой метод в устройстве, это приложение подключается к конечной точке на стороне службы в Центре Интернета вещей.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Примеры приложений, запускаемые в рамках этого краткого руководства, написаны на языке Python. На компьютере разработки должна быть установлена среда Python 2.7.x или 3.5.x.

Python, предназначенный для нескольких платформ, можно скачать на сайте [Python.org](https://www.python.org/downloads/).

Текущую версию Python на компьютере, на котором ведется разработка, можно проверить, выполнив одну из следующих команд:

```python
python --version
```

```python
python3 --version
```

Если вы это еще не сделали, загрузите пример проекта Python по адресу https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip и извлеките ZIP-архив.

## <a name="create-an-iot-hub"></a>Создание Центра Интернета вещей

Если вы закончили работу с предыдущим [руководством по отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-python.md), этот шаг можно пропустить.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Регистрация устройства

Если вы закончили работу с предыдущим [руководством по отправке данных телеметрии с устройства в Центр Интернета вещей](quickstart-send-telemetry-python.md), этот шаг можно пропустить.

Устройство должно быть зарегистрировано в Центре Интернета вещей, прежде чем оно сможет подключиться. В этом кратком руководстве для регистрации имитируемого устройства используется Azure Cloud Shell.

1. Выполните приведенные ниже команды в Azure Cloud Shell, чтобы добавить расширение CLI Центра Интернета вещей и создать удостоверение устройства. 

    **YourIoTHubName.** Замените этот заполнитель именем центра Интернета вещей.

    **MyPythonDevice** — это имя, присвоенное зарегистрированному устройству. Используйте MyPythonDevice, как показано ниже. Если вы выбрали другое имя для устройства, используйте его при работе с этим руководством и обновите имя устройства в примерах приложений перед их запуском.

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

2. Выполните следующую команду в Azure Cloud Shell, чтобы получить _строку подключения_ зарегистрированного устройства:

    **YourIoTHubName.** Замените этот заполнитель именем центра Интернета вещей.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Запишите строку подключения устройства, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства.

3. Чтобы разрешить внутреннему приложению подключаться к Центру Интернета вещей и получать сообщения, вам необходима _строка подключения к службе_. Следующая команда извлекает строку подключения службы для Центра Интернета вещей:

    **YourIoTHubName.** Замените этот заполнитель именем центра Интернета вещей.

    ```azurecli-interactive
    az iot hub show-connection-string \
      --hub-name YourIoTHubName \
      --output table
    ```

    Запишите строку подключения к службе, которая выглядит так:

   `HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={YourSharedAccessKey}`

    Это значение понадобится позже в рамках этого краткого руководства. Строка подключения к службе отличается от строки подключения к устройству.

## <a name="listen-for-direct-method-calls"></a>Ожидание передачи данных при вызове прямого метода

Приложение имитированного устройства подключается к конечной точке конкретного устройства в Центре Интернета вещей, отправляет имитированные данные телеметрии и ожидает передачи данных при вызове прямого метода из центра. В рамках этого краткого руководства вызов прямого метода из центра инициирует изменение интервала, с которым устройство отправляет данные телеметрии. После выполнения прямого метода имитированное устройство отправляет подтверждение в центр.

1. В окне терминала на локальном компьютере перейдите в корневую папку примера проекта Python. Затем перейдите в папку **iot-hub\Quickstarts\simulated-device-2**.

1. Откройте файл **SimulatedDevice.py** в любом текстовом редакторе.

    Замените значение переменной `CONNECTION_STRING` записанной ранее строкой подключения к устройству. Сохраните изменения в файле **SimulatedDevice.py**.

1. Установите необходимые библиотеки для приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Запустите приложение имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение имитированного устройства отправляет данные телеметрии в Центр Интернета вещей:

    ![Запуск виртуального устройства](./media/quickstart-control-device-python/SimulatedDevice-1.png)

## <a name="call-the-direct-method"></a>Вызов прямого метода

Внутреннее приложение подключается к конечной точке на стороне службы в Центре Интернета вещей. Приложение выполняет вызовы прямых методов к устройству через Центр Интернета вещей и ожидает подтверждений. Внутреннее приложение Центра Интернета вещей обычно работает в облаке.

1. В другом окне терминала на локальном компьютере перейдите в корневую папку примера проекта Python. Затем перейдите в папку **iot-hub\Quickstarts\back-end-application**.

1. Откройте файл **BackEndApplication.py** в любом текстовом редакторе.

    Замените значение переменной `CONNECTION_STRING` записанной ранее строкой подключения к службе. Сохраните изменения в файле **BackEndApplication.py**.

1. Установите необходимые библиотеки для приложения имитированного устройства, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    pip install azure-iothub-service-client future
    ```

1. Запустите внутреннее приложение, выполнив в окне терминала на локальном компьютере следующие команды:

    ```cmd/sh
    python BackEndApplication.py
    ```

    На следующем снимке экрана показан пример выходных данных, когда приложение выполняет вызов прямого метода к устройству и получает подтверждение:

    ![Запуск внутреннего приложения](./media/quickstart-control-device-python/BackEndApplication.png)

    После запуска внутреннего приложения в окне консоли, в котором выполняется имитированное устройство, отобразится сообщение и изменится интервал, с которым это приложение отправляет сообщения:

    ![Изменения в имитированном клиенте](./media/quickstart-control-device-python/SimulatedDevice-2.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Дополнительная информация

В рамках этого краткого руководства вы вызвали прямой метод в устройстве из внутреннего приложения, а также ответили на этот вызов в приложении имитированного устройства.

Чтобы узнать, как маршрутизировать сообщения с устройства в облако в разные расположения в облаке, перейдите к следующему руководству.

> [!div class="nextstepaction"]
> [Маршрутизация сообщений с помощью Центра Интернета вещей (Java)](tutorial-routing.md)