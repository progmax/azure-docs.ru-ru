---
title: Развертывание Avere vFXT для Azure
description: Шаги по развертыванию кластера Avere vFXT в Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: d7c207f89b9cb50f940f071fbbf6ee81b4d44976
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164329"
---
# <a name="deploy-the-vfxt-cluster"></a>Развертывание кластера vFXT

Наиболее простым способом создания кластера vFXT является использование контроллера кластера, который представляет собой виртуальную машину с требуемыми сценариями, шаблонами и программной инфраструктурой для создания кластера vFXT и управления им.

Развертывание нового кластера vFXT состоит из следующих шагов:

1. [Создание контроллера кластера](#create-the-cluster-controller-vm).
1. При использовании хранилища BLOB-объектов Azure [создайте конечную точку хранилища](#create-a-storage-endpoint-if-using-azure-blob) в своей виртуальной сети.
1. [Подключение к контроллеру кластера](#access-the-controller). Остальные шаги выполняются с виртуальной машины контроллера кластера. 
1. [Создание роли доступа](#create-the-cluster-node-access-role) для узлов кластера. Прототип предоставляется.
1. [Настройка сценария создания кластера](#edit-the-deployment-script) для типа кластера vFXT, который нужно создать.
1. [Выполнение сценария создания кластера ](#run-the-script).

Дополнительные сведения о шагах развертывания кластера и планировании см. в статьях [Plan your Avere vFXT system](avere-vfxt-deploy-plan.md) (Планирование системы Avere vFXT) и [Avere vFXT for Azure - deployment overview](avere-vfxt-deploy-overview.md) (Общие сведения о развертывании Avere vFXT для Azure). 

Когда вы выполните инструкции в этом документе, у вас будет виртуальная сеть, подсеть, контроллер и кластер vFXT, как показано на следующей схеме:

![схема с виртуальной сетью, содержащей дополнительное хранилище BLOB-объектов, и подсетью, содержащей три сгруппированные виртуальные машины с метками узлов vFXT и кластера vFXT и одну виртуальную машину с меткой контроллера кластера](media/avere-vfxt-deployment.png)

Перед запуском убедитесь, что у вас есть следующие необходимые компоненты:  

1. [Новая подписка](avere-vfxt-prereqs.md#create-a-new-subscription).
1. [Разрешения владельца подписки](avere-vfxt-prereqs.md#configure-subscription-owner-permissions).
1. [Квота для кластера vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster).

При необходимости вы можете создать роль узла кластера [перед](avere-vfxt-pre-role.md) созданием контроллера кластера, но проще сделать это после.

## <a name="create-the-cluster-controller-vm"></a>Создание виртуальной машины контроллера кластера

Первый шаг — создание виртуальной машины, где будет выполняться создание и настройка узлов кластера vFXT. 

Контроллер кластера представляет собой виртуальную машину под управлением Linux с предварительно установленными программным обеспечением и сценариями для создания кластера Avere vFXT. Для нее не требуется значительная вычислительная мощность или большой объем хранилища, поэтому вы можете выбрать недорогие варианты. Эта виртуальная машина будет периодически использоваться на протяжении всего жизненного цикла кластера vFXT.

Существует два способа создания виртуальной машины контроллера кластера. В целях упрощения процесса [ниже](#create-controller---arm-template) предоставляется [шаблон Azure Resource Manager](#create-controller---arm-template), но вы также можете создать контроллер с помощью [образа Azure Marketplace](#create-controller---azure-marketplace-image). 

Вы можете создать группу ресурсов как часть создания контроллера.

> [!TIP]
>
> Решите, следует ли использовать общедоступный IP-адрес в контроллере кластера. Общедоступный IP-адрес обеспечивает более удобный доступ к кластеру vFXT, но создает небольшую угрозу безопасности.
>
>  * Благодаря наличию общедоступного IP-адреса контроллер кластера можно использовать в качестве узла перехода для подключения к кластеру Avere vFXT за пределами частной подсети.
>  * Если вы не настроили общедоступный IP-адрес в контроллере, для доступа к кластеру необходимо использовать другой узел перехода, подключение VPN или ExpressRoute. Например, создайте контроллер в виртуальной сети с настроенным VPN-подключением.
>  * Если вы создаете контроллер с общедоступным IP-адресом, необходимо защитить виртуальную машину контроллера с помощью группы безопасности сети. Для обеспечения доступа в Интернет разрешите доступ только через порт 22.

### <a name="create-controller---resource-manager-template"></a>Создание контроллера с помощью шаблона Resource Manager

Чтобы создать узел контроллера кластера на портале, нажмите расположенную ниже кнопку "Развертывание в Azure". Этот шаблон развертывания создает виртуальную машину, которая создаст кластер Avere vFXT и будет управлять им.

[![кнопка создания контроллера](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fazuredeploy.json)

Введите следующие данные.

В разделе **Основные сведения**:  

* **Подписку** для кластера.
* **группу ресурсов** для кластера; 
* **Местоположение.** 

В разделе **Параметры**:

* Укажите, следует ли создавать виртуальную сеть.

  * Если вы создадите виртуальную сеть, контроллеру кластера будет назначен общедоступный IP-адрес, чтобы вы смогли получит к нему доступ. Для этой виртуальной сети также будет создана группа безопасности сети, которая разрешает входящий трафик только через порт 22.
  * Если для подключения к контроллеру кластера нужно использовать ExpressRoute или VPN, задайте значение `false` и в остальных полях укажите существующую виртуальную сеть. Контроллер кластера будет использовать эту виртуальную сеть для взаимодействия по сети. 

* Группа ресурсов виртуальной сети, ее имя и имя подсети. Введите имена имеющихся ресурсов (если используется имеющаяся виртуальная сеть) или введите новые имена, если создается новая виртуальная сеть.
* **Имя контроллера**. Укажите имя для виртуальной машины контроллера.
* Имя администратора контроллера. По умолчанию используется имя `azureuser`.
* Ключ SSH. Вставьте открытый ключ, чтобы связать его с именем администратора. Дополнительные сведения см. в статье [Краткая инструкция: создание и использование пары из открытого и закрытого ключей SSH для виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

В разделе **Terms and conditions** (Условия использования): 

* Прочитайте условия предоставления услуг и установите флажок, чтобы принять их. 

  > [!NOTE] 
  > Если вы не являетесь владельцем подписки, попросите владельца принять условия, выполнив шаги, описанные в разделе [Принятие условий программного обеспечения заранее](avere-vfxt-prereqs.md#accept-software-terms-in-advance). 


Когда все будет готово, щелкните **Приобрести**. Через пять или шесть минут узел контроллера будет запущен.

Посетите страницу выходных данных для сбора сведений о контроллере, необходимых для создания кластера. Подробнее о создании кластера см. [здесь](#information-needed-to-create-the-cluster).

### <a name="create-controller---azure-marketplace-image"></a>Создание контроллера с помощью образа Azure Marketplace

Найдите шаблон контроллера, выполнив поиск в Azure Marketplace по имени ``Avere``. Выберите шаблон **Avere vFXT for Azure Controller** (Контроллер Avere vFXT для Azure).

Примите условия (если вы этого еще не сделали) и включите программный доступ для образа Marketplace, щелкнув ссылку "Хотите выполнить развертывание программным способом?" под кнопкой **Создать**.

![Снимок экрана со ссылкой на программный доступ, которая находится под кнопкой "Создать"](media/avere-vfxt-deploy-programmatically.png)

Нажмите кнопку **Включить** и сохраните параметр.

![Снимок экрана с изображением щелчка мыши для включения программного доступа](media/avere-vfxt-enable-program.png)

Вернитесь на главную страницу шаблона **Avere vFXT for Azure Controller** (Контроллер Avere vFXT для Azure) и щелкните **Создать**. 

На первой панели введите или подтвердите следующие основные параметры:

* **Подписка**
* **Группа ресурсов**: введите новое имя, если вы хотите создать новую группу.
* **Имя виртуальной машины**: имя контроллера.
* **Регион**
* **Параметры доступности**: избыточность не требуется.
* **Образ**: образ узла контроллера Avere vFXT.
* **Размер**: оставьте значение по умолчанию или выберите другой недорогой тип.
* **Учетная запись администратора**: укажите способ входа в контроллер кластера. 
  * Выберите метод "Имя пользователя и пароль" или "Открытый ключ SSH" (рекомендуется).
  
    > [!TIP] 
    > Ключ SSH более безопасен. Дополнительные сведения см. в статье [Краткая инструкция: создание и использование пары из открытого и закрытого ключей SSH для виртуальных машин Linux в Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows). 
  * Укажите имя пользователя. 
  * Вставьте ключ SSH или введите и подтвердите пароль.
* **Правила входящего порта**: при использовании общедоступного IP-адреса откройте порт 22 (SSH).

Нажмите кнопку **Далее**, чтобы задать параметры диска:

* **Тип диска ОС**: используйте значение по умолчанию (HDD).
* **Use unmanaged disks** (Использование неуправляемых дисков): необязательно.
* **Диски данных**: не используйте.

Нажмите кнопку **Далее**, чтобы перейти к параметрам сети:

* **Виртуальная сеть**. Выберите виртуальную сеть для контроллера или введите имя для создания новой виртуальной сети. Если вы не хотите использовать общедоступный IP-адрес в контроллере, разместите его в виртуальной сети с ExpressRoute или другим предварительно настроенным методом доступа.
* **Подсеть**. Выберите подсеть в пределах виртуальной сети (необязательно). При создании виртуальной сети вы можете создать и подсеть.
* **Общедоступный IP-адрес**. Если вы хотите использовать общедоступный IP-адрес, укажите его в этом разделе. 
* **Группа безопасности сети**. Оставьте параметр по умолчанию (**Базовая**). 
* **Общедоступные входящие порты**. Если используется общедоступный IP-адрес, используйте этот элемент управления, чтобы пропускать SSH-трафик. 
* **Ускоренная сеть**. Эта возможность недоступна для этой виртуальной машины.

Нажмите кнопку **Далее**, чтобы задать параметры управления:

* **Диагностика загрузки**. Укажите значение **Выкл.**
* **Диагностика гостевой ОС**. Оставьте отключенным.
* **Учетная запись хранения диагностики**. При необходимости выберите или укажите новую учетную запись для хранения данных диагностики.
* **Управляемое удостоверение службы**. Укажите для этого параметра значение **Вкл.**, в результате чего для контроллера кластера будет создан субъект-служба Azure AD.
* **Автозавершение работы**. Оставьте отключенным. 

На этом этапе вы можете щелкнуть **Просмотр и создание**, если вы не хотите использовать теги экземпляров. В противном случае щелкните **Далее** дважды, чтобы пропустить страницу **Конфигурация гостевой ОС** и перейти на страницу тегов. После завершения щелкните **Просмотр и создание**. 

После того как ваш выбор будет проверен, нажмите кнопку **Создать**.  

Создание занимает пять или шесть минут.

## <a name="create-a-storage-endpoint-if-using-azure-blob"></a>Создание конечной точки хранилища (при использовании больших двоичных объектов Azure)

Если вы используете хранилище BLOB-объектов Azure в качестве внутреннего хранилища данных, необходимо создать конечную точку службы хранилища в виртуальной сети. Благодаря этой [конечной точке службы](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) трафик больших двоичных объектов Azure остается в локальной среде, а не маршрутизируется через Интернет.

1. На портале слева щелкните **Виртуальные сети**.
1. Выберите виртуальную сеть для своего контроллера. 
1. В области слева щелкните **Конечные точки службы**.
1. В верхней части страницы щелкните **Добавить**.
1. Оставьте службу ``Microsoft.Storage`` без изменений и выберите подсеть контроллера.
1. В нижней части страницы щелкните **Добавить**.

  ![Снимок экрана портала Azure с заметками для шагов создания конечной точки службы](media/avere-vfxt-service-endpoint.png)

## <a name="information-needed-to-create-the-cluster"></a>Необходимая информация для создания кластера

После создания контроллера кластера убедитесь, что у вас есть сведения, требуемые для выполнения следующих действий. 

Для подключения к контроллеру необходима следующая информация: 

* Имя пользователя контроллера и SSH-ключ (или пароль).
* IP-адрес контроллера или другой способ подключения к виртуальной машине контроллера.

Информация, необходимая для кластера: 

* Имя группы ресурсов
* Расположение Azure. 
* Имя виртуальной сети
* Имя подсети
* Имя роли узла кластера — это имя задается при создании роли, описанной [ниже](#create-the-cluster-node-access-role).
* Имя учетной записи хранения, если создается контейнер больших двоичных объектов.

Если вы создали узел контроллера с помощью шаблона Resource Manager, вы можете получить сведения из [выходных данных шаблона](#find-template-output). 

Если для создания контроллера использовался образ из Azure Marketplace, вы указали большинство из этих элементов напрямую. 

Найдите отсутствующие элементы, перейдя на страницу сведений о виртуальной машине контроллера. Например, выберите **Все ресурсы** и найдите имя контроллера, а затем щелкните на него, чтобы просмотреть подробные сведения.

### <a name="find-template-output"></a>Поиск выходных данных шаблона

Чтобы найти эти сведения в выходных данных шаблона Resource Manager, выполните следующую процедуру:

1. Перейдите к группе ресурсов для контроллера кластера.

1. В левой части страницы щелкните **Развертывания**, а затем — **Microsoft.Template**.

   ![Страница портала группы ресурсов с выбранным пунктом "Развертывания" в области слева и отображенным в таблице именем развертывания Microsoft.Template](media/avere-vfxt-deployment-template.png)

1. В левой части страницы щелкните **Выходные данные**. Скопируйте значения в каждом из полей. 

   ![страница выходных данных с полями SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK, SUBNET и SUBNET_ID, расположенными справа от меток](media/avere-vfxt-template-outputs.png)

## <a name="access-the-controller"></a>Доступ к контроллеру

Чтобы выполнить остальные шаги по развертыванию, вам необходимо подключиться к контроллеру кластера.

1. Способ подключения к контроллеру кластера зависит от вашей настройки.

   * Если у контроллера есть общедоступный IP-адрес, установите к контроллеру SSH-подключение по IP-адресу, используя заданное имя администратора (например, ``ssh azureuser@40.117.136.91``).
   * Если у контроллера нет общедоступного IP-адреса, установите подключение VPN или [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) к вашей виртуальной сети.

1. После входа в контроллер выполните аутентификацию с помощью команды `az login`. Скопируйте код аутентификации, указанный в оболочке, а затем в веб-браузере перейдите по адресу [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) и пройдите аутентификацию в системе Майкрософт. Вернитесь к оболочке для подтверждения.

   ![Выходные данные команды AZ login в командной строке, отображающие ссылку для открытия в браузере и код аутентификации](media/avere-vfxt-azlogin.png)

1. Добавьте свою подписку, выполнив эту команду, используя идентификатор своей подписки: ```az account set --subscription YOUR_SUBSCRIPTION_ID```

## <a name="create-the-cluster-node-access-role"></a>Создание роли доступа к узлу кластера

> [!NOTE] 
> * Если вы не являетесь владельцем подписки и роль доступа еще не создана, владелец подписки должен выполнить эти шаги или же процедуру, описанную в статье [Create the Avere vFXT cluster runtime access role without a controller](avere-vfxt-pre-role.md) (Создание роли доступа к среде выполнения кластера Avere vFXT без контроллера).
> 
> * Внутренние пользователи корпорации Майкрософт должны использовать существующую роль с именем "Оператор среды выполнения кластеров Avere", вместо того чтобы создавать новую. 

[Управление доступом на основе ролей](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) обеспечивает авторизацию узлов кластера vFXT, позволяя им выполнять необходимые задачи.  

В рамках обычной операции кластера vFXT отдельные узлы vFXT должны считывать свойства ресурса Azure, управлять хранилищем, а также параметрами сетевого интерфейса других узлов. 

1. Откройте файл ``/avere-cluster.json`` в редакторе в контроллере.

   ![консоль, где отображается команда создания списка, а затем — vi /avere-cluster.json](media/avere-vfxt-open-role.png)

1. Измените файл, добавив идентификатор своей подписки и удалив строку над ним. Сохраните файл как ``avere-cluster.json``.

   ![Текстовый редактор консоли, отображающий идентификатор подписки и выбранную строку, которую нужно удалить](media/avere-vfxt-edit-role.png)

1. Создайте роль с помощью следующей команды:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

На следующем шаге вы передадите имя роли в сценарий создания кластера. 

## <a name="create-nodes-and-configure-the-cluster"></a>Создание узлов и настройка кластера

Чтобы создать кластер Avere vFXT, измените один из примеров сценариев, включенных в контроллер, и запустите его там. Примеры сценариев находятся в корневом каталоге (`/`) в контроллере кластера.

* Если вы хотите создать контейнер больших двоичных объектов для использования в качестве внутренней системы хранения Avere vFXT, выполните сценарий ``create-cloudbacked-cluster``.

* Если вы добавите хранилище позже, используйте сценарий ``create-minimal-cluster``.

> [!TIP]
> Сценарии-прототипы для добавления узлов и уничтожения кластера vFXT также включены в каталог `/` виртуальной машины контроллера кластера.

### <a name="edit-the-deployment-script"></a>Изменение сценария развертывания

Откройте пример сценария в редакторе. Возможно, потребуется сохранить настроенный сценарий под другим именем, чтобы избежать перезаписи исходного примера.

Введите значения для этих переменных сценария.

* Имя группы ресурсов

  * Если вы используете компоненты сети или хранилища, которые находятся в разных группах ресурсов, раскомментируйте переменные и укажите имена этих групп. 

```python
# Resource groups
# At a minimum specify the resource group.  If the network resources live in a
# different group, specify the network resource group.  Likewise for the storage
# account resource group.
RESOURCE_GROUP=
#NETWORK_RESOURCE_GROUP=
#STORAGE_RESOURCE_GROUP=
```

* Имя расположения.
* Имя виртуальной сети
* Имя подсети
* Имя роли для среды выполнения Azure AD. Если вы следовали примеру, приведенному в разделе [Создание роли доступа к узлу кластера](#create-the-cluster-node-access-role), используйте ``avere-cluster``. 
* Имя учетной записи хранения (в случае создания контейнера больших двоичных объектов).
* Имя кластера. У вас не может быть два кластера vFXT с одинаковым именем в одной группе ресурсов. Присвойте уникальное имя каждому кластеру.
* Пароль администратора. Выберите защищенный пароль для мониторинга и администрирования кластера. Этот пароль назначается пользователю ``admin``. 
* Тип экземпляра узла. Сведения см. в разделе [vFXT node sizes](avere-vfxt-deploy-plan.md#vfxt-node-sizes) (Размеры узлов vFXT).
* Размер кэша узла. Сведения см. в разделе [vFXT node sizes](avere-vfxt-deploy-plan.md#vfxt-node-sizes) (Размеры узлов vFXT).

Сохраните файл и выйдите из редактора.

### <a name="run-the-script"></a>Запуск сценария

Запустите сценарий, введя имя созданного файла. (Пример: `./create-cloudbacked-cluster-west1`.)  

> [!TIP]
> В случае потери подключения попробуйте запустить эту команду внутри [терминального мультиплексора](http://linuxcommand.org/lc3_adv_termmux.php), например `screen` или `tmux`.  

Выходные данные также записываются в `~/vfxt.log`.

После выполнения сценария скопируйте IP-адрес управления, который необходим для администрирования кластера.

![Выходные данные сценария в командной строке, содержащие IP-адрес управления в конце](media/avere-vfxt-mgmt-ip.png)

## <a name="next-step"></a>Дальнейшие действия

Теперь, когда кластер запущен и вам известен его IP-адрес управления, вы можете [подключиться к средству конфигурации кластера](avere-vfxt-cluster-gui.md), чтобы включить поддержку и при необходимости добавить хранилище.
