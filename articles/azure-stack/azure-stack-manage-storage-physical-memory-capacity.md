---
title: Управление емкостью физической памяти для Azure Stack | Документация Майкрософт
description: Мониторинг доступного объема хранилища и управление им для Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/28/2018
ms.author: mabrigg
ms.reviewer: Thomas.Roettinger
ms.openlocfilehash: 0516ee7a8319b85765280b4c84f5febec8343ada
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965621"
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Управление емкостью физической памяти для Azure Stack

*Область применения: интегрированные системы Azure Stack*

Чтобы увеличить общую емкость доступной памяти для Azure Stack, можно добавить дополнительный объем памяти. Физический сервер в Azure Stack также называется *узлом единицы масштабирования*. Все узлы единиц масштабирования, входящие в одну единицу масштабирования, должны иметь одинаковый объем памяти.

> [!note]  
> Прежде чем продолжить, обратитесь к документации изготовителя оборудования, чтобы узнать, поддерживается ли в этом оборудовании физическое обновление памяти. По контракту на поддержку с поставщиком оборудования OEM от поставщика может требоваться выполнять физическую замену серверной стойки и обновление встроенного ПО устройства.

На блок-схеме ниже показан общий процесс добавления памяти для каждого узла единицы масштабирования.

![Добавление памяти в каждый узел единицы масштабирования](media/azure-stack-manage-storage-physical-capacity/process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Добавление памяти в существующий узел
Следующие инструкции дают общее представление о процессе добавления памяти. 

> [!Warning]  
Не выполняйте приведенные ниже действия, не ознакомившись с документацией изготовителя оборудования.

> [!Warning]  
Вся единица масштабирования должна быть отключена, так как последовательное обновление памяти не поддерживается.

1. Остановите Azure Stack, следуя инструкциям, приведенным в статье [Запуск и остановка Azure Stack](azure-stack-start-and-stop.md).
2. Обновите память на каждом физическом компьютере, используя документацию изготовителя оборудования.
3. Запустите Azure Stack, следуя инструкциям, приведенным в статье [Запуск и остановка Azure Stack](azure-stack-start-and-stop.md).

## <a name="next-steps"></a>Дополнительная информация

 - Сведения об управлении учетными записями хранения, включая их поиск, восстановление и освобождение емкости хранилища в соответствии с бизнес-потребностями, см. в статье [Управление учетными записями хранения в Azure Stack](azure-stack-manage-storage-accounts.md).
 - Сведения, короткие помогают оператору облака Azure Stack отслеживать емкость хранилища развертывания Azure Stack и управлять ею, приведены в статье [Управление емкостью хранилища для Azure Stack](azure-stack-manage-storage-shares.md). 
