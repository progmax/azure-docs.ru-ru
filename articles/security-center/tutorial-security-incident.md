---
title: Руководство по работе с центром безопасности Azure. Реагирование на инциденты в центре безопасности Azure | Документация Майкрософт
description: Руководство по работе с центром безопасности Azure. Реагирование на инциденты в центре безопасности Azure
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 181e3695-cbb8-4b4e-96e9-c4396754862f
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: facea1f0c9c92a07d888163cc44f67d927698002
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849621"
---
# <a name="tutorial-respond-to-security-incidents"></a>Руководство. Реагирование на инциденты безопасности.
Центр безопасности постоянно анализирует ваши рабочие нагрузки в гибридном облаке, используя расширенные инструменты аналитики и анализа угроз, чтобы оповещать вас о вредоносных действиях. Кроме того, вы можете интегрировать в центр безопасности оповещения от других продуктов и служб безопасности, а также создать настраиваемые оповещения на основе индикаторов или источников аналитики. Создав оповещение, выполните действие, необходимое для анализа и исправления. Из этого учебника вы узнаете следующее:

> [!div class="checklist"]
> * Анализ оповещений, связанных с безопасностью
> * дальнейший анализ для определения причины ошибки и области инцидента системы безопасности;
> * поиск данных безопасности, которые помогут проанализировать инцидент.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования
Для выполнения инструкций этого руководства требуется ценовая категория центра безопасности "Стандартный". Вы можете бесплатно опробовать центр безопасности ценовой категории "Стандартный". Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/security-center/). Следуйте инструкциям в [кратком руководстве по центру безопасности Azure](security-center-get-started.md), чтобы обновить ценовую категорию до уровня "Стандартный".

## <a name="triage-security-alerts"></a>Анализ оповещений, связанных с безопасностью
Центр безопасности отображает все оповещения системы безопасности в едином представлении. Оповещения системы безопасности сортируются по уровню серьезности, и по возможности похожие оповещении объединяются в инцидент системы безопасности. Во время рассмотрения оповещений и инцидентов следуйте этим рекомендациям:

- Закрывайте оповещения, которые не требуют дополнительных действий, например, если оповещение вызвано ложно положительным результатом.
- Предпринимайте действия для устранения известных атак, например, блокируйте сетевой трафик с вредоносных IP-адресов.
- Определяйте оповещения, которые требуют дополнительного анализа.


1. В главном меню центра безопасности в разделе **Обнаружение** выберите пункт **Оповещения системы безопасности**:

  ![Оповещения безопасности](./media/tutorial-security-incident/tutorial-security-incident-fig1.png)  

2. В списке оповещений выберите инцидент системы безопасности, который представляет коллекцию предупреждений, чтобы узнать больше. Откроется окно **Обнаружен инцидент системы безопасности**.

  ![Инцидент](./media/tutorial-security-incident/tutorial-security-incident-fig2.png)

3. В верхней части этого окна вы найдете описание инцидента и список оповещений, которые являются частью этого инцидента. Щелкните оповещение, которое нужно проанализировать позднее для получения дополнительных сведений.

  ![Инцидент](./media/tutorial-security-incident/tutorial-security-incident-fig3.png)

  Оповещения могут быть разных типов. Дополнительные сведения о типах оповещений системы безопасности и возможных действиях по исправлению см. в статье [Основные сведения об оповещениях системы безопасности в центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Оповещения, которые можно закрыть, щелкните правой кнопкой мыши и выберите действие **Закрыть**:

  ![Предупреждение](./media/tutorial-security-incident/tutorial-security-incident-fig4.png)

4. Если первопричина и область вредоносных действий неизвестны, перейдите к следующему шагу для дальнейшего анализа.

## <a name="investigate-an-alert-or-incident"></a>Анализ оповещения или инцидента
1. На странице **Оповещение системы безопасности** нажмите кнопку **Начать анализ**. Если вы уже начали анализ, название кнопки изменится на **Продолжить исследование**.

  ![Исследование](./media/tutorial-security-incident/tutorial-security-incident-fig5.png)

  Схема анализа является графическим представлением сущностей, которые связаны с этим оповещением или инцидентом системы безопасности. Если щелкнуть сущность на схеме, сведения об этой сущности отобразят новые сущности и схема развернется. Свойства выбранной на схеме сущности отображаются на панели справа. Сведения на каждой вкладке будут разными в зависимости от выбранной сущности. В процессе анализа просмотрите все важные сведения, чтобы лучше понять действия злоумышленника.

2. Если требуются дополнительные доказательства или нужно дополнительно проанализировать сущности, которые были обнаружены во время этого анализа, перейдите к следующему шагу.

## <a name="search-data-for-investigation"></a>Поиск данных для анализа

С помощью поиска в центре безопасности вы можете найти дополнительные доказательства скомпрометированных систем и дополнительные сведения о сущностях, которые являются частью анализа.

Чтобы выполнить поиск, откройте панель мониторинга **центра безопасности** панели мониторинга, щелкните **Поиск** в области навигации слева, выберите рабочую область, содержащую сущности, которые требуется найти, введите поисковый запрос и нажмите кнопку поиска.

## <a name="clean-up-resources"></a>Очистка ресурсов
Другие руководства в этой серии созданы на основе этого документа. Если вы планируете продолжить работу со следующими руководствами, продолжайте использовать уровень "Стандартный" и оставьте включенной автоматическую подготовку. Если вы не планируете продолжать или хотите вернуться к уровню "Бесплатный", выполните следующие действия.

1. Вернитесь в главное меню центра безопасности и выберите пункт **Политика безопасности**.
2. Выберите подписку или политику, которую хотите вернуть на уровень "Бесплатный". Откроется окно **Политика безопасности**.
3. В разделе **Компоненты политики** выберите **Ценовая категория**.
4. Выберите уровень **Бесплатный**, чтобы изменить уровень подписки со стандартного на бесплатный.
5. Щелкните **Сохранить**.

Чтобы отключить автоматическую подготовку, выполните следующие действия:

1. Вернитесь в главное меню центра безопасности и выберите пункт **Политика безопасности**.
2. Выберите подписку, в которой нужно отключить автоматическую подготовку.
3. В колонке **Security policy – Data Collection** (Политика безопасности — сбор данных) в разделе **Подключение** выберите **Отключить**, чтобы отключить автоматическую подготовку.
4. Щелкните **Сохранить**.

>[!NOTE]
> При отключении автоматической подготовки агент Microsoft Monitoring Agent не будет удален с виртуальных машин Azure, на которых он был подготовлен. Если отключить ее, мониторинг безопасности ваших ресурсов будет ограничен.
>

## <a name="next-steps"></a>Дополнительная информация
Из этого руководства вы узнали о возможностях центра безопасности, которые следует использовать, чтобы реагировать на угрозы безопасности:

> [!div class="checklist"]
> * Инцидент системы безопасности, который состоит из связанных оповещений ресурса
> * Схема анализа, которая является графическим представлением сущностей, связанных с этим оповещением или инцидентом системы безопасности.
> * Возможности поиска для поиска дополнительные доказательств скомпрометированных систем.

Дополнительные сведения об анализе в центре см. здесь:

> [!div class="nextstepaction"]
> [Анализ инцидентов и оповещений](security-center-investigation.md)
