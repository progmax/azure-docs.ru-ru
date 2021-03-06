---
title: Сведения об HBase в Azure HDInsight
description: Введение в Apache HBase в HDInsight — базу данных NoSQL на основе Hadoop. Изучите варианты использования и сравните HBase с другими кластерами Hadoop.
keywords: bigtable nosql, что такое hbase, apache hbase, hbase, обзор habase
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: eaf69ffdd7aa0964860f90b1f98d542175ea086b
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315415"
---
# <a name="what-is-apache-hbase-in-hdinsight-a-nosql-database-that-provides-bigtable-like-capabilities-for-apache-hadoop"></a>Что такое Apache HBase в HDInsight: база данных NoSQL, которая предоставляет возможности, схожие с BigTable, для Apache Hadoop
[Apache HBase](http://hbase.apache.org/) — это база данных NoSQL с открытым кодом, созданная на основе [Apache Hadoop](https://hadoop.apache.org/) по типу [Google BigTable](https://cloud.google.com/bigtable/). HBase обеспечивает прямой доступ и строгую согласованность для больших объемов неструктурированных и слабоструктурированных данных, упорядоченных в семейства столбцов.

С точки зрения пользователя HBase напоминает базу данных. Данные хранятся в строках и столбцах таблицы, данные в строке группируются по семейству столбцов. База данных HBase не имеет схемы в том смысле, что ни столбцы, ни типы хранимых в них данных не нужно определять до использования. Открытый код линейно масштабируется, чтобы обрабатывать петабайты данных на тысячах узлов. Он может полагаться на избыточность данных, пакетную обработку и другие особенности, которые предусмотрены распределенными приложениями в экосистеме Hadoop.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>Как база данных Apache HBase реализована в Azure HDInsight?

HDInsight HBase предлагается в форме управляемого кластера, интегрированного в среду Azure. Кластеры настроены на хранение данных непосредственно в [службе хранилища Azure](./../hdinsight-hadoop-use-blob-storage.md), что обеспечивает небольшую задержку и повышенную гибкость в выборе соотношения производительности и стоимости. Это позволяет клиентам создавать интерактивные веб-сайты, работающие с большими наборами данных, создавать службы, которые хранят данные с датчиков и телеметрию миллионов конечных точек, и анализировать эти данные с помощью заданий Hadoop. HBase и Hadoop являются хорошими отправными точками для создания проекта Azure, работающего с большими данными. В частности, это позволит приложениям реального времени работать с большими наборами данных.

Реализация HDInsight использует масштабируемую архитектуру HBase для обеспечения автоматического сегментирования таблиц, строгой согласованности для чтения и записи, а также автоматического перехода на другой ресурс. Производительность повышается за счет кэширования операций чтения в памяти и потоковой записи с высокой пропускной способностью Также для HDInsight HBase доступна подготовка виртуальных сетей. Кластер HBase можно создать внутри виртуальной сети. Дополнительные сведения см. в статье [Create HBase clusters on HDInsight in Azure Virtual Network](./apache-hbase-provision-vnet.md) (Создание кластеров HBase в виртуальной сети Azure).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>Как происходит управление данными в HDInsight HBase?
Управление данными в HBase может осуществляться с помощью команд `create`, `get`, `put` и `scan` из оболочки HBase. Данные записываются в базу данных с использованием команды `put` и считываются с помощью команды `get`. Команда `scan` используется для получения данных из нескольких строк таблицы. Данными также можно управлять с использованием интерфейса HBase API для C#, для которого имеется клиентская библиотека, работающая поверх HBase REST API. Запросы к базе данных HBase также могут осуществляться с помощью [Apache Hive](https://hive.apache.org/). Начальные сведения об этих моделях программирования приведены в статье [Get started with an Apache HBase example in HDInsight](./apache-hbase-tutorial-get-started-linux.md) (Приступая к работе с Hadoop в HDInsight). Также доступны сопроцессоры, что позволяет обрабатывать данные в узлах, на которых размещена база данных.

> [!NOTE]
> Thrift не поддерживается HBase в HDInsight.
>

## <a name="scenarios-use-cases-for-apache-hbase"></a>Сценарии: варианты использования Apache HBase
Классическим вариантом использования (для которого, собственно и была создана технология BigTable и HBase как ее расширение) является веб-поиск. Поисковые системы строят индексы, которые сопоставлены терминам, содержащимся на веб-страницах. Но есть много других вариантов использования, для которых подходит HBase; некоторые из них перечислены в этом разделе.

* Хранилище ключ-значение
  
    HBase можно использовать как хранилище пар «ключ-значение», оно подходит для управления системами обмена сообщениями. Facebook использует HBase для системы обмена сообщениями, она идеально подходит для хранения и управления интернет-коммуникациями. WebTable использует HBase для поиска и управления таблицами, извлеченными из веб-страниц.
* Данные от датчиков
  
    HBase удобно использовать для записи данных, накопленных при сборе из различных источников. К ним относятся социальные аналитики, временные ряды, обновления интерактивных информационных панели и счетчиков, а также управление системами ведения журналов аудита. Примеры включают в себя терминал для биржевой торговли Bloomberg и база данных Open Time Series Database (OpenTSDB), которая сохраняет и предоставляет доступ к показателям работоспособности серверных систем.
* Запросы в режиме реального времени
  
    [Apache Phoenix](https://phoenix.apache.org/) — это система запросов SQL для Apache HBase. Доступ к системе осуществляется с помощью драйвера JDBC, она позволяет создавать запросы и управлять таблицами HBase с использованием SQL.
* HBase как платформа
  
    Поверх HBase могут выполняться приложения, используя ее как хранилище данных. Например, это используется в Phoenix, [OpenTSDB](http://opentsdb.net/), Kiji и Titan. Также возможна интеграция приложений с HBase. К ним относятся [Apache Hive](https://hive.apache.org/), [Apache Pig](https://pig.apache.org/), [Solr](http://lucene.apache.org/solr/), [Apache Storm](http://storm.apache.org/), [Apache Flume](https://flume.apache.org/), [Apache Impala](https://impala.apache.org/), [Apache Spark](https://spark.apache.org/), [Ganglia](http://ganglia.info/) и [Apache Drill](https://drill.apache.org/).

## <a name="next-steps"></a>Дальнейшие действия
* [Начало работы с примером Apache HBase в HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Создание кластеров HBase в виртуальной сети Azure](./apache-hbase-provision-vnet.md).
* [Настройка репликации кластера HBase в виртуальных сетях Azure](apache-hbase-replication.md)
* [Создание приложений Java для Apache HBase](./apache-hbase-build-java-maven-linux.md)

## <a name="see-also"></a>Дополнительные материалы
* [Apache HBase](https://hbase.apache.org/)
* [Справочное руководство по Apache HBase](https://hbase.apache.org/book.html)
* [Bigtable: распределенная система хранения структурированных данных](http://research.google.com/archive/bigtable.html)
* [Apache HBase/Phoenix – Tips, Tricks & Best Practices in Azure HDInsight](https://blogs.msdn.microsoft.com/ashish/2016/08/28/hdinsight-hbase-faq/) (Полезные советы и рекомендации по использованию Apache HBase и Phoenix в Azure HDInsight)




