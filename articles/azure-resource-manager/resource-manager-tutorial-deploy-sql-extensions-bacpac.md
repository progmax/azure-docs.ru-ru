---
title: Импорт BACPAC-файлов SQL с помощью шаблонов Azure Resource Manager | Документация Майкрософт
description: Сведения о том, как использовать расширение Базы данных SQL для импорта BACPAC-файлов SQL с помощью шаблонов Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/06/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 249356644772ae75b12f5c940ff5f9ed49b2c795
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2018
ms.locfileid: "52994991"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Руководство. Импорт BACPAC-файлов SQL с помощью шаблонов Azure Resource Manager

Здесь приведены сведения о том, как использовать расширение Базы данных SQL Azure для импорта BACPAC-файлов SQL с помощью шаблонов Azure Resource Manager. Артефакты развертывания — это любые файлы, которые помимо файла основного шаблона необходимы для выполнения развертывания. BACPAC-файл является артефактом. В этом руководстве вы создадите шаблон для развертывания Azure SQL Server, Базы данных SQL и импорта BACPAC-файла. Сведения о развертывании расширений виртуальной машины Azure с помощью шаблонов Azure Resource Manager см. в статье [Руководство. Развертывание расширений виртуальной машины с помощью шаблонов Azure Resource Manager](./resource-manager-tutorial-deploy-vm-extensions.md).

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * Подготовка BACPAC-файла
> * Открытие шаблона быстрого запуска
> * Изменение шаблона
> * Развертывание шаблона
> * Проверка развертывания

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этой статьей необходимо иметь следующее.

* [Visual Studio Code](https://code.visualstudio.com/) с расширением средств Resource Manager. См. в разделе [Краткое руководство. Создание шаблона Azure Resource Manager c помощью Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Для повышения уровня безопасности используйте пароль, созданный для учетной записи администратора SQL Server. Ниже приведен пример создания пароля.

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Для защиты криптографических ключей и других секретов используйте Azure Key Vault. Дополнительные сведения см. в статье [Руководство. Интеграция Azure Key Vault в развертывание шаблона Resource Manager](./resource-manager-tutorial-use-key-vault.md). Мы также рекомендуем обновлять пароль каждые три месяца.

## <a name="prepare-a-bacpac-file"></a>Подготовка BACPAC-файла

BACPAC-файл совместно используется в [учетной записи хранения Azure](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac) с общим доступом. Сведения о создании собственного файла см. в статье [Экспорт базы данных SQL Azure в BACPAC-файл](../sql-database/sql-database-export.md). Если вы решили опубликовать файл в своем расположении, необходимо обновить шаблон позже в этом руководстве.

## <a name="open-a-quickstart-template"></a>Открытие шаблона быстрого запуска

Шаблоны быстрого запуска Azure являются репозиторием для шаблонов Resource Manager. Вместо создания шаблона с нуля можно найти пример шаблона и настроить его. Шаблон, используемый в этом руководстве, называется [Deploy an Azure SQL Server with Threat Detection](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/) (Развертывание Azure SQL Server с обнаружением зависимостей).

1. В Visual Studio Code выберите **Файл**>**Открыть файл**.
2. Скопируйте приведенный ниже URL-адрес и вставьте его в поле **Имя файла**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json
    ```
3. Чтобы открыть файл, выберите **Открыть**.

    В шаблоне определено три ресурса:

    * `Microsoft.Sql/servers`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
    * `Microsoft.SQL/servers/securityAlertPolicies`. Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
    * `Microsoft.SQL.servers/databases`.  Ознакомьтесь со статьей о [справочнике по шаблонам](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).

    Прежде чем настраивать шаблон, рекомендуется получить основные сведения о нем.
4. Выберите **Файл**>**Сохранить как**, чтобы сохранить файл на локальный компьютер с именем **azuredeploy.json**.

## <a name="edit-the-template"></a>Изменение шаблона

Добавьте в шаблон два дополнительных ресурса.

* Чтобы разрешить расширению Базы данных SQL импортировать BACPAC-файлы, необходимо разрешить доступ к службам Azure. Добавьте следующий код JSON в определение SQL Server:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2015-05-01-preview",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    Шаблон должен выглядеть так:

    ![Развертывание BACPAC-файла в расширении SQL с помощью Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Добавьте ресурс расширения Базы данных SQL в определение базы данных с помощью следующего кода JSON:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    Шаблон должен выглядеть так:

    ![Развертывание BACPAC-файла в расширении SQL с помощью Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Сведения об определении ресурса см. в [справочнике о расширении Базы данных SQL](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions). Ниже приведены некоторые важные элементы.

    * **dependsOn**. Ресурс расширения должен быть создан после создания базы данных SQL.
    * **storageKeyType**. Тип ключа к хранилищу данных, который необходимо использовать. Значение может быть `StorageAccessKey` или `SharedAccessKey`. Так как предоставленный BACPAC-файл совместно используется в учетной записи хранения Azure с общим доступом, здесь используется SharedAccessKey.
    * **storageKey**. Ключ к хранилищу данных, который необходимо использовать. Если тип ключа к хранилищу данных — SharedAccessKey, он должен начинаться с "?".
    * **storageUri**. Код URI хранилища, который необходимо использовать. Если вы решили не использовать указанный BACPAC-файл, необходимо обновить значения.
    * **administratorLoginPassword**. Пароль администратора SQL. Используйте сгенерированный пароль. См. раздел [Предварительные требования](#prerequisites).

## <a name="deploy-the-template"></a>Развертывание шаблона

Дополнительные сведения о процедуре развертывания шаблона см. в [этом разделе](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Вместо этого используйте следующий сценарий развертывания PowerShell:

```azurepowershell
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the SQL admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmResourceGroupDeployment -Name $deploymentName `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile azuredeploy.json
```

Используйте сгенерированный пароль. См. раздел [Предварительные требования](#prerequisites).

## <a name="verify-the-deployment"></a>Проверка развертывания

На портале выберите Базу данных SQL из только что развернутой группы ресурсов. Выберите **Редактор запросов (предварительная версия)**, а затем введите учетные данные администратора. Должны отобразиться две таблицы, импортированные в базу данных:

![Развертывание BACPAC-файла в расширении SQL с помощью Azure Resource Manager](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Если ресурсы Azure больше не нужны, их можно удалить. Для этого необходимо удалить группу ресурсов.

1. На портале Azure в меню слева выберите **Группа ресурсов**.
2. В поле **Filter by name** (Фильтровать по имени) введите имя группы ресурсов.
3. Выберите имя группы ресурсов.  В группе ресурсов должно появится шесть ресурсов.
4. В главном меню выберите **Удалить группу ресурсов**.

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы развернули SQL Server, Базу данных SQL и импортированный BACPAC-файл. BACPAC-файл хранится в учетной записи хранения Azure. К нему может получить доступ любой пользователь, у которого есть URL-адрес. Сведения о защите BACPAC-файла (артефакта) см. в следующем ресурсе:

> [!div class="nextstepaction"]
> [Защита артефактов](./resource-manager-tutorial-secure-artifacts.md)
