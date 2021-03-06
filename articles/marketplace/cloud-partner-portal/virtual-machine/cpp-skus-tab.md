---
title: Вкладка с номерами SKU виртуальных машин на портале Cloud Partner в Azure Marketplace | Документация Майкрософт
description: Описание вкладки с номерами SKU, которая используется при создании предложения виртуальной машины в Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 4ecc259d40cdcba93a484f27e27191e967f10ff1
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639366"
---
# <a name="virtual-machine-skus-tab"></a>Вкладка с номерами SKU виртуальных машин

Вкладка **Номера SKU** на странице **Новое предложение** позволяет создать один или несколько номеров SKU, чтобы связать их с новым предложением.  Несколько номеров SKU позволяют различать варианты решения, характеризующиеся разными сочетаниями наборов функций, типами образов виртуальных машин, пропускной способностью и масштабируемостью, моделями выставления счетов и другими свойствами.

## <a name="create-a-sku"></a>Создание номера SKU

Изначально новое предложение связано с номерами SKU, поэтому следует выбрать **New SKU** (Новый номер SKU) и создать хотя бы один такой номер.

![Кнопка New SKU (Новый номер SKU) на вкладке New Offer (Новое предложение) для виртуальных машин](./media/publishvm_005.png)

<br/>

Откроется диалоговое окно **New SKU** (Новый номер SKU).  Введите идентификатор для нового SKU и щелкните **ОК**. (Ниже описаны правила именования для идентификаторов.)  Теперь на вкладке **SKUs** (Номера SKU) отобразятся поля, доступные для редактирования.    Символ звездочки (*) возле имени поля означает, что оно является обязательным.

<!-- TD: This tab has been updated, now has "Old Pricing" and "Simplified Currency Pricing" sections"! -->

![Вкладка SKU (Номер SKU) в окне New Offer (Новое предложение) для виртуальных машин](./media/publishvm_006.png)

В следующей таблице описаны назначение, содержимое и формат полей данных:

<!-- TD: I took a new screenshot, and the fields differ somewhat from description in the VM Pub Guide.  Needs review. -->

