---
title: Руководство. Развертывание приложения ASP.NET в Azure Service Fabric с помощью Azure DevOps Projects
description: Azure DevOps Projects упрощает начало работы с Azure. С помощью DevOps Projects вы можете развернуть приложения ASP.NET Core на платформе Azure Service Fabric, выполнив несколько простых действий.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 2bba5d54c2b6298c2dd8059d47e5975ad3f176c8
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52264766"
---
# <a name="tutorial-deploy-your-aspnet-core-app-to-azure-service-fabric-by-using-azure-devops-projects"></a>Руководство. Развертывание приложения ASP.NET в Azure Service Fabric с помощью Azure DevOps Projects

Azure DevOps Projects представляет собой упрощенный интерфейс, к которому вы можете подключить существующий код и репозиторий Git. Можно выбрать пример приложения, чтобы создать конвейер непрерывной интеграции (CI) и непрерывной поставки (CD) в Azure. 

DevOps Projects также позволяет:
* автоматически создавать ресурсы Azure, например Azure Service Fabric;
* создавать и настраивать конвейер выпуска в Azure DevOps с использованием принципов CI/CD;
* создавать ресурсы Azure Application Insights для мониторинга.

Изучив данный учебник, вы научитесь:

> [!div class="checklist"]
> * создавать приложение ASP.NET Core с помощью DevOps Projects и развертывать в Service Fabric;
> * настраивать Azure DevOps и подписку Azure; 
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * Фиксация изменений в Git и автоматическое развертывание в Azure
> * Очистка ресурсов

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Вы можете получить бесплатную подписку с помощью [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="use-devops-projects-to-create-an-aspnet-core-app-and-deploy-it-to-service-fabricc"></a>Создание приложения ASP.NET Core с помощью DevOps Projects и его развертывание в Service Fabric

DevOps Projects позволяет создать конвейер CI/CD в Azure Pipelines. Вы можете создать новую организацию Azure DevOps или использовать существующую. Azure DevOps Projects также позволяет создать ресурсы Azure, такие как кластер Service Fabric, в требуемой подписке Azure.

1. Войдите на [портале Azure](https://portal.azure.com).

1. В области слева выберите **Создать ресурс**.

1. В поле поиска введите **DevOps Projects**, а затем выберите **Создать**.

    ![Панель мониторинга DevOps Projects](_img/azure-devops-project-github/fullbrowser.png)

1. Выберите **.NET**, а затем **Далее**.

1. В разделе **Выберите платформу приложений** выберите значение **ASP.NET Core** и нажмите кнопку **Далее**.

1. Выберите **Кластер Service Fabric** и щелкните **Далее**. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Настройка Azure DevOps и подписки Azure

1. Создайте новую организацию Azure DevOps или выберите существующую. 

1. Задайте имя проекту Azure DevOps. 

1. Выберите подписку Azure.

1. Чтобы просмотреть дополнительные параметры конфигурации Azure и определить размер узла виртуальной машины и операционную систему для кластера Service Fabric, выберите **Изменить**.  
    В этой области отображаются параметры конфигурации типа и расположения служб Azure.
 
1. Выйдите из области конфигурации Azure и щелкните **Готово**.  
    Через несколько минут процесс завершится. В результате создается кластер Service Fabric и пример приложения ASP.NET Core в репозитории Git, настроенном для организации Azure DevOps, выполняется конвейер CI/CD, а приложение развертывается в Azure. 

    По завершении процедуры на портале Azure отобразится панель мониторинга DevOps Projects. Кроме того, вы перейти к панели мониторинга DevOps Projects непосредственно на вкладке **Все ресурсы** на портале Azure. 

    Панель мониторинга позволяет просматривать репозиторий кода Azure DevOps, конвейер CI/CD и кластер Service Fabric. Можно настроить дополнительные параметры конвейера CI/CD в Azure Repos. В области справа выберите **Обзор**, чтобы просмотреть выполняющееся приложение.

## <a name="examine-the-ci-pipeline"></a>Просмотр конвейера CI

DevOps Projects автоматически настраивает конвейер CI/CD в Azure Pipelines. Вы можете изучить и настроить конвейер. Чтобы ознакомиться с ним, сделайте следующее:

1. Перейдите к панели мониторинга DevOps Projects.

1. В верхней части панели мониторинга DevOps Projects выберите **Конвейеры сборки**.  
    На вкладке браузера отобразится конвейер сборки нового проекта.

1. Выберите поле **Состояние** и щелкните значок многоточия (…).  
    В появившемся меню содержится несколько параметров для действий, таких как постановка в очередь новой сборки, приостановка сборки и изменение конвейера сборки.

1. Выберите **Изменить**

1. В этой области можно просмотреть различные задачи для конвейера сборки.  
    При сборке выполняются различные задачи, такие как получение исходного кода из репозитория Git, восстановление зависимостей и публикация результатов, используемых для развертывания.

1. Выберите имя конвейера сборки в верхней части соответствующей области. 

1. Под именем конвейера сборки щелкните **Журнал**.  
    В этой области отображается журнал аудита последних изменений сборки. Azure DevOps отслеживает все изменения, внесенные в конвейер сборки, и позволяет сравнивать версии.

1. Выберите **Триггеры**.  
    DevOps Projects автоматически создает триггер CI, а при каждой фиксации в репозитории запускается новая сборка. При необходимости выберите для процесса непрерывной интеграции включение или исключение ветвей.

1. Щелкните **Период удержания**.  
    В зависимости от сценария можно указать политики для хранения или удаления определенного количества сборок.

## <a name="examine-the-cd-pipeline"></a>Просмотр конвейера CD

DevOps Projects автоматически создает и настраивает необходимые шаги для развертывания из организации Azure DevOps в подписку Azure. Эти шаги включают настройку подключения к службе Azure для аутентификации Azure DevOps в подписке Azure. Также автоматически создается конвейер выпуска, обеспечивающий непрерывное развертывание в Azure. Чтобы получить дополнительные сведения о конвейере выпуска, сделайте следующее:

1. Последовательно выберите **Сборка и выпуск** и **Выпуски**.  
    Служба DevOps Projects создаст конвейер выпуска для управления развертываниями в Azure.

1. Нажмите многоточие (...) рядом с конвейером выпуска и выберите **Изменить**.  
    Конвейер выпуска содержит *конвейер*, в котором определен процесс выпуска.

1. В разделе **Артефакты** выберите **Удалить**.  
    Конвейер сборки, который вы изучали на предыдущих этапах, создает выходные данные для артефакта. 

1. Справа от значка **Удалить** выберите **Триггер непрерывного развертывания**.  
    В этом конвейере выпуска активирован триггер непрерывного развертывания, выполняющий развертывание каждый раз, когда становится доступным новый артефакт сборки. При желании можно отключить триггер, чтобы выполнять развертывание вручную. 

1. В области справа выберите **Просмотреть выпуски**, чтобы отобразился журнал выпусков.

1. Нажмите многоточие (...) рядом с конвейером выпуска и выберите **Открыть**.  
    Отобразится меню с несколькими элементами, такими как сводка выпуска, связанные рабочие элементы и тесты.

1. Щелкните **Фиксации**.  
    В этом представлении отображаются фиксации кода, связанные с текущим развертыванием. Сравните выпуски, чтобы просмотреть различия между развертываниями.

1. Выберите **Журналы**.  
    Журналы содержат полезную информацию о процессе развертывания. Их можно просматривать во время и после развертывания.

## <a name="commit-changes-to-git-and-automatically-deploy-them-to-azure"></a>Фиксация изменений в Git и их автоматическое развертывание в Azure 

 > [!NOTE]
 > Ниже описана процедура для проверки конвейера CI/CD путем простого изменения текста.

Теперь вы готовы к совместной работе над приложением с использованием процесса CI/CD для автоматического развертывания последних результатов работы на своем веб-сайте. Каждое изменение в репозитории Git запускает сборку, а выпуск обеспечивает развертывание этих изменений в Azure. Используйте описанную в этом разделе процедуру или другой метод, чтобы применять изменения к репозиторию. Например, можно скопировать репозиторий Git в предпочитаемое вами средство или интегрированную среду разработки, а затем отправлять изменения в этот репозиторий.

1. В меню Azure DevOps последовательно выберите **Код** > **Файлы** и перейдите к репозиторию.

1. Откройте каталог *Views\Home*, затем нажмите многоточие (...) рядом с файлом *Index.cshtml* и щелкните **Изменить**.

1. Внесите изменения в файл, например, измените определенный текст внутри одного из тегов div. 

1. В правом верхнем углу выберите **Зафиксировать**, после чего снова нажмите **Зафиксировать**, чтобы отправить изменения.  
    Через несколько минут начнется сборка, а затем будет выполнен выпуск для развертывания изменений. Можно отслеживать состояние сборки на панели мониторинга DevOps Projects или в браузере с помощью функции ведения журнала Azure DevOps в реальном времени.

1. По завершения выпуска обновите приложение, чтобы проверить внесенные изменения.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы выполняете тестирование, можно очистить ресурсы, чтобы избежать дополнительных расходов. Можно удалить кластер Azure Service Fabric и связанные с ним ресурсы, созданные в рамках этого руководства, если в них отпадет необходимость. Для этого воспользуйтесь функцией **Удалить** на панели мониторинга DevOps Projects.

> [!IMPORTANT]
> Следующая процедура безвозвратно удаляет ресурсы. Функция *Удалить* безвозвратно удаляет в Azure и в Azure DevOps данные, созданные в проекте DevOps Projects. Выполняйте эту процедуру только после того, как внимательно прочли предупреждение.

1. На портале Azure перейдите к панели мониторинга DevOps Projects.
1. В правом верхнем углу выберите **Удалить**. 
1. В окне предупреждения выберите **Да**, чтобы *удалить ресурсы без возможности восстановления*.

## <a name="next-steps"></a>Дополнительная информация

При необходимости вы можете изменить конвейеры CI/CD Azure, если это потребуется для вашей команды. Вы также можете использовать этот шаблон CI/CD в качестве шаблона для других конвейеров. Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * создание приложения ASP.NET Core с помощью DevOps Projects и его развертывание в Service Fabric;
> * настройка Azure DevOps и подписки Azure; 
> * Просмотр конвейера CI
> * Просмотр конвейера CD
> * фиксация изменений в Git и их автоматическое развертывание в Azure;
> * Очистка ресурсов

Дополнительные сведения о Service Fabric и микрослужбах

> [!div class="nextstepaction"]
> [Define your multi-stage continuous deployment (CD) process](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)(Определение процесса многоэтапного непрерывного развертывания (CD))
