---
title: Хранение резервных копий базы данных SQL Azure до 10 лет | Документы Майкрософт
description: Узнайте, как база данных SQL Azure поддерживает хранение полных резервных копий базы данных в течение 10 лет.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/24/2018
ms.openlocfilehash: 7fe34423e706054daf84eaa8baf45fe201a661c9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026183"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Хранение резервных копий базы данных SQL Azure до 10 лет

В соответствии с нормативными требованиями, стандартами соответствия или бизнес-задачами многие приложения должны хранить резервные копии баз данных более 7–35 дней, то есть дольше, чем при [автоматическом резервном копировании](sql-database-automated-backups.md) базы данных SQL Azure. Используя возможность долгосрочного хранения (LTR), можно хранить полные резервные копии указанной базы данных SQL в хранилище BLOB-объектов [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) сроком до 10 лет. Впоследствии любую резервную копию можно восстановить как новую базу данных.

> [!NOTE]
> LTR можно включить для базы данных, размещенной в логических серверах базы данных SQL Azure. Он пока недоступен для баз данных, размещенных в Управляемых экземплярах.
> 

## <a name="how-sql-database-long-term-retention-works"></a>Принципы работы долгосрочного хранения базы данных SQL

При долгосрочном хранении (LTR) резервных копий используются полные резервные копии базы данных, которые [создаются автоматически](sql-database-automated-backups.md) для восстановления до точки во времени (PITR). Эти резервные копии копируются в другие большие двоичные объекты хранения, если настроена политика LTR.
Существует возможность настройки политики LTR для каждой базы данных SQL с указанием частоты копирования резервных копий для BLOB-объектов долгосрочного хранения. Для обеспечения подобной гибкости нужно определить политику с применением комбинации четырех параметров: еженедельное (W), ежемесячное (М), ежегодное (Y) хранение копий, а также в определенные недели года (WeekOfYear). Если указать W, одна резервная копия каждую неделю будет копироваться для долгосрочного хранения. Если указать M, в первую неделю каждого месяца одна резервная копия копируется для долгосрочного хранения. Если указать Y, одна резервная копия в течение недели года, указанной в параметре WeekOfYear, копируется для долгосрочного хранения. Каждая резервная копия хранится в хранилище долгосрочного хранения в течение срока, заданного в соответствующих параметрах. 

Примеры:

-  W = 0, M = 0, Y = 5, WeekOfYear = 3

   3-я полная ежегодная резервная копия хранится в течение 5 лет.
- W = 0, M = 3, Y = 0

   Первая полная ежемесячная резервная копия хранится в течение 3 месяцев.

- W = 12, M = 0, Y = 0

   Каждая полная недельная резервная копия хранится в течение 12 недель.

- W = 6, M = 12, Y = 10, WeekOfYear = 16

   Каждая полная недельная резервная копия хранится в течение 6 недель. За исключением первой полной месячной резервной копии, которая подлежит хранению в течение 12 месяцев. За исключением полной резервной копии, сделанной на 16-ю неделю года, которая подлежит хранению в течение 10 лет. 

В следующей таблице приведена периодичность и сроки долгосрочного хранения резервных копий согласно следующей политике:

W = 12 недель (84 дня), M = 12 месяцев (365 дней), Y = 10 лет (3650 дней), WeekOfYear = 15 (неделя после 15 апреля)

   ![Пример долгосрочного хранения](./media/sql-database-long-term-retention/ltr-example.png)


 
Если изменить указанную выше политику и задать значение W = 0 (отмена еженедельных резервных копий), периодичность резервного копирования изменится согласно порядку, приведенному в таблице выше, на указанные сроки. Размеры хранилища, необходимые для хранения этих резервных копий, сократятся соответственно. 

> [!NOTE]
1. Копии LTR создаются службой хранилища Azure, и поэтому процесс копирования не влияет на производительность имеющейся базы данных.
2. Политика применяется к будущим резервным копиям. (например, если параметр WeekOfYear указан при настройке политики, то первая резервная копия LTR создается в следующему году.) 
3. Восстановление базы данных из хранилища долгосрочного хранения (LTR) можно выполнить, выбрав необходимую резервную копию по метке времени.   Базу данных можно восстановить на любой доступный сервер с той же подпиской, что и у исходной базы данных. 
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Георепликация и долгосрочное хранение резервных копий

Если вы используете активную георепликацию или группы отработки отказа в своем решении обеспечения непрерывности бизнеса, необходимо подготовиться к возможным отказам и настроить аналогичную политику LTR в базе данных-получателе с георепликацией. Это не увеличит стоимость хранилища LTR, поскольку резервные копии не генерируются из вторичных ресурсов. Резервные копии будут создаваться только когда вторичная база данных становится первичной. Таким образом, можно обеспечить бесперебойную генерацию резервных копий LTR при активации отработки отказа и переход основной базы данных на вторичную область. 

> [!NOTE]
Когда исходная база данных-источник восстановится после сбоя, что приводит к ее отказу, она станет новой вторичной. Таким образом создание резервной копии не будет возобновлено, и существующая политика LTR не вступит в силу, пока она снова станет первичной. 
> 

## <a name="configure-long-term-backup-retention"></a>Настройка долгосрочного хранения резервных копий

Подробные сведения о настройке долгосрочного хранения с помощью портала Azure или с помощью PowerShell см. в статье [Настройка долгосрочного хранения резервных копий для Базы данных SQL Azure и восстановление из резервной копии](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Дополнительная информация

Поскольку резервные копии базы данных защищают данные от случайного повреждения или удаления, они являются важной частью любой стратегии непрерывности бизнес-процессов и аварийного восстановления. Дополнительные сведения о других решениях для базы данных SQL, обеспечивающих непрерывность бизнес-процессов, см. в статье [Обзор обеспечения непрерывности бизнес-процессов с помощью базы данных SQL Azure](sql-database-business-continuity.md).
