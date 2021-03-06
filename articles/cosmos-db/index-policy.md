---
title: Политики индексирования Azure Cosmos DB | Документация Майкрософт
description: Сведения об осуществлении индексации в Azure Cosmos DB. Узнайте, как настроить и изменить политику индексирования для автоматического индексирования и повышения производительности.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: ffb70ce8c26b7774e90801271c55cd8a80906c90
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628852"
---
# <a name="indexing-policy-in-azure-cosmos-db"></a>Политика индексирования в Azure Cosmos DB

Вы можете переопределить политику индексирования, указанную по умолчанию для контейнера Azure Cosmos, настроив следующие параметры:

* **Включить или исключить элементы и пути для индексирования.** Вы можете отключить или включить индексирование для конкретных элементов в индексе, когда вставляете или заменяете элементы в контейнере. Кроме того, можно включать или исключать конкретные пути и (или) свойства для индексирования в контейнерах. Пути можно указывать с подстановочными знаками, например, *.

* **Настройка типов индекса.** В дополнение к путям с индексированным диапазоном можно добавлять индексы других типов, например пространственные.

* **Настройка режимов индекса.** С помощью политики индексирования для контейнера вы можете настроить разные режимы индексирования, например *Consistent* (Согласованный) или *None* (Нет).

## <a name="indexing-modes"></a>Режимы индексирования 

Azure Cosmos DB поддерживает два режима индексирования, которые вы можете указать для контейнера Azure Cosmos. Политика индексирования позволяет настроить следующие два режима индексирования: 

* **Consistent** (Согласованный). Если для контейнера Azure Cosmos указана такая политика, при запросах к этому контейнеру соблюдается тот же уровень согласованности, который указан для точечных операций чтения (т. е. строгая согласованность, согласованность с ограниченным устареванием, согласованность сеанса или итоговая согласованность). 

  Индекс обновляется синхронно с обновлением самих элементов. Например, индекс обновляется при операциях insert, replace, update и delete для включенного в него элемента. Согласованное индексирование поддерживает согласованность запросов, воздействуя на производительность записи. Влияние на производительность записи зависит от количества путей, включенных в индексирование, и уровня согласованности. Режим согласованного индексирования предназначен для рабочих нагрузок, требующих быстрой записи и немедленных запросов.

* **None** (Нет). У контейнера с этим режимом индексирования нет связанного индекса. Обычно это применимо для тех случаев, когда база данных Azure Cosmos используется как хранилище пар "ключ-значение", а доступ к элементам осуществляется только по значению идентификатора.

  > [!NOTE]
  > Настройка режима индексирования None (Нет) имеет побочный эффект — удаляется любой существующий индекс. Используйте этот вариант, если во всех режимах доступа вам нужен только идентификатор или ссылка на себя.

Уровни согласованности запросов поддерживаются так же, как для обычных операций чтения. База данных Azure Cosmos возвращает ошибку, если вы запрашиваете контейнер с режимом индексирования None (Нет). Запросы можно выполнять как сканирования, явным образом указав заголовок **x-ms-documentdb-enable-scans** в REST API или параметр запроса **EnableScanInQuery** с помощью пакета SDK для .NET. Некоторые возможности запросов, например ORDER BY, сейчас не совместимы с **EnableScanInQuery**, так как требуют наличия соответствующего индекса.

## <a name="modifying-the-indexing-policy"></a>Изменение политики индексирования

В Azure Cosmos DB вы можете в любое время обновить политику индексирования для контейнера. Изменение в политике индексирования в контейнере Azure Cosmos может привести к изменению формы индекса. Такое изменение затрагивает индексируемые пути, их точность, а также модель согласованности самого индекса. Изменение политики индексирования фактически требует преобразования старого индекса в новый.

### <a name="index-transformations"></a>Преобразования индексов

Все преобразования индексов выполняются в сети. Элементы, индексированные по старой политике, эффективно преобразуются в формат новой политики без снижения доступности для записи или подготовленной для контейнера пропускной способности. Согласованность операций чтения и записи, выполняемых с помощью REST API, пакетов SDK или из хранимых процедур и триггеров, не затрагивается во время преобразования индекса.

Изменение политики индексирования — это асинхронная операция, время выполнения которой зависит от количества элементов, подготовленной пропускной способности и размера элементов. Когда выполняется переиндексация, запрос может вернуть не все подходящие результаты, если для него используется изменяемый индекс. Но при этом запрос не вернет ошибку и не приведет к сбою. Во время переиндексации запросы являются согласованными в конечном счете вне зависимости от конфигурации режима индексирования. Когда преобразование индекса завершится, согласованные результаты будут отображаться, как и прежде. Это относится к запросам, выполняемым с помощью любого интерфейса — REST API, пакетов SDK или из хранимых процедур и триггеров. Преобразование индекса выполняется асинхронно в фоновом режиме в репликах на свободных ресурсах, доступных для конкретной реплики.

Все преобразования индексов выполняются на месте. Azure Cosmos DB не поддерживает вторую копию индекса. Это означает, что при преобразовании индекса не требуется дополнительное место на диске в контейнере.

При изменении политики индексирования то, как применяются изменения для перехода от старого индекса к новому, больше зависит от конфигураций режима индексирования, чем от других параметров, таких как включенные или исключенные пути, типы индекса и точность.

Если в старой и новой политиках используется режим индексирования **Consistent** (Согласованный), то база данных Azure Cosmos выполняет преобразование индекса в сети. Во время выполнения преобразования нельзя применить другое изменение политики индексирования с согласованным режимом индексирования. При переходе в режим индексирования None (Нет) индекс немедленно удаляется. Переход в режим индексирования None (Нет) удобен, если необходимо отменить выполняемое преобразование и начать "с нуля" с другой политикой индексирования.

Чтобы преобразование индекса завершилось успешно, в контейнере должно быть достаточно дискового пространства. Если контейнер использует всю квоту хранилища, преобразование индекса будет приостановлено. Преобразование индекса автоматически возобновится при появлении доступного места, например, если вы удалите некоторые элементы.

## <a name="modifying-the-indexing-policy---examples"></a>Изменение политики индексирования (примеры)

Ниже приведены самые распространенные варианты использования, при которых нужно изменять политику индексации.

* Если вам нужны согласованные результатов во время обычной работы, но при массовом импорте данных вы возвращаетесь к режиму индексирования **None** (Нет).

* Если вы хотите применить новые функции индексирования для уже существующих контейнеров Azure Cosmos DB, например геопространственные запросы, для которых необходим пространственный индекс, или запросы ORDER BY и запросы диапазона строки, для которых необходим индекс диапазона строки.

* Если вы хотите вручную выбрать свойства для индексирования и постепенно изменять их в соответствии с характеристиками рабочих нагрузок.

* Если вы хотите настроить точность индексирования для повышения производительности запросов или уменьшения используемого объема хранилища.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об индексировании см. по следующим ссылкам:

* [Общие сведения об индексировании](index-overview.md)
* [Типы индексов](index-types.md)
* [Пути индексов](index-paths.md)
* [Как управлять политикой индексирования](how-to-manage-indexing-policy.md)