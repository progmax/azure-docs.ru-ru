---
title: Заметки о выпуске Служб мультимедиа Azure версии 3 | Документация Майкрософт
description: Чтобы вы оставались в курсе последних разработок, в этой статье предоставлены последние обновления Служб мультимедиа Azure версии 3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/21/2018
ms.author: juliako
ms.openlocfilehash: 598587a0fe726ccf65f062833f84b352ca03c077
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315534"
---
# <a name="azure-media-services-v3-release-notes"></a>Заметки о выпуске Служб мультимедиа Azure версии 3 

Чтобы вы оставались в курсе последних разработок, в этой статье предоставлены такие сведения:

* Последние выпуски.
* Известные проблемы
* Исправления ошибок
* Нерекомендуемые функции.
* Планы по изменениям.

## <a name="known-issues"></a>Известные проблемы

> [!NOTE]
> В настоящее время вы не можете использовать портал Azure для управления ресурсами версии 3. Используйте [REST API](https://aka.ms/ams-v3-rest-sdk), CLI или один из поддерживаемых пакетов SDK.

Дополнительные сведения см. в статье [Руководство по миграции из версии 2 в версию 3 Служб мультимедиа](migrate-from-v2-to-v3.md#known-issues).

## <a name="november-2018"></a>Ноябрь 2018 г.

Модуль CLI 2.0 теперь доступен для [Службы мультимедиа Azure, общедоступная версия 3](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) — версия 2.0.50.

### <a name="new-commands"></a>Новые команды

- [az ams account](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [az ams account-filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [az ams asset](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [az ams asset-filter](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [az ams content-key-policy](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [az ams job](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [az ams live-event](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [az ams live-output](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [az ams streaming-endpoint](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [az ams streaming-locator](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [az ams account mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) — позволяет управлять зарезервированными единицами мультимедиа

### <a name="new-features-and-breaking-changes"></a>Новые функции и критические изменения

#### <a name="asset-commands"></a>Команды для ресурсов

- Добавлены аргументы ```--storage-account``` и ```--container```.
- Добавлены значения по умолчанию для времени истечения срока действия (23 часа от текущего момента) и разрешений (чтение) в команду ```az ams asset get-sas-url```.

#### <a name="job-commands"></a>Команды для заданий

- Добавлены аргументы ```--correlation-data``` и ```--label```.
- Аргумент ```--output-asset-names``` переименован в ```--output-assets```. Теперь он принимает список ресурсов, разделенных пробелами, в формате assetName=label. Ресурс без метки можно передать следующим образом: assetName=.

#### <a name="streaming-locator-commands"></a>Команды для потокового указателя

- Базовая команда ```az ams streaming locator``` заменена на ```az ams streaming-locator```.
- Добавлены аргументы ```--streaming-locator-id``` и ```--alternative-media-id support```.
- Обновлен аргумент ```--content-keys argument```.
- Аргумент ```--content-policy-name``` переименован в ```--content-key-policy-name```.

#### <a name="streaming-policy-commands"></a>Команды для политики потоковой передачи

- Базовая команда ```az ams streaming policy``` заменена на ```az ams streaming-policy```.
- Добавлена поддержка параметров шифрования в ```az ams streaming-policy create```.

#### <a name="transform-commands"></a>Команды преобразования

- Аргумент ```--preset-names``` заменен на ```--preset```. Теперь можно одновременно задавать только один вывод/набор параметров (для добавления дополнительных нужно запустить команду ```az ams transform output add```). Также можно задать пользовательский параметр StandardEncoderPreset, указав путь к пользовательскому файлу JSON.
- ```az ams transform output remove``` теперь можно выполнять путем передачи выходного индекса для удаления.
- Аргументы ```--relative-priority, --on-error, --audio-language and --insights-to-extract``` добавлены в команды ```az ams transform create``` и ```az ams transform output add```.

## <a name="october-2018---ga"></a>Октябрь 2018 г. Общедоступная версия

В этом разделе описываются октябрьские обновления Служб мультимедиа Azure (AMS).

### <a name="rest-v3-ga-release"></a>Общедоступный выпуск REST версии 3

[Общедоступный выпуск REST версии 3](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) включает дополнительные API для динамических событий, фильтров манифестов на уровне ресурса и учетной записи и поддержку DRM.

#### <a name="azure-resource-management"></a>API управления ресурсами Azure 

Благодаря поддержке API управления ресурсами Azure доступен унифицированный API управления и операций (теперь все в одном месте).

Начиная с этого выпуска, для создания динамических событий можно использовать шаблоны Resource Manager.

#### <a name="improvement-of-asset-operations"></a>Улучшение операций с ресурсами 

Появились следующие улучшения:

- прием из URL-адреса HTTP или URL-адреса SAS хранилища BLOB-объектов Azure;
- указание собственных имен контейнеров для ресурсов; 
- простая поддержка выходных данных для создания пользовательских рабочих процессов с помощью службы "Функции Azure".

#### <a name="new-transform-object"></a>Новый объект преобразования

Новый объект **Преобразование** упрощает модель кодирования. Новый объект позволяет легко создавать и совместно использовать кодировку шаблонов Resource Manager и предустановок. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Аутентификация Azure Active Directory и RBAC

Аутентификация Azure AD и управление доступом на основе ролей (RBAC) обеспечивают безопасные преобразования, динамические события, политики ключей содержимого и предоставление ресурсов для ролей или пользователей в Azure AD.

#### <a name="client-sdks"></a>Клиентские пакеты SDK  

Языки, поддерживаемые в Службах мультимедиа версии 3: .NET Core, Java, Node.js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Обновления кодирования в реальном времени

Представлены следующие обновления кодирования в реальном времени:

- Новый режим с низкой задержкой для трансляции (10 секунд для полного процесса).
- Улучшенная поддержка RTMP (повышенная стабильность и поддержка исходного кодировщика).
- Безопасный прием RTMPS.

    При создании динамических событий вы получите 4 URL-адреса для приема. 4 URL-адреса для приема практически идентичны, они имеют тот же токен потоковой передачи (AppId) с отличной частью номера порта. Два из этих URL-адресов являются первичным и резервным для RTMPS. 
- 24-часовая система поддержки перекодирования. 
- Улучшена поддержка рекламных сигналов в RTMP с помощью SCTE35.

#### <a name="improved-event-grid-support"></a>Улучшенная поддержка Сетки событий

Вы увидите следующие улучшения поддержки Сетки событий:

- Интеграция службы "Сетка событий Azure" для упрощения разработки с использованием Logic Apps и службы "Функции Azure". 
- Подписка на события Службы кодирования, динамических каналов и др.

### <a name="cmaf-support"></a>Поддержка CMAF

Поддержка шифрования CMAF и cbcs для Apple HLS (iOS 11 и выше) и проигрывателей MPEG-DASH, поддерживающих CMAF.

### <a name="video-indexer"></a>Индексатор видео

Общедоступный выпуск службы "Индексатор видео" вышел в августе. Новые сведения о поддерживаемых функциях см. в статье [Что такое Индексатор видео?](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json) 

### <a name="plans-for-changes"></a>Планы по изменениям.

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
Модуль Azure CLI 2.0, который включает в себя операции со всеми функциями (включая динамические события, политики ключа содержимого, фильтры учетной записи и ресурсов, политики потоковой передачи), ожидается в ближайшее время. 

### <a name="known-issues"></a>Известные проблемы

Эта проблема повлияет только на пользователей, которые используют API предварительной версии для фильтров ресурса или учетной записи.

Если вы создали фильтры учетных записей или ресурсов в период с 28.09 по 12.10 с помощью CLI или API для Служб мультимедиа версии 3, необходимо удалить все эти фильтры и повторно создать их из-за конфликта версий. 

## <a name="may-2018---preview"></a>Май 2018 г. Предварительная версия

### <a name="net-sdk"></a>Пакет SDK для .NET

Пакет SDK для .NET предоставляет следующие возможности:

* **Преобразования** и **задания**, позволяющие кодировать или анализировать мультимедийное содержимое. Примеры см. в руководстве по [потоковой передаче](stream-files-tutorial-with-api.md) и [анализе видео](analyze-videos-tutorial-with-api.md).
* **Указатели потоковой передачи**, отвечающие за публикацию и потоковую передачу содержимого на устройства пользователей.
* **Политики потоковой передачи** и **ключа содержимого**, отвечающие за настройку доставки ключей и защиту содержимого (DRM) при его доставке.
* **События прямой трансляции** и **выходные данные запущенной прямой трансляции**, отвечающие за прием и потоковую трансляцию содержимого.
* **Ресурсы**, отвечающие за хранение и публикацию содержимого в службу хранилища Azure. 
* **Конечные точки потоковой передачи**, позволяющие настраивать и масштабировать динамические упаковки, шифрование и потоковую передачу мультимедийного содержимого в реальном времени и по запросу.

### <a name="known-issues"></a>Известные проблемы

* При отправке задания можно указать исходное видео для приема, используя URL-адреса HTTPS, URL-адреса SAS или пути к файлам, находящимся в хранилище BLOB-объектов Azure. В настоящее время AMS версии 3 не поддерживает кодирование блочной передачи по URL-адресам HTTPS.

## <a name="next-steps"></a>Дополнительная информация

[Обзор](media-services-overview.md)
