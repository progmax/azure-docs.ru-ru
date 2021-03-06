---
title: Управление секретами приложения "Сетка Azure Service Fabric" | Документация Майкрософт
description: Управляйте секретами приложений, чтобы безопасно создать и развернуть приложение "Сетка Service Fabric".
services: service-fabric-mesh
keywords: секретные коды
author: aljo
ms.author: aljo
ms.date: 11/28/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: chackdan
ms.openlocfilehash: d92726ebc2cd4c6c44afdb2d2a9f53ab5441ac32
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/05/2018
ms.locfileid: "52891971"
---
# <a name="manage-service-fabric-mesh-application-secrets"></a>Управление секретами приложения "Сетка Service Fabric"
Сетка Service Fabric поддерживает секреты в качестве ресурсов Azure. Секрет сетки Service Fabric может быть любой конфиденциальной текстовой информацией, например строками подключения к хранилищу, паролями или другими значениями, которые должны храниться и передаваться безопасно. В этой статье показано, как использовать службу Secure Store Service Fabric для развертывания и поддержки секретов.

Секрет приложения сетки состоит из следующих компонентов.
* Ресурс **Секреты** — контейнер, в котором хранятся текстовые секреты. Секреты, находящиеся в пределах ресурса **Секреты**, хранятся и передаются безопасно.
* Один или несколько ресурсов **Секреты/Значения**, которые хранятся в контейнере ресурса **Секреты**. Каждый ресурс **Секреты/Значения** отличается номером версии. Изменить версию ресурса **Секреты/Значения** невозможно, можно только добавить новую версию.

Управление секретами заключается в следующих действиях:
1. объявление ресурса сетки **Секреты** в файле YAML модели ресурсов Azure или файле JSON с помощью типа inlinedValue и определения contentType SecretsStoreRef;
2. объявление ресурсов сетки **Секреты/Значения** в файле YAML модели ресурсов Azure или файле JSON, который будет храниться в ресурсе **Секреты** (из шага 1);
3. изменение приложения сетки для ссылки на значения секретов сетки;
4. развертывание или последовательное обновление приложения сетки для использования значений секретов;
5. использование команд Azure CLI "az" для управления жизненным циклом службы Secure Store.

