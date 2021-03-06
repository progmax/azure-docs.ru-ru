---
title: Элемент пользовательского интерфейса Azure InfoBox | Документация Майкрософт
description: Сведения об элементе пользовательского интерфейса Microsoft.Common.TextBlock для портала Azure.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2018
ms.author: tomfitz
ms.openlocfilehash: abd1329f2ebac90bf846dfd5fc5b307ddb5e52bd
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095486"
---
# <a name="microsoftcommoninfobox-ui-element"></a>Элемент пользовательского интерфейса Microsoft.Common.InfoBox
Элемент управления, который добавляет поле сведений. Поле содержит важный текст или предупреждения, которые помогают пользователям понять, какие значения нужно указать. Он также может быть связан с URI, по которому можно получить более подробную информацию.

## <a name="ui-sample"></a>Пример элемента пользовательского интерфейса
![Microsoft.Common.InfoBox](./media/managed-application-elements/microsoft.common.infobox.png)


## <a name="schema"></a>Схема
```json
{
  "name": "text1",
  "type": "Microsoft.Common.InfoBox",
  "visible": true,
  "options": {
    "icon": "None",
    "text": "Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo.",
    "uri": "https://www.microsoft.com"
  }
}
```

## <a name="remarks"></a>Примечания

* Установите для параметра `icon` одно из следующих значений: **Нет**, **Сведения**, **Предупреждение** или **Ошибка**.
* Свойство `uri` необязательное.

## <a name="sample-output"></a>Пример выходных данных

```json
"Nullam eros mi, mollis in sollicitudin non, tincidunt sed enim. Sed et felis metus, rhoncus ornare nibh. Ut at magna leo."
```

## <a name="next-steps"></a>Дополнительная информация
* Общие сведения о создании определений пользовательского интерфейса см. в статье [Начало работы с CreateUiDefinition](create-uidefinition-overview.md).
* Дополнительные сведения об общих свойствах элементов пользовательского интерфейса см. в статье [Элементы CreateUiDefinition](create-uidefinition-elements.md).
