---
title: Сведения об использовании ExpressRoute и Azure Site Recovery для аварийного восстановления и миграции | Документация Майкрософт
description: Сведения об использовании Azure ExpressRoute и службы Azure Site Recovery для аварийного восстановления и миграции.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: 14d74c879ee37306f541c89d2ba325109a4ff7f3
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214874"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Использование Azure ExpressRoute и Azure Site Recovery

Microsoft Azure ExpressRoute позволяет переносить локальные сети в облако Microsoft по частному подключению, которое обеспечивается поставщиком услуг подключения. ExpressRoute позволяет устанавливать подключения к облачным службам Майкрософт, таким как Microsoft Azure, Office 365 и Dynamics 365.

Эта статья содержит сведения об использовании ExpressRoute и Azure Site Recovery для аварийного восстановления и миграции.

## <a name="expressroute-circuits"></a>Каналы ExpressRoute

Канал ExpressRoute представляет собой логическое подключение вашей локальной сети к облачным службам Майкрософт через поставщика услуг подключения. Таких каналов ExpressRoute может быть несколько. Каждый канал может находиться в том же или в других регионах и подключаться к среде через различных поставщиков услуг связи. Дополнительные сведения о каналах ExpressRoute см. [здесь](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>Домены маршрутизации ExpressRoute

С каналом ExpressRoute связано несколько доменов маршрутизации:
-   [Частный пиринг Azure](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). Службы вычислений Azure, а именно виртуальные машины (IaaS) и облачные службы (PaaS), развернутые в виртуальной сети, могут подключаться через домен частного пиринга. Домен частного пиринга считается доверенным расширением вашей основной сети в Microsoft Azure.
-   [Общедоступный пиринг Azure](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Такие службы, как служба хранилища Azure, базы данных SQL и веб-сайты, предлагаются по общедоступным IP-адресам. Вы можете частным образом подключаться к службам, размещенным в общедоступных IP-адресах (включая виртуальные IP-адреса ваших облачных служб) через домен маршрутизации общедоступного пиринга. Общедоступный пиринг признан нерекомендуемым для вновь создаваемых объектов, и следует использовать пиринг Майкрософт вместо служб Azure PaaS.
-   [Пиринг Майкрософт](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Подключение к веб-службам Майкрософт (таким как Office 365, Dynamics 365 и Azure PaaS) осуществляется через пиринг Майкрософт. Пиринг Майкрософт является рекомендуемым доменом маршрутизации для подключения к службам Azure PaaS.

Дополнительные сведения о доменах маршрутизации ExpressRoute и их сравнение см. [здесь](../expressroute/expressroute-circuit-peerings.md#routing-domain-comparison).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>Репликация из локальной среды в Azure с помощью ExpressRoute

Azure Site Recovery позволяет осуществлять аварийное восстановление и миграцию в Azure для локальных [виртуальных машин Hyper-V](hyper-v-azure-architecture.md), [виртуальных машин VMware](vmware-azure-architecture.md) и [физических серверов](physical-azure-architecture.md). Для всех сценариев репликации из локальной среды в Azure соответствующие данные отправляются и сохраняются в учетной записи хранения Azure. Во время репликации вам не требуется вносить плату за виртуальные машины. При выполнении отработки отказа в Azure служба Site Recovery автоматически создаст виртуальные машины IaaS Azure.

Site Recovery реплицирует данные в учетную запись хранения Azure через общедоступную конечную точку. Чтобы использовать ExpressRoute для репликации Site Recovery, можно воспользоваться [общедоступным пирингом](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) или [пирингом Майкрософт](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Пиринг Майкрософт является рекомендуемым доменом маршрутизации для репликации. После отработки отказа виртуальных машин или серверов в виртуальную сеть Azure к ним можно получить доступ с помощью [частного пиринга](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). Репликация через частный пиринг не поддерживается.

На следующей схеме показан комбинированный сценарий: ![репликация из локальной среды в Azure с помощью ExpressRoute](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>Репликация из Azure в Azure с помощью ExpressRoute

Azure Site Recovery позволяет выполнять аварийное восстановление [виртуальных машин Azure](azure-to-azure-architecture.md). В зависимости от того, используют ли виртуальные машины Azure [управляемые диски Azure](../virtual-machines/windows/managed-disks-overview.md), данные репликации отправляются в учетную запись хранения Azure или на реплику управляемого диска в целевом регионе Azure. Конечные точки репликации являются открытыми, но трафик репликации для виртуальных машин Azure по умолчанию не выходит в Интернет, независимо от того, в каком регионе Azure находится исходная виртуальная сеть. Вы можете переопределить маршрут системы Azure по умолчанию для префикса адреса 0.0.0.0/0, используя [настраиваемый маршрут](../virtual-network/virtual-networks-udr-overview.md#custom-routes), и перенаправить трафик виртуальных машин на локальный сетевой виртуальный модуль (NVA), но такая конфигурация не рекомендуется для репликации Site Recovery. При использовании настраиваемых маршрутов рекомендуется создать в виртуальной сети [конечную точку службы виртуальной сети](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) для хранилища, чтобы трафик репликации не покидал границ Azure.

По умолчанию ExpressRoute для репликации при аварийном восстановлении виртуальной машины Azure не требуется. После отработки отказа виртуальных машин в целевой регион Azure вы можете обращаться к ним через [частный пиринг](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

Если вы уже используете ExpressRoute для подключения из локального центра обработки данных к виртуальным машинам Azure в исходном регионе, можно запланировать переподключение ExpressRoute в целевом регионе отработки отказа. Вы можете использовать тот же канал ExpressRoute для подключения к целевому региону через новое подключение к виртуальной сети или использовать отдельный канал ExpressRoute и подключение для аварийного восстановления. Различные возможные сценарии описаны [здесь](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute).

Репликацию виртуальных машин Azure можно выполнить в любой регион Azure, входящий в тот же географический кластер, как описано [здесь](../site-recovery/azure-to-azure-support-matrix.md#region-support). Если выбранный целевой регион Azure находится не в том же геополитическом регионе, что и исходный регион, вам потребуется ExpressRoute ценовой категории "Премиум". Дополнительные сведения см. в разделе [Расположения ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) и на странице [цен на ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Дополнительная информация
- Дополнительные сведения о [каналах ExpressRoute](../expressroute/expressroute-circuit-peerings.md).
- Дополнительные сведения о [доменах маршрутизации ExpressRoute](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Дополнительные сведения о [расположениях ExpressRoute](../expressroute/expressroute-locations.md).
- Дополнительные сведения об аварийном восстановлении [виртуальных машин Azure с помощью ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).
