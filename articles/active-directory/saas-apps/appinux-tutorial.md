---
title: Руководство по интеграции Azure Active Directory с Appinux | Документация Майкрософт
description: Узнайте, как настроить единый вход между Azure Active Directory и Appinux.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f329341b-fb77-42e5-b6a6-0cd641d19670
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeedes
ms.openlocfilehash: a9b8e7738f2974b4c672af0e50e2ad510f0d41c2
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50129052"
---
# <a name="tutorial-azure-active-directory-integration-with-appinux"></a>Руководство. Интеграция Azure Active Directory с Appinux

В этом руководстве описано, как интегрировать Appinux с Azure Active Directory (Azure AD).

Интеграция Appinux с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Appinux.
- Вы можете включить автоматический вход пользователей в Appinux (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Appinux, вам потребуется:

- подписка Azure AD;
- подписка Appinux с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Appinux из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-appinux-from-the-gallery"></a>Добавление Appinux из коллекции

Чтобы настроить интеграцию Appinux с Azure AD, необходимо добавить Appinux из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Appinux из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]

3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **Appinux**, выберите **Appinux** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Appinux в списке результатов](./media/appinux-tutorial/tutorial_appinux_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в Appinux с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Appinux соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Appinux.

Чтобы настроить и проверить единый вход Azure AD в Appinux, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Appinux](#creating-an-appinux-test-user)** нужно для того, чтобы в Appinux существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Appinux.

**Чтобы настроить единый вход Azure AD в Appinux, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **Appinux** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Выбрать метод единого входа** щелкните **Выбрать** для режима **SAML**, чтобы включить единый вход.

    ![Настройка единого входа](./media/appinux-tutorial/tutorial_general_301.png)

3. На странице **Настройка единого входа с помощью SAML** щелкните **Изменить**, чтобы открыть диалоговое окно **Базовая конфигурация SAML**.

    ![Настройка единого входа](./media/appinux-tutorial/tutorial_appinux_editurl.png)

4. В разделе **Базовая конфигурация SAML** выполните приведенные ниже действия.

    ![Сведения о домене и URL-адресах единого входа приложения Appinux](./media/appinux-tutorial/tutorial_appinux_url1.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<Appinux_SUBDOMAIN>.appinux.com`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<Appinux_SUBDOMAIN>.appinux.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`

    > [!NOTE]
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки Appinux](https://support.appinux.com).

5. Приложение Appinux ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На странице **Настройка единого входа с помощью SAML** нажмите кнопку **Изменить**, чтобы открыть диалоговое окно **Атрибуты пользователя**.

    ![Раздел "Атрибут"](./media/appinux-tutorial/edit_attribute.png)

    ![Раздел "Атрибут"](./media/appinux-tutorial/tutorial_namespace.png)

6. В разделе **Утверждения пользователя** диалогового окна **Атрибуты пользователя** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия.

    | **Имя** | **Пространство имен** | **Атрибут источника**|
    | ---------|---------------| --------- |
    | `givenname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.givenname` |
    | `surname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.surname` |
    | `emailaddress` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.mail` |
    | `name` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.userprincipalname` |
    | `UserType` | `http://bcv.appinux.com/claims` | `Provide the value as per your organization` |
    | `Tag` | `http://appinux.com/Tag` | `Provide the value as per your organization` |
    | `Role` | `http://schemas.microsoft.com/ws/2008/06/identity/claims/role` | `user.assignedroles` |
    | `email` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/email` | `user.mail` |
    | `wanshort` | `http://appinux.com/windowsaccountname2` | `extractmailprefix([userprincipalname])` |
    | `nameidentifier` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.employeeid` |

    a. Щелкните **Добавить новое утверждение**, чтобы открыть диалоговое окно **Управление утверждениями пользователя**.

    ![Создать атрибут](./media/appinux-tutorial/new_save_attribute.png)

    ![Добавление атрибута](./media/appinux-tutorial/new_attribute_details.png)

    b. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.

    c. В текстовом поле **Пространство имен** введите значение пространства имен, показанное для этой строки.

    d. В качестве источника выберите **Атрибут**.

    д. В списке **Атрибут источника** введите значение атрибута, отображаемое для этой строки.

    Е. Нажмите кнопку **ОК**.

    ж. Выберите команду **Сохранить**.

7. На странице **сертификата подписи SAML** в разделе **Сертификат подписи SAML** щелкните **Скачать**, чтобы скачать **XML метаданных федерации**, а затем сохраните метаданные на компьютере.

    ![Ссылка для скачивания сертификата](./media/appinux-tutorial/tutorial_appinux_certificate.png) 

8. Чтобы настроить единый вход на стороне **Appinux**, отправьте [группе поддержки Appinux](https://support.appinux.com) скачанный **XML-файл метаданных**. Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

1. На портале Azure в области слева выберите **Azure Active Directory**, **Пользователи**, а затем — **Все пользователи**.

    ![Создание пользователя Azure AD][100]

2. В верхней части экрана выберите **Новый пользователь**.

    ![Создание тестового пользователя Azure AD](./media/appinux-tutorial/create_aaduser_01.png) 

3. В разделе свойств пользователя сделайте следующее.

    ![Создание тестового пользователя Azure AD](./media/appinux-tutorial/create_aaduser_02.png)

    a. В поле **Имя** введите **BrittaSimon**.
  
    b. В поле **Имя пользователя** введите **brittasimon@yourcompanydomain.extension**.  
    Например, BrittaSimon@contoso.com

    c. Выберите **Свойства**, установите флажок **Показать пароль** и запишите значение, которое отображается в поле "Пароль".

    d. Нажмите кнопку **Создать**.

### <a name="creating-an-appinux-test-user"></a>Создание тестового пользователя Appinux

Цель этого раздела — создать пользователя Britta Simon в Appinux. Appinux поддерживает JIT-подготовку и она включена по умолчанию. В этом разделе никакие действия с вашей стороны не требуются. Пользователь будет создан автоматически при попытке получить доступ к Appinux (если он еще не создан).
>[!Note]
>Чтобы создать пользователя вручную, обратитесь к  [группе поддержки Appinux](https://support.appinux.com).

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к Appinux.

1. На портале Azure перейдите в колонку **Корпоративные приложения** и выберите **Все приложения**.

    ![Назначение пользователя][201]

2. В списке приложений выберите **Appinux**.

    ![Настройка единого входа](./media/appinux-tutorial/tutorial_appinux_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** из списка пользователей выберите **Britta Simon**, а затем в верхней части экрана нажмите кнопку **Выбрать**.

6. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.

### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув плитку Appinux на панели доступа, вы автоматически войдете в приложение Appinux.
Дополнительные сведения о панели доступа см. в статье с [общими сведениями о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appinux-tutorial/tutorial_general_01.png
[2]: ./media/appinux-tutorial/tutorial_general_02.png
[3]: ./media/appinux-tutorial/tutorial_general_03.png
[4]: ./media/appinux-tutorial/tutorial_general_04.png

[100]: ./media/appinux-tutorial/tutorial_general_100.png

[200]: ./media/appinux-tutorial/tutorial_general_200.png
[201]: ./media/appinux-tutorial/tutorial_general_201.png
[202]: ./media/appinux-tutorial/tutorial_general_202.png
[203]: ./media/appinux-tutorial/tutorial_general_203.png
