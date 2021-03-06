---
title: 'Устранение сбоя службы Azure Backup: состояние гостевого агента "Недоступно"'
description: Симптомы, причины и способы устранения проблем в работе службы Azure Backup, связанных с агентом, расширением и дисками.
services: backup
author: genlin
manager: cshepard
keywords: Azure backup; VM agent; Network connectivity;
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: d8b78551a762b4388344aaf3b44e7472127737ae
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977120"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Устранение неполадок службы Azure Backup. Проблемы с агентом или расширением

В этой статье описано, как устранять ошибки службы Azure Backup, связанные с установлением связи с агентом виртуальной машины и расширением.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable — агенту виртуальной машины не удается установить связь со службой Azure Backup

**Код ошибки**: UserErrorGuestAgentStatusUnavailable. <br>
**Сообщение об ошибке**: "Агенту виртуальной машины не удается установить связь со службой Azure Backup".<br>

После регистрации виртуальной машины в службе Backup и добавления ее в расписание служба Backup инициирует задание, взаимодействуя с агентом виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:<br>
**Причина 1. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  .  
**Причина 2. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 3. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  .  
**Причина 4. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 5. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError — не удалось запросить состояние моментального снимка в агенте виртуальной машины

**Код ошибки**: GuestAgentSnapshotTaskStatusError.<br>
**Сообщение об ошибке**: "Не удалось запросить состояние моментального снимка в агенте виртуальной машины". <br>

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 2. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 3. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached — достигнуто максимальное количество точек восстановления в коллекции

**Код ошибки**: UserErrorRpCollectionLimitReached. <br>
**Сообщение об ошибке**: "Достигнуто максимальное количество точек восстановления в коллекции". <br>
* Эта ошибка может произойти, если в группе ресурсов точек восстановления есть блокировка, предотвращающая автоматическую очистку точки восстановления.
* Эта ошибка также может произойти при активации нескольких операций резервного копирования в день. Сейчас мы рекомендуем выполнять только одну операцию резервного копирования в день, так как мгновенные точки восстановления хранятся в течение 7 дней, а с виртуальной машиной можно связать только 18 мгновенных точек восстановления в любой момент времени. <br>

Рекомендуемое действие:<br>
Чтобы устранить эту проблему, удалите блокировку группы ресурсов и повторите операцию активации очистки.

> [!NOTE]
    > Служба Backup создает группу ресурсов, отличную от группы ресурсов виртуальной машины, для сохранения коллекции точек восстановления. Пользователям рекомендуется не блокировать группу ресурсов, созданную для использования службой Backup. Формат именования группы ресурсов, созданной службой Backup: AzureBackupRG_`<Geo>`_`<number>`. Например, AzureBackupRG_northeurope_1.

