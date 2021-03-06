---
title: Руководство по интеграции Azure Active Directory с ArcGIS Online | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в ArcGIS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 12ab224481c519db36ae21dd11916649ff0bfbe3
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269040"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Руководство по интеграции Azure Active Directory с ArcGIS Online

В этом руководстве описано, как интегрировать ArcGIS Online с Azure Active Directory (Azure AD).

Интеграция ArcGIS Online с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к ArcGIS Online.
- Вы можете включить автоматический вход пользователей в ArcGIS Online (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с ArcGIS Online, вам потребуется:

- подписка Azure AD;
- подписка ArcGIS Online с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде.
Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление ArcGIS Online из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-arcgis-online-from-the-gallery"></a>Добавление ArcGIS Online из коллекции

Чтобы настроить интеграцию ArcGIS Online с Azure AD, необходимо добавить ArcGIS Online из коллекции в список управляемых приложений SaaS.

**Чтобы добавить ArcGIS Online из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![изображение](./media/arcgis-tutorial/selectazuread.png)

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![изображение](./media/arcgis-tutorial/a_select_app.png)
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![изображение](./media/arcgis-tutorial/a_new_app.png)

4. В поле поиска введите **ArcGIS Online**, выберите **ArcGIS Online** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

     ![изображение](./media/arcgis-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в ArcGIS Online с использованием данных тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в ArcGIS Online соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в ArcGIS Online.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в ArcGIS Online.

Чтобы настроить и проверить единый вход Azure AD в ArcGIS Online, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя ArcGIS Online](#create-an-arcgis-online-test-user)** нужно для того, чтобы в ArcGIS Online также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как на портале Azure включить единый вход Azure AD и настроить его в приложении ArcGIS Online.

**Чтобы настроить единый вход Azure AD в ArcGIS Online, выполните следующие действия.**

1. На [портале Azure](https://portal.azure.com/) на странице интеграции с приложением **ArcGIS Online** выберите **Единый вход**.

    ![изображение](./media/arcgis-tutorial/b1_b2_select_sso.png)

2. В верхней части экрана щелкните **Изменить режим единого входа**, чтобы выбрать режим **SAML**.

      ![изображение](./media/arcgis-tutorial/b1_b2_saml_ssso.png)

3. В диалоговом окне **Select a Single sign-on method** (Выбор метода единого входа) щелкните **Выбрать** для режима **SAML**, чтобы включить единый вход.

    ![изображение](./media/arcgis-tutorial/b1_b2_saml_sso.png)

4. На странице **Set up Single Sign-On with SAML** (Настройка единого входа с помощью SAML) нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![изображение](./media/arcgis-tutorial/b1-domains_and_urlsedit.png)

5. В разделе **Basic SAML Configuration** (Базовая настройка SAML) выполните следующие действия:

    a. В текстовое поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<companyname>.maps.arcgis.com`.

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `<companyname>.maps.arcgis.com`.

    ![изображение](./media/arcgis-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Для получения этих значений обратитесь в [группу поддержки ArcGIS Online](http://support.esri.com/en/).

6. В разделе **Сертификат для подписи SAML** нажмите **Загрузить**, чтобы загрузить **XML-метаданные федерации**, а затем сохраните XML-файл на своем компьютере.

    ![изображение](./media/arcgis-tutorial/federationxml.png)

7. Для автоматизации настройки в **ArcGIS Online** необходимо установить **Расширение браузера "Безопасный вход в мои приложения"**, щелкнув **Установить расширение**.

    ![изображение](./media/arcgis-tutorial/install_extension.png)

8. После добавления расширения в браузере щелкните **Установка ArcGIS Online**, чтобы перейти к приложению ArcGIS Online. После этого укажите учетные данные администратора для входа в ArcGIS Online. Расширение браузера автоматически настроит приложение и автоматизирует шаги 9–13.

9. Если необходимо вручную настроить ArcGIS Online, откройте новое окно веб-браузера, зайдите на сайт компании ArcGIS в роли администратора и выполните следующие шаги.

10. Щелкните **EDIT SETTINGS** (Изменить параметры).

    ![Изменение параметров](./media/arcgis-tutorial/ic784742.png "Изменение параметров")

11. Щелкните **Security**(Безопасность).

    ![Безопасность](./media/arcgis-tutorial/ic784743.png "Безопасность")

12. В разделе **Enterprise Logins** (Корпоративные имена для входа) установите флажок **Set Identity Provider** (Задать поставщик удостоверений).

    ![Корпоративные имена входа](./media/arcgis-tutorial/ic784744.png "Корпоративные имена входа")

13. На странице **Назначение поставщика удостоверений** выполните следующие действия.

    ![Назначение поставщика удостоверений](./media/arcgis-tutorial/ic784745.png "Назначение поставщика удостоверений")

    a. В текстовое поле **Name** (Имя) введите название своей организации.

    b. Для параметра **Metadata for the Enterprise Identity Provider will be supplied using** (В предоставлении метаданных для корпоративного поставщика удостоверений будет использоваться) выберите значение **Файл**.

    c. Чтобы отправить загруженный файл метаданных, нажмите кнопку **Выбрать файл**.

    d. Щелкните **SET IDENTITY PROVIDER** (Задать поставщик удостоверений).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD 

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем выберите **All users** (Все пользователи).

    ![изображение](./media/arcgis-tutorial/d_users_and_groups.png)

2. В верхней части экрана выберите **Новый пользователь**.

    ![изображение](./media/arcgis-tutorial/d_adduser.png)

3. В разделе свойства пользователя сделайте следующее:

    ![изображение](./media/arcgis-tutorial/d_userproperties.png)

    a. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension**.  
    Например, BrittaSimon@contoso.com

    c. Выберите **Свойства**, установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="create-an-arcgis-online-test-user"></a>Создание тестового пользователя ArcGIS Online

Чтобы пользователи Azure AD могли выполнять вход в ArcGIS Online, они должны быть подготовлены в ArcGIS Online.  
В случае ArcGIS Online подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Войдите в клиент **ArcGIS** .

2. Щелкните **INVITE MEMBERS** (Пригласить участников).
   
    ![Приглашение участников](./media/arcgis-tutorial/ic784747.png "Приглашение участников")

3. Щелкните переключатель **Add members automatically without sending an email** (Добавлять участников автоматически без отправки электронного сообщения) и нажмите кнопку **NEXT** (Далее).
   
    ![Автоматическое добавление участников](./media/arcgis-tutorial/ic784748.png "Автоматическое добавление участников")

4. На странице диалогового окна **Участники** выполните следующие действия.
   
     ![Добавление и просмотр](./media/arcgis-tutorial/ic784749.png "Добавление и просмотр")
    
     a. Введите в поля **Email** (Адрес электронной почты), **First name** (Имя) и **Last name** (Фамилия) соответствующие данные действующей учетной записи Azure AD, которую необходимо подготовить.
  
     b. Нажмите кнопку **ADD AND REVIEW** (Добавить и просмотреть).
5. Просмотрите введенные данные и нажмите кнопку **ADD MEMBERS** (Добавить участников).
   
    ![Добавление участника](./media/arcgis-tutorial/ic784750.png "Добавление участника")
        
    > [!NOTE]
    > Владелец учетной записи Azure Active Directory получит по электронной почте сообщение со ссылкой для активации учетной записи.

### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к ArcGIS Online.

1. На портале Azure перейдите в колонку **Корпоративные приложения** и выберите **All applications** (Все приложения).

    ![изображение](./media/arcgis-tutorial/d_all_applications.png)

2. Из списка приложений выберите **ArcGIS Online**.

    ![изображение](./media/arcgis-tutorial/d_all_application.png)

3. В меню слева выберите **Пользователи и группы**.

    ![изображение](./media/arcgis-tutorial/d_leftpaneusers.png)

4. Нажмите кнопку **Добавить**, затем в диалоговом окне **Добавление назначения** выберите **Users and groups** (Пользователи и группы).

    ![изображение](./media/arcgis-tutorial/d_assign_user.png)

4. В диалоговом окне **Users and groups** (Пользователи и группы) из списка пользователей выберите **Britta Simon**. В верхней части экрана нажмите кнопку **Выбрать**.

5. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "ArcGIS Online" на панели доступа, вы автоматически войдете в приложение ArcGIS Online.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



