---
title: Настройка мониторинга в Azure Digital Twins | Документация Майкрософт
description: Настройка мониторинга в Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: adgera
ms.openlocfilehash: 1c8f1931a29ae9769f7d8ad57a184e3240105a1a
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945827"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Настройка мониторинга в Azure Digital Twins

Azure Digital Twins поддерживает надежные механизмы ведения журнала, мониторинга и аналитики. Разработчики решений могут использовать Azure Log Analytics, журналы диагностики, ведение журнала действий и другие службы для реализации сложных задач, касающихся мониторинга приложений Интернета вещей. Можно сочетать несколько вариантов ведения журнала для запрашивания или отображения записей в нескольких службах и получения подробных сведений в журналах для многих служб.

В этой статье перечислены варианты ведения журнала и мониторинга, а также способы их сочетания, характерные для Azure Digital Twins.

## <a name="review-activity-logs"></a>Просмотр журналов действий

[Журналы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) Azure дают возможность быстрого анализа истории операций и событий на уровне подписки по каждому экземпляру службы Azure.

События уровня подписки включают:

* создание ресурсов;
* удаление ресурсов;
* создание секретов приложений;
* интеграция с другими службами.

Ведение журнала действий по умолчанию включено для Azure Digital Twins. Чтобы получить сведения о нем на портале Azure, выполните указанные ниже действия.

1. Выберите экземпляр Azure Digital Twins.
1. Выберите **Журнал действий**, чтобы открыть эту панель отображения:

    ![Журнал действий][1]

Расширенные данные о ведении журнала действий вы найдете, выполнив указанные ниже действия.

1. Выберите параметр **Журналы**, чтобы отобразить панель **Обзор аналитики журнала действий**:

    ![Выбор][2]

1. В разделе **Обзор аналитики журнала действий** указаны важнейшие сводные данные по журналу действий.

    ![Обзор аналитики журнала действий][3]

>[!TIP]
>Используйте **журналы действий** для быстрого анализа событий на уровне подписки.

## <a name="enable-customer-diagnostic-logs"></a>Включение журналов диагностики для клиента

В Azure можно настроить не только ведение журнала действий, но и [параметры диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) для каждого экземпляра Azure. Журналы действий относятся к событиям на уровне подписки, а журналы диагностики дают информацию об истории операций самих ресурсов.

Примеры данных в журнале диагностики:

* время выполнения определяемой пользователем функции;
* код состояния отклика для успешного запроса API;
* получение ключа приложения из хранилища.

Чтобы включить для экземпляра ведение журнала диагностики, сделайте следующее:

1. Откройте нужный ресурс на портале Azure.
1. Щелкните **Параметры диагностики**.

    ![Параметры диагностики, экран первый][4]

1. Щелкните **Включить диагностику**, чтобы начать сбор данных (если не был включен ранее).
1. Заполните предложенные поля и выберите, как и где будут храниться данные.

    ![Параметры диагностики, экран второй][5]

    Журналы диагностики часто сохраняются с использованием параметра [Хранилище файлов Azure](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide) и предоставляются с помощью [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal). Вы можете выбрать оба варианта.

>[!TIP]
>Используйте **журналы диагностики**, чтобы получить ценные сведения об операциях с ресурсами.

## <a name="azure-monitor-and-log-analytics"></a>Azure Monitor и Log Analytics

Приложения Интернета вещей собирают в одно целое разрозненные ресурсы, устройства, расположения и данные. Подробное ведение журналов позволяет получать сведения о каждом конкретном элементе, службе и компоненте архитектуры приложений в целом, но часто бывает нужна обобщенная оценка для обслуживания и отладки.

Azure Monitor включает мощную службу Log Analytics, которая позволяет централизованно просматривать и анализировать разные источники ведения журналов. Поэтому платформа Azure Monitor удобна для анализа журналов в сложных приложениях Интернета вещей.

Примеры использования:

* запрашивание данных нескольких журналов диагностики;
* просмотр журналов для нескольких определяемых пользователем функций;
* отображение журналов для двух или нескольких служб за определенный промежуток времени.

Запрашивание подробных данных журналов обеспечивается [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries). Чтобы настроить эти мощные функции:

1. Найдите **Log Analytics** на портале Azure.
1. Вы увидите доступные экземпляры **Log Analytics**. Выберите один из них и щелкните **Журналы**, чтобы создать запрос.

    ![Служба Log Analytics][6]

1. Если у вас нет экземпляра **Log Analytics**, создайте рабочую область, нажав кнопку **Добавить**:

    ![Создание OMS][7]

Когда завершится подготовка экземпляра **Log Analytics**, вы сможете применять мощные запросы для поиска записей сразу по нескольким журналам или поиска по определенным условиям с помощью параметра **Управление журналами**:

   ![Управление журналами][8]

Дополнительные сведения о мощных операциях с запросами см. в статье [о начале работы с запросами](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

> [!NOTE]
> При первой отправке событий в **Log Analytics** возможна задержка до 5 минут.

Кроме того, Azure Log Analytics предоставляет функциональные службы уведомлений об ошибках и предупреждениях, которые можно просмотреть по ссылке **Диагностика и решение проблем**:

   ![Уведомления об ошибках и предупреждениях][9]

>[!TIP]
>Используйте **Log Analytics**, чтобы запрашивать данные журнала для нескольких функций приложений, подписок и служб.

## <a name="other-options"></a>Другие варианты

Azure Digital Twins также поддерживает ведение журналов для отдельных приложений и аудит безопасности. Подробный обзор всех вариантов ведения журналов Azure, доступных для экземпляра Azure Digital Twins, вы найдете в статье [Аудит журналов Azure](https://docs.microsoft.com/azure/security/azure-log-audit).

## <a name="next-steps"></a>Дополнительная информация

Узнайте больше о [журналах действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) Azure.

Глубже изучите параметры системы диагностики Azure в [обзоре журналов диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

Получите дополнительные сведения об [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal).

<!-- Images -->
[1]: media/how-to-configure-monitoring/activity-log.png
[2]: media/how-to-configure-monitoring/activity-log-select.png
[3]: media/how-to-configure-monitoring/log-analytics-overview.png
[4]: media/how-to-configure-monitoring/diagnostic-settings-one.png
[5]: media/how-to-configure-monitoring/diagnostic-settings-two.png
[6]: media/how-to-configure-monitoring/log-analytics.png
[7]: media/how-to-configure-monitoring/log-analytics-oms.png
[8]: media/how-to-configure-monitoring/log-analytics-management.png
[9]: media/how-to-configure-monitoring/log-analytics-notifications.png