---
title: Отключение проверки адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C | Документация Майкрософт
description: В этой статье демонстрируется, как отключить проверку адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004606"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Отключение проверки адреса электронной почты во время регистрации потребителя в Azure Active Directory B2C 
Когда этот параметр включен, служба Azure Active Directory (Azure AD) B2C предоставляет потребителю возможность зарегистрироваться для получения доступа к приложениям, предоставив адрес электронной почты и создав локальную учетную запись. Чтобы убедиться, что адреса электронной почты являются действительными, Azure AD B2C требует от потребителей выполнить их проверку в процессе регистрации. Также предотвращаются вредоносные автоматизированные процессы, создающие для приложений фальшивые учетные записи.

Некоторые разработчики приложений предпочитают пропустить проверку адреса электронной почты в процессе регистрации. При таком сценарии потребители должны будут проверить адрес позже. Для этого в Azure AD B2C можно отключить проверку адреса электронной почты. Это позволяет упростить процедуру регистрации, а также дает разработчикам возможность разделить потребителей на прошедших проверку адреса и на тех, кто такую проверку еще не прошел.

По умолчанию в политиках регистрации определено, что проверка адреса включена. Чтобы ее выключить, следуйте приведенным ниже инструкциям.

1. В зависимости от своих настроек регистрации выберите **Sign-up policies** (Политики регистрации) или **Sign-up or sign-in policies** (Политики регистрации или входа).
2. Откройте политику (например B2C_1_SiUp), щелкнув ее. 
3. Щелкните **Изменить** в верхней части колонки.
4. Щелкните **Page UI Customization** (Настройка пользовательского интерфейса страницы).
5. Щелкните **страницу регистрации локальной учетной записи**.
6. В столбце **Name** (Имя) в разделе **Sign-up attributes** (Атрибуты регистрации) щелкните **Email Address** (Адрес электронной почты).
7. Переключите параметр **Require verification** (Требовать проверку) в положение **No** (Нет).
8. Несколько раз нажмите кнопку **ОК** внизу, пока не откроется колонка **Изменение политики**.
9. Щелкните **Сохранить** в верхней части колонки. Готово!

> [!NOTE]
> Отключение проверки адреса электронной почты в процессе регистрации может привести к получению спама. Если вы отключаете эту проверку по умолчанию, то рекомендуется добавить собственную систему проверки.
> 
>