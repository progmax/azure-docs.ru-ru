---
title: API-интерфейс, пакет SDK и ресурсы .NET для библиотеки Bulk Executor (Azure Cosmos DB) | Документы Майкрософт
description: Сведения об API и пакетах SDK .NET для библиотеки Bulk Executor, в том числе даты выхода, даты снятия с учета и изменения, внесенные в каждую версию пакета SDK .NET для Bulk Executor в Azure Cosmos DB.
author: tknandu
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: ae9560296e37ff5492c07e69e6ba0eb5539915c8
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52308501"
---
# <a name="net-bulk-executor-library-download-information"></a>Библиотека Bulk Executor .NET — информация о скачивании 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Веб-канал изменений в .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Поставщик ресурсов REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Bulk Executor — .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor — Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Описание**</td><td>Библиотека Bulk Executor позволяет клиентским приложениям выполнять массовые операции в учетных записях Azure Cosmos DB. Библиотека Bulk Executor предоставляет пространства имен BulkImport, BulkUpdate и BulkDelete. Модуль BulkImport может оптимизировать массовый прием документов, обеспечивая использование максимального объема пропускной способности, подготовленной для коллекции. Модуль BulkUpdate позволяет выполнять массовое обновление существующих данных в контейнерах Azure Cosmos DB в качестве исправлений. Модуль BulkDelete может оптимизировать массовое удаление документов, обеспечивая использование максимального объема пропускной способности, подготовленной для коллекции.</td></tr>

<tr><td>**Скачивание пакета SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**Библиотека BulkExecutor в GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**Документация по API**</td><td>[Справочная документация по API .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Начало работы**</td><td>[Начало работы с пакетом SDK для .NET для библиотеки Bulk Executor](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Текущая поддерживаемая платформа**</td><td>Microsoft .NET Framework 4.5.2, 4.6.1 и .NET Standard 2.0 </td></tr>
</table></br>

## <a name="release-notes"></a>Заметки о выпуске

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Включено MongoBulkExecutor для поддержки .NET Standard 2.0. Благодаря этой возможности выпуск функционально эквивалентен выпуску 1.3.0 с добавлением поддержки .NET Standard 2.0 в качестве целевой платформы.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-preview

* Добавлена .NET Standard 2.0 в качестве одной из поддерживаемых целевых платформ, чтобы библиотека BulkExecutor работала с приложениями .NET Core.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Добавлена перегрузка операции BulkDelete для учетных записей SQL API для приема ключа секции и кортежи идентификатора документа для удаления.
* Добавлена перегрузка операции BulkDelete для учетных записей SQL API для приема RequestOptions, содержащих ключ секции, указывающий значение ключа секции, в дополнение к использованию его в качестве фильтра во входном запросе, указывающем документы, которые требуется удалить.
* Устранена проблема, вызывавшая проблему форматирования в агенте пользователя, используемом в BulkExecutor.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Внесены улучшения в импорт BulkExecutor и API-интерфейсы обновления для прозрачной адаптации к гибкому масштабированию контейнера Cosmos DB при превышении текущей емкости хранилища без вызова исключений.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Зависимость пакета SDK для .NET DocumentDB обновлена до версии 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Устранена проблема, при которой библиотека BulkExecutor выдавала ошибку JSRT во время импорта в фиксированные коллекции.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Добавлена поддержка операции BulkDelete для учетных записей API Azure Cosmos DB SQL.
* Добавлена поддержка операции BulkImport для учетных записей API Azure Cosmos DB MongoDB.
* Зависимость пакета SDK для .NET DocumentDB обновлена до версии 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Добавлена поддержка операции BulkImport для учетных записей API Azure Cosmos DB Gremlin.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Исправлена незначительная ошибка операции BulkImport для учетных записей API Azure Cosmos DB SQL.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Добавлена поддержка операции BulkImport и BulkUpdate для учетных записей API Azure Cosmos DB SQL.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о библиотеке Java Bulk Executor см. в следующей статье:

[Библиотека массового исполнителя Java — информация о скачивании](sql-api-sdk-bulk-executor-java.md)
