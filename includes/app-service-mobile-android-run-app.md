---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 505eac0996129a17b6b68e8ab4ea2d4fc80fd473
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132960"
---
1. Войдите на [портал Azure]. Щелкните **Просмотреть все** > **Мобильные приложения** и выберите только что созданную серверную часть приложения. В параметрах мобильного приложения щелкните **Быстрый старт** > **Android**. В разделе **Настройка клиентского приложения** щелкните **Загрузить**. Загрузится полный проект Android для приложения, предварительно настроенный для подключения к серверу. 
2. Откройте проект при помощи **Android Studio**, выберите **Import project (Eclipse ADT, Gradle, etc.)** (Импорт проекта (Eclipse ADT, Gradle и т. д.)). Обязательно используйте эту функцию импорта, чтобы избежать ошибок JDK.
3. Нажмите кнопку **Run 'app'** (Запустить приложение), чтобы создать проект и запустить приложение в симуляторе Android.
4. В приложении введите содержательный текст, например *Работа с учебником* , и нажмите кнопку «Add» (Добавить). Это отправляет запрос POST на ранее развернутый внутренний сервер Azure. Сервер вносит данные из запроса в таблицу TodoItem SQL и возвращает сведения о только что сохраненных элементах в мобильное приложение. В мобильном приложении эти данные отображаются в списке. 
   
    ![](./media/app-service-mobile-android-quickstart/mobile-quickstart-startup-android.png)

[портал Azure]: https://portal.azure.com/
