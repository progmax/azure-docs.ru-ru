---
title: Подключение учетной записи Google Cloud Platform к Cloudyn в Azure | Документы Майкрософт
description: Подключение учетной записи Google Cloud Platform для просмотра затрат и сведений об использовании данных в отчетах Cloudyn.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/05/2018
ms.topic: conceptual
ms.service: cost-management
manager: benshy
ms.custom: ''
ms.openlocfilehash: 1877acbd39f4e312e3a567e092bb0bcf7531b96b
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276335"
---
# <a name="connect-a-google-cloud-platform-account"></a>Подключение учетной записи Google Cloud Platform

Имеющуюся учетную запись Google Cloud Platform можно подключить к Cloudyn. После этого сведения о затратах и использовании станут доступны для просмотра в отчетах Cloudyn. Информация в этой статье поможет вам настроить учетную запись Google и подключить ее к Cloudyn.

> [!NOTE]
> Компания Google изменила политику безопасности учетных записей, тем самым предотвратив установление новых подключений между Cloudyn и Google. Cloudyn продолжает собирать данные Google для пользователей, которые уже установили подключение Cloudyn к Google. Тем не менее в настоящее время добавить новые учетные записи Google в Cloudyn невозможно. Команда Cloudyn не знает, когда возобновится поддержка добавления новых учетных записей Google в Cloudyn. Мы удалим это примечание при возобновлении поддержки.

## <a name="collect-project-information"></a>Сбор сведений о проекте

Начните сбор сведений о проекте.

1. Войдите в консоль Google Cloud Platform по адресу[https://console.cloud.google.com](https://console.cloud.google.com).
2. Просмотрите сведения о проекте, который вы хотите реализовать в Cloudyn, и найдите **имя проекта** и **идентификатор проекта**. Сохраните данные для использования на следующих шагах.  
    ![Консоль Google Cloud Platform](./media/connect-google-account/gcp-console01.png)
3. Если выставление счетов не включено и не связано с проектом, создайте учетную запись выставления счетов. Дополнительные сведения см. в разделе [Создание учетной записи выставления счетов](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create\_a\_new\_billing\_account).

## <a name="enable-storage-bucket-billing-export"></a>Включение экспорта данных выставления счетов контейнеров хранилища

Cloudyn извлекает данные выставления счетов Google из контейнеров хранилища. Сохраните сведения об **имени контейнера** и **префиксе отчета** для последующего использования во время регистрации Cloudyn.

При использовании Google Cloud Storage для хранения отчетов об использовании взимается минимальная плата. Дополнительные сведения см. на странице [цен на службу облачного хранилища](https://cloud.google.com/storage/pricing).

1. Если экспорт выставленных счетов в файл не включен, следуйте инструкциям в разделе [Как включить экспорт выставленных счетов в файл](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). Для этого можно использовать формат экспорта выставленных счетов CSV или JSON.
2. В противном случае в консоли Google Cloud Platform перейдите к разделу **Выставление счета** > **Billing export** (Экспорт выставленного счета). Запишите **имя контейнера** и **префикс отчета** выставления счетов.  
    ![Экспорт выставленных счетов](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Включение API-интерфейсов Google Cloud Platform

Для сбора информации о потреблении и ресурсах Cloudyn требуется включить следующие API Google Cloud Platform:

- API BigQuery;
- Google Cloud SQL;
- API Google Cloud Datastore;
- Google Cloud Storage;
- API Google Cloud Storage JSON;
- API Google Compute Engine.

### <a name="enable-or-verify-apis"></a>Включение и проверка API

1. В консоли Google Cloud Platform выберите проект, который вы хотите зарегистрировать в Cloudyn.
2. Перейдите к разделу **APIs & Services** (API и службы)  > **Библиотека**.
3. Используйте поиск, чтобы найти все перечисленные ранее API.
4. Убедитесь, что для каждого API отображается состояние **API enabled** (API включен). В противном случае выберите **Включить**.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Добавление учетной записи Google Cloud в Cloudyn

1. Откройте портал Cloudyn с портала Azure или перейдите по адресу [https://azure.cloudyn.com](https://azure.cloudyn.com/) и войдите.
2. Щелкните **Параметры** (значок шестеренки), а затем выберите **Cloud Accounts** (Облачные учетные записи).
3. На странице **управления учетными записями** выберите вкладку **Google Accounts** (Учетные записи Google), а затем щелкните **Add new +** (Добавить новый элемент +).
4. В поле **Google Account Name** (Имя учетной записи Google) введите адрес электронной почты для учетной записи выставления счетов, а затем щелкните **Далее**.
5. В диалоговом окне аутентификации Google выберите или введите учетную запись Google, а затем **предоставьте** cloudyn.com доступ к учетной записи.
6. Добавьте записанную ранее информацию о проекте запроса. Введите **идентификатор проекта**, имя **проекта**, имя контейнера **выставления счетов** и префикс отчета **файла выставления счетов**, а затем щелкните **Сохранить**.  
    ![Добавление проекта Google](./media/connect-google-account/add-project.png)

Учетная запись Google отобразится в списке учетных записей с указанием **Проверка подлинности выполнена**. Под ним должно отображаться имя и идентификатор проекта Google с символом зеленой галочки. Состояние учетной записи должно быть **Завершено**.

В течение нескольких часов в отчетах Cloudyn появятся сведения об издержках и использовании Google.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о Cloudyn см. в руководстве для Cloudyn [Просмотр сведений об использовании и затратах](./tutorial-review-usage.md).
