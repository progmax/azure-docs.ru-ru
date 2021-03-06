---
title: Предоставление высокодоступных баз данных MySQL в Azure Stack | Документация Майкрософт
description: Узнайте, как создать компьютер размещения для поставщика ресурсов сервера MySQL и высокодоступные базы данных MySQL с помощью Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: bee684409b2ef3fffeb9f175c2b469d3736b6484
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993836"
---
# <a name="tutorial-offer-highly-available-mysql-databases"></a>Руководство. Предоставление высокодоступных баз данных MySQL

Оператор Azure Stack может настроить виртуальные машины сервера для размещения баз данных сервера MySQL. После успешного создания кластера MySQL, которым управляет Azure Stack, пользователи, подписанные на службы MySQL, могут легко создавать высокодоступные базы данных MySQL.

В этом руководстве показано, как использовать элементы Marketplace Azure Stack для создания [MySQL с кластером репликации](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Это решение использует несколько виртуальных машин для репликации баз данных с главного узла на определенное количество реплик. После создания кластер можно добавить в качестве сервера размещения MySQL Azure Stack. После этого пользователи могут создавать высокодоступные базы данных MySQL.

> [!IMPORTANT]
> Элемент Azure Stack Marketplace **MySQL с репликацией** может быть доступен не для всех сред облачной подписки Azure. Прежде чем продолжать выполнение оставшейся части руководства, убедитесь, что элемент Marketplace доступен для вашей подписки.

Освещаются следующие темы:

> [!div class="checklist"]
> * создание кластера сервера MySQL из элементов Marketplace;
> * создание сервера размещения MySQL Azure Stack;
> * создание высокодоступной базы данных MySQL.

В этом руководстве будет создан и настроен кластер сервера MySQL с тремя виртуальными машинами с помощью доступных элементов Azure Stack Marketplace. 

Прежде чем выполнять действия, описанные в этом руководстве, убедитесь, что [поставщик ресурсов сервера MySQL](azure-stack-mysql-resource-provider-deploy.md) успешно установлен и что приведенные ниже компоненты доступны в Azure Stack Marketplace.

> [!IMPORTANT]
> Все следующие элементы необходимы для создания кластера MySQL.

- [MySQL с репликацией](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Это шаблон решения Bitnami, который используется для развертывания кластера MySQL.
- [Debian 8 "Jessie"](https://azuremarketplace.microsoft.com/marketplace/apps/credativ.Debian). Debian 8 "Jessie" с бэкпортированным ядром для Microsoft Azure предоставляется компанией Credativ. Debian GNU/Linux является одним из наиболее распространенных дистрибутивов Linux.
- [Настраиваемый сценарий для Linux 2.0](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft.custom-script-linux?tab=Overview). Расширение пользовательских сценариев — это средство для выполнения задач настройки виртуальной машины после ее подготовки. После добавления расширения к виртуальной машине можно загружать сценарии из службы хранилища Azure и запускать их на виртуальной машине. Задачи расширения пользовательских сценариев можно автоматизировать с помощью командлетов Azure PowerShell и кроссплатформенного интерфейса командной строки Azure.
- Расширение VM Access для Linux 1.4.7. Расширение VM Access позволяет сбросить пароль, ключ SSH или конфигурации SSH, благодаря чему вы можете восстановить доступ к виртуальной машине. Вы также можете добавить или удалить пользователя с помощью пароля или ключа SSH, используя это расширение. Оно предназначено для виртуальных машин Linux.

Сведения о том, как добавлять элементы в Azure Stack Marketplace, см. в статье [Общие сведения об Azure Stack Marketplace](azure-stack-marketplace.md).

Кроме того, вам потребуется клиент SSH, например [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), чтобы войти в системы виртуальных машин Linux после их развертывания.

## <a name="create-a-mysql-server-cluster"></a>Создание кластера сервера MySQL 
Используйте инструкции, описанные в этом разделе, чтобы развернуть кластер сервера MySQL с помощью элемента Marketplace [MySQL с репликацией](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/bitnami.mysql-cluster). Этот шаблон развертывает три экземпляра сервера MySQL, настроенные в высокодоступном кластере MySQL. По умолчанию он создает перечисленные ниже ресурсы:

- виртуальную сеть;
- группу безопасности сети;
- Учетная запись хранения.
- группа доступности;
- три сетевых интерфейса (по одному для каждой виртуальной машины по умолчанию);
- общедоступный IP-адрес (для основной виртуальной машины кластера MySQL);
- три виртуальные машины Linux для размещения кластера MySQL.

1. 
[!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Выберите **\+** **Создать ресурс** > **Вычисление**, а затем **MySQL with Replication** (MySQL с репликацией).

   ![Развертывание настраиваемого шаблона](media/azure-stack-tutorial-mysqlrp/1.png)

3. Укажите основные сведения о развертывании на странице **Основные**. Просмотрите значения по умолчанию, при необходимости внесите изменения и нажмите кнопку **ОК**.<br><br>Как минимум укажите следующее.
   - Имя развертывания (по умолчанию — mymysql).
   - Пароль корневого каталога приложения. Укажите буквенно-цифровой пароль из 12 знаков **без специальных символов**.
   - Имя базы данных приложения (по умолчанию — bitnami).
   - Число создаваемых виртуальных машин реплик базы данных MySQL (по умолчанию — 2).
   - Выберите подписку, которую необходимо использовать.
   - Выберите группу ресурсов или создайте новую.
   - Выберите расположение (по умолчанию для ASDK оно является локальным).

   [![](media/azure-stack-tutorial-mysqlrp/2-sm.PNG "Основы развертывания")](media/azure-stack-tutorial-mysqlrp/2-lg.PNG#lightbox)

4. На странице **Environment Configuration** (Настройка среды) укажите следующие сведения и нажмите кнопку **ОК**. 
   - Пароль или открытый ключ SSH, который будет использоваться для аутентификации Secure Shell (SSH). Если используется пароль, он должен содержать буквы, цифры и **может** включать специальные символы.
   - Размер виртуальной машины (по умолчанию — Standard D1 v2).
   - Размер диска с данными в ГБ. После этого нажмите кнопку **ОК**.

   [![](media/azure-stack-tutorial-mysqlrp/3-sm.PNG "Настройка среды")](media/azure-stack-tutorial-mysqlrp/3-lg.PNG#lightbox)

5. Просмотрите **сводные сведения** о развертывании. При необходимости можно скачать настроенный шаблон и параметры, а затем нажать кнопку **ОК**.

   [![](media/azure-stack-tutorial-mysqlrp/4-sm.PNG "Сводка")](media/azure-stack-tutorial-mysqlrp/4-lg.PNG#lightbox)

6. Нажмите кнопку **Создать** на странице **Купить**, чтобы начать развертывание.

   ![Купить](media/azure-stack-tutorial-mysqlrp/5.png)

    > [!NOTE]
    > Развертывание займет около часа. Перед продолжением убедитесь, что развертывание завершено и кластер MySQL полностью настроен. 

7. После успешного выполнения всех развертываний ознакомьтесь с элементами группы ресурсов и выберите элемент общедоступного IP-адреса **mysqlip**. Запишите общедоступный IP-адрес и его полное доменное имя для кластера.<br><br>Эти сведения необходимо будет предоставить оператору Azure Stack, чтобы он мог создать сервер размещения MySQL, использующий этот кластер MySQL.


### <a name="create-a-network-security-group-rule"></a>Создание правила группы безопасности сети
По умолчанию на виртуальной машине узла не настроен открытый доступ к MySQL. Поставщику ресурсов Azure Stack MySQL для подключения к кластеру MySQL и управления им необходимо создать правило входящего трафика для группы безопасности сети.

1. На портале администратора перейдите к группе ресурсов, созданной при развертывании кластера MySQL, и выберите группу безопасности сети (**default-subnet-sg**):

   ![открыт](media/azure-stack-tutorial-mysqlrp/6.png)

2. Выберите **Правила безопасности для входящего трафика**, а затем щелкните **Добавить**.<br><br>Введите значение **3306** в поле **Диапазон портов назначения** и при желании добавьте описание в поля **Имя** и **Описание**. Щелкните "Добавить", чтобы закрыть диалоговое окно с правилом безопасности для входящего трафика.

   ![открыт](media/azure-stack-tutorial-mysqlrp/7.png)

### <a name="configure-external-access-to-the-mysql-cluster"></a>Настройка внешнего доступа к кластеру MySQL
Перед добавлением кластера MySQL в качестве узла размещения сервера MySQL Azure Stack необходимо включить внешний доступ.

1. В этом примере вместе с клиентом SSH используется [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) для входа на главный компьютер MySQL с компьютера, который может получить доступ к общедоступному IP-адресу. Имя основной виртуальной машины MySQL обычно заканчивается на **0**, и у него есть назначенный общедоступный IP-адрес.<br><br>Используйте общедоступный IP-адрес и войдите в виртуальную машину с помощью имени пользователя **bitnami** и созданного ранее пароля приложения без специальных символов.

   ![LinuxLogin](media/azure-stack-tutorial-mysqlrp/bitnami1.png)


2. Используйте следующую команду в окне клиента SSH, чтобы убедиться, что служба bitnami включена и запущена. При появлении запроса введите пароль bitnami снова:

   `sudo service bitnami status`

   ![Проверка службы](media/azure-stack-tutorial-mysqlrp/bitnami2.png)

3. Создайте учетную запись удаленного доступа, которая будет использоваться сервером размещения MySQL Azure Stack для подключения к MySQL и последующего выхода из клиента SSH.<br><br>Выполните следующие команды для входа в MySQL в качестве пользователя root, используя пароль пользователя root, созданный ранее, и создайте пользователя-администратора, заменив *\<username\>* и *\<password\>* именем пользователя и паролем в соответствии со своей средой. В этом примере должен быть создан пользователь с именем **sqlsa**. Кроме того, здесь используется надежный пароль:

   ```mysql
   mysql -u root -p
   create user <username>@'%' identified by '<password>';
   grant all privileges on *.* to <username>@'%' with grant option;
   flush privileges;
   ```
   ![Создание пользователя-администратора](media/azure-stack-tutorial-mysqlrp/bitnami3.png)


4. Запишите новые сведения о пользователе MySQL.<br><br>Необходимо будет предоставить эти имя пользователя и пароль, а также общедоступный IP-адрес или полное доменное имя общедоступного IP-адреса кластера оператору Azure Stack, чтобы он мог создать сервер размещения MySQL с помощью этого кластера MySQL.


## <a name="create-an-azure-stack-mysql-hosting-server"></a>Создание сервера размещения MySQL Azure Stack
После создания и правильной настройки кластера сервера MySQL оператор Azure Stack должен создать сервер размещения MySQL Azure Stack, чтобы предоставить дополнительную емкость для пользователей, которые создают базы данных. 

Обязательно используйте общедоступный IP-адрес или полное доменное имя для общедоступного IP-адреса основной виртуальной машины кластера MySQL, которые были записаны ранее при создании группы ресурсов кластера MySQL (**mysqlip**). Кроме того, оператору необходимо знать учетные данные аутентификации MySQL Server, которые вы создали для удаленного доступа к базе данных кластера MySQL.

> [!NOTE]
> Оператор Azure Stack должен выполнить этот шаг на портале администрирования.

Используя общедоступный IP-адрес кластера MySQL и информацию для аутентификации MySQL, оператор Azure Stack теперь может [создать сервер размещения MySQL с помощью нового кластера MySQL](azure-stack-mysql-resource-provider-hosting-servers.md#connect-to-a-mysql-hosting-server). 

Кроме того, убедитесь, что вы создали планы и предложения, чтобы сделать создание базы данных MySQL доступным для пользователей. Оператору необходимо добавить службу **Microsoft.MySqlAdapter** к плану и создать квоту специально для высокодоступных баз данных. Дополнительные сведения о создании планов см. в статье [Обзор планов, предложений, квот и подписок](azure-stack-plan-offer-quota-overview.md).

> [!TIP]
> Службу **Microsoft.MySqlAdapter** нельзя добавить в планы до [завершения развертывания поставщика ресурсов сервера MySQL](azure-stack-mysql-resource-provider-deploy.md).



## <a name="create-a-highly-available-mysql-database"></a>Создание высокодоступной базы данных MySQL
После того как оператор Azure Stack создал, настроил и добавил кластер MySQL в качестве сервера размещения MySQL Azure Stack, пользователь клиента с подпиской, обеспечивающей возможности базы данных сервера MySQL, может создавать высокодоступные базы данных MySQL, выполнив действия, описанные в этом разделе. 

> [!NOTE]
> Выполните эти шаги с пользовательского портала Azure Stack в качестве пользователя клиента с подпиской, обеспечивающей возможности сервера MySQL (службу Microsoft.MySQLAdapter).

1. 
[!INCLUDE [azs-user-portal](../../includes/azs-user-portal.md)]

2. Выберите **\+****Создать ресурс** > **Данные \+ Хранилище**, а затем **База данных MySQL**.<br><br>Предоставьте необходимую информацию о свойствах базы данных, включая имя, параметры сортировки, используемую подписку и местоположение для развертывания. 

   ![Создание базы данных MySQL](./media/azure-stack-tutorial-mysqlrp/createdb1.png)

3. Выберите **SKU**, а затем соответствующий номер SKU сервера размещения MySQL. В этом примере оператор Azure Stack создал номер SKU **MySQL-HA** для поддержки высокого уровня доступности для баз данных кластера MySQL.

   ![Выбор номера SKU](./media/azure-stack-tutorial-mysqlrp/createdb2.png)

4. Выберите **Вход** > **Create a new login** (Создать новое имя для входа) и укажите учетные данные аутентификации MySQL, которые будут использоваться для новой базы данных. По завершении нажмите кнопку **ОК**, а затем — **Создать** для запуска процесса развертывания базы данных.

   ![Добавление имени входа](./media/azure-stack-tutorial-mysqlrp/createdb3.png)

5. После успешного завершения развертывания базы данных MySQL просмотрите свойства базы данных, чтобы найти строку подключения, используемую для соединения с новой высокодоступной базой данных. 

   ![Просмотр строки подключения](./media/azure-stack-tutorial-mysqlrp/createdb4.png)

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * создание кластера сервера MySQL из элементов Marketplace;
> * создание сервера размещения MySQL Azure Stack;
> * создание высокодоступной базы данных MySQL.

Перейдите к следующему руководству, чтобы изучить дальнейшие действия:
> [!div class="nextstepaction"]
> [Предложение по веб-приложениям](azure-stack-tutorial-app-service.md)
