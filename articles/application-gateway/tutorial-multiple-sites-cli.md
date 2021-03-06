---
title: Создание шлюза приложений для размещения нескольких веб-сайтов с помощью Azure CLI
description: Узнайте, как создать шлюз приложений, на котором размещено несколько веб-сайтов, с помощью Azure CLI.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/14/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 27db76087165e37db936e802a01ddc4ecd269f4c
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874436"
---
# <a name="tutorial-create-an-application-gateway-that-hosts-multiple-web-sites-using-the-azure-cli"></a>Руководство. Создание шлюза приложений, на котором размещено несколько веб-сайтов, с помощью Azure CLI

Чтобы настроить [размещение нескольких веб-сайтов](multiple-site-overview.md) при создании [шлюза приложений](overview.md), можно использовать Azure CLI. В этом руководстве описано, как создать внутренние пулы адресов с помощью масштабируемых наборов виртуальных машин. Затем вы настроите прослушиватели и правила на основе принадлежащих вам доменов, чтобы обеспечить передачу веб-трафика на соответствующие серверы в пулах. В этом руководстве предполагается, что вам принадлежат несколько доменов. Для примера здесь используются домены *www.contoso.com* и *www.fabrikam.com*.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Настройка сети
> * Создание шлюза приложений
> * Создание серверных прослушивателей
> * Создание правил маршрутизации
> * создание масштабируемых наборов виртуальных машин с внутренними пулами.
> * создание записи CNAME в домене.

![Пример маршрутизации нескольких сайтов](./media/tutorial-multiple-sites-cli/scenario.png)


