---
title: Примеры Azure CLI для Azure Cosmos DB
description: Примеры Azure CLI для создания учетных записей, баз данных, контейнеров, регионов и брандмауэров Azure Cosmos DB и управления ими.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 48beb93fbc5952951fff1ed31e5f8625faf78ccd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850403"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Примеры Azure CLI для Azure Cosmos DB

В следующей таблице содержатся ссылки на примеры сценариев Azure CLI для Azure Cosmos DB. Страницы справки для всех команд интерфейса командной строки Azure Cosmos DB доступны в [справочнике по Azure CLI](/cli/azure/cosmosdb).

| |  |
|---|---|
|**Создание учетной записи, базы данных и контейнеров Azure Cosmos DB**||
| [Создание учетной записи API SQL](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Создание отдельной учетной записи, базы данных и контейнера API SQL в Azure Cosmos DB. |
| [Создание учетной записи API MongoDB](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Создание отдельной учетной записи, базы данных и коллекции API MongoDB в Azure Cosmos DB. |
| [Создание учетной записи API Gremlin](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Создание отдельной учетной записи, базы данных и графа API Gremlin в Azure Cosmos DB. |
| [Создание учетной записи API Cassandra](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Создание отдельной учетной записи и базы данных API Cassandra в Azure Cosmos DB. |
| [Создание учетной записи API таблицы](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Создание отдельной учетной записи, базы данных и таблицы API таблиц в Azure Cosmos DB. |
|**Масштабирование Azure Cosmos DB**||
| [Масштабирование пропускной способности контейнера](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Изменение подготовленной пропускной способности контейнера.|
| [Репликация учетной записи базы данных Azure Cosmos DB в нескольких регионах и настройка приоритетов отработки отказа](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Глобальная репликация данные учетной записи в нескольких регионах с указанным приоритетом отработки отказа.|
|**Безопасность базы данных Azure Cosmos DB**||
| [Получение ключей учетной записи](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Возвращение первичного и вторичного главных ключей для записи и первичного и вторичного ключей только для чтения для учетной записи.|
| [Получение строки подключения MongoDB](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Получает строку подключения для подключения приложения MongoDB к учетной записи базы данных Azure Cosmos DB.|
| [Повторное создание ключей учетной записи](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Повторное создание ключей для учетной записи.|
| [Создание брандмауэра](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Создание политики контроля доступа для входящих IP-адресов для разрешения доступа к учетной записи только с утвержденных компьютеров и (или) из облачных служб.|
|**Высокий уровень доступности, аварийное восстановление, архивация и восстановление**||
| [Настройка политики отработки](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Настройка приоритета отработки отказа для каждого региона, в котором реплицируется учетная запись.|
|**Подключение Azure Cosmos DB к ресурсам**||
| [Подключение веб-приложения к Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Создание и подключение базы данных Azure Cosmos DB и веб-приложения Azure.|
|||
