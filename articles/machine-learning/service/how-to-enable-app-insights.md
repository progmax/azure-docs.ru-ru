---
title: Включение Application Insights для Службы машинного обучения Azure
description: Узнайте, как настроить Application Insights для служб, развернутых с помощью Службы машинного обучения Azure
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 9e0f07e744aaf5f1c35666b40285937dce6dd4de
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275060"
---
# <a name="monitor-your-azure-machine-learning-models-with-application-insights"></a>Мониторинг моделей машинного обучения в Azure с помощью Application Insights

В этой статье описано, как настроить Azure Application Insights для службы машинного обучения Azure. Application Insights можно использовать для отслеживания следующих параметров:
* частоты запросов, времени отклика и частоты сбоев;
* частоты зависимостей, времени отклика и частоты сбоев;
* Исключения.

[Дополнительные сведения об Application Insights](../../application-insights/app-insights-overview.md). 

>[!NOTE]
> Код в этой статье был протестирован с пакетом SDK для службы "Машинное обучение Azure" версии 0.1.74.


## <a name="prerequisites"></a>Предварительные требования
* Подписка Azure. Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://aka.ms/AMLfree), прежде чем начать работу.
* Должны быть установлены рабочая область машинного обучения Azure, локальный каталог со скриптами и пакет SDK машинного обучения Azure для Python. В руководстве по [настройке среды разработки](how-to-configure-environment.md) описано, как получить эти обязательные компоненты.
* Обученная модель машинного обучения для развертывания в службе Azure Kubernetes (AKS) или в экземпляре контейнера Azure (ACI). Если у вас ее нет, см. руководство по [обучению модели классификации изображений](tutorial-train-models-with-aml.md).


## <a name="enable-and-disable-from-the-sdk"></a>Включение и отключение с помощью пакета SDK

### <a name="update-a-deployed-service"></a>Обновление развернутой службы
1. Найдите службу в рабочей области. Значение `ws` обозначает имя рабочей области.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Обновите службу и включите Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Трассировка пользовательских журналов в службе
Если требуется выполнять трассировку пользовательских журналов, выполните инструкции из руководства по стандартному процессу развертывания для [AKS](how-to-deploy-to-aks.md) или [ACI](how-to-deploy-to-aci.md). Затем сделайте следующее:

1. измените файл оценки, добавив выражения print;
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Обновите конфигурацию службы.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Создайте образ и разверните его в [AKS](how-to-deploy-to-aks.md) или [ACI](how-to-deploy-to-aci.md).  

### <a name="disable-tracking-in-python"></a>Отключение наблюдения в Python

Чтобы отключить Application Insights, используйте следующий код:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    
## <a name="enable-and-disable-in-the-portal"></a>Включение и отключение с помощью портала

Вы можете включить или отключить Azure Application Insights на портале Azure.

1. Откройте рабочую область на [портале Azure](https://portal.azure.com).

1. На вкладке **Развертывания** выберите службу, для которой требуется включить Application Insights.

   [![Список служб на вкладке "Развертывания"](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Выберите **Изменить**

   [![Кнопка "Изменить"](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. В разделе **Дополнительные параметры** установите флажок **Включить диагностику AppInsights**.

   [![Установленный флажок для включения диагностики](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Чтобы применить изменение, в верхней части экрана выберите **Изменить**. 

### <a name="disable"></a>Disable
1. Откройте рабочую область на [портале Azure](https://portal.azure.com).
1. Откройте **Развертывания**, затем выберите службу и щелкните **Изменить**.

   [![Кнопка "Изменить"](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. В разделе **Дополнительные параметры** снимите флажок **Включить диагностику AppInsights**. 

   [![Снятый флажок включения диагностики](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Чтобы применить изменение, в верхней части экрана выберите **Изменить**. 
 

## <a name="evaluate-data"></a>Данные оценки
Данные службы будут храниться в учетной записи Application Insights в той же группе ресурсов, что и рабочая среда службы машинного обучения Azure.
Чтобы их просмотреть:
1. Откройте рабочее пространство Службы машинного обучения на [портале Azure](https://portal.azure.com) и щелкните ссылку Application Insights.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Выберите вкладку **Обзор**, чтобы увидеть базовый набор метрик для своей службы.

   [![Обзор](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Чтобы выполнить поиск в пользовательских трассировках, выберите **Аналитика**.
4. В разделе схемы выберите **Трассировки**. Затем выберите **Запуск**, чтобы выполнить запрос. Данные будут отображаться в формате таблицы и сопоставляться с пользовательскими вызовами в файле оценки. 

   [![Пользовательские трассировки](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Дополнительные сведения об использовании службы Application Insights см. в статье [Что такое Azure Application Insights?](../../application-insights/app-insights-overview.md)
    

## <a name="example-notebook"></a>Пример записной книжки

В записной книжке [00.Getting Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) показаны основные понятия из этой статьи.  Получите эту записную книжку:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Дополнительная информация
Также данные можно собирать с моделей в рабочей среде. Ознакомьтесь со статьей [Сбор данных для моделей в рабочей среде](how-to-enable-data-collection.md). 


## <a name="other-references"></a>Прочие ссылки
* [Azure Monitor для контейнеров](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json)
