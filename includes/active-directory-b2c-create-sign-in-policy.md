---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 19e7c919345c0f56b274737840f8150f7d710501
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133383"
---
Если вам нужно добавить в приложение только функцию входа, воспользуйтесь политикой **входа**. Эта политика описывает действия, которые пользователь должен выполнить во время входа, и содержимое маркеров, которые должно получить приложение при успешном входе.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
Щелкните **Политики входа**.

Нажмите **+Добавить** в верхней части колонки.

**Имя** определяет имя политики входа, используемое приложением. Например, введите **SiIn**.

Щелкните **Поставщики удостоверений** и выберите **Local Account SignIn** (Локальная учетная запись для входа). При необходимости можно также выбрать поставщиков удостоверений в социальных сетях, если они уже настроены. Последовательно выберите **ОК**.

Щелкните **Утверждения приложения**. Здесь вы можете выбрать утверждения, которые необходимо возвращать в маркерах, отправляемых приложению после успешного входа. Например, выберите **Отображаемое имя**, **Поставщик удостоверений**, **Почтовый индекс** и **ИД объекта пользователя**. Последовательно выберите **ОК**.

Нажмите кнопку **Создать**. Обратите внимание, что в колонке **Политики регистрации** только что созданная политика отображается в следующем виде: **B2C_1_SiIn** (фрагмент **B2C\_1\_** добавляется автоматически).

Откройте политику, выбрав **B2C_1_SiIn**.

В раскрывающемся списке **Приложения** выберите пункт **Приложение B2C Contoso**, а в списке **URL-адрес ответа / URI перенаправления** — `https://localhost:44321/`.

Щелкните **Запустить сейчас**. Откроется новая вкладка браузера, и вы сможете взглянуть на процесс входа в ваше приложение глазами пользователя.

> [!NOTE]
> Создание, обновление и вступление политики в силу занимает около минуты.
>