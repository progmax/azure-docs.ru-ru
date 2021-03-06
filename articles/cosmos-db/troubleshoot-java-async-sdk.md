---
title: Диагностика и устранение неполадок с пакетом SDK Java Async для Azure Cosmos DB | Документация Майкрософт
description: Воспользуйтесь такими функциями, как ведение журнала на стороне клиента, и другими сторонними инструментами для выявления, диагностики и устранения проблем, связанных с Azure Cosmos DB.
services: cosmos-db
author: moderakh
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 10/28/2018
ms.author: moderakh
ms.devlang: java
ms.component: cosmosdb-sql
ms.openlocfilehash: 2c73cda19a3f8b9b7c5ab493c0dfcd6c2e7be745
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2018
ms.locfileid: "51820245"
---
# <a name="troubleshoot-issues-when-you-use-the-java-async-sdk-with-azure-cosmos-db-sql-api-accounts"></a>Устранение неполадок при использовании пакета SDK Async Java с учетными записями API SQL для Azure Cosmos DB
В этой статье рассматриваются распространенные проблемы при использовании [пакета SDK Java Async](sql-api-sdk-async-java.md) с учетными записями API SQL для Azure Cosmos DB, а также связанные с этим обходные пути, средства и действия для диагностики.
Пакет SDK Java Async обеспечивает клиентское логическое представление для доступа к API SQL для Azure Cosmos DB. В этой статье описываются средства и подходы, которые помогут вам, если вы столкнетесь с проблемами.

Начните с этого списка:

