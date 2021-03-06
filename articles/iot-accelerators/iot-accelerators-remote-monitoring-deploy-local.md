---
title: Локальное развертывание решения для удаленного мониторинга (через Visual Studio IDE) в Azure | Документация Майкрософт
description: В этом руководстве показано, как развернуть акселератор решений для удаленного мониторинга на локальном компьютере с помощью Visual Studio для тестирования и разработки.
author: avneet723
manager: hegate
ms.author: avneet723
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 5068f0277726b7c468aa24d0629c4350b60b78b5
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2018
ms.locfileid: "51287614"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---visual-studio"></a>Локальное развертывание акселератора решений для удаленного мониторинга в Visual Studio

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

В этой статье показано, как развернуть акселератор решений для удаленного мониторинга на локальном компьютере в целях тестирования и разработки. Вы узнаете, как запустить микрослужбы в Visual Studio. Для развертывания локальных микрослужб используются следующие облачные службы: Центр Интернета вещей, Cosmos DB, Azure Streaming Analytics и службы Аналитики временных рядов Azure.

Если вы хотите запустить акселератор решений для удаленного мониторинга в Docker на локальном компьютере, см. статью [Deploy the Remote Monitoring solution accelerator locally — Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md) (Локальное развертывание ускорителя решения удаленного мониторинга в Docker).

## <a name="prerequisites"></a>Предварительные требования

Для развертывания служб Azure, используемых акселератором решений для удаленного мониторинга, требуется активная подписка Azure.

Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Настройки компьютера

Для завершения локального развертывания необходимо установить следующие средства на локальный компьютер разработчика:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Visual Studio](https://visualstudio.microsoft.com/)
* [Nginx](http://nginx.org/en/download.html)
* [Node.js версии 8](https://nodejs.org/). Это программное обеспечение является необходимым компонентом для интерфейса командной строки PCS, который используется в сценариях для создания ресурсов Azure. Не используйте Node.js версии 10.

> [!NOTE]
> Служба Visual Studio доступна для Windows и Mac.

[!INCLUDE [iot-accelerators-local-setup](../../includes/iot-accelerators-local-setup.md)]

## <a name="run-the-microservices"></a>Запуск микрослужб

В этом разделе вы запустите микрослужбы для удаленного мониторинга. Веб-интерфейс запускается в собственной среде, служба "Симулятор устройств" — в Docker, а микрослужбы — в Visual Studio.

### <a name="run-the-web-ui"></a>Запуск веб-интерфейса

На этом этапе вы запустите веб-интерфейс. Перейдите в папку **webui** в локальной копии репозитория и выполните следующие команды:

```cmd
npm install
npm start
```

### <a name="run-the-device-simulation-service"></a>Запустите службу моделирования устройств

Выполните следующую команду, чтобы запустить контейнер Docker для службы моделирования устройств. Служба моделирует устройства для решения удаленного мониторинга.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Разверните все микрослужбы на локальном компьютере.

Ниже показано, как запускать микрослужбы удаленного мониторинга в Visual Studio 2017:

1. Запустите Visual Studio 2017.
1. Откройте решение **remote-monitoring.sln** в папке **services** в локальной копии репозитория.
1. В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите пункт **Свойства**.
1. Выберите **Общие свойства > Запускаемый проект**.
1. Выберите **Несколько запускаемых проектов** и задайте **Запуск** в качестве **действия** для следующих проектов:
    * WebService (asa-manager\WebService);
    * WebService (auth\WebService);
    * WebService (config\WebService);
    * WebService (device-telemetry\WebService);
    * WebService (iothub-manager\WebService);
    * WebService (storage-adapter\WebService).
1. Нажмите кнопку **ОК**, чтобы сохранить изменения.
1. Щелкните **Отладка > Начать отладку** для сборки и запуска веб-служб на локальном компьютере.

Для каждой веб-службы откроется командная строка и окно веб-браузера. В командной строке вы увидите выходные данные из запущенной службы, а окно браузера позволяет отслеживать состояние. Не закрывайте окна командной строки или веб-страницы, так как работа веб-службы прекратится.

### <a name="start-the-stream-analytics-job"></a>Запуск задания Stream Analytics

Чтобы запустить задание Stream Analytics, выполните следующие действия:

1. Перейдите на [портал Azure](https://portal.azure.com).
1. Перейдите к **группе ресурсов**, созданной для решения. Имя группы ресурсов совпадает с именем решения, выбранного при запуске сценария **start.cmd**.
1. В списке ресурсов щелкните **Задание Stream Analytics**.
1. На странице **Обзор** задания Stream Analytics нажмите кнопку **Запуск**. Затем, чтобы немедленно начать задание, щелкните **Пуск**.

### <a name="configure-and-run-nginx"></a>Настройка и запуск NGINX

Настройте обратный прокси-сервер, чтобы связать веб-приложение и микрослужбы, запущенные на локальном компьютере:

* Скопируйте файл **nginx.conf** из папки **webui\scripts\localhost** в каталог установки **nginx\conf**.
* Запустите **nginx**.

Дополнительные сведения о запуске **nginx** см. в разделе [nginx для Windows](http://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Подключение к панели мониторинга

Для доступа к панели мониторинга решения для удаленного мониторинга перейдите по адресу [http://localhost:9000](http://localhost:9000) в браузере.

## <a name="clean-up"></a>Очистка

Чтобы избежать ненужных расходов, после завершения тестирования удалите облачные службы из подписки Azure. Чтобы удалить службы, перейдите на [портал Azure](https://ms.portal.azure.com) и удалите группу ресурсов, созданную при запуске сценария **start.cmd**.

Также можно удалить локальную копию репозитория удаленного мониторинга, созданную при клонировании исходного кода из GitHub.

## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы развернули решение для удаленного мониторинга, [изучите возможности панели мониторинга решения](quickstart-remote-monitoring-deploy.md).
