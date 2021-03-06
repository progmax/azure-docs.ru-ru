---
title: Публикация шаблона решения Azure | Документация Майкрософт
description: Публикация шаблона решения в Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: pbutlerm
ms.openlocfilehash: 333eebfa1bae919c43164572c63f2de4f7251fe0
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261623"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Публикация шаблона решения в Azure Marketplace

Эта статья содержит инструкции по публикации шаблона решения в виде предложения Azure Marketplace.

## <a name="prerequisites"></a>Предварительные требования

К публикации шаблона решения в Azure Marketplace применяются следующие технические и другие требования.

### <a name="technical"></a>Технические требования

- [Описание структуры и синтаксиса шаблонов Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates).
- Шаблоны быстрого запуска Azure:
    - [Документация по шаблонам быстрого запуска Azure](https://azure.microsoft.com/documentation/templates/)
    - [Документация по шаблонам быстрого запуска Azure на сайте GitHub](https://github.com/azure/azure-quickstart-templates)
 - [Создание пользовательского интерфейса портала Azure для управляемого приложения](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)
 - Включите [определение потребления услуг клиентами](./../azure-partner-customer-usage-attribution.md), чтобы отслеживать потребление ресурсов Azure для клиентских развертываний программного обеспечения в Azure.

### <a name="non-technical-business-requirements"></a>Требования нетехнического характера (бизнес-требования)

- Ваша компания (или ее филиал) должна находиться в стране, на территории которой могут осуществляться продажи в Azure Marketplace.
- Продукт должен иметь лицензию, совместимую с моделями выставления счетов, которые поддерживает Azure Marketplace.
- Вы должны предоставить клиентам техническую поддержку экономически обоснованным образом (на платной или бесплатной основе либо посредством оказания поддержки силами сообщества).
- Вы несете ответственность за лицензирование программного обеспечения, а также любых зависимостей от стороннего программного обеспечения.
- Предоставьте маркетинговые материалы, отвечающие критериям вашего предложения, соблюдение которых требуется для добавления приложения в Azure Marketplace и на портал Azure.
- Подтвердите согласие с условиями политик участия и соглашения для издателей Azure Marketplace.
- Примите условия использования, заявление о конфиденциальности Майкрософт и соглашение по программе сертификации Microsoft Azure.

## <a name="before-you-begin"></a>Перед началом работы

После удовлетворения всех предварительных требований можно приступить к созданию предложения шаблона решения. Прежде чем начать, просмотрите следующее сведения о предложении и номере SKU.

**ПРЕДЛОЖЕНИЕ**

Предложение приложения Azure соответствует классу предложения продукта от издателя. Если у вас есть новый тип решения или приложения, которое вы хотели бы добавить в Azure Marketplace, можно разместить его в качестве нового предложения. Предложение — это коллекция SKU. Каждое предложение отображается в виде отдельной сущности в Azure Marketplace.

**SKU**

SKU — это наименьшая единица предложения, доступная для приобретения. Номера SKU в одном классе продуктов (предложении) позволяют разделить разные поддерживаемые функции. Например, предложение может быть управляемым или неуправляемым, а также поддерживать разные модели выставления счетов.

Используйте несколько номеров SKU в следующих сценариях:
- Если нужна поддержка разных моделей выставления счетов, таких как использование собственной лицензии (BYOL) или оплата по мере использования (PAYG).
- Каждый номер SKU поддерживает разные наборы функций с разными ценами.

В Azure Marketplace номер SKU отображается внутри родительского предложения, а на портале Azure — в качестве отдельной сущности, доступной для приобретения.

## <a name="to-create-a-new-offer"></a>Создание предложения

1.  Войдите на [облачный портал для партнеров](http://cloudpartner.azure.com/).

2.  В левой панели навигации щелкните **+ New offer** (+ Создать предложение) и выберите **Приложения Azure**.

    ![Создание нового предложения](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  В области **Новое предложение** выберите **Редактор**.

    ![Редактор нового предложения](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  В разделе **Редактор** необходимо предоставить сведения в следующих представлениях:
    - Параметры предложения
    - Номера SKU
    - Marketplace
    - Поддержка

Каждое представление содержит набор полей для заполнения. Обязательные для заполнения поля отмечены красной звездочкой (\*).

## <a name="to-configure-offer-settings"></a>Настройка параметров предложения

1. Настройте следующие поля **идентификатора предложения** в параметрах предложения.

    **Идентификатор предложения**

     Уникальный идентификатор для предложения в профиле издателя. Он отображается в URL-адресах продукта, шаблонах Azure Resource Manager и отчетах о выставлении счетов. Идентификатор может содержать только строчные буквы, цифры или дефисы (-). Он не может заканчиваться дефисом, а его длина не должна превышать 50 символов. 
    >[!Note]
    >После активации предложения это поле блокируется.

    **Идентификатор издателя**

    Раскрывающийся список профилей издателей. Выберите здесь профиль издателя, от имени которого будет опубликовано предложение. 
    >[!Note]
    >После активации предложения это поле блокируется.

    **Имя**

    Отображаемое имя для предложения. Это имя отображается в Azure Marketplace и на портале Azure. Его длина не должна превышать 50 символов. Для имени предложения следует учесть следующие рекомендации:
    -  Укажите для своего продукта узнаваемое название торговой марки. 
    - Не включайте в него название организации, если это не входит в стратегию продвижения продукта.
    - Если вы продвигаете продукт через свой веб-сайт, убедитесь, что данное имя полностью совпадает с используемым на сайте.

2. Нажмите кнопку **Сохранить**, чтобы завершить установку параметров
для предложения.

## <a name="to-create-skus"></a>Создание номеров SKU
------------------

1. Выберите **Номера SKU**. 

    ![Новый номер SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    Идентификатор SKU представляет собой уникальный идентификатор номера SKU в рамках предложения. Он отображается в URL-адресах продукта, шаблонах Resource Manager и отчетах о выставлении счетов. Для идентификатора SKU действуют следующие требования:
    - Его длина не должна превышать 50 символов.
    - Идентификатор может содержать только строчные буквы, цифры или дефисы (-).
    - Идентификатор не может заканчиваться дефисом.

    >[!Note]
    >После добавления номера SKU он появится в списке номеров SKU в соответствующем представлении. Чтобы просмотреть сведения о номере SKU, выберите его имя. 

2. Щелкните **Новый номер SKU**, чтобы ввести сведения, как показано на следующем снимке экрана. 

    ![Сведения о номере SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>Параметры номера SKU

Укажите следующие параметры номера SKU

- **Заголовок** — заголовок для номера SKU. Это значение отображается для соответствующего элемента в коллекции.
- **Сводка** — краткое сводное описание номера SKU. (Максимальная длина составляет 100 символов.)
- **Описание** — подробное описание номера SKU.
- **Тип SKU** — раскрывающийся список со следующими значениями: "Управляемое приложение (Предварительная версия)" и "Шаблон решения". Для нашего примера выберите **Шаблон решения**.
- **Cloud Availability** (Доступность в облаке) — расположение номера SKU. По умолчанию используется **общедоступное облако Azure**.

### <a name="package-details"></a>Сведения о пакете

Завершив настройку номера SKU, укажите следующие сведения о пакете.

![Сведения о пакете](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Текущая версия** — версия отправляемого пакета. Теги версий должны иметь формат X.Y.Z, где X, Y и Z обозначают целые числа.
- **Файл пакета** — пакет содержит следующие файлы, собранные в единый ZIP-файл.
    -   MainTemplate.json — файл шаблона развертывания, который используется для развертывания решения или приложения и создания ресурсов, определенных для этого решения. Дополнительные сведения см. в статье [о создании файлов шаблона развертывания](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template).
    -   createUIDefinition.json — этот файл используется порталом Azure, чтобы создать пользовательский интерфейс для подготовки решения или приложения. Подробнее см. статью [Создание пользовательского интерфейса на портале Azure для управляемого приложения](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview).

    >[!IMPORTANT]
    >Этот пакет должен содержать все другие вложенные шаблоны или скрипты, которые потребуются для успешной подготовки этого приложения. Файлы mainTemplate.json и createUIDefinition.json следует поместить в корневую папку.

## <a name="to-configure-the-marketplace"></a>Настройка Marketplace

В представлении Marketplace можно настроить поля, которые отображаются для предложения в [Azure Marketplace](https://azuremarketplace.microsoft.com) и на [портале Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview Subscription Ids (Идентификаторы подписок для предварительного просмотра)

Список идентификаторов подписок Azure, которые должны получить доступ к предложению после его публикации. Эти разрешенные подписки позволяют протестировать предложение перед его активацией. Партнерский портал позволяет разрешить до 100 подписок.

### <a name="suggested-categories"></a>Suggested Categories (Предлагаемые категории)

Выберите из предоставленного списка до пяти категорий, которые точнее всего описывают ваше предложение. Выбранные категории будут использоваться для сопоставления вашего предложения с категориями продуктов, доступными в [Azure Marketplace](https://azuremarketplace.microsoft.com) и на [портале Azure](https://portal.azure.com/).

В следующих примерах показаны сведения о Marketplace в Azure Marketplace и на портале Azure.

**Azure Marketplace**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**портал Azure**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


### <a name="logo-guidelines"></a>Рекомендации по логотипам

Следуйте приведенным ниже указаниям для логотипов, передаваемых на портал Cloud Partner.

-   Дизайн Azure отличается простой цветовой палитрой. Используйте в логотипах небольшое число основных и дополнительных цветов.

-   Цвета темы портала Azure — белый и черный. Старайтесь не использовать эти цвета для фона своих логотипов. Используйте цвета, которые сделают ваши логотипы четко видимыми на портале Azure. Мы рекомендуем простые основные цвета.

    >[!Note] 
    >Если вы используете прозрачный фон, логотип и текст не должны быть белого, черного или синего цвета.

-   Не используйте в логотипе градиентный фон.

-   Не размещайте текст на логотипе, в том числе название компании или торговой марки. Логотип должен быть *плоским*. Также избегайте градиентов.

-   Логотип не должен быть растянутым.

#### <a name="hero-logo"></a>Имиджевый логотип

Имиджевый логотип использовать необязательно. Издатель может не загружать такой логотип. После передачи логотип нельзя удалить. Партнер также должен следовать рекомендациям для имиджевых логотипов Azure Marketplace.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Рекомендации для значка имиджевого логотипа

-   Отображаемое имя издателя, название плана и развернутая сводка предложения отображаются в белом цвете. Не используйте светлые цвета для фона. Фон имиджевого значка не может быть черным, белым или прозрачным.

-   Как только предложение вносится в список, в имиджевый логотип программно вставляется отображаемое имя издателя, название плана, развернутая сводка предложения и кнопка "Создать". Не вводите текст при разработке имиджевого логотипа. Оставьте справа от логотипа пустое место размером 415 x 100 пикселей, смещенное на 370 пикселей от левого края.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>Настройка поддержки

Используйте представление поддержки, чтобы указать следующие сведения:

- контактные данные службы поддержки компании, например инженеров;
- контактные данные службы поддержки клиентов.

## <a name="to-publish-the-offer"></a>Публикация предложения

Последним шагом является публикация предложения. Нажмите кнопку **Опубликовать**.