* Ознакомьте с разделом [Распространенные проблемы и их обходные решения] в этой статье.
* Взгляните на пакет SDK [с открытым кодом на GitHub](https://github.com/Azure/azure-cosmosdb-java). Он имеет [раздел проблем](https://github.com/Azure/azure-cosmosdb-java/issues), который активно отслеживается. Вы можете там найти уже готовое обходное решение похожей проблемы.
* Просмотрите [советы по повышению производительности](performance-tips-async-java.md) и следуйте рекомендациям.
* Следуйте указаниям в оставшейся части этой статьи, если решение не нашлось. Затем [сообщите о проблеме в GitHub](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-workarounds"></a>Распространенные проблемы и их обходные решения

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Проблемы с сетью, ошибка из-за истечения времени ожидания чтения Netty, низкая пропускная способность, высокая задержка

#### <a name="general-suggestions"></a>Общие рекомендации
* Убедитесь, что приложение выполняется в том же регионе, который указан для вашей учетной записи Azure Cosmos DB. 
* Проверьте использование ЦП на узле, где выполняется приложение. Если используются 90 или более процентов ЦП, запустите приложение на узле с лучшей конфигурацией. Можно также распределить нагрузку на большее число компьютеров.

#### <a name="connection-throttling"></a>Регулирование подключения
Регулирование подключения может произойти из-за [Ограничение числа подключений на узле] или [Нехватка портов SNAT (PAT) Azure].

##### <a name="connection-limit-on-host"></a>Ограничение числа подключений на узле
Некоторые системы Linux (например, Red Hat) имеют ограничение на общее число открытых файлов. Сокеты в Linux реализованы как файлы, поэтому это число также существенно ограничивает общее число подключений.
Выполните следующую команду:

```bash
ulimit -a
```
Максимальное разрешенное количество открытых файлов, которые определены как "nofile", должно быть по крайней мере вдвое больше, чем размер пула подключений. Дополнительные сведения см. в статье [с советами по улучшению производительности](performance-tips-async-java.md).

##### <a name="snat"></a>Нехватка портов SNAT (PAT) Azure

Если ваше приложение развернуто в службе "Виртуальные машины Azure" без общедоступного IP-адреса, по умолчанию [порты Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) используются для установления подключений к любой конечной точке вне вашей виртуальной машины. Количество разрешенных подключений от виртуальной машины к конечной точке Azure Cosmos DB ограничивается [конфигурацией Azure SNAT](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports).

 Порты Azure SNAT используются, только если у виртуальной машины есть частный IP-адрес и процесс на виртуальной машине пытается установить подключение к общедоступному IP-адресу. Избежать ограничений Azure SNAT можно двумя способами:

* Добавьте конечную точку службы Azure Cosmos DB к подсети виртуальной сети для службы "Виртуальные машины Azure". Дополнительные сведения см. в статье [Конечные точки служб виртуальной сети Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    При включении конечной точки службы запросы больше не отправляются с общедоступного IP-адреса в Azure Cosmos DB. Вместо этого отправляются идентификаторы виртуальной сети и подсети. Это изменение может привести к сбою брандмауэра, если разрешены только общедоступные IP-адреса. Если вы используете брандмауэр, при включении конечной точки службы добавьте подсеть в брандмауэре с помощью [списков управления доступом для виртуальных сетей](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl).
* Назначьте общедоступный IP-адрес виртуальной машине Azure.

#### <a name="http-proxy"></a>Прокси-сервер HTTP

При использовании прокси-сервера HTTP убедитесь, что он может поддерживать число подключений, указанное в свойстве `ConnectionPolicy` пакета SDK.
В противном случае возникнут проблемы с подключением.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Недопустимый шаблон кодировки: блокировка потока ввода-вывода Netty

Пакет SDK использует библиотеку [Netty](https://netty.io/) для ввода-вывода, чтобы связываться с Azure Cosmos DB. Пакет SDK имеет асинхронные API и использует неблокирующие API Netty для ввода-вывода. Операции ввода-вывода, присущие пакету SDK, выполняются в потоках Netty для ввода-вывода. Число потоков Netty для ввода-вывода настроено так, чтобы совпадать с числом ядер ЦП компьютера, где выполняется приложение. 

Потоки Netty для ввода-вывода предназначены только для операций Netty для ввода-вывода без блокировки. Пакет SDK возвращает результат вызова API, касающийся одного из потоков Netty для ввода-вывода, в код приложения. Если приложение выполняет операцию длительное время после получения результатов касательно потока Netty, пакет SDK может не располагать достаточным количеством потоков ввода-вывода для выполнения внутренних операций ввода-вывода. Кодирование такого приложения может привести к низкой пропускной способности, длительной задержке и сбоям `io.netty.handler.timeout.ReadTimeoutException`. Обходное решение заключается в том, чтобы переключить поток, когда вы знаете, что операция займет некоторое время.

Рассмотрим, например, следующий фрагмент кода. Можно выполнить длительную работу, которая занимает более нескольких миллисекунд, в потоке Netty. В таком случае рано или поздно возникнет состояние, при котором не будет потока Netty для ввода-вывода, способного обрабатывать операции ввода-вывода. В результате произойдет сбой ReadTimeoutException.
```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
   Обходное решение заключается в изменении потока, в котором вы выполняете длительные операции. Определите отдельный экземпляр планировщика для приложения.
   ```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
   ```
   Может потребоваться выполнить длительную работу (например, такую, которая требует значительных вычислений или блокировки ввода-вывода). В этом случае необходимо переключение потока на рабочую роль, предоставляемую параметром `customScheduler` с помощью API `.observeOn(customScheduler)`.
```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Используя `observeOn(customScheduler)`, вы освобождаете поток Netty для ввода-вывода и переключаетесь на собственный поток, предоставляемый настраиваемым планировщиком. Это изменение позволяет решить проблему. Сбой `io.netty.handler.timeout.ReadTimeoutException` больше возникать не будет.

### <a name="connection-pool-exhausted-issue"></a>Проблема исчерпания пула подключений

`PoolExhaustedException` является ошибкой на стороне клиента. Такой сбой означает, что рабочая нагрузка приложения больше той, которую может обрабатывать пул подключений пакета SDK. Увеличьте размер пула подключений или распределите нагрузку между несколькими приложениями.

### <a name="request-rate-too-large"></a>Высокая частота запросов
Этот сбой происходит на сервере. Он означает, что вы использовали подготовленную пропускную способность. Повторите попытку позже. Если эта ошибка возникает часто, попробуйте увеличить пропускную способность коллекции.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Ошибка подключения к эмулятору Azure Cosmos DB

Сертификат HTTPS эмулятора для Azure Cosmos DB является самозаверяющим. Чтобы SDK работал с эмулятором, необходимо импортировать сертификат эмулятора в Java TrustStore. Дополнительные сведения см. в статье [Экспорт сертификатов эмулятора для Azure Cosmos DB](local-emulator-export-ssl-certificates.md).


## <a name="enable-client-sice-logging"></a>Включение ведения журнала для пакета SDK клиента

Пакет SDK Async Java использует SLF4j как интерфейс ведения журнала, который поддерживает запись на популярные платформы ведения журналов, такие как log4j и logback.

Например, если вы хотите использовать log4j как платформу ведения журналов, добавьте в путь к классу Java следующие библиотеки:

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Добавьте также конфигурацию log4j.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Дополнительные сведения см. в [руководстве по ведению журналов с использованием sfl4j](https://www.slf4j.org/manual.html).

## <a name="netstats"></a>Сетевая статистика ОС
Запустите команду netstat, чтобы понять, сколько подключений находится в состоянии `ESTABLISHED`, `CLOSE_WAIT` и т. д.

В Linux вы можете запустить следующую команду:
```bash
netstat -nap
```
Отфильтруйте результат для отображения только подключений к конечной точке Azure Cosmos DB.

Количество подключений к конечной точке Azure Cosmos DB в состоянии `ESTABLISHED` не должно превышать настроенного размера пула подключений.

Много подключений к конечной точке Azure Cosmos DB могут быть в состоянии `CLOSE_WAIT`. Возможно, более 1000. Такое большое количество указывает, что подключения быстро устанавливаются и быстро разрываются. Подобная ситуация может привести к проблемам. Дополнительные сведения см. в разделе [Распространенные проблемы и их обходные решения].

 <!--Anchors-->
[Распространенные проблемы и их обходные решения]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Ограничение числа подключений на узле]: #connection-limit-on-host
[Нехватка портов SNAT (PAT) Azure]: #snat


