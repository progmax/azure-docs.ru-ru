---
title: Предварительная версия защиты паролем Azure AD
description: Блокировка ненадежных паролей в локальной версии Active Directory с помощью предварительной версии функции защиты паролем Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: ca412e94f65c7e1ed9a547ec9dcabc62fac7d42f
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741833"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Предварительная версия. Применение защиты паролем Azure AD для Windows Server Active Directory

|     |
| --- |
| Защита паролем Azure AD и пользовательский список заблокированных паролей — это функции Azure Active Directory, которые предоставляются в режиме общедоступной предварительной версии. См. дополнительные сведения о [дополнительных условиях использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Защита паролем Azure AD — это новая функция в общедоступной предварительной версии на платформе Azure Active Directory (Azure AD), позволяющая улучшить политики паролей в организации. Для локального развертывания Azure AD защита паролем использует глобальный и настраиваемый списки заблокированных паролей, которые хранятся в Azure AD, и выполняет одинаковые локальные проверки облачных изменений Azure AD.

Защита паролем Azure AD состоит из трех программных компонентов.

* Прокси-служба защиты паролем Azure AD выполняется на любом компьютере, присоединенном к домену, в текущем лесу Active Directory. Она перенаправляет запросы с контроллеров домена в Azure AD и возвращает ответ от Azure AD на контроллер домена.
* Служба агента контроллера домена защиты паролем Azure AD получает запросы на подтверждение пароля из библиотеки DLL фильтрации паролей агента контроллера домена, обрабатывает их с помощью доступной на данный момент политики паролей и возвращает результат (выполнено/сбой). Эта служба отвечает за периодический (ежечасный) вызов прокси-службы защиты паролем Azure AD для получения новых версий политики паролей. Взаимодействие для вызовов к прокси-службе защиты паролем Azure AD и от нее обрабатывается через RPC (удаленный вызов процедур) по протоколу TCP. После получения новые политики хранятся в папке sysvol, где они реплицируются с другими контроллерами домена. Служба агента контроллера домена также отслеживает папку sysvol на предмет изменений в случае записи в другие контроллеры домена новой политики паролей. Если уже имеется подходящая недавняя политика, проверка прокси-службы защиты паролем Azure AD будет пропущена.
* Библиотека DLL фильтрации паролей агента контроллера домена получает от операционной системы запросы проверки пароля и пересылает их в службу агента контроллера домена защиты паролем Azure AD, выполняющуюся локально на контроллере домена.

![Взаимодействие компонентов защиты паролем Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="license-requirements"></a>Требования лицензий

Преимущества списка глобально заблокированных паролей применяются ко всем пользователям Azure Active Directory (Azure AD).

Пользовательский список заблокированных паролей требует наличия лицензий Azure AD Basic.

Защита паролем Azure AD для Windows Server Active Directory требует наличия лицензий Azure AD Premium.

Дополнительные сведения о лицензировании, включая расходы, можно найти на [сайте с ценами на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Загрузка

Для защиты паролем Azure AD имеется два обязательных установщика, которые можно скачать из [Центра загрузки Майкрософт](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="answers-to-common-questions"></a>Ответы на часто задаваемые вопросы

* Для контроллеров домена подключение к Интернету не требуется. Подключение к Интернету требуется только для компьютеров, на которых работает прокси-служба защиты паролем Azure AD.
* На контроллерах домена нет открытых сетевых портов.
* Изменения схемы Active Directory не требуются.
* Это программное обеспечение использует имеющийся контейнер Active Directory и объекты схемы serviceConnectionPoint.
* Минимальные требования домена Active Directory или функционального уровня леса (DFL\FFL) не установлены.
* Программное обеспечение не создает и не требует предоставления учетных записей в доменах Active Directory, которые оно защищает.
* Добавочное развертывание поддерживается с учетом такого компромисса: политика паролей применяется, только если установлен агент контроллера домена.
* Рекомендуется установить агент контроллера домена на всех контроллерах домена, чтобы обеспечить защиту паролем. 
* Защита паролем Azure AD не является подсистемой применения политик в режиме реального времени. Между изменением конфигурации политики пароля и его применением на всех контроллерах домена может возникнуть задержка.

## <a name="next-steps"></a>Дополнительная информация

[Развертывание защиты паролем Azure AD](howto-password-ban-bad-on-premises-deploy.md)
