---
title: Регистрация нового устройства Azure IoT Edge с помощью портала | Документация Майкрософт
description: Регистрация нового устройства IoT Edge с помощью портала Azure
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6657203c76bc03a262fbcbd30b5bf74b5be140eb
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2018
ms.locfileid: "51577504"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Регистрация нового устройства Azure IoT Edge на портале Azure

Чтобы использовать устройства Интернета вещей в Azure IoT Edge, нужно зарегистрировать их в Центре Интернета вещей. После регистрации устройства вы получите строку подключения, которую можно использовать при настройке устройства для рабочих нагрузок IoT Edge. 

В этой статье объясняется, как зарегистрировать новое устройство IoT Edge с помощью портала Azure.

## <a name="prerequisites"></a>Предварительные требования

* [Центр Интернета вещей](../iot-hub/iot-hub-create-through-portal.md) в подписке Azure. 

## <a name="create-a-device"></a>Создание устройства

На портале Azure устройства IoT Edge создаются и администрируются независимо от других устройств, подключенных к Центру Интернета вещей. 

1. Войдите на [портал Azure](https://portal.azure.com) и перейдите к своему Центру Интернета вещей. 
2. В меню выберите **IoT Edge**.
3. Выберите **Добавить устройство IoT Edge**. 
4. Укажите описательный идентификатор устройства. 
5. Щелкните **Сохранить**. 

## <a name="view-all-devices"></a>Просмотр данных обо всех устройствах

Все устройства IoT Edge, подключенные к Центру Интернета вещей, перечислены на странице **IoT Edge**. 

## <a name="retrieve-the-connection-string"></a>Получение строки подключения

Когда все будет готово к настройке устройства, вам понадобится строка подключения, которая связывает физическое устройство с его идентификатором в Центре Интернета вещей.

1. На странице **IoT Edge** на портале выберите идентификатор устройства в списке устройств IoT Edge. 
2. Скопируйте значение в поле **Строка подключения (первичный ключ)** или **Строка подключения (вторичный ключ)**. 

## <a name="next-steps"></a>Дополнительная информация

Подробнее о [развертывании модулей на устройстве с помощью портала Azure](how-to-deploy-modules-portal.md)
