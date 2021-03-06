---
title: Подключение устройства DevKit к приложению Azure IoT Central | Документация Майкрософт
description: Вы узнаете, как разработчик устройства может подключить устройство MXChip IoT DevKit к приложению Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: dccbd2d87b5a5616c25caed070a337eff9fa753e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956658"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Подключение устройства MXChip IoT DevKit к приложению Azure IoT Central

В этой статье описано, каким образом разработчик устройства может подключить устройство MXChip IoT DevKit (DevKit) к приложению Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Перед началом работы

Чтобы выполнить действия, описанные в этой статье, необходимо следующее:

1. Приложение Azure IoT Central, созданное на основе шаблона приложения **Sample Devkits** (Образец Devkits). Дополнительные сведения см. в [кратком руководстве по созданию приложения](quick-deploy-iot-central.md).
1. Устройство DevKit. Чтобы приобрести устройство DevKit, посетите сайт [MXChip IoT DevKit](http://mxchip.com/az3166).


## <a name="sample-devkits-application"></a>Пример **приложения Devkits**

Приложение, созданное из шаблона приложения **Sample Devkits** (Образец Devkits), включает в себя шаблон приложения **MXChip** со следующими характеристиками: 

- Телеметрия, содержащая данные об измерении **влажности**, **температуры**, **давления**, **манометра** (измеряется по оси X, Y, Z), **акселерометра** (измеряется по оси X, Y, Z) и **гироскопа** (измеряется по оси X, Y, Z) устройства.
- Состояние, которое содержит пример измерения **состояния устройства**.
- Измерение события **Нажата кнопка B**. 
- Параметры отображения **напряжения**, **текущего значения**, **скорости вращения вентилятора** и переключателя **IR**.
- Свойства, содержащие свойство **серийного номера** устройства и **расположения устройства**, которое является свойством расположения также в свойстве облака **Произведено в**. 


Подробные сведения о конфигурации см. в разделе [сведений о шаблоне устройства MXChip](howto-connect-devkit.md#mxchip-device-template-details).


## <a name="add-a-real-device"></a>Добавление реального устройства

В приложении Azure IoT Central добавьте реальное устройство на основе шаблона устройства **MXChip** и запишите сведения о подключении для устройства (**идентификатор области, идентификатор устройства и первичный ключ**).

1. Добавьте **реальное устройство** из Device Explorer — нажмите **+ Создать > Реальное**, чтобы добавить реальное устройство.
    * Введите идентификатор устройства **<span style="color:Red">(должен состоять из символов нижнего регистра)</span>** или используйте предложенный идентификатор устройства.
    * Введите имя устройства или используйте предложенное имя
    
    ![Add Device (Добавление устройства)](media/concepts-connectivity/add-device.png)


1. Получите сведения о подключении, например **идентификатор области, идентификатор устройства и первичный ключ**, для добавленного устройства, нажав кнопку **Подключить** на странице устройства.
 
    ![Сведения о подключении](media/concepts-connectivity/device-connect.PNG)

3. Обязательно сохраните эти сведения, так как вы будете временно отключены от Интернета при подготовке устройства DevKit. 


### <a name="prepare-the-devkit-device"></a>Подготовка устройства DevKit

> [!NOTE]
> Если вы ранее использовали устройство и сохраняли учетные данные для Wi-Fi, для того чтобы перенастроить устройство для использования другой сети Wi-Fi, строки подключения или измерения телеметрии, одновременно нажмите на клавиатуре клавиши **A** и **B**. Если это не помогло, нажмите кнопку **сброса** и повторите попытку.



#### <a name="to-prepare-the-devkit-device"></a>Чтобы подготовить устройство DevKit, сделайте следующее:


1. Скачайте последнюю версию встроенного ПО Azure IoT Central для MXChip на [этой](https://aka.ms/iotcentral-docs-MXChip-releases) странице на сайте GitHub.
1. Подключите устройство DevKit к компьютеру, на котором будет выполняться разработка, с помощью USB-кабеля. В Windows в проводнике откроется диск, сопоставленный с хранилищем на устройстве DevKit. Например, этот диск может называться **AZ3166 (D:)**.
1. Перетащите файл **iotCentral.bin** в окно диска. По завершении копирования устройство перезагрузится с новой версией встроенного ПО.

1. При перезапуске устройства DevKit отображается следующий экран:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Если на экране отображается что-либо еще, сбросьте устройство и нажмите одновременно кнопки **A** и **B** на устройстве, чтобы перезагрузить его. 

1. Устройство находится в режиме точки доступа. Вы можете подключаться к точке доступа Wi-Fi с компьютера или мобильного устройства.

1. На компьютере, телефоне или планшете подключитесь к сети Wi-Fi с именем, отображаемым на экране устройства. При подключении к этой сети у вас не будет доступа к Интернету. Это состояние является нормальным. Вы подключены к этой сети только на некоторое время, пока выполняется настройка устройства.

1. Откройте веб-браузер и перейдите по адресу [http://192.168.0.1/start](http://192.168.0.1/start). Откроется следующая веб-страница:

    ![Страница конфигурации устройства](media/howto-connect-devkit/configpage.png)

    На этой веб-странице сделайте следующее. 
    - Добавьте имя сети Wi-Fi. 
    - Введите пароль сети Wi-Fi.
    - Укажите ПИН-код, который отображается на ЖК-мониторе устройства. 
    - Введите сведения о подключении **идентификатор области, идентификатор устройства и первичный ключ** для устройства (вы сохранили эти сведения ранее).      
    - Выберите все доступные измерения телеметрии. 

1. Если выбрать **Настройка устройства**, откроется следующая страница:

    ![Устройство настроено](media/howto-connect-devkit/deviceconfigured.png)

1. Нажмите кнопку **Reset** (Сбросить) на устройстве.


## <a name="view-the-telemetry"></a>Просмотр телеметрии

При перезапуске устройства DevKit на экране устройства можно увидеть следующее:

* количество отправленных сообщений телеметрии;
* количество сбоев;
* количество полученных требуемых свойств и отправленных передаваемых свойств.

> [!NOTE]
> Если устройство зацикливается во время подключения, возможно, оно *Заблокировано* в IoT Central. *Разблокируйте* устройство, чтобы оно подключилось к приложению.

Встряхните устройство, чтобы увеличить количество отправленных передаваемых свойств. Устройство отправляет случайное число в качестве свойства устройства **Серийный номер**.

Вы можете просмотреть измерения телеметрии и значения передаваемых свойств, а также настроить параметры в Azure IoT Central:

1. Используйте **Device Explorer**, чтобы перейти на страницу **Measurements** (Измерения) реального устройства MXChip, которое вы добавили:

    ![Переход к реальному устройству](media/howto-connect-devkit/realdevicenew.png)

1. На странице **Measurements** (Измерения) можно просмотреть данные телеметрии, поступающие из устройства MXChip.

    ![Просмотр данных телеметрии из реального устройства](media/howto-connect-devkit/devicetelemetrynew.png)

1. На странице **Свойства** можно просмотреть последний серийный номер и сведения о расположении устройства, переданные устройством:

    ![Просмотр свойств устройства](media/howto-connect-devkit/devicepropertynew.png)

1. На странице **Параметры** можно обновить параметры устройства MXChip:

    ![Просмотр параметров устройства](media/howto-connect-devkit/devicesettingsnew.png)

1. На странице **Панель мониторинга** вы увидите карту расположения.

    ![Просмотр панели мониторинга устройства](media/howto-connect-devkit/devicedashboardnew.png)


## <a name="download-the-source-code"></a>Скачивание исходного кода

Если вы хотите просмотреть и изменить код устройства, его можно скачать с сайта GitHub. Если вы планируете изменить код, необходимо следовать этим инструкциям по [подготовке среды разработки](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) для операционной системы настольных устройств.

Чтобы скачать исходный код, выполните следующую команду на настольном компьютере:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

Исходный код будет скачан в папку с именем `iot-central-firmware`. 

> [!NOTE]
> Если **git** не установлен в среде разработки, его можно скачать здесь: [https://git-scm.com/download](https://git-scm.com/download).

## <a name="review-the-code"></a>Просмотр кода

В редакторе Visual Studio Code, который был установлен во время подготовки среды разработки, откройте папку `AZ3166` в папке `iot-central-firmware`: 

![Visual Studio Code.](media/howto-connect-devkit/vscodeview.png)

Чтобы узнать, каким образом данные телеметрии передаются в приложение Azure IoT Central, откройте файл **main_telemetry.cpp** в исходной папке.

Функция `buildTelemetryPayload` создает полезные данные телеметрии JSON, используя данные из датчиков устройства.

Функция `sendTelemetryPayload` вызывает `sendTelemetry` в **iotHubClient.cpp**, чтобы отправить полезные данные JSON в Центр Интернета вещей, используемый приложением Azure IoT Central.

Чтобы узнать, каким образом значения свойств передаются в приложение Azure IoT Central, откройте файл **main_telemetry.cpp** в исходной папке.

Функция `telemetryLoop` отправляет передаваемое свойство **doubleTap**, если акселерометр обнаруживает двойное касание. Акселерометр использует функцию `sendReportedProperty` в исходном файле **iotHubClient.cpp**.

Код в исходном файле **iotHubClient.cpp** использует функции из [пакетов SDK Microsoft Azure IoT и библиотек для C](https://github.com/Azure/azure-iot-sdk-c), чтобы взаимодействовать с Центром Интернета вещей.

Сведения о способах изменения, создания и отправки примера кода на устройство, см. в файле **readme.md** в папке `AZ3166`.

## <a name="mxchip-device-template-details"></a>Сведения о шаблоне устройства MXChip 

Приложение, созданное из шаблона приложения примера Devkits, включает в себя шаблон устройства MXChip со следующими характеристиками:

### <a name="measurements"></a>Измерения

#### <a name="telemetry"></a>Телеметрия 

| Имя поля     | Units  | Минимальная | Максимальная | Число десятичных знаков |
| -------------- | ------ | ------- | ------- | -------------- |
| Влажность       | %      | 0       | 100     | 0              |
| temp           | °C     | –40     | 120     | 0              |
| pressure       | гПа    | 260     | 1260    | 0              |
| magnetometerX  | мГс | –1000   | 1000    | 0              |
| magnetometerY  | мГс | –1000   | 1000    | 0              |
| magnetometerZ  | мГс | –1000   | 1000    | 0              |
| accelerometerX | mg     | –2000   | 2000    | 0              |
| accelerometerY | mg     | –2000   | 2000    | 0              |
| accelerometerZ | mg     | –2000   | 2000    | 0              |
| gyroscopeX     | милиградусов/с   | –2000   | 2000    | 0              |
| gyroscopeY     | милиградусов/с   | –2000   | 2000    | 0              |
| gyroscopeZ     | милиградусов/с   | –2000   | 2000    | 0              |


#### <a name="states"></a>States 
| ИМЯ          | Отображаемое имя   | ОБЫЧНЫЙ РЕЖИМ | ВНИМАНИЕ! | ОПАСНОСТЬ! | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Состояние устройства   | Зеленый  | Оранжевый  | Красный    | 

#### <a name="events"></a>События 
| ИМЯ             | Отображаемое имя      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Нажата кнопка B  | 

### <a name="settings"></a>Параметры

Числовые параметры

| Отображаемое имя | Имя поля | Units | Число десятичных знаков | Минимальная | Максимальная | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Напряжение      | setVoltage | В | 0              | 0       | 240     | 0       |
| Текущее значение      | setCurrent | Амперы  | 0              | 0       | 100     | 0       |
| Скорость вращения вентилятора    | fanSpeed   | Об/мин   | 0              | 0       | 1000    | 0       |

Параметры переключения

| Отображаемое имя | Имя поля | Включение текста | Отключение текста | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ВКЛ      | ВЫКЛ.      | Отключить     |

### <a name="properties"></a>properties

| type            | Отображаемое имя | Имя поля | Тип данных |
| --------------- | ------------ | ---------- | --------- |
| Свойство устройства | Серийный номер   | dieNumber  | number    |
| Свойство устройства | Расположение устройства   | location  | location    |
| текст            | Произведено в     | manufacturedIn   | Недоступно       |



## <a name="next-steps"></a>Дополнительная информация

Вы узнали, как подключить устройство DevKit к Azure IoT Central, а значит вы готовы к следующему шагу.

* [Подготовка и подключение Raspberry Pi](howto-connect-raspberry-pi-python.md)
