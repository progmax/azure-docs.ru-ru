---
title: 'Портал Azure: создание Управляемого экземпляра SQL | Документация Майкрософт'
description: Создание Управляемого экземпляра SQL, сетевой среды и виртуальной машины клиента для доступа.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: 4b8c67cfff89b54b4776ebc8b4586cd8f52950b3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342618"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Краткое руководство. Создание Управляемого экземпляра Базы данных SQL Azure

В этом кратком руководстве описано создание [Управляемого экземпляра](sql-database-managed-instance.md) Базы данных SQL Azure на портале Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Создание управляемого экземпляра

Ниже показано, как создать Управляемый экземпляр.

1. Выберите **Создать ресурс** в верхнем левом углу окна портала Azure.
2. Найдите **Управляемый экземпляр**, а затем выберите **Управляемый экземпляр SQL Azure** .
3. Нажмите кнопку **Создать**.

   ![Создание управляемого экземпляра](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Укажите запрошенные сведения в форме **Управляемого экземпляра**, используя данные в таблице ниже.

   | Параметр| Рекомендуемое значение | Описание |
   | ------ | --------------- | ----------- |
   | **Подписка** | Ваша подписка | Подписка, в которой есть разрешение на создание ресурсов |
   |**Имя управляемого экземпляра**|Любое допустимое имя|Сведения о допустимых именах см. в статье [Соглашения об именовании](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Имя для входа администратора управляемого экземпляра**|Любое допустимое имя пользователя|Сведения о допустимых именах см. в статье [Соглашения об именовании](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Не используйте serveradmin. Это имя зарезервировано для роли уровня сервера.|
   |**Пароль**|Любой допустимый пароль|Пароль должен включать минимум 16 символов и соответствовать [определенным требованиям к сложности](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Местоположение.**|Расположение, в котором хотите создать Управляемый экземпляр|Дополнительные сведения о регионах Azure см. [здесь](https://azure.microsoft.com/regions/).|
   |**Виртуальная сеть**|Щелкните **Создать виртуальную сеть** или выберите допустимую виртуальную сеть и подсеть.| Если поля сети и подсети затенены, прежде чем выбирать эти значения в качестве целевого объекта для нового Управляемого экземпляра, нужно [изменить их в соответствии с требованиями сети](sql-database-managed-instance-configure-vnet-subnet.md). См. дополнительные сведения о требованиях к [настройке виртуальной сети для Управляемого экземпляра Базы данных SQL Azure](sql-database-managed-instance-connectivity-architecture.md). |
   |**Группа ресурсов**|Новая или существующая группа ресурсов|Допустимые имена групп ресурсов см. в статье о [правилах и ограничениях именования](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|

   ![форма управляемого экземпляра](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Чтобы использовать Управляемый экземпляр Базы данных SQL Azure в качестве вторичной группы отработки отказа экземпляра, выберите просмотр и укажите управляемый экземпляр DnsAzurePartner. Эта функция доступна в предварительной версии и не показана на прилагающемся снимке экрана.
6. Щелкните поле **Ценовая категория**, чтобы выбрать размер вычислительных ресурсов и ресурсов хранения, а также просмотреть варианты ценовой категории. Ценовая категория общего назначения с 32 ГБ памяти и 16 виртуальными ядрами — значение по умолчанию.
7. Укажите нужный объем хранилища и количество виртуальных ядер, используя ползунки или текстовые поля.
8. По завершении нажмите кнопку **Применить** для сохранения выбранных параметров.  
9. Нажмите кнопку **Создать**, чтобы развернуть Управляемый экземпляр.
10. Щелкните значок **Уведомления**, чтобы просмотреть состояние развертывания.

    ![ход выполнения развертывания управляемого экземпляра](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Щелкните **Развертывание выполняется**, чтобы открыть окно Управляемого экземпляра для дальнейшего мониторинга состояния развертывания.

> [!IMPORTANT]
> Первый экземпляр в подсети обычно развертывается намного дольше, чем последующие экземпляры. Не стоит из-за этого отменять развертывание. Создание второго Управляемого экземпляра в подсети занимает всего пару минут.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Проверка ресурсов и получение полного имени сервера

После успешного завершения развертывания, проверьте созданные ресурсы и получите полное имя сервера для использования в последующих кратких руководствах.

1. Откройте группу ресурсов для Управляемого экземпляра и просмотрите его ресурсы, которые были созданы для вас в кратком руководстве [Создание Управляемого экземпляра](#create-a-managed-instance).

2. Выберите Управляемый экземпляр.

   ![Ресурсы Управляемого экземпляра](./media/sql-database-managed-instance-get-started/resources.png)

3. На вкладке **Обзор** найдите свойство **Узел** и скопируйте полный адрес узла для Управляемого экземпляра.

   ![Ресурсы Управляемого экземпляра](./media/sql-database-managed-instance-get-started/host-name.png)

   Имя будет выглядеть примерно так: **имя_компьютера.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о подключении к Управляемому экземпляру, см. в.
  - Обзор вариантов подключения см. в статье [Подключение приложения к Управляемому экземпляру Базы данных SQL](sql-database-managed-instance-connect-app.md).
  - Краткое руководство по подключению к Управляемому экземпляру с виртуальной машины Azure см. в статье [Configure Azure VM to connect to an Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vm.md) (Настройка виртуальной машине Azure для подключения к Управляемому экземпляру Базы данных SQL Azure).
  - Краткое руководство по подключению к Управляемому экземпляру с локального клиентского компьютера с помощью подключения "точка — сеть" см. в статье [Настройка подключения "точка — сеть" к Управляемому экземпляру Базы данных SQL Azure с локального компьютера](sql-database-managed-instance-configure-p2s.md).
- Чтобы восстановить имеющуюся базу данных SQL Server из локальной среды в Управляемый экземпляр, можно использовать [Azure Database Migration Service для миграции](../dms/tutorial-sql-server-to-managed-instance.md) или [команду T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) для восстановления из файла резервной копии базы данных.
- Сведения о расширенном мониторинге производительности для управляемого экземпляра базы данных с использованием встроенных средств анализа проблем см. в статье о [мониторинге базы данных SQL Azure с помощью Аналитики SQL Azure](../azure-monitor/insights/azure-sql.md).
