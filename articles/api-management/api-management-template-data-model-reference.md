---
title: Справочник по модели данных шаблона в службе управления API Azure | Документация Майкрософт
description: Сведения о представлениях сущностей и типов для распространенных элементов, используемых в моделях данных для шаблонов портала разработчика в службе управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 8c21ed737cab98c9136e1c1991997ff3931a4c9d
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447203"
---
# <a name="azure-api-management-template-data-model-reference"></a>Справочник по модели данных шаблона в службе управления API Azure
В этой статье описываются представления сущностей и типов для распространенных элементов, используемых в моделях данных для шаблонов портала разработчика в службе управления API Azure.  
  
 Дополнительные сведения о работе с шаблонами см. в статье [Настройка портала разработчика в службе управления API Azure с помощью шаблонов](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

Портал разработчика недоступен для ценовой категории "Потребление".

## <a name="reference"></a>Справочные материалы

-   [API](#API)  
-   [Сводные данные API](#APISummary)  
-   [Приложения](#Application)  
-   [Вложение](#Attachment)  
-   [Пример кода](#Sample)  
-   [Comment](#Comment)  
-   [Фильтрация](#Filtering)  
-   [Верхняя часть](#Header)  
-   [HTTP-запрос](#HTTPRequest)  
-   [HTTP-ответ](#HTTPResponse)  
-   [Проблема](#Issue)  
-   [Операция](#Operation)  
-   [Меню операций](#Menu)  
-   [Элемент меню операций](#MenuItem)  
-   [Разбиение по страницам](#Paging)  
-   [Параметр](#Parameter)  
-   [Продукт](#Product)  
-   [Поставщик](#Provider)  
-   [Представление](#Representation)  
-   [Подписка](#Subscription)  
-   [Сводка по подписке](#SubscriptionSummary)  
-   [Сведения об учетной записи пользователя](#UserAccountInfo)  
-   [Вход пользователя](#UseSignIn)  
-   [Регистрация пользователя](#UserSignUp)  
  
##  <a name="API"></a> API  
 Сущность `API` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|id|строка|Идентификатор ресурса. Однозначно идентифицирует API в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `apis/{id}`, где `{id}` — идентификатор API. Это свойство доступно только для чтения.|  
|name|строка|Имя API. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|description|строка|Описание API. Не может быть пустым. Может содержать теги форматирования HTML. Максимальная длина составляет 1000 символов.|  
|serviceUrl|строка|Абсолютный URL-адрес внутренней службы, реализующей этот API.|  
|path|строка|Относительный URL-адрес, однозначно идентифицирующий этот API и все его пути к ресурсам в пределах экземпляра службы управления API. Он добавляется к базовому URL-адресу конечной точки API, указанному во время создания экземпляра службы, чтобы сформировать общедоступный URL-адрес для этого API.|  
|protocols|массив чисел|Описывает, на каких протоколах могут вызываться операции в данном API. Допустимые значения: `1 - http`, `2 - https` или оба.|  
|authenticationSettings|[Параметры проверки подлинности сервера авторизации](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Коллекция параметров проверки подлинности, входящих в этот API.|  
|subscriptionKeyParameterNames|object|Необязательное свойство, которое может использоваться для указания пользовательских имен для параметров запроса и (или) заголовка, содержащих ключ подписки. Это свойство должно содержать хотя бы одно из двух следующих свойств.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> Сводные данные API  
 Сущность `API summary` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|id|строка|Идентификатор ресурса. Однозначно идентифицирует API в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `apis/{id}`, где `{id}` — идентификатор API. Это свойство доступно только для чтения.|  
|name|строка|Имя API. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|description|строка|Описание API. Не может быть пустым. Может содержать теги форматирования HTML. Максимальная длина составляет 1000 символов.|  
  
##  <a name="Application"></a> Приложение  
 Сущность `application` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Уникальный идентификатор приложения.|  
|Название|строка|Название приложения.|  
|ОПИСАНИЕ|строка|Описание приложения.|  
|URL-адрес|URI|URI для приложения.|  
|Version (версия)|строка|Сведения о версии приложения.|  
|Требования|строка|Описание требований для приложения.|  
|Состояние|number|Текущее состояние приложения.<br /><br /> — 0: зарегистрировано.<br /><br /> — 1: отправлено.<br /><br /> — 2: опубликовано.<br /><br /> — 3: отклонено.<br /><br /> — 4: не опубликовано.|  
|RegistrationDate|Datetime|Дата и время регистрации приложения.|  
|CategoryId|number|Категория приложения (финансы, развлечения и т. д.)|  
|DeveloperId|строка|Уникальный идентификатор разработчика, отправившего приложение.|  
|Вложения|коллекция сущностей [Вложение](#Attachment)|Вложения для приложения, например снимки экрана или значки.|  
|Значок|[Вложение](#Attachment)|Значок для приложения.|  
  
##  <a name="Attachment"></a> Вложение  
 Сущность `attachment` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|UniqueId|строка|Уникальный идентификатор вложения.|  
|URL-адрес|строка|URL-адрес ресурса.|  
|type|строка|Тип вложения.|  
|ContentType|строка|Тип носителя вложения.|  
  
##  <a name="Sample"></a> Пример кода  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|title|строка|Имя операции.|  
|snippet|строка|Это свойство является устаревшим и не должно использоваться.|  
|brush|строка|Какая расцветка синтаксиса кода будет использоваться при отображении примера кода. Допустимые значения: `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby` и `csharp`.|  
|шаблон|строка|Имя данного шаблона примера кода.|  
|текст|строка|Заполнитель для части примера фрагмента кода.|  
|метод|строка|Метод HTTP, используемый для операции.|  
|scheme|строка|Протокол, используемый для запроса операции.|  
|path|строка|Путь к операции.|  
|query|строка|Пример строки запроса с заданными параметрами.|  
|host|строка|URL-адрес шлюза службы управления API для API, содержащего эту операцию.|  
|headers|коллекция сущностей [Заголовок](#Header)|Заголовки для этой операции.|  
|parameters|коллекция сущностей [Параметр](#Parameter)|Параметры, определенные для этой операции.|  
  
##  <a name="Comment"></a> Комментарий  
 Сущность `API` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|number|Идентификатор комментария.|  
|CommentText|строка|Текст комментария. Может содержать теги HTML.|  
|DeveloperCompany|строка|Название компании, где работает разработчик.|  
|PostedOn|Datetime|Дата и время, когда был оставлен комментарий.|  
  
##  <a name="Issue"></a> Проблема  
 Сущность `issue` имеет следующие свойства.  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Уникальный идентификатор проблемы.|  
|ApiID|строка|Идентификатор API, в котором была обнаружена эта проблема.|  
|Название|строка|Название проблемы.|  
|ОПИСАНИЕ|строка|Описание проблемы.|  
|SubscriptionDeveloperName|строка|Имя разработчика, который сообщил о проблеме.|  
|IssueState|строка|Текущее состояние проблемы. Возможные значения: Proposed, Opened, Closed.|  
|ReportedOn|Datetime|Дата и время, когда была обнаружена проблема.|  
|Комментарии|коллекция сущностей [Комментарий](#Comment)|Комментарии к этой проблеме.|  
|Вложения|коллекция сущностей [Вложение](api-management-template-data-model-reference.md#Attachment)|Все вложения, добавленные к проблеме.|  
|Службы|коллекция сущностей [API](#API)|API, на которые подписан пользователь, отправивший проблему.|  
  
##  <a name="Filtering"></a> Фильтрация  
 Сущность `filtering` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Модель|строка|Текущий поисковый запрос или `null` при его отсутствии.|  
|Placeholder|строка|Текст, отображаемый в поле поиска, если не указан поисковый запрос.|  
  
##  <a name="Header"></a> Заголовок  
 В этом разделе описывается представление `parameter`.  
  
|Свойство|ОПИСАНИЕ|type|  
|--------------|-----------------|----------|  
|name|строка|Имя параметра.|  
|description|строка|Описание параметра.|  
|value|строка|Значение заголовка.|  
|typeName|строка|Тип данных значения заголовка.|  
|options|строка|Параметры.|  
|обязательно|Логическое|Указывает, требуется ли заголовок.|  
|readOnly|Логическое|Указывает, доступен ли заголовок только для чтения.|  
  
##  <a name="HTTPRequest"></a> HTTP-запрос  
 В этом разделе описывается представление `request`.  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|description|строка|Описание запроса операции.|  
|Заголовки|коллекция сущностей [Заголовок](#Header)|Заголовки запросов.|  
|parameters|массив [параметров](#Parameter)|коллекция параметров запросов операций|  
|representations|массив [представлений](#Representation)|коллекция представлений запросов операций|  
  
##  <a name="HTTPResponse"></a> Ответ HTTP  
 В этом разделе описывается представление `response`.  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|statusCode|положительное целое число|Код состояния ответа операции.|  
|description|строка|Описание ответа операции.|  
|representations|массив [представлений](#Representation)|коллекция представлений ответов операций|  
  
##  <a name="Operation"></a> Операция  
 Сущность `operation` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|id|строка|Идентификатор ресурса. Однозначно идентифицирует операцию в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `apis/{aid}/operations/{id}`, где `{aid}` — идентификатор API, а `{id}` — идентификатор операции. Это свойство доступно только для чтения.|  
|name|строка|Имя операции. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|description|строка|Описание операции. Не может быть пустым. Может содержать теги форматирования HTML. Максимальная длина составляет 1000 символов.|  
|scheme|строка|Описывает, на каких протоколах могут вызываться операции в данном API. Допустимые значения: `http`, `https` или оба (`http` и `https`).|  
|uriTemplate|строка|Шаблон относительного URL-адреса, определяющий целевой ресурс для этой операции. Может включать параметры. Пример: `customers/{cid}/orders/{oid}/?date={date}`|  
|host|строка|URL-адрес шлюза управления API, на котором размещен API.|  
|httpMethod|строка|Метод HTTP операции.|  
|запрос|[HTTP-запрос](#HTTPRequest)|Сущность, содержащая сведения о запросе.|  
|responses|массив [ответов HTTP](#HTTPResponse)|массив сущностей [ответов HTTP](#HTTPResponse) операции|  
  
##  <a name="Menu"></a> Меню операций  
 Сущность `operation menu` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|ApiId|строка|Идентификатор текущего API.|  
|CurrentOperationId|строка|Идентификатор текущей операции.|  
|Действие|строка|Тип меню.|  
|MenuItems|коллекция сущностей [Элемент меню операций](#MenuItem)|Операции для текущего API.|  
  
##  <a name="MenuItem"></a> Элемент меню операций  
 Сущность `operation menu item` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Идентификатор операции.|  
|Название|строка|Описание операции.|  
|HttpMethod|строка|Метод HTTP, используемый для операции.|  
  
##  <a name="Paging"></a> Разбиение по страницам  
 Сущность `paging` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Страница|number|Номер текущей страницы.|  
|PageSize|number|Максимальное число результатов, отображаемых на одной странице.|  
|TotalItemCount|number|Число элементов для отображения.|  
|ShowAll|Логическое|Указывает, следует ли показывать все результаты на одной странице.|  
|PageCount|number|Число страниц с результатами.|  
  
##  <a name="Parameter"></a> Параметр  
 В этом разделе описывается представление `parameter`.  
  
|Свойство|ОПИСАНИЕ|type|  
|--------------|-----------------|----------|  
|name|строка|Имя параметра.|  
|description|строка|Описание параметра.|  
|value|строка|Значение параметра.|  
|options|Массив строк|Значения, определяемые для параметров запроса.|  
|обязательно|Логическое|Указывает, является ли параметр обязательным.|  
|kind|number|Указывает, является ли этот параметр параметром пути (1) или параметром строки запроса (2).|  
|typeName|строка|Тип параметра.|  
  
##  <a name="Product"></a> Продукт  
 Сущность `product` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Идентификатор ресурса. Однозначно идентифицирует продукт в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `products/{pid}`, где `{pid}` — идентификатор продукта. Это свойство доступно только для чтения.|  
|Название|строка|Имя продукта. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|ОПИСАНИЕ|строка|Описание продукта. Не может быть пустым. Может содержать теги форматирования HTML. Максимальная длина составляет 1000 символов.|  
|Термины|строка|Условия использования продукта. Они будут представлены разработчикам во время оформления подписки на продукт. Им понадобится принять эти условия, чтобы завершить процедуру оформления.|  
|ProductState|number|Указывает, опубликован ли продукт. Опубликованные продукты доступны разработчикам на портале разработчика. Неопубликованные продукты видны только администраторам.<br /><br /> Допустимые значения для состояния продукта:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Логическое|Указывает, может ли пользователь одновременно иметь несколько подписок на этот продукт.|  
|MultipleSubscriptionsCount|number|Максимальное число подписок для этого продукта, которые может иметь пользователь.|  
  
##  <a name="Provider"></a> Поставщик  
 Сущность `provider` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|properties|словарь строк|Свойства для этого поставщика проверки подлинности.|  
|authenticationType|строка|Тип поставщика (Azure Active Directory, Facebook, учетная запись Google, учетная запись Майкрософт, Twitter).|  
|Caption|строка|Отображаемое имя поставщика.|  
  
##  <a name="Representation"></a> Представление  
 В этом разделе описывается `representation`.  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|сontentType|строка|Указывает зарегистрированный или пользовательский тип содержимого для этого представления, например `application/xml`.|  
|sample|строка|Пример представления.|  
  
##  <a name="Subscription"></a> Подписка  
 Сущность `subscription` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Идентификатор ресурса. Однозначно идентифицирует подписку в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `subscriptions/{sid}`, где `{sid}` — идентификатор подписки. Это свойство доступно только для чтения.|  
|ProductId|строка|Идентификатор ресурса продукта, на который оформлена подписка. Значение является допустимым относительным URL-адресом в формате `products/{pid}`, где `{pid}` — идентификатор продукта.|  
|ProductTitle|строка|Имя продукта. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|ProductDescription|строка|Описание продукта. Не может быть пустым. Может содержать теги форматирования HTML. Максимальная длина составляет 1000 символов.|  
|ProductDetailsUrl|строка|Относительный URL-адрес страницы сведений о продукте.|  
|state|строка|Состояние подписки. Возможны следующие состояния.<br /><br /> —-  `0 - suspended`: подписка заблокирована, и подписчик не может вызвать ни один API продукта.<br /><br /> — - `1 - active`: подписка активна.<br /><br /> — - `2 - expired`: срок действия подписки истек, и она была деактивирована.<br /><br /> — - `3 - submitted`: запрос разработчика на подписку выполнен, но еще не был утвержден или отклонен.<br /><br /> —-  `4 - rejected`: администратор отклонил запрос на подписку.<br /><br /> — - `5 - cancelled`: подписка была отменена разработчиком или администратором.|  
|DisplayName|строка|Отображаемое имя подписки.|  
|CreatedDate|dateTime|Дата создания подписки в формате ISO 8601: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Логическое|Указывает, может ли текущий пользователь отменить подписку.|  
|IsAwaitingApproval|Логическое|Указывает, ожидает ли подписка утверждения.|  
|StartDate|dateTime|Дата начала действия подписки в формате ISO 8601: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|dateTime|Время истечения срока действия подписки в формате ISO 8601: `2014-06-24T16:25:00Z`.|  
|NotificationDate|dateTime|Дата уведомления для подписки в формате ISO 8601: `2014-06-24T16:25:00Z`.|  
|primaryKey|строка|Первичный ключ подписки. Максимальная длина составляет 256 символов.|  
|secondaryKey|строка|Вторичный ключ подписки. Максимальная длина составляет 256 символов.|  
|CanBeRenewed|Логическое|Указывает, можно ли обновить подписку для текущего пользователя.|  
|HasExpired|Логическое|Указывает, истек ли срок действия подписки.|  
|IsRejected|Логическое|Указывает, отклонен ли запрос на подписку.|  
|CancelUrl|строка|Относительный URL-адрес для отмены подписки.|  
|RenewUrl|строка|Относительный URL-адрес для возобновления подписки.|  
  
##  <a name="SubscriptionSummary"></a> Сводка по подписке  
 Сущность `subscription summary` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Идентификатор|строка|Идентификатор ресурса. Однозначно идентифицирует подписку в текущем экземпляре службы управления API. Значение является допустимым относительным URL-адресом в формате `subscriptions/{sid}`, где `{sid}` — идентификатор подписки. Это свойство доступно только для чтения.|  
|DisplayName|строка|Отображаемое имя подписки|  
  
##  <a name="UserAccountInfo"></a> Сведения об учетной записи пользователя  
 Сущность `user account info` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|FirstName|строка|Имя. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|LastName|строка|Фамилия. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|Email|строка|Электронная почта. Не может быть пустым и должно быть уникальным в пределах экземпляра службы. Максимальная длина составляет 254 символа.|  
|Пароль|строка|Пароль учетной записи пользователя.|  
|NameIdentifier|строка|Идентификатор учетной записи, совпадающий с адресом электронной почты пользователя.|  
|ProviderName|строка|Имя поставщика проверки подлинности.|  
|IsBasicAccount|Логическое|Значение true, если эта учетная запись зарегистрирована с использованием электронной почты и пароля. Значение false, если учетная запись зарегистрирована с помощью поставщика.|  
  
##  <a name="UseSignIn"></a> Вход пользователя  
 Сущность `user sign in` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|Email|строка|Электронная почта. Не может быть пустым и должно быть уникальным в пределах экземпляра службы. Максимальная длина составляет 254 символа.|  
|Пароль|строка|Пароль учетной записи пользователя.|  
|ReturnUrl|строка|URL-адрес страницы, на которой пользователь щелкнул ссылку входа.|  
|RememberMe|Логическое|Указывает, следует ли сохранять сведения о текущем пользователе.|  
|RegistrationEnabled|Логическое|Указывает, включена ли регистрация.|  
|DelegationEnabled|Логическое|Указывает, включен ли делегированный вход.|  
|DelegationUrl|строка|URL-адрес делегированного входа, если он включен.|  
|SsoSignUpUrl|строка|Единый URL-адрес входа для пользователя, если он имеется.|  
|AuxServiceUrl|строка|Если текущий пользователь является администратором, это ссылка на экземпляр службы на портале Azure.|  
|Поставщики|коллекция сущностей [Поставщик](#Provider)|Поставщики проверки подлинности для этого пользователя.|  
|UserRegistrationTerms|строка|Условия, которые пользователь должен принять перед входом.|  
|UserRegistrationTermsEnabled|Логическое|Указывает, включены ли условия использования.|  
  
##  <a name="UserSignUp"></a> Регистрация пользователя  
 Сущность `user sign up` имеет следующие свойства:  
  
|Свойство|type|ОПИСАНИЕ|  
|--------------|----------|-----------------|  
|PasswordConfirm|Логическое|Значение, используемое элементом управления для [регистрации](api-management-page-controls.md#sign-up).|  
|Пароль|строка|Пароль учетной записи пользователя.|  
|PasswordVerdictLevel|number|Значение, используемое элементом управления для [регистрации](api-management-page-controls.md#sign-up).|  
|UserRegistrationTerms|строка|Условия, которые пользователь должен принять перед входом.|  
|UserRegistrationTermsOptions|number|Значение, используемое элементом управления для [регистрации](api-management-page-controls.md#sign-up).|  
|ConsentAccepted|Логическое|Значение, используемое элементом управления для [регистрации](api-management-page-controls.md#sign-up).|  
|Email|строка|Электронная почта. Не может быть пустым и должно быть уникальным в пределах экземпляра службы. Максимальная длина составляет 254 символа.|  
|FirstName|строка|Имя. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|LastName|строка|Фамилия. Не может быть пустым. Максимальная длина составляет 100 символов.|  
|UserData|строка|Значение, используемое элементом управления [регистрацией](api-management-page-controls.md#sign-up).|  
|NameIdentifier|строка|Значение, используемое элементом управления для [регистрации](api-management-page-controls.md#sign-up).|  
|ProviderName|строка|Имя поставщика проверки подлинности.|

## <a name="next-steps"></a>Дополнительная информация
Дополнительные сведения о работе с шаблонами см. в статье [Настройка портала разработчика в службе управления API Azure с помощью шаблонов](api-management-developer-portal-templates.md).
