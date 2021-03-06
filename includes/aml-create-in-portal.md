---
title: включение файла
description: включение файла
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 09/24/2018
ms.openlocfilehash: edcb2ecb74255ddbb8d601cb69565fb401b756d2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886366"
---
Войдите на [портал Azure](https://portal.azure.com/) с помощью учетных данных для используемой подписки Azure. 

Панель мониторинга для рабочей области портала поддерживается только в браузерах Microsoft Edge, Chrome и Firefox.

   ![Портал Azure](./media/aml-create-in-portal/portal-dashboard.png)

Выберите **Создать ресурс** в левом верхнем углу портала.

   ![Создание ресурса на портале Azure](./media/aml-create-in-portal/portal-create-a-resource.png)

В строке поиска введите **машинное обучение**. Выберите результат поиска **рабочей области Службы машинного обучения**.

   ![Поиск рабочей области](./media/aml-create-in-portal/allservices-search.PNG)

Прокрутите вниз панель **рабочей области Службы машинного обучения** и щелкните **Создать**, чтобы начать создание.

   ![Создание](./media/aml-create-in-portal/portal-create-button.png)

Настройте рабочую область на панели **рабочей области Службы машинного обучения**.

   Поле|ОПИСАНИЕ
   ---|---
   имя рабочей области. |Введите уникальное имя для идентификации рабочей области. Мы используем имя docs-ws. Имена должны быть уникальными в группе ресурсов. Используйте имя, которое легко запомнить и отличить от рабочих областей, созданных другими пользователями.  
   Подписка |Выберите подписку Azure, которую нужно использовать.
   Группа ресурсов | Используйте группу ресурсов, которая есть в подписке, или введите имя, чтобы создать группу ресурсов. Группа ресурсов — это контейнер, содержащий связанные ресурсы для решения Azure. Мы используем имя docs-aml. 
   Расположение | Выберите ближайшее к пользователям и ресурсам данных расположение. В нем будет создана рабочая область.

   ![Создание рабочей области](./media/aml-create-in-portal/workspace-create.png)

Чтобы начать процесс создания, выберите **Создать**. Создание рабочей области может занять несколько минут.

Состояние развертывания можно проверить, щелкнув значок уведомлений (колокольчик) на панели инструментов.

   ![Состояние создания рабочей области](./media/aml-create-in-portal/notifications.png)

По завершении процесса появится сообщение об успешном развертывании. Оно также присутствует в области уведомлений. Чтобы просмотреть новую рабочую область, выберите **Перейти к ресурсу**.
