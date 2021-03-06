---
title: Настройка целевых объектов вычислений для обучения моделей с помощью службы машинного обучения Azure | Документы Майкрософт
description: Узнайте, как выбирать и настраивать среды обучения (целевые объекты вычислений), которые используются для обучения моделей машинного обучения. Служба машинного обучения Azure позволяет легко переключаться между средами обучения. Начните обучение на локальном компьютере, а затем, если вам потребуется увеличить масштаб, переключитесь на облачный целевой объект вычислений.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 7eacc475145dac61db1717f1860e22cedd022262
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231453"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Выбор и использование целевого объекта вычислений для обучения вашей модели

С помощью службы машинного обучения Azure можно обучать свою модель в нескольких разных средах. Эти среды называются __целевыми объектами вычислений__ и могут быть локальными или облачными. Из этого документа вы узнаете о поддерживаемых целевых объектах вычислений и способах их использования.

Целевой объект вычислений — это ресурс, который запускает ваш сценарий обучения или размещает вашу модель, если она развертывается как веб-служба. Создавать такие объекты и управлять ими можно с помощью пакета SDK для машинного обучения Azure или интерфейса командной строки. Если у вас есть целевые объекты вычислений, созданные другим процессом (например, порталом Azure или Azure CLI), вы можете привязать их к рабочей области службы машинного обучения Azure.

Вы можете начать с работы на локальном компьютере, а затем увеличить масштаб и перейти в другие среды, такие как удаленные виртуальные машины обработки и анализа данных с поддержкой GPU или Azure Batch AI. 

>[!NOTE]
> Код в этой статье был протестирован с пакетом SDK для службы "Машинное обучение Azure" версии 0.168. 

## <a name="supported-compute-targets"></a>Поддерживаемые целевые объекты вычислений

Машинное обучение Azure поддерживает следующие целевые объекты вычислений.

