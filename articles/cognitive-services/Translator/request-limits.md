---
title: Ограничения запросов — API перевода текстов
titleSuffix: Azure Cognitive Services
description: В этой статье перечислены ограничения запросов для API перевода текстов. Плата взимается на основе количества знаков, а не частоты запроса с ограничением 5000 знаков на запрос. Ограничения знаков определяются подпиской, а F0 ограничивается 2 миллионами знаков в час.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: erhopf
ms.openlocfilehash: 29ef4cc594a3335d37bd813c0b682b248f96cf22
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2018
ms.locfileid: "51858333"
---
# <a name="request-limits-for-translator-text"></a>Ограничения запросов API перевода текстов

В этой статье перечислены ограничения регулирования для API перевода текстов. Услуги включают перевод, транслитерацию, определение длины предложения, определение языка и альтернативные переводы.

## <a name="character-limits-per-request"></a>Ограничение по количеству знаков на запрос

Каждый запрос ограничивается 5000 знаков. Плата взимается на основе количества знаков, а не запросов. Рекомендуется отправлять короткие запросы и настроить сохранение запросов, ожидающих выполнения, в любой момент времени.

Для количества ожидающих запросов в API перевода текстов ограничения не предусмотрены.

## <a name="character-limits-per-hour"></a>Ограничение по количеству знаков за час

Ограничение по количеству знаков за час определяется уровнем подписки API перевода текстов. При достижении или превышении этих ограничений вы получите, вероятнее всего, такой ответ о превышении квоты:

| Уровень | Допустимое число знаков |
|------|-----------------|
| F0 | 2 миллиона знаков в час |
| S1 | 40 миллионов знаков в час |
| S2 | 40 миллионов знаков в час |
| S3 | 120 миллионов знаков в час |
| S4 | 200 миллионов знаков в час |

Эти ограничения применимы только к универсальным системам Майкрософт. Для настраиваемых систем перевода, которые используют Microsoft Translator Hub, допустимое число знаков — 1800 в секунду.

## <a name="latency"></a>Latency

Максимальная задержка Перевода текстов составляет 13 секунд. За это время вы получите результат или ответ об истечении времени ожидания. Как правило, ответы возвращаются в таком диапазоне времени: 150–300 миллисекунд. Время отклика будет отличаться в зависимости от размера или запроса и языковой пары.

## <a name="sentence-length-limits"></a>Допустимая длина предложения

При использовании функции [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) допустимая длина предложения составляет 275 знаков. Исключения применимы для следующих языков:

| Язык | Код | Допустимое число знаков |
|----------|------|-----------------|
| Китайский | zh | 132 |
| Немецкий | de | 290 |
| Итальянский | it | 280 |
| Японский | ja | 150 |
| Португальский | pt | 290 |
| Испанский | es | 280 |
| Итальянский | it | 280 |
| Тайский | th | 258 |

> [!NOTE]
> Это ограничение не применяется к переводам.

## <a name="next-steps"></a>Дополнительная информация

* [Цены](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Доступность по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [API перевода текстов v3.0](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
