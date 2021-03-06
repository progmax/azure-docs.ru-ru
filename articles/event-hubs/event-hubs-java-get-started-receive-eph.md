---
title: Получение событий от Центров событий Azure с помощью Java | Документация Майкрософт
description: Узнайте основные сведения о получении событий от Центров событий с помощью Java.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 08/26/2018
ms.author: shvija
ms.openlocfilehash: dce7c4067ba6d96bf14f4e3300d951b594afe930
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240638"
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Получение событий от Центров событий Azure с помощью Java

Центры событий — это высокомасштабируемая система, способная принимать миллионы событий в секунду, благодаря которой приложения могут обрабатывать и анализировать большие объемы данных от подключенных устройств и приложений. После сбора данных в Центрах событий их можно преобразовать и сохранить с помощью любого поставщика аналитики в реальном времени или в кластере хранилища.

Дополнительные сведения см. в [обзоре Центров событий][Event Hubs overview].

В этом руководстве показано, как получать события из концентратора событий с помощью консольного приложения, написанного на языке Java.

## <a name="prerequisites"></a>Предварительные требования

Для работы с данным руководством вам потребуется:

* Среда разработки Java. Для этого руководства предполагается использование среды [Eclipse](https://www.eclipse.org/).
* Активная учетная запись Azure. Если у вас еще нет подписки Azure, создайте [бесплатная учетная запись][], прежде чем начинать работу.

Код в этом руководстве основан на [коде EventProcessorSample в GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample). Изучите его, чтобы получить полное представление о рабочем приложении.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Прием сообщений через EventProcessorHost в Java

**EventProcessorHost** — это класс Java, который упрощает прием событий от концентраторов событий путем управления постоянными контрольными точками и параллельно принимает сообщения от этих концентраторов событий. С помощью класса EventProcessorHost можно разделить события между несколькими получателями, даже если они размещены на разных узлах. В этом примере показано, как использовать EventProcessorHost для одного получателя.

### <a name="create-a-storage-account"></a>Создание учетной записи хранения

Для использования класса EventProcessorHost необходимо настроить [учетную запись хранения Azure][Azure Storage account]:

1. Войдите на [портал Azure][Azure portal] и щелкните **+ Create a resource** (+ Создать ресурс) в левой части экрана.
2. Щелкните **Хранилище**, а затем — **Учетная запись хранения**. В окне **Создание учетной записи хранения** введите имя учетной записи хранения. Заполните остальные поля, выберите нужный регион и нажмите кнопку **Создать**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Выберите созданную учетную запись хранения и нажмите кнопку **Ключи доступа**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Скопируйте значение key1 во временное расположение. Оно будет использоваться далее в этом руководстве.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Создание проекта Java с помощью EventProcessorHost

Клиентская библиотека Java для службы "Центры событий" доступна для использования в проектах из [центрального репозитория Maven][Maven Package]. Ссылаться на нее можно, используя приведенное ниже объявление зависимости в файле проекта Maven. Текущая версия артефакта azure-eventhubs-eph — 2.0.1, а артефакта azure-eventhubs — 1.0.2:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.0.1</version>
</dependency>
```

Для различных типов сред сборки можно явно получить последние выпущенные JAR-файлы из [центрального репозитория Maven][Maven Package].  

1. Следующий пример сначала создает новый проект Maven для приложения консоли или оболочки в избранной среде разработки Java. Класс называется `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Создайте новый класс с именем `EventProcessorSample`с помощью следующего кода: Замените значения заполнителей значениями, которые использовались при создании концентратора событий и учетной записи хранения:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Создайте еще один класс с именем `EventProcessor` при помощи следующего кода:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

В данном учебнике используется один экземпляр EventProcessorHost. Чтобы увеличить пропускную способность, можно запустить несколько экземпляров EventProcessorHost, желательно на разных компьютерах.  Кроме того, такое решение обеспечит избыточность. В этом случае различные экземпляры автоматически координируются друг с другом для распределения нагрузки полученных событий. Если каждый из нескольких получателей должен обрабатывать *все* события, то необходимо использовать понятие **ConsumerGroup** . При получении события от разных компьютеров может оказаться полезным указать имена экземпляров EventProcessorHost в компьютерах (или ролях), где они развернуты.

## <a name="publishing-messages-to-eventhub"></a>Публикация сообщений в EventHub

Прежде чем сообщения будут получены потребителями, они должны быть опубликованы издателями в разделах. Следует отметить, что при синхронной публикации сообщения в концентраторе событий с помощью метода sendSync() в объекте com.microsoft.azure.eventhubs.EventHubClient сообщение может быть отправлено в определенный раздел или распределиться по всем доступным разделам циклически, в зависимости от того, указан ли ключ раздела.

Если указана строка, представляющая ключ раздела, раздел для отправки события определяется по хэшу ключа.

Если ключ раздела не задан, сообщения будут отправляются поочередно во все доступные разделы.

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

## <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>Реализация пользовательского CheckpointManager для EventProcessorHost (EPH)

API предоставляет механизм реализации пользовательского диспетчера контрольных точек для сценариев, где реализация по умолчанию несовместима с вашим вариантом использования.

Диспетчер контрольных точек по умолчанию использует хранилище BLOB-объектов, но вы можете переопределить диспетчер контрольных точек собственной реализацией и использовать любое хранилище для резервного копирования в этой реализации диспетчера контрольных точек.

Создайте класс, который реализует интерфейс com.microsoft.azure.eventprocessorhost.ICheckpointManager.

Используйте свою реализацию пользовательского диспетчера контрольных точек (com.microsoft.azure.eventprocessorhost.ICheckpointManager).

В реализации можно переопределить механизм создания контрольных точек по умолчанию и реализовать свои собственные контрольные точки на основе хранилища данных (SQL Server, CosmosDB, Кэш Redis и т. д.). Мы рекомендуем, чтобы хранилище, используемое для резервного копирования в вашей реализации диспетчера контрольных точек, было доступно всем экземплярам EPH, которые обрабатывают события для группы потребителей.

Вы можете использовать любое хранилище данных, которое доступно в вашей среде.

Класс com.microsoft.azure.eventprocessorhost.EventProcessorHost предоставляет два конструктора, которые позволяют переопределить диспетчер контрольных точек для EventProcessorHost.

## <a name="next-steps"></a>Дополнительная информация
В этом кратком руководстве вы создали приложение Java, которое получает сообщения из концентратора событий. Чтобы узнать, как отправлять события в концентратор событий с помощью Java, изучите статью [Отправка событий в Центры событий Azure с помощью Java](event-hubs-java-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
[бесплатная учетная запись]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
