---
title: Обзор службы "Настройка состояния службы автоматизации Azure"
description: Обзор DSC службы "Настройка состояния службы автоматизации Azure", условий использования и распространенных проблем
keywords: PowerShell DSC, настройка требуемого состояния, PowerShell DSC для Azure
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1f28f642d1a5fc30055c73a4b7d60c076c83d204
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250107"
---
# <a name="azure-automation-state-configuration-overview"></a>Обзор службы "Настройка состояния службы автоматизации Azure"

"Настройка состояния службы автоматизации Azure" — это служба Azure, которая позволяет создавать и компилировать [конфигурации](/powershell/dsc/configurations)PowerShell Desired State Configuration (DSC), управлять ими, импортировать [ресурсы DSC](/powershell/dsc/resources), а также назначать конфигурации целевым узлам, и все это в облаке.

## <a name="why-use-azure-automation-state-configuration"></a>Преимущества службы "Настройка состояния службы автоматизации Azure"

Служба "Настройка состояния службы автоматизации Azure" предоставляет ряд преимуществ по сравнению с использованием DSC за пределами Azure.

### <a name="built-in-pull-server"></a>Встроенный опрашивающий сервер

Служба "Настройка состояния службы автоматизации Azure" предоставляет опрашивающий сервер DSC, подобный [службе DSC компонента Windows](/powershell/dsc/pullserver), чтобы целевые узлы автоматически получали конфигурации, соответствовали требуемому состоянию и сообщали о соответствии. Встроенный опрашивающий сервер в службе автоматизации Azure избавляет от необходимости устанавливать и обслуживать собственный опрашивающий сервер. Служба автоматизации Azure может работать с виртуальными и физическими машинами под управлением Windows или Linux, размещенными в облаке или локально.

### <a name="management-of-all-your-dsc-artifacts"></a>Управление всеми артефактами DSC

Служба "Настройка состояния службы автоматизации Azure" обеспечивает тот же слой управления для [Desired State Configuration PowerShell](/powershell/dsc/overview), который служба автоматизации Azure предлагает для скриптов PowerShell.

Вы можете управлять всеми конфигурациями, ресурсами и целевыми узлами DSC с помощью портала Azure или PowerShell.

![Снимок экрана со страницей службы автоматизации Azure](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Импорт данных отчетов в Log Analytics

Узлы, управление которыми осуществляется с помощью "Настройка состояния службы автоматизации Azure", отправляют подробные отчеты с данными о состоянии на встроенный опрашивающий сервер. В службе "Настройка состояния службы автоматизации Azure" можно настроить отправку этих данных в рабочую область Log Analytics. Сведения об отправке данных о состоянии State Configuration в рабочую область Log Analytics см. в статье [Пересылка данных отчетов "Настройка состояния службы автоматизации Azure" в Log Analytics](automation-dsc-diagnostics.md).

## <a name="network-planning"></a>Настройка сети

Для обмена данными между State Configuration (DSC) и службой автоматизации требуются следующие порт и URL-адреса:

* порт: только исходящий трафик Интернета через TCP-порт 443.
* Глобальный URL-адрес: *.azure-automation.net.
* Глобальный URL-адрес US Gov (Вирджиния): *.azure automation.us
* Служба агента: https://\<ИД рабочей области\>.agentsvc.azure-automation.net

При определении исключений рекомендуется использовать адреса из списка. Если вам нужны IP-адреса, вы можете скачать [список диапазонов IP-адресов центров обработки данных Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). Файл обновляется еженедельно и содержит развернутые в настоящее время диапазоны и все предстоящие изменения диапазонов IP-адресов.

При наличии учетной записи службы автоматизации, определенной для конкретного региона, можно ограничить обмен данными с таким региональным центром обработки данных. В таблице ниже содержатся записи DNS для каждого региона.

| **Регион** | **Запись DNS** |
| --- | --- |
| Западно-центральная часть США | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Центрально-южная часть США |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Восток США 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Центральная Канада |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Западная Европа |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Северная Европа |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Юго-Восточная Азия |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Центральная Индия |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Восточная часть Японии |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Юго-Восточная Австралия |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Южная часть Великобритании | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| Правительство штата Вирджиния | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Для списка IP-адресов региона вместо его имен скачайте XML-файл [IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/details.aspx?id=41653) из Центра загрузки Майкрософт и ознакомьтесь с ним.

> [!NOTE]
> В XML-файле IP-адресов центра обработки данных Azure приводится список диапазонов IP-адресов, используемых в центрах обработки данных Microsoft Azure. Этот файл включает диапазоны вычисления, хранения и SQL.
>
>Обновленный файл публикуется еженедельно. Он отражает развернутые в настоящее время диапазоны и все предстоящие изменения IP-адресов. Новые диапазоны, появившиеся в файле, не будут использоваться в центрах обработки данных по крайней мере одну неделю.
>
> Скачивайте новый XML-файл каждую неделю и вносите соответствующие изменения на своем сайте, чтобы правильно определять службы, выполняемые в Azure. Пользователям Azure ExpressRoute следует обратить внимание, что этот файл используется для того, чтобы обновлять протокол BGP в пространстве Azure в первую неделю каждого месяца.

## <a name="introduction-video"></a>Видео: общие сведения

Предпочитаете смотреть, а не читать? Посмотрите следующий видеоролик, выпущенный в мае 2015 г., когда впервые было объявлено о создании службы "Настройка состояния службы автоматизации Azure".

> [!NOTE]
> Несмотря на то, что основные концепции и жизненный цикл, о которых рассказывается в этом видеоролике, по-прежнему актуальны, с момента записи этого видеоролика служба "Настройка состояния службы автоматизации Azure" сделала большой шаг вперед. Теперь она стала общедоступной, оснащена более функциональным пользовательским интерфейсом на портале Azure и поддерживает множество дополнительных возможностей.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Дополнительная информация

- Чтобы приступить к работе со службой "Настройка состояния службы автоматизации Azure", см. сведения в [этой статье](automation-dsc-getting-started.md).
- Дополнительные сведения о подключении узлов см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).
- Сведения о компилировании конфигураций DSC, которые затем можно назначить целевым узлам, см. в статье [Компилирование конфигураций в Azure Automation DSC](automation-dsc-compile.md).
- Справочник по командлетам PowerShell для службы "Настройка состояния службы автоматизации Azure" см. в [этой статье](/powershell/module/azurerm.automation/#automation).
- Сведения о ценах см. на странице [с ценами на службу "Настройка состояния службы автоматизации Azure"](https://azure.microsoft.com/pricing/details/automation/).
- Пример использования службы "Настройка состояния службы автоматизации Azure" и Chocolatey в конвейере непрерывного развертывания см. в [этой статье](automation-dsc-cd-chocolatey.md).