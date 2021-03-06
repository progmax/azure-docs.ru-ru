---
title: Хранение пакета AppSource в хранилище Azure и создание URL-адреса с ключом SAS | Документация Майкрософт
description: Детальные шаги, необходимые для отправки и защитить пакета AppSource.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 335590cc616035020f66cfc5208e21d4c5128fe6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808241"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Хранение пакета AppSource в хранилище Azure и создание URL-адреса с ключом SAS
=============================================================================

Для обеспечения безопасности файлов, всем партнерам необходимо сохранить файл пакета AppSource в учетной записи хранилища BLOB-объектов Azure и использовать ключ SAS для совместного использования. Файл пакета будет извлечен из места хранения Azure для сертификации и будет использоваться в пробных версий AppSource.

Чтобы отправить данные в хранилище BLOB-объектов Azure, выполните следующие действия.

1.  Перейдите по ссылке <http://azure.microsoft.com> и создайте бесплатную пробную версию или учетную запись выставления счетов.

2.  Войдите на [портал Azure](http://portal.azure.com/).

3.  Создайте новую учетную запись хранения нажав кнопку **+ Создать** и переместитесь к учетной записи **Данные + Хранилище**.

  ![Колонка "Данные + Хранилище" на портале Microsoft Azure](media/CRMScreenShot7.png)

4. Введите **Имя** и **Группу ресурсов** и нажмите кнопку **Создать**.

  ![Создание учетной записи хранения на портале Azure](media/CRMScreenShot8.png)

5. Перейдите к только что созданной группе ресурсов и создайте контейнер больших двоичных объектов.

  ![Отправка пакета в роли большого двоичного объекта, с помощью портала Microsoft Azure](media/CRMScreenShot9.png)

6.  Если это еще не сделано, скачайте и установите [Обозреватель службы хранилища Azure](http://storageexplorer.com/).

7.  Откройте Обозреватель службы хранилища и используйте значок для подключения к учетной записи хранения Azure.

8.  Перейдите в созданному контейнеру больших двоичных объектов и для добавления пакета в ZIP-файл нажмите кнопку **Отправить**.

  ![Отправка пакета с помощью Обозревателя хранилищ Microsoft Azure](media/CRMScreenShot10.png)

9.  Щелкните правой кнопкой мыши на контейнер и выберите **Получить подписанный URL-адрес**.

  ![Получение подписанного URL-адреса файла Azure](media/CRMScreenShot11.png)

10.  Измените **время окончания срока действия**, чтобы сделать SAS активной в течение месяца, а затем нажмите **Создать**.

  ![Изменение даты истечения срока действия SAS файла Azure](media/CRMScreenShot12.png)

11.  Скопируйте поле URL-адреса и сохранить его для последующего использования. При создании связанных предложений, необходимо ввести этот URL-адрес. 

  ![Копирование URL-адреса SAS файла Azure](media/CRMScreenShot13.png)

