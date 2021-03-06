---
title: Делегирование подсети в Azure NetApp Files | Документация Майкрософт
description: Сведения о том, как делегировать подсеть в Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/13/2018
ms.author: b-juche
ms.openlocfilehash: fc8ec5c3bbe0157f5737e2eb757478357aa6e867
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2018
ms.locfileid: "53414307"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Делегирование подсети в Azure NetApp Files 

Вам следует делегировать подсеть в Azure NetApp Files.   При создании тома необходимо указать делегированную подсеть.

## <a name="about-this-task"></a>Сведения об этой задаче
* Мастер создания новой подсети по умолчанию настраивает маску сети /24, обеспечивающую 251 доступный IP-адрес. Использование этой маски подсети, которая предоставляет 16 пригодных IP-адресов, достаточно для службы.
* В делегированной подсети невозможно назначить группу безопасности сети или конечную точку службы. Попытка к сбою делегирования подсети.
* В каждой виртуальной сети Azure можно делегировать только одну подсеть для Azure NetApp Files.
* Доступ к тому из пиринговой виртуальной сети в настоящее время не поддерживается.

## <a name="steps"></a>Действия 
1.  Перейдите к колонке **Виртуальные сети** на портале Azure и выберите виртуальную сеть, которую нужно использовать для Azure NetApp Files.    

1. Выберите **Подсети** из колонки виртуальной сети и нажмите кнопку **+Подсеть**. 

1. Создайте новую подсеть для использования в Azure NetApp Files, заполнив следующие обязательные поля на странице добавления подсети:
    * **Имя**. Укажите имя подсети.
    * **Диапазон адресов**. Укажите диапазон IP-адресов.
    * **Делегирование подсети**. Выберите **Microsoft.NetApp/volumes**. 

      ![Делегирование подсети](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Можно также создать и делегировать подсеть при [создании тома для Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Дополнительная информация  
* [Создание тома для Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Узнайте об интеграции виртуальной сети для служб Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)