|  **Поле**       |     **Описание**                                                          |
|  ---------       |     ---------------                                                          |
|  *Параметры номера SKU*   |  |
| **Идентификатор номера SKU**       | Идентификатор создаваемого номера SKU.  Длина имени не должна превышать 50 знаков, оно может содержать строчные буквы, цифры и дефисы (-), но не может заканчиваться дефисом.  Его нельзя будет изменить после публикации предложения.  |
|  *Сведения о номере SKU*   |  |
| **Заголовок**        | Понятное имя предложения, которое будет отображаться в Marketplace. Максимальная длина составляет 50 символов. |
| **Сводка**      | Краткое описание предложения, которое будет отображаться в Marketplace. Максимальная длина составляет 100 символов. |
| **Описание**  | Текст описания, который предоставляет более подробные характеристики предложения.  <!-- TD: max len/guidance? 3k characters -->  |
| **Hide this SKU** (Скрыть этот номер SKU) | Определяет, будет ли номер SKU виден для клиентов в Marketplace.  Номер SKU можно скрыть, если вы хотите предоставлять его только через шаблоны решений, но не для отдельного приобретения.  Также эта возможность удобна при начальном тестировании и публикации временных или сезонных предложений. |
| **Cloud Availability** (Доступность в облаках) | Определяет, в каких облаках будет доступен этот номер SKU.  По умолчанию устанавливается только общедоступная версия Azure.  Microsoft Azure для государственных организаций — это облако сообщества государственных организаций с контролируемым доступом для федеральных, государственных, местных или племенных органов и их сертифицированных партнеров.  Дополнительные сведения об Azure для государственных организаций см. на [этой странице](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome). |
| **Is this a Private SKU?** (Это частный номер SKU?) | Определяет частный или общедоступный режим использования номера SKU. По умолчанию используется значение **No** (Нет). Это значит, что номер является общедоступным.  Дополнительные сведения см. в статье об [общедоступных и частных номерах SKU](../../cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md). |
| **Country/Region Availability** (Доступность по странам и регионам) | Определяет, в каких странах или регионах мира этот номер SKU будет доступен для приобретения. Выберите по меньшей мере одну страну или регион. <!-- TD: Is this parameter an AMP visibility control or a contractual one, or both? --> |  
|  *Цены*   |  |
| **License Model** (Модель лицензирования)| Стандартизированная модель выставления счетов, которая будет использоваться для этого номера SKU.  Если вы выберете вариант **Usage-based monthly billed SKU** (SKU с ежемесячным выставлением счетов на основе использования), откроется дополнительный раздел для настройки цен на каждое ядро и выбора бесплатного пробного периода.  Этот же раздел позволяет экспортировать прайс-лист в Excel или импортировать сохраненный ранее. Подробнее см. статью [Варианты выставления счетов за использование Azure Marketplace](../../billing-options-azure-marketplace.md). | 
|  *VM Images* (Образы виртуальных машин)   |  |
| **Operating System Family** (Семейство операционных систем) | Определяет базовую ОС для решения виртуальной машины: Windows или Linux. |
| **Select Operating System Type** (Выберите тип операционной системы) | Определяет поставщика или выпуск выбранной выше операционной системы. |
| **OS Friendly Name** (Понятное имя операционной системы) | Имя операционной системы, которое будет отображаться для клиентов.  |
| **Recommended VM Sizes** (Рекомендуемые размеры виртуальных машин) | Позволяет выбрать до шести рекомендуемых размеров виртуальных машин из стандартизированного списка.  Хотя эти рекомендации будут явным образом предлагаться потенциальным клиентам, они смогут выбрать любой размер виртуальной машины, совместимый с образом решения. | 
| **Open Ports** (Открытые порты)| Открытые порты и протоколы, которые будут поддерживаться для этого номера SKU.  Эта конфигурация должна совпадать с настройками виртуальной сети, которую вы указали для решения виртуальной машины. Эти параметры вступают в силу после развертывания виртуальной машины. Вы можете изменить параметры порта после публикации номера SKU. Дополнительные сведения см. в статье [Как открыть порты для виртуальной машины Windows с помощью портала Azure](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal). <br/>Ко всем виртуальным машинам добавляются следующие сетевые сопоставления по умолчанию. &emsp; Windows: 3389 -> 3389 TCP, 5986 -> 5986 TCP; &emsp; Linux: 22 -> 22, TCP (SSH). |
| **Disk Version** (Версия диска)  | Номер версии диска и URL-адрес диска, связанные с решением виртуальной машины. Версию диска следует указывать в формате [семантических версий](http://semver.org/), например: `<major>.<minor>.<patch>`.  В качестве URL-адреса укажите URI подписанного URL-адреса, созданный для виртуального жесткого диска операционной системы.  Для каждого номера SKU можно указать до восьми версий диска, но на Azure Marketplace будет отображаться только одна (последняя) версия диска. Другие версии будут доступны для просмотра только через API.  <!--TD: Add more specific link to API --> <br/> Дополнительный раздел **New data disk** (Новый диск данных) позволяет подключить к виртуальной машине до 15 дисков данных.  После публикации номера SKU с определенными значениями версий виртуальной машины и связанных дисков данных вы не сможете изменить эти настройки.  Добавляемые к номеру SKU дополнительные версии виртуальной машины должны поддерживать такое же количество дисков данных. <br/> Если вы еще не создали образ виртуальной машины под управлением Azure, это поле можно заполнить позднее.  Сведения о создании связанных ресурсов виртуальной машины см. в статье о [создании технических ресурсов для виртуальной машины](./cpp-create-technical-assets.md).  
|  |  |

<!-- TD: The CPP UX warning msg indicates that underscores are also supported in these SKU IDs. I suspect this might be true for other identifiers. --> 

<br/> Щелкните **Save** (Сохранить), чтобы сохранить настройки. На следующей вкладке введите информацию о том, поддерживает ли ваше предложение [тестовый выпуск](./cpp-test-drive-tab.md).


## <a name="additional-pricing-considerations"></a>Дополнительные сведения о ценах

Представленная выше модель ценообразования является базовым описанием.  Она может измениться в любой момент, а также зависит от локальных или региональных налоговых правил и политик ценообразования корпорации Майкрософт. 

### <a name="simplified-currency-pricing"></a>Упрощенные цены в разных валютах

С 1 сентября 2018 г. на портале стал доступен новый раздел **Simplified Currency Pricing** (Упрощенные цены в разных валютах). Корпорация Майкрософт упрощает работу с Azure Marketplace, внедряя более предсказуемую модель ценообразования и сбора оплаты от клиентов в разных регионах. Это упрощение включает уменьшение количества валют, в которых клиентам выставляются счета.  Дополнительные сведения см. в статье об [обновлении существующих предложений виртуальных машин в Azure Marketplace](./cpp-update-existing-offer.md).


### <a name="additional-information-on-taxes-and-prices"></a>Дополнительные сведения о налогах и ценах

* В некоторых странах *перечисление налогов осуществляет корпорация Майкрософт*.  Это означает, что корпорация Майкрософт собирает налоги с клиентов и перечисляет их правительству соответствующей страны.  В других странах партнеры обычно обязаны самостоятельно собирать налоги с клиентов и перечислять их правительству. Если вы хотите осуществлять продажи в странах второго типа, следует предусмотреть механизмы для расчета и оплаты соответствующих локальных налогов.  <!-- TD: Find a good reference on taxing policies. The best I found was in the UWP section: https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps -->
* После активации предложения цены уже нельзя изменить. Но вы сможете добавить или удалить поддерживаемые регионы. 
* Корпорация Майкрософт взимает стандартную плату за использование виртуальной машины Azure в дополнение к указанной стоимости номера SKU.
* Для всех регионов цены устанавливаются в местной валюте по курсу, актуальному на момент задания цены.  <!-- TD: Meaning? - Offer created, published, other? -->
* Чтобы задать цену для каждого отдельного региона, экспортируйте электронную таблицу с ценами, внесите нужные изменения и заново импортируйте ее. 

<!-- TD: The detailed information in the table and supplemental notes should be centralized in another topic, maybe "Billing Options" in AMP tree. Need to include a common section on export/import pricing-->

