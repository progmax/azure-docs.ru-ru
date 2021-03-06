---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/09/2018
ms.author: estfan
ms.openlocfilehash: 3fa71085d649ace95aa24ac87c8714a7268f5386
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52272003"
---
Чтобы точнее оценить затраты на потребление, определите, сколько сообщений или событий обычно приходят вам в любой день. Не следует основывать расчеты только на интервале опроса. Когда событие или сообщение соответствует критериям, большая часть триггеров немедленно пытается считать его, как и все ожидающие события и сообщения, которые соответствуют критериям. Такое поведение означает, что даже если выбрать более длительный интервал опроса, триггеры срабатывают на основе количества ожидающих сообщений или событий, которые соответствуют критериям для запуска рабочих процессов. К таким триггерам относятся служебная шина и концентратор событий Azure.

Например, предположим, что вы настроили триггер, который каждый день проверяет конечную точку. Когда этот триггер проверяет конечную точку и обнаруживает 15 событий, которые соответствуют критериям, он срабатывает и выполняет соответствующий рабочий процесс 15 раз. Приложения логики измеряют все действия, происходящие во время выполнения этих 15 рабочих процессов, в том числе запросы триггера. Чтобы вычислить потенциальные затраты, используйте [калькулятор цен Azure](https://azure.microsoft.com/pricing/calculator/).