**Шаг 1. [Удаление блокировки с группы ресурсов точки восстановления](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Шаг 2. [Очистка коллекции точек восстановления](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured — служба архивации не имеет достаточных разрешений на доступ к хранилищу ключей для резервного копирования зашифрованных виртуальных машин.

**Код ошибки**: UserErrorKeyvaultPermissionsNotConfigured. <br>
**Сообщение об ошибке**: "Служба архивации не имеет достаточных разрешений на доступ к хранилищу ключей для резервного копирования зашифрованных виртуальных машин". <br>

Для выполнения операции резервного копирования зашифрованных виртуальных машин необходимы разрешения на доступ к хранилищу ключей. Это можно сделать с помощью [портала Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption#provide-permissions-to-backup) или [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork — сбой операции моментального снимка, отсутствует сетевое подключение у виртуальной машины

**Код ошибки**: ExtensionSnapshotFailedNoNetwork.<br>
**Сообщение об ошибке**: "Сбой операции моментального снимка, отсутствует сетевое подключение у виртуальной машины".<br>

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:    
**Причина 1. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 2. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 3. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailedForManagedDisks — операция расширения VMSnapshot завершилась сбоем.

**Код ошибки**: ExtentionOperationFailedForManagedDisks <br>
**Сообщение об ошибке**: "Сбой в ходе работы расширения VMSnapshot".<br>

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 2. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 3. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 4. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed или BackUpOperationFailedV2 — сбой резервного копирования с внутренней ошибкой

**Код ошибки**: BackUpOperationFailed или BackUpOperationFailedV2. <br>
**Сообщение об ошибке**: "Произошла внутренняя ошибка службы Backup. Повторите операцию через несколько минут". <br>

После регистрации виртуальной машины в службе архивации Azure и добавления ее в расписание служба архивации инициирует задание, взаимодействуя с расширением виртуальной машины, чтобы создать моментальный снимок. Любое из указанных ниже условий может помешать активации создания моментального снимка, что может привести к сбою службы Backup. Выполните следующие шаги про устранению неполадок в указанном порядке, а затем повторите операцию:  
**Причина 1. [Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**.  
**Причина 2. [Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**.  
**Причина 3. [Не удалось получить состояние моментального снимка или создать моментальный снимок](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**.  
**Причина 4. [Не удалось обновить или загрузить расширение резервного копирования](#the-backup-extension-fails-to-update-or-load)**.  
**Причина 5. [Служба архивации не имеет разрешения на удаление старых точек восстановления из-за блокировки группы ресурсов](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**. <br>
**Причина 6. [Виртуальная машина не подключена к Интернету](#the-vm-has-no-internet-access)**.

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-1023gb"></a>UserErrorUnsupportedDiskSize. Сейчас служба Azure Backup не поддерживает размер диска больше 1023 ГБ

**Код ошибки**. UserErrorUnsupportedDiskSize <br>
**Сообщение об ошибке**. Сейчас служба Azure Backup не поддерживает размер диска больше 1023 ГБ <br>

Операция резервного копирования виртуальной машины с размером диска более 1023 ГБ может завершиться ошибкой, поскольку хранилище не обновлено до стека резервного копирования виртуальных машин Azure версии 2. После обновления до стека резервного копирования виртуальных машин Azure версии 2 вы сможете использовать диски размером до 4 TБ. Ознакомьтесь со следующими [преимуществами](backup-upgrade-to-vm-backup-stack-v2.md), [рекомендациями](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade), а затем перейдите к обновлению, выполнив следующие [инструкции](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  

## <a name="usererrorstandardssdnotsupported---currently-azure-backup-does-not-support-standard-ssd-disks"></a>UserErrorStandardSSDNotSupported. В настоящее время Azure Backup не поддерживает диски SSD категории "Стандартный"

**Код ошибки**. UserErrorStandardSSDNotSupported <br>
**Сообщение об ошибке**. В настоящее время Azure Backup не поддерживает диски SSD категории "Стандартный" <br>

Служба Azure Backup сейчас поддерживает диски SSD категории "Стандартный" только для хранилищ, обновленных до стека резервного копирования виртуальных машин Azure версии 2. Ознакомьтесь со следующими [преимуществами](backup-upgrade-to-vm-backup-stack-v2.md), [рекомендациями](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade), а затем перейдите к обновлению, выполнив следующие [инструкции](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).


## <a name="causes-and-solutions"></a>Причины и решения

### <a name="the-vm-has-no-internet-access"></a>Виртуальная машина не подключена к Интернету
Согласно требованиям к развертыванию виртуальная машина не имеет доступа к Интернету. Или могут быть ограничения на доступ к инфраструктуре Azure.

Чтобы расширение службы Backup работало правильно, требуется подключение к общедоступным IP-адресам Azure. Для управления моментальными снимками виртуальной машины это расширение отправляет команды к конечной точке службы хранилища Azure (URL-адрес HTTP). Если расширение не имеет доступа к общедоступному Интернету, резервное копирование завершится сбоем.

Чтобы маршрутизировать трафик виртуальных машин, можно развернуть прокси-сервер.
##### <a name="create-a-path-for-https-traffic"></a>Создание пути для трафика HTTP

1. При наличии каких-либо ограничений сети (например, группы безопасности сети) разверните прокси-сервер HTTP для маршрутизации трафика.
2. Если вы используете группу безопасности сети, добавьте в нее правила, чтобы разрешить доступ к Интернету с прокси-сервера HTTP.

Чтобы узнать, как настроить прокси-сервер HTTP для резервного копирования виртуальных машин, ознакомьтесь с разделом [Подготовка среды для резервного копирования виртуальных машин Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Архивной виртуальной машине или прокси-серверу, через которые маршрутизируется трафик, требуется доступ к общедоступным IP-адресам Azure.

####  <a name="solution"></a>Решение
Для устранения этой проблемы воспользуйтесь одним из указанных ниже способов.

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Разрешение доступа к хранилищу Azure, которое соответствует региону

Для разрешения подключений к хранилищу из определенного региона можно использовать [теги службы](../virtual-network/security-overview.md#service-tags). Убедитесь, что правило, которое позволяет получить доступ к учетной записи хранения, имеет более высокий приоритет, чем правило, которое блокирует доступ к Интернету.

![Группа безопасности сети с тегами хранилища для региона](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Пошаговые инструкции по настройке тегов службы см. в [этом видео](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Теги службы хранилища находятся на этапе предварительной версии. Они доступны только в определенных регионах. Список регионов см. в разделе [Теги служб](../virtual-network/security-overview.md#service-tags).

Если вы используете "Управляемые диски Azure", может потребоваться открыть в брандмауэрах дополнительный порт (8443).

Кроме того, если в подсети отсутствует маршрут для исходящего интернет-трафика, необходимо добавить конечную точку службы с тегом службы "Microsoft.Storage" в подсеть.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Агент установлен на виртуальной машине, но не отвечает (для виртуальных машин Windows)

#### <a name="solution"></a>Решение
Возможно, агент виртуальной машины может быть поврежден или служба остановлена. Повторная установка агента виртуальной машины поможет получить последнюю версию и возобновить обмен данными со службой.

1. Определите, запущена ли служба гостевого агента Windows в службах компьютера (services.msc) на виртуальной машине. Попробуйте перезапустить службу гостевого агента Windows и запустите резервное копирование.    
2. Если она не отображается в списке служб, на панели управления выберите **Программы и компоненты**, чтобы определить, установлена ли служба гостевого агента Windows.
4. Если гостевой агент Windows отображается в разделе **Программы и компоненты**, удалите его.
5. Скачайте и установите [последнюю версию файла MSI агента](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Чтобы выполнить установку, необходимо иметь права администратора.
6. Убедитесь, что службы гостевого агента Windows отображаются в списке служб.
7. Выполните резервное копирование по запросу:
    * На портале выберите **Моментальная архивация**.

Кроме того, убедитесь, что [Microsoft .NET 4.5 установлен](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) на виртуальной машине. Компонент .NET 4.5 необходим для взаимодействия агента виртуальной машины со службой.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Устарел агент, установленный на виртуальной машине (для виртуальных машин Linux)

#### <a name="solution"></a>Решение
Большая часть неполадок, связанных с агентом или расширением на виртуальных машинах Linux, вызваны проблемами с устаревшим агентом виртуальной машины. Чтобы устранить эту проблему, следуйте приведенным ниже общим рекомендациям.

1. Выполните указания по [обновлению агента виртуальной машины Linux ](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Мы *настоятельно рекомендуем* обновлять агент только посредством репозитория дистрибутивов. Мы не рекомендуем скачивать код агента непосредственно с портала GitHub и обновлять его. Если последняя версия агента для вашего дистрибутива недоступна, обратитесь в службу поддержки дистрибутива, чтобы получить инструкции по установке последней версии агента. Чтобы проверить наличие последней версии агента, перейдите на страницу [агента Linux для Microsoft Azure](https://github.com/Azure/WALinuxAgent/releases) в репозитории GitHub.

2. Убедитесь, что на виртуальной машине запущен агент Azure, выполнив команду `ps -e`.

 Если нужный процесс не запущен, выполните следующие команды для его перезапуска:

 * Для Ubuntu: `service walinuxagent start`.
 * Для других дистрибутивов: `service waagent start`.

3. [Настройте автоматический перезапуск агента](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Снова запустите резервное копирование для проверки. Если ошибка не исчезла, извлеките следующие журналы из виртуальной машины:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Если нам потребуется подробное ведение журнала для waagent, выполните следующие действия.

1. В файле /etc/waagent.conf найдите такую строку: **Enable verbose logging (y|n)**.
2. Для параметра **Logs.Verbose** измените значение с *n* на *y*.
3. Сохраните изменения и перезапустите waagent, выполнив шаги, описанные выше в этом разделе.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Не удалось получить состояние моментального снимка или создать моментальный снимок
Архивация виртуальных машин зависит от команды создания моментального снимка в базовой учетной записи хранения. Сбой службы Backup может произойти, если она не имеет доступа к учетной записи хранения или выполнение задачи создания моментального снимка задерживается.

#### <a name="solution"></a>Решение
Сбой задачи создания снимка может быть вызван следующими условиями.

| Причина: | Решение |
| --- | --- |
| Состояние виртуальной машины отображается неправильно из-за того, что ее работа была завершена в протоколе удаленного рабочего стола (RDP). | Когда вы завершаете работу виртуальной машины в RDP, проверьте, правильно ли отображается состояние виртуальной машины на портале. Если это не так, завершите работу виртуальной машины на портале с помощью операции **Завершение работы** на панели мониторинга виртуальной машины. |
| Виртуальная машина не может получить адрес узла или структуры по протоколу DHCP. | Чтобы архивация виртуальной машины IaaS работала, в гостевой учетной записи нужно включить протокол DHCP. Если виртуальная машина не может получить адрес узла или структуры в виде ответа DHCP 245, то она не сможет скачать или запустить какие-либо расширения. Если вам нужен статический частный IP-адрес, настройте его с помощью **портала Azure** или **PowerShell** и убедитесь, что DHCP для виртуальной машины включен. Дополнительные сведения по настройке статического IP-адреса с помощью PowerShell см. в разделах [Добавление статического внутреннего IP-адреса для существующей виртуальной машины](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm) и [Изменение метода распределения для частного IP-адреса, назначенного сетевому интерфейсу](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface).

### <a name="the-backup-extension-fails-to-update-or-load"></a>Не удалось обновить или загрузить расширение резервного копирования
Если не удается загрузить расширения, получить моментальный снимок состояния невозможно, и архивация завершится сбоем.

#### <a name="solution"></a>Решение

Удалите расширение, чтобы принудительно перезагрузить расширение VMSnapshot. Расширение перезагрузится при следующей попытке резервного копирования.

Чтобы удалить расширение, сделайте следующее:

1. На [портале Azure](https://portal.azure.com/) перейдите к виртуальной машине, с архивацией которой возникли проблемы.
2. Выберите элемент **Параметры**.
3. Выберите **Расширения**.
4. Выберите **Vmsnapshot Extension** (Расширение Vmsnapshot).
5. Выберите **Удалить**.

Если расширение VMSnapshot для виртуальной машины Linux не отображается на портале Azure, [обновите агент Azure для Linux](../virtual-machines/linux/update-agent.md), а затем выполните резервное копирование.

После этого расширение будет повторно установлено при следующей архивации.

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Удаление блокировки с группы ресурсов точки восстановления
1. Войдите на [портале Azure](http://portal.azure.com/).
2. Перейдите к параметру **Все ресурсы**, выберите группу ресурсов коллекции точек восстановления в следующем формате: AzureBackupRG_`<Geo>`_`<number>`.
3. В разделе **Параметры** выберите **Блокировки**, чтобы отобразить блокировки.
4. Чтобы удалить блокировку, щелкните многоточие и нажмите кнопку **Удалить**.

    ![Удаление блокировки ](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Очистка коллекции точек восстановления
После удаления блокировки точки восстановления нужно очистить. Чтобы очистить точки восстановления, воспользуйтесь любым из этих методов:<br>
* [Очистка коллекции точек восстановления путем выполнения нерегламентированного резервного копирования](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Очистка коллекции точек восстановления на портале Azure](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Очистка коллекции точек восстановления путем выполнения нерегламентированного резервного копирования
После удаления блокировки активируйте нерегламентированное резервное копирование вручную. Это обеспечит автоматическую очистку точек восстановления. Ожидается, что эта нерегламентированная операция вручную сначала даст сбой. Тем не менее она обеспечит автоматическую очистку, и вам не придется удалять точки восстановления вручную. После очистки следующее запланированное резервное копирование должно быть выполнено успешно.

> [!NOTE]
    > Автоматическая очистка произойдет через несколько часов после активации нерегламентированного резервного копирования вручную. Если запланированное резервное копирование по-прежнему дает сбой, попробуйте вручную удалить коллекцию точек восстановления, выполнив действия, перечисленные [здесь](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal">Очистка коллекции точек восстановления на портале Azure</a> <br>

Чтобы вручную очистить коллекцию точек восстановления, которая не очищается из-за блокировки группы ресурсов, попробуйте сделать следующее.
1. Войдите на [портале Azure](http://portal.azure.com/).
2. В меню **Концентратор** щелкните **Все ресурсы**, выберите группу ресурсов в формате AzureBackupRG_`<Geo>`_`<number>`, в которой находится ваша виртуальная машина.

    ![Удаление блокировки ](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Щелкните группу ресурсов, после чего отобразится колонка **Обзор**.
4. Выберите параметр **Показать скрытые типы**, чтобы отобразить все скрытые ресурсы. Выберите коллекции точек восстановления в следующем формате: AzureBackupRG_`<VMName>`_`<number>`.

    ![Удаление блокировки ](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Щелкните **Удалить**, чтобы очистить коллекцию точек восстановления.
6. Повторите операцию резервного копирования.
