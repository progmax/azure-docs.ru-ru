---
title: Сведения о настройке срока жизни и управлении им в Azure Cosmos DB
description: Сведения о настройке срока жизни и управлении им в Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/14/2018
ms.author: mjbrown
ms.openlocfilehash: 209234e91d0ff788bf828dae0e56f37f9921b5c1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52835035"
---
# <a name="how-to-configure-time-to-live-in-azure-cosmos-db"></a>Как настроить срок жизни в Azure Cosmos DB

В Azure Cosmos DB вы можете настроить срок жизни (TTL) на уровне контейнера или переопределить его на уровне элемента, когда он будет настроен для контейнера. Срок жизни для контейнера можно настроить с помощью портала Azure или пакетов SDK для конкретных языков. Переопределение срока жизни на уровне элемента можно настроить с использованием пакетов SDK.

## <a name="enable-time-to-live-on-a-container-using-azure-portal"></a>Включение срока жизни для контейнера с помощью портала Azure

Следуйте инструкциям ниже, чтобы включить в контейнере неограниченный срок жизни. Включите этот параметр, чтобы срок жизни можно было переопределить на уровне элемента. Вы также можете задать значение срока жизни, введя ненулевое значение, обозначающее период в секундах.

1. Войдите на [портале Azure](https://portal.azure.com/).

2. Создайте новую учетную запись Azure Cosmos или выберите имеющуюся.

3. Откройте область **Data Explorer**.

4. Выберите существующий контейнер, разверните его и измените следующие значения:

   * Откройте окно **Scale & Settings** (Параметры масштабирования).
   * В разделе **Параметры** найдите **Срок жизни**.
   * Выберите **Включен (по умолчанию)** или **Включен** и задайте значение срока жизни.
   * Щелкните **Сохранить** , чтобы сохранить изменения.

   ![Настройка срока жизни на портале Azure](./media/how-to-time-to-live/how-to-time-to-live-portal.png)

## <a name="enable-time-to-live-on-a-container-using-sdk"></a>Включение срока жизни для контейнера с помощью пакета SDK

### <a id="dotnet-enable-noexpiry"></a>Пакет SDK для .NET

```csharp
// Create a new collection with TTL enabled and without any expiration value
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = -1; //(never expire by default)

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

## <a name="set-time-to-live-on-a-container-using-sdk"></a>Настройка срока жизни для контейнера с помощью пакета SDK

### <a id="dotnet-enable-withexpiry"></a>Пакет SDK для .NET

Чтобы задать срок жизни в контейнере, необходимо ввести ненулевое положительное число, указывающее период времени в секундах. Исходя из настроенного значения срока жизни, все элементы в контейнере после последнего изменения метки времени элемента `_ts` удаляются.

```csharp
// Create a new collection with TTL enabled and a 90 day expiration
DocumentCollection collectionDefinition = new DocumentCollection();
collectionDefinition.Id = "myContainer";
collectionDefinition.PartitionKey.Paths.Add("/myPartitionKey");
collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days

DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    collectionDefinition,
    new RequestOptions { OfferThroughput = 20000 });
```

## <a name="set-time-to-live-on-an-item"></a>Настройка значения срока жизни для элемента

Срок жизни можно задать не только для контейнера, но и для элемента. Настройка срока жизни на уровне элемента переопределит значение срока жизни по умолчанию для элемента в этом контейнере.

* Чтобы задать срок жизни для элемента, необходимо ввести ненулевое положительное число, которое будет означать время в секундах, по истечении которого с момента последнего изменения элемента (метка времени `_ts`) этот элемент будет считаться устаревшим.

* Если у элемента нет поля срока жизни, то по умолчанию к элементу будет применяться срок жизни, заданный для контейнера.

* Если свойство TTL отключено на уровне контейнера, поле срока жизни в элементе будет игнорироваться, пока свойство не будет включено для этого контейнера.

### <a id="dotnet-set-ttl-item"></a>Пакет SDK для .NET

```csharp
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    [JsonProperty(PropertyName="cid")]
    public string CustomerId { get; set; }
    // used to set expiration policy
    [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
    public int? TimeToLive { get; set; }

    //...
}
// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder
{
    Id = "SO05",
    CustomerId = "CO18009186470",
    TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days
};
```

## <a name="reset-time-to-live"></a>Сброс срока жизни

Вы можете сбросить срок жизни в элементе, выполнив операции записи или обновления в элементе. Операция записи или обновления установит для `_ts` текущий момент времени, и срок жизни для элемента начнет истекать заново. Если вы хотите изменить срок жизни элемента, это поле можно обновить так же, как любое другое.

### <a id="dotnet-extend-ttl-item"></a>Пакет SDK для .NET

```csharp
// This examples leverages the Sales Order class above.
// Read a document, update its TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.TimeToLive = 60 * 30 * 30; // update time to live
response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="turn-off-time-to-live"></a>Выключение срока жизни

Если для элемента был установлен срок жизни, но вы больше не хотите, чтобы он истек, то вы можете получить элемент, удалить поле срока жизни и заменить элемент на сервере. Когда поле срока жизни удаляется из элемента, к элементу применяется значение срока жизни по умолчанию, назначенное для контейнера. Установите значение срока жизни, равное -1, чтобы предотвратить истечение срока действия элемента и не наследовать значение срока жизни из контейнера.

### <a id="dotnet-turn-off-ttl-item"></a>Пакет SDK для .NET

```csharp
// This examples leverages the Sales Order class above.
// Read a document, turn off its override TTL, save it.
response = await client.ReadDocumentAsync(
    "/dbs/salesdb/colls/orders/docs/SO05"),
    new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });

Document readDocument = response.Resource;
readDocument.TimeToLive = null; // inherit the default TTL of the collection

response = await client.ReplaceDocumentAsync(readDocument);
```

## <a name="disable-time-to-live"></a>Отключение срока жизни

Чтобы отключить срок жизни в контейнере и остановить фоновый процесс проверки на наличие просроченных элементов, нужно удалить свойство `DefaultTimeToLive` контейнера. Удаление этого свойства и выбор значения -1 имеют разный эффект. Если задано значение -1, срок действия новых элементов, добавляемых в контейнер, не будет истекать, однако это значение можно переопределить для определенных элементов в контейнере. При удалении свойства срока жизни из контейнера элементы будут устаревать, даже если их значение явно переопределило предыдущее значение срока жизни по умолчанию.

### <a id="dotnet-disable-ttl"></a>Пакет SDK для .NET

```csharp
// Get the collection, update DefaultTimeToLive to null
DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
// Disable TTL
collection.DefaultTimeToLive = null;
await client.ReplaceDocumentCollectionAsync(collection);
```

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о сроке жизни см. в следующей статье:

* [Срок жизни для данных Azure Cosmos DB](time-to-live.md)
