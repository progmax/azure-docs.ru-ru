---
title: Предложение образа контейнеров Azure | Документация Майкрософт
description: Общие сведения о процессе публикации предложения контейнера в Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: pbutlerm
ms.openlocfilehash: e40e83e16ab2bfd43c3bb5fa38e52a778694e90e
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980427"
---
# <a name="containers"></a>Контейнеры

<table> <tr> <td>В этом разделе объясняется, как опубликовать образ контейнера в <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>.  
Тип предложения контейнера поддерживает образы контейнера Docker, подготовленные в качестве экземпляров <a href="https://docs.microsoft.com/azure/aks/index">Azure Kubernetes Service</a> или <a href="https://docs.microsoft.com/azure/container-instances/container-instances-overview">экземпляров контейнера Azure</a> и размещенные в репозитории <a href="https://docs.microsoft.com/azure/container-registry">Azure Container Registry</a>. </td> <td><img src="./media/container-icon.png"  alt="Azure container icon" /></td> </tr> </table>

## <a name="offer-components"></a>Компоненты предложения

В этом разделе описываются элементы процесса публикации контейнера. Он является руководством для издателя Azure Marketplace. Публикация состоит из следующих основных частей:

- [Предварительные требования.](./cpp-prerequisites.md) Перечислены технические и бизнес-требования, которые должны быть выполнены перед созданием или публикацией предложения контейнера.
- [Создание предложения.](./cpp-create-offer.md) Перечислены шаги по созданию записи предложения контейнера с использованием Портала Cloud Partner.
- [Подготовка технических средств.](./cpp-create-technical-assets.md) Создание технических средств для решения контейнера в качестве предложения в Azure Marketplace.
- [Публикация предложения контейнера.](./cpp-publish-offer.md) Отправка предложения для публикации в Azure Marketplace.

## <a name="container-publishing-process"></a>Процесс публикации контейнера

На следующей схеме показаны основные действия публикации предложения виртуальной машины.
![Шаги публикации предложения](./media/containers-offer-process.png)

Ниже перечислены основные этапы для публикации предложения контейнера.

1. Создание предложения. Укажите подробные сведения о предложении. К этим сведениям относятся: описание предложения, маркетинговые материалы, сведения о поддержке и характеристики ресурса.
2. Создание бизнес- и технических ресурсов. Создайте бизнес-ресурсы (юридические документы и маркетинговые материалы) и технические ресурсы для связанного решения (образы контейнеров, размещенные в Реестре контейнеров Azure).
3. Создание номера SKU. Создайте номера SKU, связанные с предложением. Уникальный номер SKU нужен для каждого публикуемого образа.
4. Сертификация и публикация предложения. Оформив предложение и выполнив технические требования, вы можете отправить предложение на публикацию. В ходе отправки начинается процесс публикации. Во время этого процесса решение тестируется, проверяется, сертифицируется и активируется в Azure Marketplace.

## <a name="next-steps"></a>Дополнительная информация

Прежде чем следовать этим действиям, необходимо выполнить [технические и бизнес-требования](./cpp-prerequisites.md) для публикации контейнера в Microsoft Azure Marketplace.