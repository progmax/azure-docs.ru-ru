---
title: Включение прозрачного шифрования данных в центре безопасности Azure | Документация Майкрософт
description: В этом документе объясняется, как выполнить рекомендацию центра безопасности Azure по **включению прозрачного шифрования данных**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: dbe1b3e3515f05f9addb8d2ac9333407ea2c0984
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336651"
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Включение прозрачного шифрования данных в центре безопасности Azure
Центр безопасности Azure порекомендует включить прозрачное шифрование данных (TDE) для баз данных SQL, если оно еще не включено. Прозрачное шифрование данных обеспечивает защиту данных и помогает соблюдать нормативные требования за счет шифрования базы данных, связанных с ней резервных копий и файлов журнала транзакций в местах хранения без необходимости вносить изменения в приложение. Чтобы узнать больше, ознакомьтесь с разделом [Прозрачное шифрование данных в Базе данных SQL Azure](https://msdn.microsoft.com/library/dn948096).

Данная рекомендация относится только к службе SQL Azure и не охватывает службы SQL, работающие на виртуальных машинах.

> [!NOTE]
> В документе приводится обзор службы с помощью примера развертывания.  Он не является пошаговым руководством.
>
>

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций
1. В колонке **Рекомендации** выберите **Включить прозрачное шифрование данных**.
   ![Включение прозрачного шифрования данных][1]
2. Откроется колонка **Включение прозрачного шифрования данных для баз данных SQL** . Выберите базу данных SQL для включения прозрачного шифрования данных.
   ![Выбор базы данных SQL для включения прозрачного шифрования данных][2]
3. В колонке **Прозрачное шифрование данных** в разделе "Шифрование данных" щелкните **Вкл.**, затем щелкните **Сохранить** в верхней части ленты колонки.
   ![Включение прозрачного шифрования данных][3]

   После включения прозрачного шифрования данных для выбранной базы данных SQL **состояние шифрования** изменится на **Зашифровано**.    

   ![Состояние шифрования][4]

## <a name="see-also"></a>См. также
В этой статье объясняется, как выполнить рекомендацию центра безопасности по включению прозрачного шифрования данных. Чтобы узнать больше о прозрачном шифровании данных SQL, ознакомьтесь со следующими статьями.

* [Прозрачное шифрование данных в Базе данных SQL Azure](https://msdn.microsoft.com/library/dn948096)
* [Начало работы с прозрачным шифрованием данных (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md) — узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md) — узнайте, как наблюдать за работоспособностью ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](https://blogs.msdn.com/b/azuresecurity/) — последние новости и информация об обеспечении безопасности в Azure.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
