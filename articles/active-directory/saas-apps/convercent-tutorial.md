---
title: Руководство. Интеграция Azure Active Directory с Convercent | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в Convercent.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: add5be434ea8902de58dfdffc93b2c4829183667
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447845"
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a>Руководство. Интеграция Azure Active Directory с Convercent

В этом руководстве описано, как интегрировать Convercent с Azure Active Directory (Azure AD).

Интеграция Convercent с Azure AD обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Convercent.
- Вы можете включить автоматический вход пользователей в Convercent (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с Convercent, вам потребуется следующее:

- подписка Azure AD;
- подписка Convercent с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Convercent из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-convercent-from-the-gallery"></a>Добавление Convercent из коллекции
Чтобы настроить интеграцию Convercent с Azure AD, необходимо добавить Convercent из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Convercent из коллекции, выполните следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **Convercent**.

    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/tutorial_convercent_search.png)

1. На панели результатов выберите **Convercent** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в Convercent с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в Convercent соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Convercent.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Convercent.

Чтобы настроить и проверить единый вход в Azure AD в Convercent, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя Convercent](#creating-a-convercent-test-user)** требуется для создания в Convercent пользователя Britta Simon, связанного с соответствующим пользователем в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Convercent.

**Чтобы настроить единый вход Azure AD в Convercent, выполните следующее.**

1. На портале Azure на странице интеграции с приложением **Convercent** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/convercent-tutorial/tutorial_convercent_samlbase.png)

1. Если вы хотите настроить приложение в **режиме, инициированном поставщиком удостоверений**, то в разделе **Домены и URL-адреса приложения Convercent** выполните следующее действие:

    ![Настройка единого входа](./media/convercent-tutorial/tutorial_convercent_url.png)

    В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<instancename>.convercent.com/`
 
1. Если вы хотите настроить приложение в **режиме, инициированном поставщиком услуг**, то в разделе **Домены и URL-адреса приложения Convercent** выполните следующие действия:
    
    ![Настройка единого входа](./media/convercent-tutorial/tutorial_convercent_url1.png)

     a. Щелкните **Показать дополнительные параметры URL-адресов**. 

     b. В текстовом поле **URL-адрес для входа** введите значение в следующем формате: `https://<instancename>.convercent.com/`.

     c. В текстовое поле **Состояние ретранслятора** введите значение в следующем формате: `https://<instancename>.convercent.com/`.

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим идентификатором, URL-адресом для входа и состоянием ретранслятора. Чтобы получить их, обратитесь в [службу поддержки клиентов Convercent](http://support.convercent.com).

1. В разделе **Сертификат подписи SAML** щелкните **XML метаданных** и сохраните XML-файл на компьютере.

    ![Настройка единого входа](./media/convercent-tutorial/tutorial_convercent_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/convercent-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход для своего приложения, обратитесь в [службу поддержки Convercent](mailto:support@convercent.com) и предоставьте скачанный **XML-файл метаданных**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/convercent-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-convercent-test-user"></a>Создание тестового пользователя Convercent

Обратитесь к [группе поддержки Convercent](mailto:support@convercent.com), чтобы добавить пользователей на платформу Convercent.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как позволить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Convercent.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в Convercent, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. Из списка приложений выберите **Convercent**.

    ![Настройка единого входа](./media/convercent-tutorial/tutorial_convercent_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Convercent на панели доступа, вы автоматически войдете в приложение Convercent.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/convercent-tutorial/tutorial_general_01.png
[2]: ./media/convercent-tutorial/tutorial_general_02.png
[3]: ./media/convercent-tutorial/tutorial_general_03.png
[4]: ./media/convercent-tutorial/tutorial_general_04.png

[100]: ./media/convercent-tutorial/tutorial_general_100.png

[200]: ./media/convercent-tutorial/tutorial_general_200.png
[201]: ./media/convercent-tutorial/tutorial_general_201.png
[202]: ./media/convercent-tutorial/tutorial_general_202.png
[203]: ./media/convercent-tutorial/tutorial_general_203.png

