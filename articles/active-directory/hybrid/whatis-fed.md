---
title: Использование федерации с Azure Active Directory | Документация Майкрософт
description: Использование федерации с Azure Active Directory
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 0556a01490786ec4101176bc0437661723ce9b4f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/08/2018
ms.locfileid: "53110465"
---
# <a name="what-is-federation-with-azure-ad"></a>Использование федерации с Azure Active Directory

Федерация — это совокупность доменов, для которых установлено отношение доверия. Уровень доверия может отличаться, но обычно предусматривает проверку подлинности и почти всегда — авторизацию. Типичная федерация может включать ряд организаций, для которых установлено отношение доверия для общего доступа к набору ресурсов.

Можно создать федерацию локальной среды с помощью Azure AD, чтобы использовать ее для проверки подлинности и авторизации.  Этот метод входа гарантирует, что все проверки подлинности пользователя происходит локально.  Этот метод позволяет администраторам реализовать более строгие уровни контроля доступа. Поддерживается федерация с AD FS и PingFederate.

![Федеративная идентификация](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Если вы решите использовать федерацию со службами федерации Active Directory (AD FS), вы сможете дополнительно настроить синхронизацию хэша паролей как дополнительную защиту на случай сбоя в инфраструктуре AD FS.


## <a name="next-steps"></a>Дальнейшие действия

- [Что собой представляет гибридная идентификация](whatis-phs.md)
- [Использование Azure AD Connect с расширением Azure AD Connect Health](whatis-azure-ad-connect.md)
- [Использование синхронизации хэша паролей](whatis-phs.md)
- [Использование федерации](whatis-fed.md)
- [Использование единого входа](how-to-connect-sso.md)
- [Принцип работы федерации](how-to-connect-fed-whatis.md)
- [Федерация с PingFederate](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)