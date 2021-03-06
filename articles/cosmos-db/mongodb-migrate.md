---
title: Использование mongoimport и mongorestore с API Azure Cosmos DB для MongoDB
description: Узнайте, как использовать mongoimport и mongorestore для импорта данных в учетную запись API для MongoDB.
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 50bb34d86780dec003c63b5ff0a3884049dd47c1
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52871019"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>Руководство по переносу данных в учетную запись API MongoDB в Azure Cosmos DB

Это руководство содержит инструкции по переносу данных из MongoDB в учетную запись API MongoDB в Azure Cosmos DB. Если вы импортируете данные из MongoDB и планируете использовать их с API SQL для Azure Cosmos DB, это можно сделать с помощью [средства переноса данных](import-data.md).

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * Планирование миграции
> * Предварительные требования для миграции
> * перенос данных с помощью mongoimport;
> * перенос данных с помощью mongorestore.

Прежде чем переносить данные в учетную запись API MongoDB, убедитесь, что у вас есть образец данных MongoDB. Если у вас нет образца базы данных MongoDB, скачайте и установите [сервер сообщества MongoDB](https://www.mongodb.com/download-center), создайте базу данных и с помощью mongoimport.exe или mongorestore.exe передайте в нее пример данных. 

## <a name="plan-for-migration"></a>Планирование миграции

1. Заранее создайте и масштабируйте свои коллекции:
        
   * По умолчанию Azure Cosmos DB подготавливает новую коллекцию MongoDB, обеспечивающую пропускную способность 1000 единиц запроса в секунду (ЕЗ/с). Прежде чем начать перенос с помощью mongoimport или mongorestore, создайте все свои коллекции с помощью [портала Azure](https://portal.azure.com) или драйверов и инструментов MongoDB. Если объем данных превышает 10 ГБ, создайте [секционированную коллекцию](partition-data.md) с соответствующим ключом сегмента. Для хранения данных сущностей в коллекциях рекомендуется использовать MongoDB. Можно совместно размещать сущности сопоставимых размеров и подготавливать пропускную способность на уровне базы данных Azure Cosmos.

   * На [портале Azure](https://portal.azure.com) увеличьте показатели пропускной способности своих коллекций (с 1000 ЕЗ/с для односекционной коллекции и 2500 ЕЗ/с для сегментированной коллекции) только на время выполнения переноса. Более высокая пропускная способность позволяет избежать ограничения скорости и выполнить перенос быстрее. Чтобы снизить затраты, вы можете снизить пропускную способность сразу после переноса.

   * Помимо показателей ЕЗ/с на уровне коллекции вы также можете настроить ЕЗ/с для набора коллекций на уровне родительской базы данных. Но перед этим вам потребуется создать базу данных и коллекции, а также определить ключ сегмента для каждой коллекции.

   * Вы можете создать сегментированную коллекцию с помощью привычного средства, драйвера или пакета SDK. В этом примере для создания сегментированной коллекции используется оболочка Mongo:

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        Результат:

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. Вычислите приблизительную стоимость ЕЗ при записи одного документа:

   a. Подключение к учетной записи API MongoDB в Azure Cosmos DB из оболочки MongoDB. Инструкции доступны в статье [Подключение приложения MongoDB к Azure Cosmos DB](connect-mongodb-account.md).
    
   b. Выполните пример команды insert, используя один из примеров документов из оболочки MongoDB:
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. Выполните команду ```db.runCommand({getLastRequestStatistics: 1})```. Вы получите примерно такой ответ:
     
      ```bash
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
      ```
        
    d. Запишите значение RequestCharge (стоимость запроса).
    
1. Определите задержку между вашим компьютером и облачной службой Azure Cosmos DB:
    
    a. Включите ведение подробного журнала из оболочки MongoDB с помощью команды ```setVerboseShell(true)```.
    
    b. Выполните простой запрос к базе данных: ```db.coll.find().limit(1)```. Вы получите примерно такой ответ:

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. Перед выполнением переноса удалите вставленный документ, чтобы точно не было повторяющихся документов. Для удаления документов можно использовать команду ```db.coll.remove({})```.

1. Вычислите приблизительные значения *batchSize* и *numInsertionWorkers*:

    * Для расчета *batchSize* поделите общее число подготовленных ЕЗ на число ЕЗ, использованных для записи одного документа (на шаге 3).
    
    * Если полученное значение *batchSize* <= 24, то используйте его в качестве значения *batchSize*.
    
    * Если полученное значение *batchSize* > 24, то задайте значение *batchSize* равное 24.
    
    * Для расчета *numInsertionWorkers* используйте следующую формулу: *numInsertionWorkers = (подготовленная пропускная способность * задержка в секундах) / (размер пакета * число ЕЗ, использованных для одной операции записи)*.
        
    |Свойство|Значение|
    |--------|-----|
    |batchSize| 24 |
    |Подготовленные ЕЗ | 10 000 |
    |Latency | 0,100 с |
    |ЕЗ, использованные для записи 1 документа | 10 ЕЗ |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 ЕЗ x 0,1 с) / (24 x 10 ЕЗ) = 4,1666*

1. Выполните команду переноса. Параметры переноса данных описаны в следующих разделах.

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   Или же можно использовать mongorestore (убедитесь, что для всех коллекций установлена пропускная способность, которая равна числу ЕЗ, используемых в предыдущих вычислениях, или превышает его).
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>Предварительные требования для миграции

* **Увеличьте пропускную способность**. Продолжительность переноса данных зависит от пропускной способности, настроенной для отдельной коллекции или набора коллекций. Увеличьте пропускную способность для крупных миграций. После переноса уменьшите пропускную способность для экономии расходов. Дополнительные сведения об увеличении пропускной способности на [портале Azure](https://portal.azure.com) см. в статье [Прекращение использования уровней производительности S1, S2 и S3 в DocumentDB](performance-levels.md).

* **Включите SSL**. В Azure Cosmos DB реализуются строгие требования и стандарты безопасности. Обязательно включите SSL при взаимодействии с учетной записью. Процедуры, описанные в оставшейся части статьи, включают инструкции по включению SSL для mongoimport и mongorestore.

* **Создайте ресурсы Azure Cosmos DB**. Перед началом переноса данных заранее создайте коллекции на портале Azure. Если вы выполняете перенос в учетную запись Azure Cosmos DB, обладающую пропускной способностью уровня базы данных, обязательно укажите ключ раздела при создании коллекций Azure Cosmos DB.

## <a name="get-your-connection-string"></a>Получение строки подключения 

1. В левой панели на [портале Azure](https://portal.azure.com) щелкните запись **Azure Cosmos DB**.
1. На панели **Подписки** выберите имя своей учетной записи.
1. В колонке **Строка подключения** щелкните **Строка подключения**.

   На правой панели содержатся все сведения, необходимые для успешного подключения к учетной записи.

   ![Колонка "Строка подключения"](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>Перенос данных с помощью mongoimport

Чтобы импортировать данные в учетную запись Azure Cosmos DB, используйте следующий шаблон. Укажите *узел*, *имя пользователя* и *пароль* своей учетной записи.  

Шаблон:

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Пример:  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>Перенос данных с помощью mongorestore

Чтобы восстановить данные в учетной записи API для MongoDB, используйте приведенный ниже шаблон для выполнения импорта. Укажите *узел*, *имя пользователя* и *пароль* своей учетной записи.

Шаблон:

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Пример:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>Дополнительная информация

Вы можете перейти к следующему руководству, из которого вы узнаете, как запрашивать данные MongoDB с помощью Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Как выполнять запросы к данным в базе данных Azure Cosmos DB с помощью API для MongoDB](../cosmos-db/tutorial-query-mongodb.md)
