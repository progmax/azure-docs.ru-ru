---
title: Выбор правильного уровня согласованности для приложения, использующего Azure Cosmos DB | Документация Майкрософт
description: Выбор правильного уровня согласованности для приложения в Azure Cosmos DB.
keywords: consistency, performance, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/24/2018
ms.author: mjbrown
ms.openlocfilehash: 42128a05ad9f82ff6b202eb6566c1fea60caa760
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162442"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Выбор правильного уровня согласованности для приложения

Репликация распределенных баз данных для обеспечения высокого уровня их доступности и небольшой задержки предполагает компромисс между согласованностью чтения и такими параметрами, как доступность, время задержки и пропускная способность. Большинство коммерчески доступных распределенных баз данных предлагают разработчикам выбор между двумя моделями согласованности: строгая и итоговая согласованность. Azure Cosmos DB позволяет разработчикам выбрать одну из пяти точно определенных моделей согласованности: строгую согласованность, согласованность с ограниченным устареванием, согласованность уровня сеанса, согласованность с постоянным префиксом, а также итоговую согласованность. Каждая из этих моделей согласованности четко определена, интуитивно понятна и может использоваться для конкретных реальных сценариев. Каждая из пяти моделей обеспечивает [компромисс между доступностью и производительностью](consistency-levels-tradeoffs.md) и регулируется комплексными соглашениями об уровне обслуживания. Приведенные ниже простые рекомендации помогут вам сделать правильный выбор для большинства распространенных сценариев.

## <a name="sql-api-and-table-api"></a>API SQL или API таблиц

Если приложение создано с помощью Cosmos DB SQL API или API таблиц, учитывайте следующие аспекты.

- Для многих сценариев из реальной жизни оптимальным и рекомендуемым вариантом является согласованность на уровне сеанса. Дополнительные сведения см. в разделе [Использование маркеров сеанса](how-to-manage-consistency.md#utilize-session-tokens).

- Если вашему приложению необходима строгая согласованность, рекомендуем использовать уровень согласованности "Ограниченное устаревание".

- Если вам нужны более строгие гарантии согласованности, нежели предоставляемые согласованностью на уровне сеанса, и низкие задержки (не более десяти миллисекунд) для операций записи, рекомендуем использовать уровень согласованности "Ограниченное устаревание".  

- Если приложению необходима итоговая согласованность, рекомендуем использовать уровень согласованности с постоянным префиксом.

- Если необходимы менее строгие гарантии согласованности, нежели предоставляемые согласованностью на уровне сеанса, рекомендуем использовать уровень согласованности с постоянным префиксом.

- Если вам требуется максимально высокий уровень доступности и минимальная задержка, используйте итоговую согласованность.

## <a name="cassandra-mongodb-and-gremlin-api"></a>API Cassandra, MongoDB или Gremlin

- Дополнительные сведения о сопоставлении между уровнем согласованности чтения Apache Cassandra и уровнями согласованности Cosmos DB см. [здесь](consistency-levels-across-apis.md#cassandra-mapping).

- Дополнительные сведения о сопоставлении между уровнем операций чтения MongoDB и уровнями согласованности Cosmos DB см. в [этом разделе](consistency-levels-across-apis.md#mongo-mapping).

## <a name="consistency-guarantees-in-practice"></a>Гарантии согласованности на практике

На практике вы можете получить более строгие гарантии согласованности. Гарантии согласованности для операции чтения соответствуют актуальности и упорядочению состояния запрашиваемой базы данных. Согласованность чтения связана с упорядочением и распространением операций записи или обновления.  

* Если установлен уровень согласованности **Ограниченное устаревание**, Cosmos DB гарантирует, что клиенты всегда считывают значения предыдущей записи с задержкой, ограниченной окном устаревания.

* Если установлен **строгий** уровень согласованности, окно устаревания равно нулю, и клиенты гарантировано считают последнее зафиксированное значение операции записи.

* На остальных трех уровнях согласованности окно устаревания в значительной степени зависит от рабочей нагрузки. Например, если в базе данных нет записей, операция чтения с **итоговой** согласованностью, согласованностью на уровне **сеанса** или согласованностью с **постоянным префиксом**, скорее всего, даст те же результаты, что и операция чтения со строгим уровнем согласованности.

Если для учетной записи Cosmos DB настроен уровень согласованности, отличный от строгой согласованности, просмотрев данные метрики вероятностного ограниченного устаревания (PBS), вы узнаете вероятность получения вашими клиентами строго согласованных операций чтения для рабочей нагрузки. Эта метрика предоставляется на портале Azure. Дополнительные сведения см. в разделе [Мониторинг метрики вероятностного ограниченного устаревания (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Вероятностное ограниченное устаревание показывает степень итоговой согласованности. Эта метрика дает сведения о том, как часто вы получаете более строгую согласованность, нежели та, которая настроена для учетной записи Cosmos DB. Другими словами, вы можете увидеть вероятность (в миллисекундах) выполнения строго согласованных операций чтения для комбинации регионов записи и чтения.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения об уровнях согласованности см. по следующим ссылкам:

* [Согласование уровней согласованности в API Cosmos DB](consistency-levels-across-apis.md)
* [Компромисс между доступностью и производительностью для различных уровней согласованности](consistency-levels-tradeoffs.md)
* [Управление токенами сеанса для приложения](how-to-manage-consistency.md#utilize-session-tokens)
* [Мониторинг метрики вероятностного ограниченного устаревания (PBS)](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)