## <a name="declare-a-mesh-secrets-resource"></a>Объявление ресурса секретов сетки
Ресурс сетки "Секреты" объявляется в файле модели ресурсов Azure YAML или JSON с помощью типа inlinedValue и определения contentType SecretsStoreRef. Ресурс секретов сетки поддерживает секреты, поставляемые службой Secure Store. 
>
Ниже представлен пример объявления ресурсов сетки "Секреты" в файле JSON.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "eastus",
      "metadata": {
        "description": "Location of the resources."
      }
    },
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "MySecret.txt",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "kind": "inlinedValue",
        "description": "My Mesh Application Secret",
        "contentType": "SecretsStoreRef",
        "value": "mysecret",
      }
    },
  ]
}
```
Ниже представлен пример объявления ресурсов сетки "Секреты" в файле YAML.
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="declare-mesh-secretsvalues-resources"></a>Объявление ресурсов сетки "Секреты/Значения"
Ресурсы сетки "Секреты/Значения" зависят от ресурса сетки "Секреты", определенного на предыдущем шаге.

Относительно связи между полями "value:" и "name:" раздела "resources": во второй части строки "name:", отделенной двоеточием, указан номер версии, используемый для секрета, а имя перед двоеточием должно совпадать со значением секрета сетки, от которого она зависит. Например, для элемента ```name: mysecret:1.0``` номер версии — 1.0, а имя ```mysecret``` должно совпадать с ранее определенным ```"value": "mysecret"```.

>
Ниже представлен пример объявления ресурсов сетки "Секреты/Значения" в файле JSON.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "eastus",
      "metadata": {
        "description": "Location of the resources."
      }
    },
    "my-secret-value-v1": {
      "type": "string",
      "metadata": {
        "description": "My Mesh Application Secret Value."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
      "name": "MySecret.txt",
      "type": "Microsoft.ServiceFabricMesh/secrets",
      "location": "[parameters('location')]",
      "properties": {
        "kind": "inlinedValue",
        "description": "My Mesh Application Secret",
        "contentType": "SecretsStoreRef",
        "value": "mysecret",
      }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "mysecret:1.0",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        'Microsoft.ServiceFabricMesh/secrets/MySecret.txt'
      ],
      "properties": {
        "value": "[parameters('my-secret-value-v1)]"
      }
    }
  ]
}
```
Ниже представлен пример объявления ресурсов сетки "Секреты/Значения" в файле YAML.
```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
          Secrets:
            - name: MySecret.txt
            description: My Mesh Application Secret
            secret_type: inlinedValue
            content_type: SecretStoreRef
            value: mysecret
            - name: mysecret:1.0
            description: My Mesh Application Secret Value
            secret_type: value
            content_type: text/plain
            value: "P@ssw0rd#1234"
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

## <a name="modify-mesh-application-to-reference-mesh-secret-values"></a>Изменение приложения сетки для ссылки на значения секретов сетки
Приложения сетки Service Fabric должны учитывать две приведенные ниже строки, чтобы использовать значения секретов службы Secure Store.
1. Micrsoft.ServiceFabricMesh/Secrets.name содержит имя файла и будет содержать значение секретов в виде открытого текста.
2. Переменная среды "Fabric_SettingPath" в Windows или Linux содержит путь к каталогу, в котором файлы, содержащие значения секретов службы Secure Store, будут доступны. Для приложений сетки, размещенных в Windows — это "C:\Settings", а в Linux — "/var/settings".

## <a name="deploy-or-use-a-rolling-upgrade-for-mesh-application-to-consume-secret-values"></a>Развертывание или последовательное обновление приложения сетки для использования значений секретов
Создание ресурсов "Секреты" и/или версионных ресурсов "Секреты/Значения" ограничено объявленными развертываниями модели ресурсов. Единственный способ создать эти ресурсы — передать файл JSON или YAML модели ресурсов с помощью команды **az mesh deployment**, как показано ниже.

```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```

## <a name="azure-cli-commands-for-secure-store-service-lifecycle-management"></a>Команды Azure CLI для управления жизненным циклом службы Secure Store

### <a name="create-a-new-secrets-resource"></a>Создание нового ресурса "Секреты"
```azurecli-interactive
az mesh deployment create –-<template-file> or --<template-uri>
```
Передайте или **template-file**, или **template-uri** (но не оба).

Например: 
- az mesh deployment create — c:\MyMeshTemplates\SecretTemplate1.txt
- az mesh deployment create — https://www.fabrikam.com/MyMeshTemplates/SecretTemplate1.txt

### <a name="show-a-secret"></a>Отображение секрета
Возвращает описание секрета (но не его значение).
```azurecli-interactive
az mesh secret show --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="delete-a-secret"></a>Удаление секрета

- Удалить секрет невозможно, пока на него ссылается приложение сетки.
- При удалении ресурса "Секреты" удаляются все версии секретов и ресурсов.
```azurecli-interactive
az mesh secret delete --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="list-secrets-in-subscription"></a>Перечисление секретов в подписке
```azurecli-interactive
az mesh secret list
```
### <a name="list-secrets-in-resource-group"></a>Перечисление секретов в группе ресурсов
```azurecli-interactive
az mesh secret list -g <myResourceGroup>
```
### <a name="list-all-versions-of-a-secret"></a>Перечисление всех версий секрета
```azurecli-interactive
az mesh secretvalue list --Resource-group <myResourceGroup> --secret-name <mySecret>
```

### <a name="show-secret-version-value"></a>Отображение значения версии секрета
```azurecli-interactive
az mesh secretvalue show --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

### <a name="delete-secret-version-value"></a>Удаление значения версии секрета
```azurecli-interactive
az mesh secretvalue delete --Resource-group <myResourceGroup> --secret-name <mySecret> --version <N>
```

## <a name="next-steps"></a>Дополнительная информация 
Чтобы узнать больше о службе "Сетка Service Fabric", прочитайте этот обзор:
- [Обзор службы "Сетка Service Fabric"](service-fabric-mesh-overview.md)
