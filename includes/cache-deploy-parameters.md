---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: 1ddb81de479317a098f9de8aa5756cbaae59cb72
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331416"
---
### <a name="cacheskuname"></a>Параметр cacheSKUName
Ценовая категория нового кэша Azure Redis.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
      }
    },

В шаблоне определены значения, допустимые для этого параметра (Basic и Standard). Если значение не указано, параметру назначается значение по умолчанию (Basic). Уровень Basic предоставляет один узел с различными размерами (до 53 ГБ).
Уровень Standard предоставляет два узла (основной и реплика) с различными размерами (до 53 ГБ) и соглашением об уровне обслуживания 99,9 %.

### <a name="cacheskufamily"></a>Параметр cacheSKUFamily
Семейство для SKU.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>Параметр cacheSKUCapacity
Размер нового экземпляра кэша Azure Redis. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


В шаблоне определены значения, допустимые для этого параметра (0, 1, 2, 3, 4, 5 или 6). Если значение не указано, параметру назначается значение по умолчанию (0). Эти числа соответствуют следующим размерам кэша: 0 = 250 МБ, 1 = 1 ГБ, 2 = 2,5 ГБ, 3 = 6 ГБ, 4 = 13 ГБ, 5 = 26 ГБ, 6 = 53 ГБ

