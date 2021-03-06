---
title: Руководство. Включение в одностраничном приложении аутентификации на основе учетных записей с помощью Azure Active Directory B2C | Документация Майкрософт
description: Руководство по предоставлению пользователю данных для входа в одностраничное приложение JavaScript с помощью Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: cce76a0e97e039ec6e6c3a976d1fc7caca7fde73
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834440"
---
# <a name="tutorial-enable-single-page-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Руководство. Включение в одностраничном приложении аутентификации на основе учетных записей с помощью Azure Active Directory B2C

В этом руководстве описано использование Azure Active Directory (Azure AD) B2C для входа и регистрации пользователей в одностраничном приложении (SPA). Azure AD B2C позволяет приложениям выполнять проверку подлинности учетных записей социальных сетей, корпоративных учетных записей и учетных записей Azure Active Directory с помощью стандартных протоколов.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Зарегистрируйте пример одностраничного приложения в каталоге Azure AD B2C.
> * Создавайте потоки пользователей для регистрации пользователей, входа в систему, изменения профилей и сброса паролей.
> * Настройте пример приложения для использования каталога Azure AD B2C.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Предварительные требования

* Создайте собственный [каталог Azure AD B2C](active-directory-b2c-get-started.md).
* Установите [Visual Studio 2017](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **ASP.NET и веб-разработка**.
* Установите [пакет SDK для NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.
* Установите [Node.js](https://nodejs.org/en/download/)

## <a name="register-single-page-app"></a>Регистрация одностраничного приложения

Приложения должны быть [зарегистрированы](../active-directory/develop/developer-glossary.md#application-registration) в каталоге, чтобы иметь возможность получать [маркеры доступа](../active-directory/develop/developer-glossary.md#access-token) от Azure Active Directory. При регистрации приложения создается [идентификатор](../active-directory/develop/developer-glossary.md#application-id-client-id) для приложения в каталоге. 

Войдите на [портал Azure](https://portal.azure.com/) с правами глобального администратора каталога Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Выберите **Azure AD B2C** из списка служб на портале Azure. 

2. В разделе параметров B2C щелкните **Приложения**, а затем — **Добавить**. 

    Чтобы зарегистрировать пример веб-приложения в каталоге, используйте следующие параметры:
    
    ![Добавление нового приложения](media/active-directory-b2c-tutorials-spa/spa-registration.png)
    
    | Параметр      | Рекомендуемое значение  | Описание                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Имя** | Мой пример одностраничного приложения | Введите **имя** приложения, которое будет описанием для пользователей. | 
    | **Включить веб-приложение или веб-интерфейс API** | Yes | Выберите **Да** для одностраничного приложения. |
    | **Разрешить неявный поток** | Yes | Выберите **Да**, так как приложение использует [вход в OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL-адрес ответа** | `http://localhost:6420` | URL-адреса ответа — это конечные точки, куда Azure AD B2C возвращает все токены, запрашиваемые вашим приложением. В этом руководстве пример выполняется локально (localhost) и прослушивает порт 6420. |
    | **Включить собственный клиент** | Нет  | Так как это одностраничное приложение, а не собственный клиент, выберите "Нет". |
    
3. Чтобы зарегистрировать приложение, щелкните **Создать**.

Зарегистрированные приложения отобразятся в списке приложений для каталога Azure AD B2C. Выберите одностраничное приложение в списке. Откроется панель свойств зарегистрированного одностраничного приложения.

![Свойства одностраничного приложения](./media/active-directory-b2c-tutorials-spa/b2c-spa-properties.png)

Запишите значение **идентификатора клиента приложения**. Идентификатор уникально идентифицирует приложение и необходим при настройке приложения далее в этом руководстве.

## <a name="create-user-flows"></a>Создание потоков пользователей

Поток пользователя Azure AD B2C определяет пользовательский интерфейс для задачи идентификации. Например, распространенные потоки пользователей — это вход, регистрация, изменение паролей и профилей.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Создание потока пользователя регистрации или входа в систему

Чтобы зарегистрировать пользователей для последующего входа в веб-приложение, создайте **поток пользователя регистрации или входа в систему**.

1. На странице портала Azure AD B2C выберите **Потоки пользователей** и щелкните **Создать поток пользователя**.
2. На вкладке **Recommended** (Рекомендуется) щелкните **Sign up and sign in** (Регистрация и вход).

    Чтобы настроить поток пользователя, используйте приведенные ниже параметры.

    ![Добавление потока пользователя регистрации или входа в систему](media/active-directory-b2c-tutorials-spa/add-susi-user-flow.png)

    | Параметр      | Рекомендуемое значение  | Описание                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Имя** | SiUpIn | Введите **имя** потока пользователя. Имя потока пользователя начинается с **B2C_1_**. В примере кода используйте полное имя потока пользователя **B2C_1_SiUpIn**. | 
    | **Поставщики удостоверений** | "Регистрация по электронной почте" | Поставщик удостоверений, используемый для идентификации пользователя. |

3. В разделе **Атрибуты пользователя и утверждения** щелкните **Показать еще** и выберите приведенные ниже параметры.

    ![Добавление потока пользователя регистрации или входа в систему](media/active-directory-b2c-tutorials-spa/add-attributes-and-claims.png)

    | столбец      | Рекомендуемое значение  | ОПИСАНИЕ                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Собрать атрибут** | "Отображаемое имя" и "Почтовый индекс" | Выберите атрибуты, получаемые от пользователя во время регистрации. |
    | **Вернуть утверждение** | "Отображаемое имя", "Почтовый индекс", User is new (Новый пользователь) и User's Object ID (Идентификатор объекта пользователя). | Выберите [утверждения](../active-directory/develop/developer-glossary.md#claim), которые следует включить в [маркер доступа](../active-directory/develop/developer-glossary.md#access-token). |

4. Последовательно выберите **ОК**.
5. Нажмите кнопку **Создать**, чтобы создать поток пользователя. 

### <a name="create-a-profile-editing-user-flow"></a>Создание потока пользователя изменения профиля

Чтобы пользователи могли самостоятельно сбросить сведения о своем профиле, создайте **поток пользователя редактирования профиля**.

1. На странице портала Azure AD B2C выберите **Потоки пользователей** и щелкните **Создать поток пользователя**.
2. На вкладке **Recommended** (Рекомендуется) щелкните **Profile editing** (Изменение профиля).

    Чтобы настроить поток пользователя, используйте приведенные ниже параметры.

    | Параметр      | Рекомендуемое значение  | Описание                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Имя** | SiPe | Введите **имя** потока пользователя. Имя потока пользователя начинается с **B2C_1_**. В примере кода используйте полное имя потока пользователя **B2C_1_SiPe**. | 
    | **Поставщики удостоверений** | Локальная учетная запись для входа | Поставщик удостоверений, используемый для идентификации пользователя. |

3.  В разделе **Атрибуты пользователя** щелкните **Показать еще** и выберите приведенные ниже параметры.

    | столбец      | Рекомендуемое значение  | ОПИСАНИЕ                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Собрать атрибут** | "Отображаемое имя" и "Почтовый индекс" | Выберите атрибуты, которые пользователи смогут изменять во время редактирования профиля. |
    | **Вернуть утверждение** | "Отображаемое имя", "Почтовый индекс" и User's Object ID (Идентификатор объекта пользователя) | Выберите [утверждения](../active-directory/develop/developer-glossary.md#claim), которые следует включить в [маркер доступа](../active-directory/develop/developer-glossary.md#access-token) после редактирования профиля. |

4. Последовательно выберите **ОК**.
5. Нажмите кнопку **Создать**, чтобы создать поток пользователя. 

### <a name="create-a-password-reset-user-flow"></a>Создание потока пользователя сброса пароля

Чтобы обеспечить сброс паролей для приложения, необходимо создать **поток пользователя сброса паролей**. В этом потоке пользователя описываются действия, которые необходимо выполнить потребителям для сброса пароля, и содержимое маркеров, которые приложение должно получить при успешном завершении.

1. На странице портала Azure AD B2C выберите **Потоки пользователей** и щелкните **Создать поток пользователя**.
2. На вкладке **Recommended** (Рекомендуется) щелкните **Password reset** (Сброс пароля).

    Чтобы настроить поток пользователя, используйте приведенные ниже параметры.

    | Параметр      | Рекомендуемое значение  | Описание                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Имя** | SSPR | Введите **имя** потока пользователя. Имя потока пользователя начинается с **B2C_1_**. В примере кода используйте полное имя потока пользователя **B2C_1_SSPR**. | 
    | **Поставщики удостоверений** | Reset password using email address (Сброс пароля с использованием адреса электронной почты) | Поставщик удостоверений, используемый для идентификации пользователя. |

3. В разделе **Утверждения приложения** щелкните **Показать еще** и выберите приведенные ниже параметры.

    | столбец      | Рекомендуемое значение  | ОПИСАНИЕ                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Вернуть утверждение** | User's Object ID (Идентификатор объекта пользователя) | Выберите [утверждения](../active-directory/develop/developer-glossary.md#claim), которые следует включить в [маркер доступа](../active-directory/develop/developer-glossary.md#access-token) после сброса пароля. |

4. Последовательно выберите **ОК**.
5. Нажмите кнопку **Создать**, чтобы создать поток пользователя. 

## <a name="update-single-page-app-code"></a>Обновление одностраничного приложения

После регистрации приложения и создания потоков пользователя необходимо настроить приложение для использования каталога Azure AD B2C. С помощью этого руководства вы настроите пример одностраничного приложения JavaScript, который можно скачать из репозитория GitHub. 

[Загрузите ZIP-файл](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp/archive/master.zip) или клонируйте пример приложения с GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```
В этом примере показано, как использовать Azure AD B2C в одностраничном приложении для регистрации и входа пользователя, а также вызова защищенного веб-API. Необходимо изменить приложение так, чтобы использовать регистрацию в каталоге и настроить созданные потоки пользователей. 

Чтобы изменить параметры приложения:

1. Откройте файл `index.html` в примере одностраничного приложения Node.js.
2. Настройте пример, указав сведения о регистрации каталога Azure AD B2C. Измените следующие строки кода (не забудьте заменить значения именами используемого каталога и API):

    ```javascript
    // The current application coordinates were pre-registered in a B2C directory.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://fabrikamb2c.b2clogin.com/tfp/fabrikamb2c.onmicrosoft.com/B2C_1_<Sign-up or sign-in policy name>",
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://fabrikamb2chello.azurewebsites.net/hello',
    };
    ```

    Имя потока пользователя, используемое в этом руководстве — **B2C_1_SiUpIn**. Если вы используете другое имя потока пользователя, укажите его в значении `authority`.

## <a name="run-the-sample"></a>Запуск примера

1. Откройте командную строку Node.js.
2. Перейдите в каталог, содержащий пример приложения Node.js. Например `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Выполните следующие команды:
    ```
    npm install && npm update
    node server.js
    ```

    В окне консоли отобразится номер порта, связанный с размещаемым приложением.
    
    ```
    Listening on port 6420...
    ```

4. С помощью браузера перейдите по адресу `http://localhost:6420`, чтобы просмотреть приложение.

Пример приложения поддерживает регистрацию пользователей, вход в систему, редактирование профилей и сброс паролей. В этом руководстве показан процесс регистрации пользователя в приложении с помощью адреса электронной почты. Вы можете попытаться выполнить другие сценарии самостоятельно.

### <a name="sign-up-using-an-email-address"></a>Регистрация с помощью адреса электронной почты

1. Щелкните **Войти**, чтобы зарегистрироваться как пользователь приложения SPA. В этом случае используется поток пользователя **B2C_1_SiUpIn**, определенный на предыдущем шаге.

2. Появляется страница входа в Azure AD B2C со ссылкой для регистрации. Щелкните ссылку **Зарегистрироваться сейчас**, так как у вас нет учетной записи. 

3. В рамках рабочего процесса регистрации появляется страница для сбора данных и проверки личности пользователя с использованием адреса электронной почты. Рабочий процесс регистрации также собирает пароль пользователя и запрашиваемые атрибуты, определенные в потоке пользователя.

    Используйте действительный адрес электронной почты и подтвердите его с помощью кода проверки. Задайте пароль. Введите значения запрашиваемых атрибутов. 

    ![Рабочий процесс регистрации](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Щелкните **Создать**, чтобы создать локальную учетную запись в каталоге Azure AD B2C.

Теперь пользователь может использовать свой адрес электронной почты для входа в одностраничное приложение и работы с ним.

> [!NOTE]
> После входа в систему приложение выдаст ошибку, информирующую о недостаточных разрешениях. Эта ошибка возникает, потому что вы пытаетесь получить доступ к ресурсу из демонстрационного каталога. Так как маркер доступа действителен только для вашего каталога Azure AD, вызов API считается неавторизованным. Перейдите к следующему руководству, чтобы создать защищенный веб-API для своего каталога. 

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете использовать свой каталог Azure AD B2C при работе с другими руководствами по Azure AD B2C. [Удалите каталог Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant), если он больше не нужен.

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как создать каталог Azure AD B2C, создать протоки пользователя и обновить пример одностраничного приложения для использования каталога Azure AD B2C. Перейдите к следующему руководству, чтобы узнать, как зарегистрировать, настроить и вызвать защищенный веб-API из классического приложения.

> [!div class="nextstepaction"]
> [Примеры кода Azure AD B2C](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
