---
title: Параметры центра безопасности Azure | Документация Майкрософт
description: Настройка параметров центра безопасности Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f24b1e4a-cc36-4542-b21e-041453cdfcd8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/3/2018
ms.author: rkarlin
ms.openlocfilehash: 2aa4545aa79260bea792392e1bebf4166253fc87
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315721"
---
# <a name="security-center-settings"></a>Параметры центра безопасности
В этой статье представлен обзор параметров центра безопасности.

В политике безопасности можно настроить следующие параметры:

- **Сбор данных**. Определяет настройки подготовки агента и [сбора данных](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection).
- **Политика безопасности**. Определяет, какие параметры отслеживаются и рекомендуются центром безопасности. [Политику безопасности](security-center-azure-policy.md) можно изменить в центре безопасности. Кроме того, вы можете использовать [Политику Azure](security-center-azure-policy.md), чтобы создавать определения, определять дополнительные политики и назначать политики в группах управления. 
- **Уведомления по электронной почте.** Определяют контактные данные для обеспечения безопасности и параметры [уведомлений по электронной почте](security-center-provide-security-contact-details.md).
- **Ценовая категория**. Определяет выбор [ценовой категории](security-center-pricing.md) ("Бесплатная" или "Стандартная"). Выбранная ценовая категория определяет функции центра безопасности, доступные для выбранных ресурсов. Ценовую категорию можно указать для подписок, групп ресурсов и рабочих областей.

> [!NOTE]
> Все эти параметры можно задать для отдельной подписки. Для рабочих областей можно задать только параметры сбора данных и ценовой категории. Для групп ресурсов можно задать только ценовую категорию.
>


## <a name="who-can-edit-security-policies"></a>Кто может изменять политики безопасности?
Центр безопасности использует управление доступом на основе ролей (RBAC), в котором предусмотрены встроенные роли. Эти роли можно назначать пользователям, группам и службам в Azure. В центре безопасности пользователи видят сведения только о тех ресурсах, к которым у них есть доступ, то есть те, для которых им назначена роль *владельца*, *участника* или *читателя* подписки либо группы ресурсов, к которым относится ресурс. Помимо этих ролей, существует две конкретные роли центра обеспечения безопасности:

- **Читатель безопасности**. Пользователь с этой ролью имеет право просматривать центр безопасности, включая рекомендации, оповещения, политики и состояние работоспособности, но не может вносить изменения.
- **Администратор безопасности**. Имеет те же права, что и *читатель безопасности*, но также может обновлять политику безопасности, отклонять рекомендации и оповещения.


## <a name="next-steps"></a>Дополнительная информация
В этой статье вы ознакомились с политиками безопасности в центре безопасности Azure. Дополнительные сведения о центре безопасности Azure см. в следующих статьях:

* [Настройка политик безопасности в центре безопасности Azure](security-center-azure-policy.md) — сведения о настройке политик безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — сведения о том, как рекомендации центра безопасности могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как отслеживать работоспособность ресурсов Azure.
* [Управление оповещениями безопасности в центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
- [Защита данных в центре безопасности Azure](security-center-data-security.md) — сведения о том, как центр безопасности управляет данными и защищает их.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — получите ответы на часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](https://blogs.msdn.com/b/azuresecurity/) — последние новости и сведения об обеспечении безопасности в Azure.
