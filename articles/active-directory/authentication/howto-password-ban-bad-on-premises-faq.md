---
title: Вопросы и ответы о локальной службе защите паролем Azure AD
description: Вопросы и ответы о локальной службе защите паролем Azure AD.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: d3d42a3c81153d54690f0825368eaa950dbac18e
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314785"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Часто задаваемые вопросы о локальной службе защиты паролем Azure AD (предварительная версия)

|     |
| --- |
| Защита паролем Azure AD — это общедоступная предварительная версия функции Azure Active Directory. См. дополнительные сведения о [дополнительных условиях использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="general-questions"></a>Общие вопросы

**Вопрос. Когда будет выпущена общедоступная версия защиты паролем Azure AD?**

Мы еще не объявили дату выпуска общедоступной версии.

**Вопрос. Поддерживается ли локальная служба защиты паролем Azure AD в облаках, не являющихся общедоступными?**

Нет. Локальная служба защиты паролем Azure AD поддерживается только в общедоступных облаках.

**Вопрос. Как можно применить преимущества защиты паролем Azure AD к подмножеству локальных пользователей?**

Не поддерживается. После развертывания и включения служба защиты паролем Azure AD не разделяет пользователей — все пользователи получают одинаковые преимущества безопасности.

**Вопрос. Поддерживается ли установка защиты паролем Azure AD параллельно с другими продуктами для фильтрации по паролю?**

Да. Поддержка нескольких зарегистрированных библиотек DLL для фильтрации по паролю является основной функцией Windows и не относится к защите паролем Azure AD. Все зарегистрированные библиотеки DLL для фильтрации по паролю должны подтвердить пароль, прежде чем он будет принят.

**Вопрос. Почему для репликации SYSVOL требуется DFSR?**

Технология FRS (предшествующая технологии DFSR) имеет множество известных проблем и полностью не поддерживается в новых версиях Windows Server Active Directory. Ноль-тестирование защиты паролем Azure AD будет выполняться на доменах, для которых настроена FRS.

Дополнительные сведения приведены в следующих статьях:

[The Case for Migrating sysvol replication to DFSR](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr) (Вариант переноса репликации SYSVOL в DFSR)

[The End is Nigh for FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs) (Близится конец FRS)

**Вопрос. Почему для установки или обновления программного обеспечения агента контроллера домена требуется перезагрузка?**

Это требование связано с ключевым механизмом Windows.

**В. Имеется ли способ настроить агент контроллера домена для использования конкретного прокси-сервера?**

 Нет.

## <a name="next-steps"></a>Дополнительная информация

Если у вас есть вопрос о локальной службе защиты паролем Azure AD, которого нет в этой статье, ниже вы можете отправить нам отзыв. Спасибо!

[Развертывание защиты паролем Azure AD](howto-password-ban-bad-on-premises-deploy.md)
