---
title: 'Руководство. Загрузка данных и выполнение запросов в кластере Apache Spark в Azure HDInsight '
description: Узнайте, как загружать данные и выполнять интерактивные запросы в кластерах Spark в Azure HDInsight.
services: azure-hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.author: hrasheed
ms.date: 11/06/2018
ms.openlocfilehash: f279d7ca40eac1764ec5549aecec36b0f62034e8
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495783"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Руководство. Загрузка данных и выполнение запросов в кластере Apache Spark в Azure HDInsight

В этом руководстве описывается, как создать кадр данных из CSV-файла и как отправлять интерактивные запросы SQL Spark к кластеру [Apache Spark](https://spark.apache.org/) в Azure HDInsight. В Spark кадр данных — это распределенная коллекция данных, упорядоченных в именованных столбцах. Она эквивалентна таблице в реляционной базе данных или фрейме данных в R/Python.
 
Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Создание кадра данных из CSV-файла
> * Выполнение запросов к кадру данных

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

* Изучите руководство по [созданию кластера Apache Spark в Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-dataframe-from-a-csv-file"></a>Создание кадра данных из CSV-файла

Приложения могут создавать кадры данных прямо из файлов и папок в удаленном хранилище (например, Служба хранилища Azure или Azure Data Lake Storage), из таблиц Hive, а также из других источников данных, поддерживаемых Spark (например, Cosmos DB, База данных SQL Azure, Хранилище данных и т. д.). На снимке экрана показан моментальный снимок файла hvac.csv, используемого в этом руководстве. CSV-файл содержит все кластеры HDInsight Spark. Эти данные демонстрируют колебания температуры в некоторых зданиях.
    
![Моментальный снимок данных для интерактивных запросов Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")


1. Откройте записную книжку Jupyter, созданную на этапе выполнения предварительных требований.
2. Вставьте следующий код в пустую ячейку приложения и нажмите **SHIFT+ВВОД** для выполнения кода. Код импортирует типы, необходимые для этого сценария:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    При запуске интерактивного запроса в Jupyter в заголовке окна веб-браузера или вкладки будет отображаться состояние **(Busy)** (Занято), а также название приложения. Кроме того, рядом с надписью **PySpark** в верхнем правом углу окна будет показан закрашенный кружок. После завершения задания он изменится на кружок без заливки.

    ![Состояние интерактивного запроса Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Состояние интерактивного запроса Spark SQL")

3. Выполните следующий код, чтобы создать кадр данных и временную таблицу **hvac**. 

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

    > [!NOTE]
    > Если записная книжка создается с использованием ядра PySpark, сеанс `spark` автоматически создается при выполнении первой ячейки кода. Вам не нужно явно создавать этот сеанс.


## <a name="run-queries-on-the-dataframe"></a>Выполнение запросов к кадру данных

Когда таблица будет готова, выполните интерактивный запрос к данным.

1. В пустой ячейке приложения выполните следующий код:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Отобразятся следующие табличные данные.

     ![Таблица результатов интерактивного запроса Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Таблица результатов интерактивного запроса Spark")

3. Результаты также можно просмотреть и в других визуализациях. Чтобы увидеть результат в виде диаграммы с областями, выберите **Область** и укажите другие значения, как показано ниже.

    ![Диаграмма с областями результата интерактивного запроса Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Диаграмма с областями результата интерактивного запроса Spark")

10. Для этого в меню **Файл** записной книжки выберите пункт **Save and Checkpoint** (Сохранить и установить контрольную точку). 

11. Если вы собираетесь ознакомиться со [следующим руководством](apache-spark-use-bi-tools.md) сейчас, не закрывайте записную книжку. В противном случае завершите работу записной книжки, чтобы освободить ресурсы кластера: в меню **Файл** записной книжки выберите **Close and Halt** (Закрыть и остановить).

## <a name="clean-up-resources"></a>Очистка ресурсов

HDInsight хранит ваши данные и записные книжки Jupyter Notebook в Службе хранилища Azure или Azure Data Lake Store, что позволяет безопасно удалить неиспользуемый кластер. Плата за кластеры HDInsight взимается, даже когда они не используются. Поскольку стоимость кластера во много раз превышает стоимость хранилища, экономически целесообразно удалять неиспользуемые кластеры. Если вы планируете сразу приступить к следующему руководству, можно оставить кластер.

Откройте кластер на портале Azure и выберите **Удалить**.

![Удаление кластера HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Удаление кластера HDInsight")

Кроме того, можно выбрать имя группы ресурсов, чтобы открыть страницу группы ресурсов, а затем нажать кнопку **Удалить группу ресурсов**. Вместе с группой ресурсов вы также удалите кластер Spark в HDInsight и учетную запись хранения по умолчанию.

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

* Создание кадра данных Apache Spark.
* Выполнение запроса Spark SQL к кадру данных.

Теперь переходите к следующей статье, в которой объясняется, как перенести зарегистрированные в Apache Spark данные в средство бизнес-аналитики, например в Power BI. 
> [!div class="nextstepaction"]
> [Анализ данных с помощью средств бизнес-аналитики](apache-spark-use-bi-tools.md)

