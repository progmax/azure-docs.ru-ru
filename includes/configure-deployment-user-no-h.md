---
title: включение файла
description: включение файла
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 95d89da66935ce933fee082a5f53ee2e36ea953f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344692"
---
В Cloud Shell настройте учетные данные развертывания с помощью команды [`az webapp deployment user set`](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set). Пользователь развертывания необходим для развертывания FTP и локального репозитория Git в веб-приложении. Имя пользователя и пароль представляют уровень учетной записи. _Они отличаются от учетных данных подписки Azure._

В следующем примере замените *\<имя_пользователя>* и *\<пароль>* (включая скобки) новым именем пользователя и паролем. Имя пользователя должно быть уникальным в пределах Azure. Пароль должен содержать не менее восьми символов и включать два из трех следующих элементов: буквы, цифры и символы. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Необходимо получить выходные данные JSON с паролем, обозначенным как `null`. Если появляется сообщение об ошибке `'Conflict'. Details: 409`, измените имя пользователя. Если появляется сообщение об ошибке ` 'Bad Request'. Details: 400`, используйте более надежный пароль.

Пользователь развертывания настраивается один раз. Его можно использовать для всех развертываний Azure.

> [!NOTE]
> Запишите имя пользователя и пароль. В дальнейшем они понадобятся для развертывания веб-приложения.
>
>
