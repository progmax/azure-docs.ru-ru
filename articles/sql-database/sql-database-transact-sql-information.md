---
title: Отличия Transact-SQL базы данных SQL Azure | Документы Майкрософт
description: Инструкции Transact-SQL, которые не полностью поддерживаются в базе данных SQL Azure
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: fc8336a46f61a7c9ab7c174b5f24d907369f481c
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567576"
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>Отличия Transact-SQL базы данных SQL Azure

При [миграции базы данных](sql-database-cloud-migrate.md) из SQL Server на Azure SQL Server может оказаться, что базу данных необходимо переработать до переноса. В этой статье содержатся рекомендации, которые помогут выполнить переработку и понять основные причины ее необходимости. Чтобы обнаружить проблемы с совместимостью, используйте [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Обзор

Большинство функций Transact-SQL, используемых приложениями, полностью поддерживаются как в Microsoft SQL Server, так и в базе данных SQL Azure. Например, основные компоненты SQL, такие как типы данных, операторы, строковые, арифметические, логические функции, функции работы с курсорами, работают одинаково и на сервере SQL Server, и в базе данных SQL. Но существует несколько различий T-SQL между элементами DDL (языка определения данных) и элементами DML (языка манипулирования данными), использование которых приводит к формированию частично поддерживаемых инструкций и запросов T-SQL (будет рассматриваться далее в этой статье).

Кроме того, некоторые функции и синтаксис не поддерживаются вообще, так как база данных SQL Azure предназначена для изоляции функции от любых зависимостей в базе данных master и операционной системе. Поэтому многие действия на уровне сервера не подходят для базы данных SQL. Инструкции и параметры T-SQL недоступны, если они используются для настройки параметров серверного уровня, компонентов операционной системы, а также для задания конфигурации файловой системы. Если требуются такие возможности, их часто можно заменить соответствующими альтернативами, доступными в базе данных SQL или другой службе (компоненте) Azure.

Например, высокий уровень доступности встроен в Базу данных SQL Azure с помощью технологии, аналогичной [группам доступности AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). По этой причине в Базе данных SQL не поддерживаются все инструкции T-SQL, связанные с группами доступности, и динамические административные представления, связанные с группами доступности AlwaysOn.

Список поддерживаемых и неподдерживаемых в базе данных SQL функций см. в статье  [Сравнение функций Базы данных SQL Azure и SQL Server](sql-database-features.md). Список на этой странице дополняет сведения в статье с рекомендациями и функциями и содержит перечень инструкций Transact-SQL.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Инструкции синтаксиса Transact-SQL с частичными различиями

Основные инструкции DDL (язык определения данных) доступны, но некоторые инструкции DDL имеют расширения, связанные с размещением на диске, и неподдерживаемые функции.

- У инструкций CREATE и ALTER DATABASE есть три десятка параметров. Эти инструкции включают параметры размещения файлов, FILESTREAM и компонента Service Broker, которые применяются только к SQL Server. Они могут не иметь значения при создании баз данных до миграции, но при переносе кода T-SQL, который создает базы данных, следует сравнить [CREATE DATABASE (база данных SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx) с синтаксисом SQL Server для [CREATE DATABASE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx), чтобы убедиться, что поддерживаются все используемые параметры. CREATE DATABASE для базы данных SQL Azure также располагает параметрами для цели службы и параметрами эластичного масштабирования, которые применяются только к базе данных SQL.
- Инструкции CREATE и ALTER TABLE имеют параметры FileTable, которые нельзя использовать в базе данных SQL, так как FILESTREAM не поддерживается.
- Инструкции входа CREATE и ALTER поддерживаются, но база данных SQL не предоставляет все параметры. Чтобы сделать базу данных более переносимой, база данных SQL рекомендует по возможности вместо имен входа использовать пользователей автономной базы данных. Дополнительные сведения см. в разделах [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) и [Предоставление доступа к базе данных и управление им](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Синтаксис Transact-SQL, не поддерживаемый в базе данных SQL Azure

Помимо неподдерживаемых инструкций Transact-SQL, описанных в статье  [Сравнение функций Базы данных SQL Azure и SQL Server](sql-database-features.md), не поддерживаются также следующие инструкции и группы инструкций. Таким образом, если база данных, которую нужно перенести, использует любую их следующих функций или компонентов, перестройте T-SQL для исключения этих функций и инструкций T-SQL.

— Параметры сортировки системных объектов. — Параметры, связанные с подключением: инструкции конечной точки. База данных SQL не поддерживает аутентификацию Windows, но поддерживает аналогичную аутентификацию Azure Active Directory. Для некоторых типов аутентификации требуется последняя версия SSMS. Дополнительные сведения см. в статье  [Использование аутентификации Azure Active Directory для аутентификации с помощью SQL](sql-database-aad-authentication.md).
— Межбазовые запросы с помощью имен из трех или четырех частей. (Межбазовые запросы только для чтения поддерживаются с помощью  [запроса к эластичной базе данных](sql-database-elastic-query-overview.md).) — Межбазовые цепочки владения, параметр `TRUSTWORTHY`  — `EXECUTE AS LOGIN` (вместо него следует использовать EXECUTE AS USER).
— Шифрование не поддерживается, за исключением расширенного управления ключами. — Отправка событий: события, уведомления о событиях, уведомления о запросах. — Размещение файла : синтаксис, связанный с размещением файла базы данных и его размером, и файлы базы данных, которые автоматически управляются Microsoft Azure.
- Высокая доступность: синтаксис, связанный с обеспечением высокой доступности, для управления которым используется учетная запись Microsoft Azure. Например, синтаксис, используемый для резервного копирования, восстановления, AlwaysOn, зеркального отображения базы данных, доставки журналов и режимов восстановления.
— Средство чтения журнала: синтаксис, который зависит от средства чтения журнала, недоступного в базе данных SQL — принудительная репликация, запись измененных данных. База данных SQL может выступать подписчиком принудительной репликации.
— Функции: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes` — Оборудование: синтаксис, относящийся к параметрам сервера, связанного с оборудованием — память, рабочие потоки, фиксирование потоков на ЦП, флаги трассировки и т. д. Вместо этого укажите уровни служб и объемы вычислительных ресурсов.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE` и четырехкомпонентные имена. — .NET Framework: интеграция SQL Server со средой CLR. — Семантический поиск. — Учетные данные сервера (вместо них используйте [учетные данные базы данных](https://msdn.microsoft.com/library/mt270260.aspx)).
— Элементы серверного уровня: роли сервера, `sys.login_token`. Элементы `GRANT`, `REVOKE` и `DENY` в разрешениях серверного уровня недоступны, хотя некоторые заменяются разрешениями уровня базы данных. Некоторые полезные динамические административные представления серверного уровня имеют эквивалентные динамические административные представления уровня базы данных.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
-Параметры  `sp_configure`  и инструкция  `RECONFIGURE`. Некоторые параметры доступны в инструкции  [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- Агент SQL Server: синтаксис, который зависит от агента SQL Server или базы данных MSDB — оповещения, операторы, центральные серверы управления. Вместо этого используйте скрипты, например Azure PowerShell.
— Подсистема аудита SQL Server: вместо нее следует использовать аудит базы данных SQL.
— Трассировка SQL Server. — Флаги трассировки: некоторые элементы флагов трассировки были перемещены в режимы совместимости.
— Отладка Transact-SQL. — Триггеры: триггеры уровня сервера или входа. — Инструкция  `USE` . Чтобы изменить контекст базы данных на другую базу данных, необходимо создать подключение к новой базе данных.

## <a name="full-transact-sql-reference"></a>Полный справочник по Transact-SQL

Дополнительные сведения о грамматике языка Transact-SQL, его использовании и примеры см. в  [справочнике по Transact-SQL (ядро СУБД)](https://msdn.microsoft.com/library/bb510741.aspx) в электронной документации по SQL Server.

### <a name="about-the-applies-to-tags"></a>Сведения о тегах "Относится к"

Справочник по Transact-SQL включает в себя статьи, относящиеся к версиям SQL Server с 2008 по текущую. Под заголовком статьи есть панель значков, на которой указаны четыре платформы SQL Server и их применимость. Например, группы доступности появились в SQL Server 2012. Статья  [CREATE AVAILABILITY GROUP (Transact-SQL)](https://msdn.microsoft.com/library/ff878399.aspx) указывает, что эта инструкция применяется в  **SQL Server (начиная с версии 2012)**. Инструкция не применяется в SQL Server 2008, SQL Server 2008 R2, Базе данных SQL Azure, хранилище данных SQL Azure или Parallel Data Warehouse.

В некоторых случаях функция или инструкция, рассматриваемая в статье, может использоваться в продукте, однако по-разному поддерживаться в разных продуктах. Эти различия отмечаются по мере изложения материала. В некоторых случаях функция или инструкция, рассматриваемая в статье, может использоваться в продукте, однако по-разному поддерживаться в разных продуктах. Эти различия отмечаются по мере изложения материала. Например, статья CREATE TRIGGER доступна в базе данных SQL. Но параметр **ALL SERVER** для триггеров уровня сервера указывает, что триггеры уровня сервера нельзя использовать в базе данных SQL. Вместо них следует использовать триггеры уровня базы данных.

## <a name="next-steps"></a>Дополнительная информация

Список поддерживаемых и неподдерживаемых в базе данных SQL функций см. в статье  [Сравнение функций Базы данных SQL Azure и SQL Server](sql-database-features.md). Список на этой странице дополняет сведения в статье с рекомендациями и функциями и содержит перечень инструкций Transact-SQL.