При необходимости инструкции из этого руководства можно выполнить с помощью [Azure PowerShell](tutorial-multiple-sites-powershell.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций в этом руководстве вам понадобится Azure CLI 2.0.4 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими. Создайте группу ресурсов, используя команду [az group create](/cli/azure/group#create).

В следующем примере создается группа ресурсов с именем *myResourceGroupAG* в расположении *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Создание сетевых ресурсов 

Создайте виртуальную сеть и подсеть с именем *myAGSubnet* с помощью команды [az network vnet create](/cli/azure/network/vnet#az-net). Затем добавьте подсеть, требуемую для внутренних серверов, используя команду [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create). Создайте общедоступный IP-адрес с именем *myAGPublicIPAddress*, используя команду [az network public-ip create](/cli/azure/network/public-ip#az-network_public_ip_create).

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24

az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --address-prefix 10.0.2.0/24

az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress
```

## <a name="create-the-application-gateway"></a>Создание шлюза приложений

Шлюз приложений можно создать с помощью команды [az network application-gateway create](/cli/azure/network/application-gateway#create). При создании шлюза приложений с помощью Azure CLI укажите такие сведения о конфигурации, как емкость, номер SKU и параметры HTTP. Шлюз приложений назначается подсети *myAGSubnet* и адресу *myAGPublicIPAddress*, созданным ранее. 

```azurecli-interactive
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet \
  --subnet myAGsubnet \
  --capacity 2 \
  --sku Standard_Medium \
  --http-settings-cookie-based-affinity Disabled \
  --frontend-port 80 \
  --http-settings-port 80 \
  --http-settings-protocol Http \
  --public-ip-address myAGPublicIPAddress
```

Создание шлюза приложений может занять несколько минут. Когда шлюз приложений будет создан, вы увидите такие новые функции шлюза:

- *appGatewayBackendPool* — шлюз приложений должен иметь по крайней мере один внутренний пул адресов.
- *appGatewayBackendHttpSettings* — указывает, что для обмена данными используются порт 80 и протокол HTTP.
- *appGatewayHttpListener* — прослушиватель по умолчанию, связанный с *appGatewayBackendPool*.
- *appGatewayFrontendIP* — назначает адрес *myAGPublicIPAddress* для прослушивателя *appGatewayHttpListener*.
- *rule1* — правило маршрутизации по умолчанию, связанное с прослушивателем *appGatewayHttpListener*.

### <a name="add-the-backend-pools"></a>Добавление пулов серверной части

Добавьте пулы серверной части с именами, которые требуются для размещения внутренних серверов, с помощью команды [az network application-gateway address-pool create](/cli/azure/network/application-gateway#az-network_application_gateway_address_pool_create).

```azurecli-interactive
az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name contosoPool

az network application-gateway address-pool create \
  --gateway-name myAppGateway \
  --resource-group myResourceGroupAG \
  --name fabrikamPool
```

### <a name="add-backend-listeners"></a>Добавление серверных прослушивателей

Добавьте серверные прослушиватели, необходимые для маршрутизации трафика, при помощи команды [az network application-gateway http-listener create](/cli/azure/network/application-gateway#az-network_application_gateway_http_listener_create).

```azurecli-interactive
az network application-gateway http-listener create \
  --name contosoListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.contoso.com

az network application-gateway http-listener create \
  --name fabrikamListener \
  --frontend-ip appGatewayFrontendIP \
  --frontend-port appGatewayFrontendPort \
  --resource-group myResourceGroupAG \
  --gateway-name myAppGateway \
  --host-name www.fabrikam.com   
  ```

### <a name="add-routing-rules"></a>Добавление правил маршрутизации

Правила обрабатываются в том порядке, в котором они указаны, а трафик направляется через первое подходящее правило, независимо от точности. Например, если для одного порта имеется правило с базовым прослушивателем и правило с многосайтовым прослушивателем, для обеспечения правильной работы правило с многосайтовым прослушивателем должно быть указано перед правилом с базовым прослушивателем. 

В этом примере создаются два новых правила и удаляется правило по умолчанию, которое было сгенерировано при создании шлюза приложений. Вы можете добавить правило с помощью команды [az network application-gateway rule create](/cli/azure/network/application-gateway#az-network_application_gateway_rule_create).

```azurecli-interactive
az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name contosoRule \
  --resource-group myResourceGroupAG \
  --http-listener contosoListener \
  --rule-type Basic \
  --address-pool contosoPool

az network application-gateway rule create \
  --gateway-name myAppGateway \
  --name fabrikamRule \
  --resource-group myResourceGroupAG \
  --http-listener fabrikamListener \
  --rule-type Basic \
  --address-pool fabrikamPool

az network application-gateway rule delete \
  --gateway-name myAppGateway \
  --name rule1 \
  --resource-group myResourceGroupAG
```

## <a name="create-virtual-machine-scale-sets"></a>Создание масштабируемых наборов виртуальных машин

В этом примере вы создадите три масштабируемых набора виртуальных машин, которые поддерживают три пула серверной части в шлюзе приложений. Имена создаваемых масштабируемых наборов — *myvmss1*, *myvmss2* и *myvmss3*. Каждый масштабируемый набор содержит два экземпляра виртуальной машины, на которых устанавливаются службы IIS.

```azurecli-interactive
for i in `seq 1 2`; do

  if [ $i -eq 1 ]
  then
    poolName="contosoPool"
  fi

  if [ $i -eq 2 ]
  then
    poolName="fabrikamPool"
  fi

  az vmss create \
    --name myvmss$i \
    --resource-group myResourceGroupAG \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Azure123456! \
    --instance-count 2 \
    --vnet-name myVNet \
    --subnet myBackendSubnet \
    --vm-sku Standard_DS2 \
    --upgrade-policy-mode Automatic \
    --app-gateway myAppGateway \
    --backend-pool-name $poolName
done
```

### <a name="install-nginx"></a>Установка nginx

```azurecli-interactive
for i in `seq 1 2`; do

  az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group myResourceGroupAG \
    --vmss-name myvmss$i \
    --settings '{ "fileUris": ["https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/install_nginx.sh"],
  "commandToExecute": "./install_nginx.sh" }'

done
```

## <a name="create-a-cname-record-in-your-domain"></a>создание записи CNAME в домене.

После создания шлюза приложений с общедоступным IP-адресом можно получить DNS-адрес и использовать его для создания записи CNAME в своем домене. С помощью команды [az network public-ip show](/cli/azure/network/public-ip#az-network_public_ip_show) можно получить DNS-адрес шлюза приложений. Скопируйте значение *fqdn* для DNSSettings и используйте его в качестве значения создаваемой записи CNAME. 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [dnsSettings.fqdn] \
  --output tsv
```

Использовать записи A не рекомендуется, так как виртуальный IP-адрес может измениться после перезапуска шлюза приложений.

## <a name="test-the-application-gateway"></a>Тестирование шлюза приложений

В адресной строке браузера введите имя домена. Например, http://www.contoso.com.

![Проверка сайта contoso в шлюзе приложений](./media/tutorial-multiple-sites-cli/application-gateway-nginxtest1.png)

Введите адрес другого домена. Результат должен быть примерно таким:

![Проверка сайта fabrikam в шлюзе приложений](./media/tutorial-multiple-sites-cli/application-gateway-nginxtest2.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

При необходимости вы можете удалить группу ресурсов, шлюз приложений и все связанные ресурсы.

```azurecli-interactive
az group delete --name myResourceGroupAG --location eastus
```

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Настройка сети
> * Создание шлюза приложений
> * Создание серверных прослушивателей
> * Создание правил маршрутизации
> * создание масштабируемых наборов виртуальных машин с внутренними пулами.
> * создание записи CNAME в домене.

> [!div class="nextstepaction"]
> [Создание шлюза приложений с правилами маршрутизации на основе URL-путей](./tutorial-url-route-cli.md)