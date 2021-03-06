---
title: Руководство по интеграции Azure Active Directory с RightAnswers | Документация Майкрософт
description: Узнайте, как настроить единый вход Azure Active Directory в приложении RightAnswers.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: fc589554b6ce2bb3d6aa1f52d9eb697c211d2a88
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421674"
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Учебник. Интеграция Azure Active Directory с RightAnswers

В этом учебнике описано, как интегрировать приложение RightAnswers с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением RightAnswers обеспечивает следующие преимущества:

- С помощью Azure AD можно управлять доступом к RightAnswers.
- Вы можете включить автоматический вход пользователей в RightAnswers (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с RightAnswers, вам потребуется:

- подписка Azure AD;
- подписка RightAnswers с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление RightAnswers из коллекции
1. настройка и проверка единого входа в Azure AD.

## <a name="adding-rightanswers-from-the-gallery"></a>Добавление RightAnswers из коллекции
Чтобы настроить интеграцию RightAnswers с Azure AD, необходимо добавить RightAnswers из коллекции в список управляемых приложений SaaS.

**Чтобы добавить RightAnswers из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

1. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
1. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

1. В поле поиска введите **RightAnswers**.

    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/tutorial_rightanswers_search.png)

1. На панели результатов выберите **RightAnswers** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.
В этом разделе описана настройка и проверка единого входа Azure AD в RightAnswers с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в RightAnswers соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователям Azure AD и соответствующим пользователем в RightAnswers.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в RightAnswers.

Чтобы настроить и проверить единый вход Azure AD в RightAnswers, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
1. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.
1. **[Создание тестового пользователя RightAnswers](#creating-a-rightanswers-test-user)** требуется для создания в RightAnswers пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.
1. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;
1. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении RightAnswers.

**Чтобы настроить единый вход Azure AD в RightAnswers, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **RightAnswers** щелкните **Единый вход**.

    ![Настройка единого входа][4]

1. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

1. В разделе **Домен и URL-адреса приложения RightAnswers** выполните следующие действия.

    ![Настройка единого входа](./media/rightanswers-tutorial/tutorial_rightanswers_url.png)

    a. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<subdomain>.rightanswers.com/portal/ss/`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<subdomain>.rightanswers.com:<identifier>/portal`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов RightAnswers](https://www.rightanswers.com/contact-us/). 
 
1. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

1. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/rightanswers-tutorial/tutorial_general_400.png)

1. Чтобы настроить единый вход на стороне **RightAnswers**, отправьте скачанный **XML-файл метаданных** в [службу поддержки RightAnswers](https://www.rightanswers.com/contact-us/).

    >[!NOTE]
    >Настройка единого входа должна выполняться службой поддержки RightAnswers.
    >Как только единый вход для вашей подписки будет включен, вы получите уведомление.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/create_aaduser_01.png) 

1. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/create_aaduser_02.png) 

1. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/create_aaduser_03.png) 

1. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/rightanswers-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-rightanswers-test-user"></a>Создание тестового пользователя RightAnswers

Чтобы пользователи Azure AD могли выполнять вход в RightAnswers, они должны быть подготовлены в RightAnswers. В случае RightAnswers подготовка выполняется автоматически, так что вам не нужно ничего делать.

В случае необходимости пользователи создаются автоматически при первой попытке входа в систему.

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетных записей пользователя RightAnswers или API, предоставляемые RightAnswers для подготовки учетных записей пользователя AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к RightAnswers.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon приложению RightAnswers, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

1. В списке приложений выберите **RightAnswers**.

    ![Настройка единого входа](./media/rightanswers-tutorial/tutorial_rightanswers_app.png) 

1. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

1. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

1. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

1. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

1. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](../user-help/active-directory-saas-access-panel-introduction.md).
## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/rightanswers-tutorial/tutorial_general_203.png

