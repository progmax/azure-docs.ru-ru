---
title: Защита веб-служб Машинного обучения Azure с помощью SSL
description: Узнайте, как защитить веб-службы, развернутые с помощью службы Машинного обучения Azure. Вы можете ограничить доступ к веб-службам и защитить данные, отправленные клиентами, с помощью SSL и проверки подлинности на основе ключа.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 10/02/2018
ms.openlocfilehash: ec7b956f080837b297bac56e6237ac0672601ce7
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344490"
---
# <a name="secure-azure-machine-learning-web-services-with-ssl"></a>Защита веб-служб Машинного обучения Azure с помощью SSL

В этой статье вы узнаете, как защитить веб-службы, развернутые с помощью службы Машинного обучения Azure. Вы можете ограничить доступ к веб-службам и защитить данные, отправленные клиентами, с помощью SSL и проверки подлинности на основе ключа.

> [!Warning]
> Если вы не включите SSL, любой пользователь в Интернете сможет совершать вызовы к веб-службе.

SSL шифрует данные, отправляемые между клиентом и веб-службой. Он также используется клиентом для проверки удостоверения сервера. Аутентификация разрешена только для служб, которые предоставили SSL-сертификат и ключ.  Если вы включите SSL, при доступе к веб-службе потребуется ключ проверки подлинности.

При развертывании веб-службы, для которой включен протокол SSL, или при включении SSL для существующей развернутой веб службы действия будут одинаковы:

1. Получите имя домена.

2. Получите SSL-сертификат.

3. Разверните или обновите веб-службу со включенным параметром SSL.

4. Обновите имя DNS, чтобы оно указывало на веб-службу.

При защите веб-служб в разных [целевых объектах развертывания](how-to-deploy-and-where.md) есть небольшие отличия. 

## <a name="get-a-domain-name"></a>Получение доменного имени

Если у вас еще нет имени домена, вы можете приобрести его у __регистратора доменных имен__. Процесс различается у разных регистраторов, как и стоимость. Регистратор также предоставляет вам инструменты для управления доменным именем. Эти инструменты используются для сопоставления полного доменного имени (например, www.contoso.com) с IP-адресом, по которому размещается ваша веб-служба.

## <a name="get-an-ssl-certificate"></a>Получите SSL-сертификат.

Существует множество способов получить SSL-сертификат. Наиболее распространенным является покупка в одном из __центров сертификации__. Независимо от того, где вы получаете сертификат, вам нужны следующие файлы:

* __Сертификат__. Сертификат должен содержать полную цепочку сертификатов и должен быть в кодировке PEM.
* __Ключ__. Ключ должен быть в кодировке PEM.

При запросе сертификата вы должны предоставить полное доменное имя (FQDN) адреса, который вы планируете использовать для этой веб-службы. Например, www.contoso.com. Адрес, указанный в сертификате, и адрес, используемый клиентами, сравниваются при проверке удостоверения веб-службы. Если адреса не совпадают, клиенты получат сообщение об ошибке. 

> [!TIP]
> Если центр сертификации не может предоставить сертификат и ключ в виде файлов в кодировке PEM, вы можете использовать такую служебную программу, как [OpenSSL](https://www.openssl.org/), чтобы изменить формат.

> [!WARNING]
> Эти самозаверяющие сертификаты следует использовать только для разработки. Они не должны использоваться в рабочей среде. Самозаверяющие сертификаты могут вызвать проблемы в клиентских приложениях. Дополнительные сведения см. в документации для сетевых библиотек, используемых в клиентском приложении.

## <a name="enable-ssl-and-deploy"></a>Включение SSL и развертывание службы

Чтобы развернуть (или развернуть повторно) службу со включенным SSL, установите для параметра `ssl_enabled` значение `True` везде, где это применимо. Установите для параметра `ssl_certificate` значение файла __сертификата__, а для параметра `ssl_key` значение __ключа__. 

+ **Развертывание в Службе Azure Kubernetes (AKS)**
  
  При подготовке кластера AKS укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from azureml.core.compute import AksCompute
    
    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Развертывание в Экземплярах контейнеров Azure (ACI)**
 
  При развертывании в ACI укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from azureml.core.webservice import AciWebservice
    
    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Развертывание в программируемых пользователем вентильных матрицах (FPGA)**

  Ответ операции `create_service` содержит IP-адрес службы. IP-адрес используется при сопоставлении имени DNS с IP-адресом службы. Ответ также содержит __первичный__ и __вторичный ключ__, используемые для применения службы. Укажите значения для параметров, связанных с SSL, как показано во фрагменте кода:

    ```python
    from amlrealtimeai import DeploymentClient
    
    subscription_id = "<Your Azure Subscription ID>"
    resource_group = "<Your Azure Resource Group Name>"
    model_management_account = "<Your AzureML Model Management Account Name>"
    location = "eastus2"
    
    model_name = "resnet50-model"
    service_name = "quickstart-service"
    
    deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)
    
    with open('cert.pem','r') as cert_file:
        with open('key.pem','r') as key_file:
            cert = cert_file.read()
            key = key_file.read()
            service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
    ```

## <a name="update-your-dns"></a>Обновление DNS

Теперь необходимо обновить DNS, чтобы оно указывало на веб-службу.

+ **Для ACI и FPGA**:  

  Используйте инструменты, предоставленные вашим регистратором доменных имен, чтобы обновить запись DNS для вашего доменного имени. Запись должна указывать на IP-адрес службы.  

  В зависимости от регистратора и срока жизни, настроенного для имени домена, может потребоваться от нескольких минут до нескольких часов, прежде чем клиенты смогут разрешить имя домена.

+ **Для AKS**: 

  Обновите имя DNS на вкладке "Конфигурация" окна "Общедоступный IP-адрес" кластера AKS, как показано на изображении. Общедоступный IP-адрес можно найти как один из типов ресурсов в группе ресурсов, содержащей узлы агента AKS и другие сетевые ресурсы.

  ![Служба Машинного обучение Azure. Защита веб-служб с помощью SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)self-

## <a name="next-steps"></a>Дополнительная информация

См. сведения о том, как [использовать модель машинного обучения, развернутую в виде веб-службы](how-to-consume-web-service.md).