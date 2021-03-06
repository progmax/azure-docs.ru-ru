---
title: Распространенные вопросы о службе "Сетка Azure Service Fabric" | Документация Майкрософт
description: Изучите ответы на распространенные вопросы о службе "Сетка Azure Service Fabric".
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f80f61cbfc1f7b719e73d7d29c6948bebe84aa6c
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278316"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Распространенные вопросы о службе "Сетка Service Fabric"
Сетка Azure Service Fabric — это полностью управляемая служба, которая позволяет разработчикам развертывать приложения для микрослужб без управления виртуальными машинами, хранилищем или сетями. Эта статья содержит ответы на часто задаваемые вопросы.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Как сообщить о проблеме или задать вопрос?

Можно задать вопросы, получить ответы от инженеров Майкрософт и сообщить о проблемах в [репозитории GitHub service-fabric-mesh-preview](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Квоты и стоимость

**Какова стоимость использования предварительной версии?**

Плата за развертывание приложений или контейнеров в предварительной версии службы "Сетка" на данный момент не взимается. Тем не менее рекомендуется удалять развернутые ресурсы и не оставлять их работающими, если только они не используются для тестирования.

**Существует ли предел квоты числа ядер и объема ОЗУ?**

Да для каждой из подписок установлены следующее квоты.

- Число приложений: 5. 
- Ядер на приложение: 12. 
- Общий объем оперативной памяти на приложение: 48 ГБ. 
- Конечных точек сети и входящего трафика: 5.  
- Томов Azure, которые можно подключить: 10. 
- Число реплик службы: 3. 
- Наибольший контейнер, который вы можете развернуть, может использовать 4 ядра и 16 ГБ ОЗУ.
- Вы можете выделять для контейнеров частичные ядра с шагом в 0,5 ядра, но не более 6 ядер.

**Сколько времени приложение может находится в развернутом состоянии.**

На данный момент продолжительность времени существования приложения ограничена двумя днями. Это сделано для эффективного использования свободных ядер, выделенных для предварительного просмотра. Таким образом, вы сможете выполнять развертывание на протяжении не более чем 48 часов, после чего оно будет отключено системой. Если приложение было отключено, вы можете проверить, было ли это сделано системой, с помощью `az mesh app show`команды в Azure CLI, а также узнать, можно ли его восстановить`"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."`. 

Например:  

```cli
chackdan@Azure:~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Чтобы продолжить развертывание приложения в Сетке, удалите группу ресурсов, связанную с приложением, или самостоятельно удалите приложение и все связанные с ним ресурсы Сетки (включая сеть). 

Чтобы удалить группу ресурсов, используйте команду `az group delete <nameOfResourceGroup>`. 

## <a name="supported-container-os-images"></a>Поддерживаемые образы ОС контейнера
Ниже приведены образы ОС контейнера, которые можно использовать при развертывании служб.

- Windows: windowsservercore и nanoserver
    - Windows Server 2016
    - Windows Server, версия 1709
- Linux
    - Известные ограничения отсутствуют

## <a name="features-gaps-and-known-issues"></a>Недостатки функций и известные проблемы

**После развертывания приложения сетевой ресурс, связанный с ним, не имеет IP-адреса**

На сегодня известна проблема с IP-адресом, который назначается с задержкой. Проверьте состояние сетевого ресурса через несколько минут, чтобы просмотреть связанный IP-адрес.

**Мое приложение не может получить доступ к соответствующему сетевому ресурсу или ресурсу тома**

В модели приложения необходимо использовать полный идентификатор ресурса для сетей и томов, чтобы иметь к ним доступ. Ниже приведен пример из краткого руководства.

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**Я не вижу текущую модель приложения, поддерживающую способ шифрования моих секретов**

Да, шифрование секретов в текущей закрытой предварительной версии не поддерживается. 

**DNS работает не так, как в кластере разработки Service Fabric и службе "Сетка"**

Известна проблема, из-за которой могут по-разному указываться ссылки на службы в локальном кластере разработки и службе "Сетка Azure". В локальном кластере разработки используйте формат {имя_службы}. {имя_приложения}. В службе "Сетка Azure Service Fabric" используйте формат {имя_службы}. Сейчас служба "Сетка Azure" не поддерживает разрешение DNS между приложениями.

Другие известные проблемы с DNS при запуске кластера разработки Service Fabric в Windows 10 описаны здесь: [Практическое руководство. Отладка контейнеров Windows в Azure Service Fabric с помощью Visual Studio 2017](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**При использовании модуля интерфейса командной строки ImportError возникает ошибка "Не удается импортировать имя "sdk_no_wait"**

Если вы используете версию интерфейса командной строки, предшествующую версии 2.0.30, может произойти приведенная ниже ошибка.

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**При установке пакета расширения интерфейса командной строки возникает ошибка несоответствия имени дистрибутива**

Это не означает, что расширение не установлено. Вы сможете использовать команды интерфейса командной строки без каких-либо проблем.

**При развертывании я вижу, что затрагиваются все контейнеры, включая работающие**

Это ошибка, которая должна быть устранена в следующем обновлении для среды выполнения.

## <a name="next-steps"></a>Дополнительная информация

Чтобы узнать больше о службе "Сетка Service Fabric", прочитайте этот [обзор](service-fabric-mesh-overview.md).