|Целевой объект вычисления| Ускорение GPU | Автоматическая настройка гиперпараметров | Автоматический выбор модели | Возможность использования в конвейерах|
|----|:----:|:----:|:----:|:----:|
|[Локальный компьютер](#local)| Возможно | &nbsp; | ✓ | &nbsp; |
|[Виртуальная машина для обработки и анализа данных](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Аналитика озера данных Azure](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>* Azure Databricks и Azure Data Lake Analytics можно использовать __только__ в конвейере. Дополнительные сведения о конвейерах см. в статье [Конвейеры и служба "Машинное обучение Azure"](concept-ml-pipelines.md).

__[Экземпляры контейнеров Azure (ACI)](#aci)__  можно также использовать для обучения моделей. Это бессерверное облачное решение, которое стоит недорого и при этом легко создается и удобно в работе. ACI не поддерживает ускорение GPU, автоматическую настройку гиперпараметров или автоматический выбор модели. Кроме того, его нельзя использовать в конвейере.

Основные различия между целевыми объектами вычислений:
* __Ускорение GPU__: GPU предоставляются с виртуальной машины для обработки и анализа данных и Azure Batch AI. Возможность получения доступа к GPU на локальном компьютере зависит от установленного оборудования, драйверов и платформ.
* __Автоматическая настройка гиперпараметров__: автоматическая оптимизация гиперпараметров машинного обучения Azure помогает подобрать гиперпараметры, оптимальные для вашей модели.
* __Автоматический выбор модели__: машинное обучение Azure может рекомендовать выбор алгоритма и гиперпараметров при построении модели. Автоматический выбор модели позволяет получить высококачественную модель быстрее, чем если вы будете перебирать различные комбинации вручную. Дополнительную информацию см. в документе [Руководство. Автоматическое обучение модели классификации с помощью автоматического машинного обучения Azure](tutorial-auto-train-models.md).
* __Конвейеры__: машинное обучение Azure позволяет объединять различные задачи, такие как обучение и развертывание, в конвейер. Конвейеры могут запускаться параллельно или последовательно и представляют собой надежный механизм автоматизации. Дополнительные сведения см. в документе [Создание конвейеров машинного обучения с помощью службы машинного обучения Azure](concept-ml-pipelines.md).

Пакет SDK для машинного обучения Azure, Azure CLI или портал Azure можно использовать для создания целевых объектов вычислений. Кроме того, можно использовать уже существующие целевые объекты вычислений, добавив (присоединив) их к своей рабочей области.

> [!IMPORTANT]
> Существующий экземпляр контейнеров Azure подключить к рабочей области нельзя. Вместо этого необходимо создать новый экземпляр.
>
> В рабочей области не удается создать Azure HDInsight, Azure Databricks и Azure Data Lake Store. Вместо этого необходимо создать ресурс и вложить его в рабочую область.

## <a name="workflow"></a>Рабочий процесс

Рабочий процесс для разработки и развертывания модели с помощью машинного обучения Azure включает следующие шаги:

1. Создание сценариев обучения машинного обучения на Python.
1. Создание и настройка целевого объекта вычислений либо подключение уже существующего объекта.
1. Отправка сценариев обучения в целевой объект вычислений.
1. Проверка результатов для подбора оптимальной модели.
1. Регистрация модели в реестре моделей.
1. Развертывание модели.

> [!IMPORTANT]
> Сценарий обучения не привязывается к определенному целевому объекту вычислений. Обучение можно начать на локальном компьютере, а затем переключить целевые объекты вычислений, не переписывая весь сценарий.

Для переключения с одного целевого объекта вычислений на другой необходимо создать [конфигурацию запуска](concept-azure-machine-learning-architecture.md#run-configuration). Конфигурация запуска определяет способ выполнения сценария в целевом объекте вычислений.

## <a name="training-scripts"></a>Сценарии обучения

Когда вы начинаете обучение, передается весь каталог, содержащий ваши сценарии обучения. Создается и отправляется в целевой объект обучения моментальный снимок. Дополнительные сведения см. в разделе о [моментальных снимках](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Локальный компьютер

В процессе локального обучения можно использовать для отправки операции обучения пакет SDK. Для обучения можно использовать среду, управляемую пользователем, или среду, управляемую системой.

### <a name="user-managed-environment"></a>Среда, управляемая пользователем

В среде, управляемой пользователем, вы сами следите за тем, чтобы все необходимые пакеты были доступны в среде Python, которую вы выбрали для выполнения сценариев. Следующий фрагмент кода представляет собой пример настройки обучения для среды, управляемой пользователем:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

Записную книжку Jupyter с примером обучения в управляемой пользователем среде см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
### <a name="system-managed-environment"></a>Среда, управляемая системой

В средах, управляемых системой, зависимостями управляет Conda. Conda создает файл с именем `conda_dependencies.yml`, содержащий список зависимостей. После этого можно попросить систему создать новую среду Conda и выполнять сценарии в ней. Среды, управляемые системой, можно повторно использовать позднее при условии, что файлы `conda_dependencies.yml` останутся неизменны. 

Первоначальная настройка новой среды может потребовать несколько минут в зависимости от размера необходимых зависимостей. Следующий фрагмент кода демонстрирует создание среды, управляемой системой, которая зависит от scikit-learn:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Записную книжку Jupyter с примером обучения в управляемой системой среде см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Виртуальная машина для обработки и анализа данных

На локальном компьютере может не оказаться вычислительных ресурсов или ресурсов GPU, необходимых для обучения модели. В этом случае вы можете масштабировать процесс обучения, добавив дополнительные целевые объекты вычислений, такие как виртуальные машины обработки и анализа данных (DSVM).

> [!WARNING]
> Машинное обучение Azure поддерживает только виртуальные машины под управлением Ubuntu. При создании виртуальной машины или выборе уже существующей необходимо выбрать виртуальную машину под управлением Ubuntu.

При выполнении последующих действий SDK позволит настроить виртуальную машину для обработки и анализа данных как целевого объекта обучения:

1. Создание или присоединение виртуальной машины
    
    * Прежде чем создавать DSVM, убедитесь, что у вас еще нет DSVM с таким же именем, и если это так, создайте новую виртуальную машину:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Чтобы присоединить имеющуюся виртуальную машину как целевой объект вычислений, необходимо указать ее полное доменное имя, имя входа и пароль.  В приведенном примере замените ```<fqdn>``` на общедоступное полное доменное имя виртуальной машины или общедоступный IP-адрес. Замените ```<username>``` и ```<password>``` на имя пользователя SSH и пароль для виртуальной машины:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

1. Чтобы удалить вычислительные ресурсы, когда вы закончите, используйте следующий код:

    ```python
    dsvm_compute.delete()
    ```

Записную книжку Jupyter с примером обучения в среде "Виртуальная машина для обработки и анализа данных" см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>Azure Batch AI

Если обучение модели занимает много времени, можно распространить обучение по кластеру вычислительных ресурсов в облаке с помощью Azure Batch AI. Batch AI можно также настроить на включение ресурса GPU.

Код в следующем примере ищет существующий кластер Batch AI по имени. Если кластера нет, он будет создан:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
batchai_cluster_name = os.environ.get("BATCHAI_CLUSTER_NAME", ws.name + "gpu")
cluster_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 1)
cluster_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 3)
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_NC6")
autoscale_enabled = os.environ.get("BATCHAI_CLUSTER_AUTOSCALE_ENABLED", True)


if batchai_cluster_name in ws.compute_targets():
    compute_target = ws.compute_targets()[batchai_cluster_name]
    if compute_target and type(compute_target) is BatchAiCompute:
        print('found compute target. just use it. ' + batchai_cluster_name)
else:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = autoscale_enabled,
                                                                cluster_min_nodes = cluster_min_nodes, 
                                                                cluster_max_nodes = cluster_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Чтобы подключить имеющийся кластер Batch AI как целевой объект вычислений, необходимо указать идентификатор ресурса Azure. Чтобы получить идентификатор ресурса, на портале Azure сделайте следующее:
1. Найдите службу `Batch AI` в разделе **Все службы**.
1. Щелкните имя рабочей области, к которой относится кластер.
1. Выберите кластер.
1. Нажмите **Свойства**.
1. Скопируйте **идентификатор**.

В следующем примере пакет SDK используется для присоединения кластера к рабочей области. Замените `<name>` в приведенном примере на любое имя для вычислений. Это имя не должно совпадать с именем кластера. Замените `<resource-id>` на идентификатор ресурса Azure, описанный выше:

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

Вы также можете проверить состояние кластера Batch AI и заданий с помощью следующих команд Azure CLI:

- Проверка состояния кластера: узнать количество запущенных узлов позволяет команда `az batchai cluster list`.
- Проверка состояния заданий: узнать количество выполняемых заданий позволяет команда `az batchai job list`.

Процесс создания кластера Batch AI занимает около 5 минут.

Записную книжку Jupyter Notebook с примером обучения в кластере Batch AI см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Экземпляр контейнера Azure (ACI)

Экземпляры контейнеров Azure — это изолированные контейнеры с более быстрым временем запуска, не требующие от пользователя управления виртуальными машинами. Служба машинного обучения Azure использует контейнеры Linux, доступные в регионах westus, eastus, westeurope, northeurope, westus2 и southeastasia. Дополнительные сведения см. в разделе о [доступности в регионах](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

В следующем примере показано, как создать целевой объект вычислений с помощью SDK и использовать его для обучения модели: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

Создание целевого объекта вычислений ACI может занять от нескольких секунд до нескольких минут.

Записную книжку Jupyter с примером обучения в экземпляре контейнера Azure см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="databricks"></a>Azure Databricks

Azure Databricks — это среда, которая лежит в основе Apache Spark в облаке Azure. Ее можно использовать как целевой объект вычислений при обучении моделей с помощью конвейера машинного обучения Azure.

> [!IMPORTANT]
> Целевой объект вычислений Azure Databricks можно использовать только в конвейере машинного обучения.
>
> Перед тем как его использовать для обучения модели, необходимо создать рабочую область Azure Databricks. Чтобы создать эти ресурсы, см. [Руководство. Автоматическое машинное обучение модели классификации в службе "Машинное обучение Azure"](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Чтобы вложить Azure Databricks в качестве целевого объекта вычислений, используйте пакет SDK машинного обучения Azure и укажите следующие сведения.

* __Имя вычисления__. Имя, которое вы хотите назначить как вычислительный ресурс.
* __Идентификатор ресурса__. Идентификатор ресурса рабочей области Azure Databricks. Ниже приведен пример формата такого файла.

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Чтобы получить идентификатор ресурса, выполните следующую команду Azure CLI. Замените `<databricks-ws>` именем рабочей области Databricks.
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Маркер доступа__. Маркер доступа, который используется для проверки подлинности в Azure Databricks. Чтобы создать маркер доступа, см. документ [Проверка подлинности](https://docs.azuredatabricks.net/api/latest/authentication.html).

Следующий код демонстрирует, как вложить Azure Databricks в качестве целевого объекта вычислений.

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics — это платформа аналитики больших данных в облаке Azure. Ее можно использовать как целевой объект вычислений при обучении моделей с помощью конвейера машинного обучения Azure.

> [!IMPORTANT]
> Целевой объект вычислений Azure Data Lake Analytics можно использовать только в конвейере машинного обучения.
>
> Перед использованием его для обучения модели необходимо создать учетную запись Azure Data Lake Analytics. Чтобы создать этот ресурс, см. статью [Начало работы с Azure Data Lake Analytics с помощью портала Azure](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Чтобы вложить Azure Data Lake Analytics в качестве целевого объекта вычислений, используйте пакет SDK машинного обучения Azure и укажите следующие сведения.

* __Имя вычисления__. Имя, которое вы хотите назначить как вычислительный ресурс.
* __Идентификатор ресурса__. Идентификатор ресурса учетной записи Azure Data Lake Analytics. Ниже приведен пример формата такого файла.

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Чтобы получить идентификатор ресурса, выполните следующую команду Azure CLI. Замените `<datalakeanalytics>` именем вашей учетной записи Data Lake Analytics.
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

Следующий код демонстрирует, как вложить Data Lake Analytics в качестве целевого объекта вычислений.

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Конвейеры машинного обучения Azure работают только с хранимыми данными в хранилище данных учетной записи Data Lake Analytics по умолчанию. Если данные, с которыми нужно работать, находятся в хранилище не по умолчанию, то для копирования данных перед обучением можно использовать [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py).

## <a id="hdinsight"></a>Присоединение кластера HDInsight 

HDInsight — это популярная платформа для анализа больших данных. Она предоставляет Apache Spark, который можно использовать для обучения модели.

> [!IMPORTANT]
> Прежде чем использовать кластер HDInsight для обучения модели, его необходимо создать. Инструкции по созданию Spark в кластере HDInsight см. в документе [Создание кластера Spark в HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> При создании кластера необходимо указать имя пользователя SSH и пароль. Запишите эти значения — они вам потребуются при использовании HDInsight в качестве целевого объекта вычислений.
>
> Созданный кластер будет иметь полное доменное имя `<clustername>.azurehdinsight.net`, где `<clustername>` — это имя, которое вы указали для кластера. Этот адрес (или общедоступный IP-адрес кластера) нужно будет использовать как целевой объект вычислений.

Чтобы настроить HDInsight как целевой объект вычислений, необходимо указать полное доменное имя, а также имя входа и пароль для кластера HDInsight. В следующем примере пакет SDK используется для присоединения кластера к рабочей области. В приведенном примере замените `<fqdn>` на общедоступное полное доменное имя кластера или общедоступный IP-адрес. Замените `<username>` и `<password>` на имя пользователя SSH и пароль для кластера:

> [!NOTE]
> Чтобы узнать полное доменное имя для кластера, откройте портал Azure и выберите свой кластер HDInsight. В разделе __Обзор__ полное доменное имя будет частью записи __URL-адрес__. Просто удалите `https://` в начале значения.
>
> ![Снимок экрана с обзором кластера HDInsight и выделенной записью URL-адреса](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Выполнение обучающего прогона

Существует два способа отправки запуска на выполнения обучения:

* отправка объекта `ScriptRunConfig`;
* отправка объекта `Pipeline`.

> [!IMPORTANT]
> Целевые объекты вычислений Azure Databricks и Azure Datalake Analytics могут использоваться только в конвейере.
> Локальный целевой объект вычислений не может использоваться в конвейере.

### <a name="submit-using-scriptrunconfig"></a>Отправка с помощью `ScriptRunConfig`

Шаблон кода для отправки запуска на выполнение обучения с помощью `ScriptRunConfig` будет одинаковым, независимо от целевого объекта вычислений.

* Создайте объект `ScriptRunConfig`, используя конфигурацию запуска для целевого объекта вычислений.
* Выполните прогон.
* Дождитесь завершения прогона.

В следующем примере используется конфигурация для локального целевого объекта вычислений, управляемого системой, который вы создали ранее в этом документе:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```

Записную книжку Jupyter с примером обучения с помощью Spark в HDInsight см. здесь: [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

### <a name="submit-using-a-pipeline"></a>Отправка с помощью конвейера

Шаблон кода для отправки запуска на выполнение обучения с помощью конвейера будет одинаковым, независимо от целевого объекта вычислений.

* Для ресурсов вычисления добавьте шаг в конвейере.
* С помощью конвейера отправьте запуск на выполнение.
* Дождитесь завершения прогона.

В следующем примере используется созданный ранее целевой объект вычислений Azure Databricks.

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Дополнительные сведения о конвейерах машинного обучения см. в статье [Конвейеры и служба "Машинное обучение Azure"](concept-ml-pipelines.md).

Например, Jupyter Notebook, которые демонстрируют обучение с помощью конвейера, см. [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Просмотр и настройка вычислений с помощью портала Azure

Вы можете узнать, какие целевые объекты вычислений связаны с вашей рабочей областью, на портале Azure. Чтобы получить список этих объектов, выполните следующие действия:

1. Откройте [портал Azure](https://portal.azure.com) и перейдите к своей рабочей области.
2. Щелкните ссылку __Вычисление__ в разделе __Приложения__.

    ![Просмотр вкладки вычислений](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Создание целевого объекта вычислений

Откройте список целевых объектов вычислений согласно приведенным выше инструкциям, а затем создайте целевой объект вычислений, выполнив следующие действия:

1. Щелкните значок __+__, чтобы добавить целевой объект вычислений.

    ![Добавить вычисление ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Введите имя для целевого объекта вычислений.
1. Выберите тип вычислений для присоединения к __обучению__. 

    > [!IMPORTANT]
    > С помощью портала Azure не можно создать все типы вычислений. Ниже приведен список типов, которые можно создать для обучения в настоящее время.
    > 
    > * Виртуальная машина
    > * Искусственный интеллект пакетной службы

1. Выберите __Создать__ и заполните необходимую форму. 
1. Нажмите кнопку __Создать__
1. Чтобы увидеть состояние операции создания, выберите целевой объект вычислений в списке.

    ![Просмотр списка вычислений.](./media/how-to-set-up-training-targets/View_list.png) Ниже приведены сведения о выбранном целевом объекте вычислений.
    ![Подробнее](./media/how-to-set-up-training-targets/vm_view.PNG)
1. Теперь вы можете выполнить прогон для этих целевых объектов, как описано выше.

### <a name="reuse-existing-compute-in-your-workspace"></a>Повторное использование существующих вычислений в рабочей области

Откройте список целевых объектов вычислений согласно приведенным выше инструкциям, а затем используйте существующий целевой объект вычислений еще раз, выполнив следующие действия:

1. Щелкните значок **+**, чтобы добавить целевой объект вычислений.
2. Введите имя для целевого объекта вычислений.
3. Выберите тип вычислений для присоединения к обучению.

    > [!IMPORTANT]
    > С помощью портала не можно вложить все типы вычислений.
    > Ниже приведен список типов, которые можно вложить для обучения.
    > 
    > * Виртуальная машина
    > * Искусственный интеллект пакетной службы

1. Выберите вариант "Использовать существующий".
    - При присоединении кластеров Batch AI выберите целевой объект вычислений из раскрывающегося списка, выберите рабочую область Batch AI и кластер Batch AI, а затем нажмите кнопку **Создать**.
    - При присоединении виртуальной машины введите IP-адрес, имя пользователя и пароль, закрытый и открытого ключи и порт и нажмите кнопку "Создать".

    > [!NOTE]
    > Майкрософт рекомендует использовать ключи SSH, поскольку они безопаснее, чем пароли. Пароли уязвимы для атак методом подбора, в то время как ключи SSH задействуют криптографические подписи. Сведения о создании ключей SSH для использования на виртуальных машинах Azure см. в следующих документах:
    >
    > * [Создание и использование ключей SSH в Linux или macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Создание и использование ключей SSH в Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. Состояние подготовки можно узнать, выбрав целевой объект вычислений в списке вычислений.
6. Теперь вы можете выполнить прогон для этих целевых объектов.

## <a name="examples"></a>Примеры
Основные понятия, описанные в этой статье, демонстрируют следующие записные книжки:
* [01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm)
* [01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci)
* [01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark)
* [tutorials/01.train-models.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb)

Получите записные книжки: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Дополнительная информация

* [Справка по пакету SDK для машинного обучения Azure](https://aka.ms/aml-sdk)
* [Руководство. Обучение модели](tutorial-train-models-with-aml.md)
* [Где следует развертывать модели](how-to-deploy-and-where.md)
* [Создание конвейеров машинного обучения с помощью службы машинного обучения Azure](concept-ml-pipelines.md)
