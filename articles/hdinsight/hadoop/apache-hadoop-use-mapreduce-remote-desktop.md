---
title: Использование MapReduce и удаленного рабочего стола с Apache Hadoop в HDInsight в Azure
description: Информация об использовании удаленного рабочего стола для подключения к Apache Hadoop в HDInsight и выполнения заданий MapReduce.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: c0040958cbf748d3eafb3ee60806b064e4e0b1ba
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435792"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight-with-remote-desktop"></a>Использование MapReduce в Apache Hadoop в HDInsight с помощью удаленного рабочего стола
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Из этой статьи вы узнаете, как подключиться к Apache Hadoop в кластере HDInsight с помощью удаленного рабочего стола, а затем выполнить задания MapReduce с помощью команды Hadoop.

> [!IMPORTANT]  
> Удаленный рабочий стол доступен только в кластерах HDInsight на платформе Windows. Linux — это единственная операционная система, используемая для работы с HDInsight 3.4 или более поздних версий. Дополнительные сведения см. в разделе [Приближается дата прекращения сопровождения HDI версии 3.3](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> При использовании HDInsight 3.4 или более поздней версии см. сведения о подключении к кластеру HDInsight и выполнении заданий MapReduce: [Использование MapReduce с Hadoop в HDInsight с помощью SSH](apache-hadoop-use-mapreduce-ssh.md).

## <a id="prereq"></a>Предварительные требования
Чтобы выполнить действия, описанные в этой статье, необходимо следующее:

* Кластер HDInsight на платформе Windows (Hadoop в HDInsight).
* Клиентский компьютер под управлением Windows 10, Windows 8 или Windows 7.

## <a id="connect"></a>Подключение к удаленному рабочему столу
Запустите протокол удаленного рабочего стола для кластера HDInsight, а затем выполните подключение, следуя инструкциям раздела [Подключение к кластерам HDInsight с использованием RDP](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Использование команды Hadoop
После подключения к рабочему столу кластера HDInsight сделайте следующее, чтобы выполнить задание MapReduce с помощью команды Hadoop:

1. На рабочем столе HDInsight запустите **командную строку Hadoop**. Откроется новое окно командной строки в каталоге **c:\apps\dist\hadoop-&lt;номер_версии>**.

   > [!NOTE]  
   > При обновлении Hadoop номер версии изменяется. Для поиска пути можно использовать переменную среды **HADOOP_HOME**. Например, `cd %HADOOP_HOME%` изменит каталоги на каталог Hadoop без необходимости знать номер версии.
   >
   >
2. Чтобы выполнить пример задания MapReduce с помощью команды **Hadoop** , используйте следующую команду:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    При этом запустится класс **wordcount**, содержащийся в текущем каталоге в файле **hadoop-mapreduce-examples.jar**. В качестве входных данных он использует документ **wasb://example/data/gutenberg/davinci.txt**, а выходные данные сохраняются в **wasb:///example/data/WordCountOutput**.

   > [!NOTE]
   > Дополнительные сведения об этом задании MapReduce и данные для примера см. в разделе <a href="hdinsight-use-mapreduce.md">Использование MapReduce в HDInsight в Hadoop</a>.
   >
   >
3. Задание выдает информацию о ходе обработки, а по завершении задания возвращает информацию, аналогичную приведенной ниже:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. После завершения используйте следующую команду, чтобы вывести список выходных файлов, сохраненных в **wasb://example/data/WordCountOutput**.

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Должно отобразиться два файла: **_SUCCESS** и **part-r-00000**. Файл **part-r-00000** содержит выходные данные этого задания.

   > [!NOTE]
   > Некоторые задания MapReduce могут разделять результаты на несколько файлов **part-r-№№№№№** . В этом случае используйте суффикс №№№№№, чтобы определить порядок файлов.
   >
   >
5. Чтобы просмотреть выходные данные, используйте следующую команду:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    При этом отобразится список слов, содержащихся в файле **wasb://example/data/gutenberg/davinci.txt**, а также количество вхождений каждого из них. Ниже приведен пример данных, которые будут содержаться в файле.

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Сводка
Как видите, команда Hadoop позволяет с легкостью выполнять задания MapReduce в кластере HDInsight и просматривать выходные данные задания.

## <a id="nextsteps"></a>Дальнейшие действия
Общая информация о заданиях MapReduce в HDInsight:

* [Использование MapReduce в Hadoop в HDInsight](hdinsight-use-mapreduce.md)

Дополнительная информация о других способах работы с Hadoop в HDInsight:

* [Использование Apache Hive с Apache Hadoop в HDInsight](hdinsight-use-hive.md)
* [Использование Apache Pig с Apache Hadoop в HDInsight](hdinsight-use-pig.md)
