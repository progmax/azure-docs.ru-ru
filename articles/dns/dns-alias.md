---
title: Обзор записей псевдонимов Azure DNS
description: Общие сведения о поддержке записей псевдонимов в Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52b42e964e7abe207064aff49f7f8f27f8476ef4
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092848"
---
# <a name="azure-dns-alias-records-overview"></a>Обзор записей псевдонимов Azure DNS

Записи псевдонимов Azure DNS являются квалификациями на набор записей DNS. Они позволяют ссылаться на другие ресурсы Azure из вашей зоны DNS. Например, вы можете создать набор записей псевдонимов, который ссылается на общедоступный IP-адрес Azure вместо записи A. Псевдоним набора записей динамически указывает на экземпляр сервиса общедоступного IP-адреса Azure. В результате псевдоним набора записей просто обновляется во время разрешения DNS.

Набор записей псевдонимов поддерживается для следующих типов записей в зоне DNS Azure: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Записи псевдонимов для типов записей A или AAAA для диспетчера трафика Microsoft Azure поддерживаются только для типов внешней конечной точки. Необходимо указать IPv4 или IPv6-адрес для внешних конечных точек в диспетчере трафика. В идеале используйте статические IP-адреса.

## <a name="capabilities"></a>Возможности

- **Выберите ресурс общедоступного IP-адреса из набора DNS-записей A/AAAA**. Можно создать набор записей A/AAAA и сделать его набором записей псевдонимов, указывающим на ресурс общедоступного IP-адреса.

- **Выберите профиль диспетчера трафика из набора DNS-записей A/AAAA/CNAME**. Можно выбрать CNAME профиля диспетчера трафика из набора DNS-записей CNAME. Например, contoso.trafficmanager.net. Теперь также можно выбрать профиль диспетчера трафика, который имеет внешние конечные точки из наборов записей A или AAAA в зоне DNS.

   > [!NOTE]
   > Записи псевдонимов для типов записей A или AAAA для диспетчера трафика поддерживаются только для типов внешней конечной точки. Необходимо указать IPv4 или IPv6-адрес для внешних конечных точек в диспетчере трафика. В идеале используйте статические IP-адреса.
   
- **Выберите другой набор DNS-записей в пределах той же зоны**. Записи псевдонимов могут ссылаться на другие наборы записей одного типа. Например, набор записей DNS CNAME может быть псевдонимом для другого набора записей CNAME того же типа. Такой подход полезен в том случае, если вы хотите, чтобы некоторые наборы записей были псевдонимами, а некоторые ими не были.

## <a name="scenarios"></a>Сценарии
Существует несколько распространенных сценариев для записей псевдонимов.

### <a name="prevent-dangling-dns-records"></a>Предотвратить несвязанные DNS-записи
 В зонах Azure DNS записи псевдонимов можно использовать для плотного отслеживания жизненного цикла ресурсов Azure. Ресурсы включают общедоступный IP-адрес или профиль диспетчера трафика. Одной из распространенных проблем с традиционными DNS-записями являются "несвязанные записи". Эта проблема возникает особенно часто с типами записей A/AAAA или CNAME. 

В традиционной записи зоны DNS, если целевой IP-адрес или CNAME больше не существуют, запись зоны DNS не знает об этом. В результате запись необходимо обновить вручную. В некоторых организациях ручное обновление может не произойти вовремя. Оно также может быть проблематичным из-за разделения ролей и соответствующих уровней доступа.

Например, роль, имеющая права на удаление записи CNAME или IP-адрес приложения. Но она не имеет прав на обновление записи DNS, указывающей на эти целевые объекты. В результате могут произойти задержки между моментом удаления IP-адреса или CNAME и записи DNS, которая указывает на нее. Эта задержка может потенциально привести сбой.

Записи псевдонимов устраняют сложности, связанные с этим сценарием. Они помогают предотвратить появление несвязанных ссылок. Возьмем, например, запись DNS, которая является псевдонимом и указывает на общедоступный IP-адрес или профиль диспетчера трафика. Если эти ресурсы удаляются, то запись псевдонима DNS удаляется одновременно с ними. Это гарантирует, что пользователи никогда не пострадают от сбоя.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Автоматическое обновление зоны DNS при изменении IP-адреса приложения

Этот сценарий похож на предыдущий. Возможно, приложение перемещается, или основная виртуальная машина перезапускается. При изменении IP-адреса для основного общедоступного IP-ресурса запись псевдонима обновляется. Чтобы избежать потенциальных проблем безопасности, направляйте пользователей к другому приложению со старым IP-адресом.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Размещайте приложения с балансировкой нагрузки в апексе зоны

Протокол DNS запрещает назначение записей, отличных от A или AAAA, в апексе зоны. Например, contoso.com. Это ограничение создает проблемы владельцам приложений, которые размещают приложения с балансировкой нагрузки за диспетчером трафика. Ссылаться на профиль диспетчера трафика из записи апекса зоны невозможно. В результате владельцам приложений необходимо использовать обходной путь. Перенаправление на прикладном уровне должно осуществляться из апекса зоны в другой домен. Например, перенаправление из contoso.com в www.contoso.com. Это упорядочение дает точку сбоя для функции переадресации.

С помощью записей псевдонимов эта проблема устранена. Теперь владельцы приложений могут направить свои записи в апексе зоны на профиль диспетчера трафика, который имеет внешние конечные точки. Владельцы приложений могут указывать на тот же профиль диспетчера трафика, который используется для любого другого домена в их зоне DNS. Например, contoso.com и www.contoso.com могут оба указывать на один и тот же профиль диспетчера трафика. Это происходит до тех пор, пока профиль диспетчера трафика будет содержать только настроенные внешние конечные точки.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о записях псевдонима см. в следующих статьях:

- [Руководство по настройке записи псевдонима для ссылки на общедоступный IP-адрес Azure](tutorial-alias-pip.md)
- [Руководство. Настройка записи псевдонима для поддержки вершинных доменных имен с помощью диспетчера трафика](tutorial-alias-tm.md)
- [DNS: часто задаваемые вопросы](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
