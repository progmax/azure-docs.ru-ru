---
title: 'Доменные службы Azure Active Directory: функции | Документация Майкрософт'
description: Функции доменных служб Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: ergreenl
ms.openlocfilehash: cd4fdb1e59f5b85fdfd8eb7ca17b77d02dcdac05
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157043"
---
# <a name="azure-ad-domain-services"></a>Доменные службы Azure AD
## <a name="features"></a>Функции
В управляемых доменах доменных служб Azure AD доступны следующие функции.

* **Простое развертывание:** доменные службы Azure AD для каталога Azure AD можно включить всего несколькими щелчками мыши. Управляемый домен включает в себя только облачные учетные записи пользователей и учетные записи пользователей, синхронизированные из локального каталога.
* **Поддержка присоединения к домену:** вы можете легко присоединить к домену компьютеры в виртуальной сети Azure, в которой доступен ваш управляемый домен. Присоединение к домену для клиента Windows операционных систем сервера происходит незаметно для доменов, поддерживаемых доменными службами Azure AD. Также можно использовать средства автоматического присоединения к домену для таких доменов.
* **Один экземпляр домена для каждого каталога Azure AD:** вы можете создать один домен Active Directory для каждого каталога Azure AD.
* **Создание доменов с пользовательскими именами:** с помощью доменных служб Azure AD вы можете создавать домены с пользовательскими именами (например, contoso100.com). Можно использовать проверенные и непроверенные доменные имена. Кроме того, можно создать домен с суффиксом домена (т. е. *.onmicrosoft.com), предоставляемым каталогом Azure AD.
* **Интеграция с Azure AD:** для доменных служб Azure AD не нужно настраивать репликацию или управлять ею. Учетные записи пользователей, членство в группах и учетные данные пользователя (пароли) из вашего каталога Azure AD автоматически доступны в доменных службах Azure AD. Новые пользователи, группы или изменения атрибутов в клиенте Azure AD или в локальном каталоге автоматически синхронизируются с доменными службами Azure AD.
* **Проверка подлинности NTLM и Kerberos:** с поддержкой проверки подлинности NTLM и Kerberos можно развертывать приложения, использующие встроенную проверку подлинности Windows.
* **Использование корпоративных учетных данных и паролей:** пароли для пользователей клиента Azure AD подходят для доменных служб Azure AD. Пользователи могут использовать свои корпоративные учетные данные для присоединения компьютеров к домену, входа в систему в интерактивном режиме или через удаленный рабочий стол, а также для аутентификации в управляемом домене.
* **Поддержка привязки LDAP и чтения LDAP:** для аутентификации пользователей в доменах, поддерживаемых доменными службами Azure AD, можно использовать приложения, использующие привязки LDAP. Кроме того, приложения, использующие операции чтения LDAP для запроса атрибутов пользователя или компьютера из каталога, также смогут работать с доменными службами Azure AD.
* **Защищенный LDAP (LDAPS)** : можно включить доступ к каталогу через протокол LDAPS. Защищенный доступ LDAP доступен в виртуальной сети по умолчанию. Тем не менее можно дополнительно включить безопасный LDAP для доступа через Интернет.
* **Групповая политика:** можно использовать один встроенный объект групповой политики (GPO) для каждого контейнера пользователя или компьютера, чтобы обеспечить соответствие учетных записей пользователей, а также компьютеров, присоединенных к домену, обязательным политикам безопасности. Кроме того, можно создать пользовательский объект групповой политики и назначить его пользовательским подразделениям для [управления групповой политикой](active-directory-ds-admin-guide-administer-group-policy.md).
* **Управление DNS** : участники группы администраторов контроллера домена AAD могут управлять DNS для управляемого домена с помощью таких привычных инструментов администрирования DNS, как оснастка MMC для администрирования DNS.
* **Создание пользовательских подразделений**: участники группы администраторов контроллера домена AAD могут создавать пользовательские подразделения внутри управляемого домена. Этим пользователям предоставляется полный набор административных привилегий для пользовательских подразделений, то есть они могут добавлять или удалять учетные записи служб, компьютеры, группы и т. д. в данных пользовательских подразделениях.
* **Доступность в различных регионах Azure:** на странице [служб Azure по регионам](https://azure.microsoft.com/regions/#services/) можно узнать, в каких регионах Azure доступны доменные службы Azure AD.
* **Высокий уровень доступности:** доменные службы AD Azure обеспечивают высокий уровень доступности вашего домена. Эта функция обеспечивает большее время непрерывной работы службы и отказоустойчивость. Встроенный мониторинг предоставляет средства автоматического восстановления после сбоев путем развертывания новых экземпляров для замены неисправных экземпляров и для обеспечения непрерывной работы домена.
* **Защита учетной записи AD с помощью блокировки:** учетные записи пользователей блокируются на 30 минут, если в течение 2 минут используются пять недействительных паролей. Учетные записи автоматически разблокируются через 30 минут.
* **Использование привычных средств управления:** вы можете использовать привычные средства управления Windows Server Active Directory, такие как центр администрирования Active Directory или Active Directory PowerShell, для администрирования управляемых доменов.
