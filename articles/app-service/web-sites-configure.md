---
title: Настройка веб-приложений в службе приложений Azure
description: Настройка веб-приложения в службе приложений Azure
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.openlocfilehash: 73d2da542c4f7da0933187d800f562de76bfb3e6
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443514"
---
# <a name="configure-web-apps-in-azure-app-service"></a>Настройка веб-приложений в службе приложений Azure

В этом разделе рассматривается настройка веб-приложения с помощью [портал Azure].

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a>Параметры приложения
1. На [портал Azure] откройте колонку для веб-приложения.
3. Щелкните **Параметры приложения**.

![Параметры приложения][configure01]

В колонке **Параметры приложения** параметры сгруппированы по нескольким категориям.

### <a name="general-settings"></a>Общие параметры
**Версии Framework**. Укажите эти параметры, если приложение использует какие-либо из этих инфраструктур разработки: 

* **.NET Framework**: укажите версию .NET Framework. 
* **PHP**: укажите версию PHP или установите значение **Выключено**, чтобы отключить PHP. 
* **Java**: выберите версию Java или задайте **Выключено**, чтобы отключить Java. Используйте параметр **Веб-контейнер** , чтобы выбрать версию Tomcat или Jetty.
* **Python**: выберите версию Python или задайте **Выключено**, чтобы отключить Python.

По техническим причинам включение Java для веб-приложения отключает использование .NET, PHP и Python.

<a name="platform"></a>
**Платформа**. Выберите разрядность среды выполнения приложения – 32 или 64 бита. Для 64-разрядной среды требуется ценовая категория "Базовый" или "Стандартный". Ценовые категории "Бесплатный" и "Общий" всегда используются в 32-разрядной среде.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

**Веб-сокеты**. Установите значение **Включено**, чтобы разрешить использование протокола WebSocket (например, если ваше веб-приложение использует [ASP.NET SignalR] или [socket.io](https://socket.io/)).

<a name="alwayson"></a>
**Всегда включено**. По умолчанию в случае простоя в течение определенного периода времени веб-приложения будут выгружены. Это позволяет сэкономить системные ресурсы. В режиме Basic и Standard вы можете включить функцию **Всегда включено** , чтобы приложение постоянно оставалось загруженным. Если приложение выполняет непрерывные веб-задания или веб-задания, активированные с помощью выражения CRON, следует включить функцию **Всегда включено**, иначе веб-задания не будут выполняться правильно.

**Версия управляемого конвейера**. Задает [режим конвейера]IIS. Оставьте здесь значение "Интегрированный" (по умолчанию), кроме тех случаев, когда имеется устаревшее веб-приложение, для которого требуется предыдущая версия IIS.

**Версия HTTP**. Задайте значение **2.0**, чтобы обеспечить поддержку протокола [HTTPS/2](https://wikipedia.org/wiki/HTTP/2). 

> [!NOTE]
> Большинство современных браузеров поддерживают протокол HTTP/2 только через TLS, а для незашифрованного трафика продолжает использоваться протокол HTTP/1.1. Чтобы гарантировать, что клиентские браузеры подключаются к приложению с помощью HTTP/2, [приобретите сертификат Службы приложений](web-sites-purchase-ssl-web-site.md) для пользовательского домена приложения или [привяжите сторонний сертификат](app-service-web-tutorial-custom-ssl.md).

**Сходство для маршрутизации запросов приложений (ARR)**. В приложении, масштабированном на нескольких экземплярах виртуальных машин, файлы cookie сходства ARR гарантируют, что клиент направляется в один и тот же экземпляр в течение сеанса. Чтобы повысить производительность приложений без учета состояния, необходимо **отключить** этот параметр.   

**Автоматическое переключение**. При включении автоматического переключения для слота развертывания служба приложений будет автоматически переводить веб-приложение в рабочий режим после каждого обновления в этом слоте. Дополнительную информацию см. в статье [Развертывание промежуточных слотов для веб-приложений в службе приложения Azure](web-sites-staged-publishing.md).

### <a name="debugging"></a>Отладка
**Удаленная отладка**. Включение удаленной отладки. Если этот параметр включен, вы можете использовать удаленный отладчик Visual Studio для прямого подключения к веб-приложению. Удаленная отладка останется включенной на 48 часов. 

### <a name="app-settings"></a>Параметры приложения
Этот раздел содержит пары "имя и значение", которые веб-приложение будет загружать при запуске. 

* Для приложений .NET эти параметры вносятся в конфигурацию .NET `AppSettings` во время выполнения, переопределяя текущие настройки. 
* Для службы приложений в Linux или Веб-приложений для контейнеров, если у вас есть вложенная ключевая структура json имени, например `ApplicationInsights:InstrumentationKey`, вам понадобиться `ApplicationInsights__InstrumentationKey` в качестве имени ключа. Так что обратите внимание, что все `:` должны быть заменены на `__` (т. е. двойным знаком подчеркивания).
* Во время выполнения приложения PHP, Python, Java и Node могут обращаться к этим параметрам как к переменным среды. Для каждого параметра приложения создаются две переменные среды: одна с именем, указанным в параметре приложения, и другая с префиксом APPSETTING_. Обе содержат одинаковое значение.

Параметры приложения всегда шифруются при хранении.

Параметры приложения можно разрешить из Key Vault с помощью [ссылок на Key Vault](app-service-key-vault-references.md).

### <a name="connection-strings"></a>Строки подключения
Строки подключения для связанных ресурсов. 

Для приложений .NET эти строки подключения будут внесены в параметры конфигурации .NET `connectionStrings` во время выполнения, переопределяя имеющиеся записи, для которых значение ключа равно имени связанной базы данных. 

Для приложений PHP, Python, Java и Node эти параметры будут доступны во время выполнения в виде переменных среды. В качестве префикса будет использоваться тип подключения. Префиксы переменных среды: 

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* База данных SQL: `SQLAZURECONNSTR_`
* Пользовательская: `CUSTOMCONNSTR_`

Например, если строка подключения MySql будет иметь имя `connectionstring1`, доступ к ней будет выполняться через переменную среды `MYSQLCONNSTR_connectionString1`.

Строки подключения всегда шифруются при хранении.

Строки подключения можно разрешить из Key Vault с помощью [ссылок на Key Vault](app-service-key-vault-references.md).

### <a name="default-documents"></a>Стандартные документы
Документ по умолчанию — это веб-страница, которая отображается при открытии корневого URL-адреса веб-сайта.  Используется первый найденный файл в списке. 

Веб-приложения могут использовать модули, для которых выполняется маршрутизация на основе URL-адреса вместо выдачи статичного содержимого. В таком случае документ по умолчанию не используется.    

### <a name="handler-mappings"></a>Сопоставления обработчиков
Добавьте пользовательские обработчики скриптов, которые будут обрабатывать запросы для файлов с определенными расширениями. 

* **Расширение**. Расширение обрабатываемого файла, например *.php или handler.fcgi. 
* **Путь к обработчику сценариев**. Абсолютный путь к обработчику сценариев. Запросы к файлам, совпадающим с расширением, будут обрабатываться обработчиком сценариев. Используйте путь `D:\home\site\wwwroot` для указания корневого каталога веб-приложения.
* **Дополнительные аргументы**. Необязательные аргументы для командной строки обработчика сценариев. 

### <a name="virtual-applications-and-directories"></a>Виртуальные приложения и каталоги
Чтобы настроить виртуальные приложения и каталоги, укажите каждый виртуальный каталог и его соответствующий физический путь относительно корневого каталога веб-сайта. Или установите флажок **Приложение** , чтобы отметить виртуальный каталог как приложение.

## <a name="enabling-diagnostic-logs"></a>Включение журналов диагностики
Включение журналов диагностики

1. В столбце веб-приложений щелкните **Все параметры**.
2. Щелкните **Журналы диагностики**. 

Параметры записи журналов диагноcтики из веб-приложения, поддерживающего ведение журналов. 

* **Ведение журнала приложений**. Эта функция записывает журналы приложений в файловую систему. Ведение журнала продолжается в течение 12 часов. 

**Уровень**. Если включено ведение журналов приложений, этот параметр задает объем данных, который будет записан (ошибка, предупреждение, сведения или подробные сведения).

**Ведение журнала веб-сервера**. Журналы сохраняются в расширенном формате журнала W3C. 

**Подробные сообщения об ошибках**. Сохраняет HTM-файлы с подробными сведениями об ошибках. Файлы сохраняются в папке /LogFiles/DetailedErrors. 

**Трассировка неудачно завершенных запросов**. Журналы неудачных запросов к XML-файлам. Файлы сохраняются в каталоге "/LogFiles/W3SVC*xxx*", где xxx — это уникальный идентификатор. Эта папка содержит XSL-файл и один или несколько XML-файлов. Обязательно загрузите XSL-файл, т. к. предоставляет средства форматирования и фильтрации содержимого XML-файлов.

Для просмотра файлов журнала необходимо создать учетные данные FTP, как описано ниже.

1. В столбце веб-приложений щелкните **Все параметры**.
2. Щелкните **Учетные данные развертывания**.
3. Введите имя пользователя и пароль.
4. Выберите команду **Сохранить**.

![Сброс учетных данных развертывания][configure03]

Полным именем пользователя FTP будет "app\username", где *app* — название вашего веб-приложения. Имя пользователя указано в колонке веб-приложения в разделе **Основные сведения**.

![Учетные данные развертывания FTP][configure02]

## <a name="other-configuration-tasks"></a>Другие задачи по настройке
### <a name="ssl"></a>SSL
В стандартном и базовом режиме можно загрузить SSL-сертификаты для настраиваемого домена. Дополнительные сведения см. в статье [Привязывание существующего настраиваемого SSL-сертификата к веб-приложениям Azure](app-service-web-tutorial-custom-ssl.md). 

Чтобы просмотреть загруженные сертификаты, щелкните **Все параметры** > **Личные домены и SSL**.

### <a name="domain-names"></a>Имена доменов
Добавление имени личного домена для веб-приложения. Дополнительные сведения см. в статье [Сопоставление существующего настраиваемого DNS-имени с веб-приложениями Azure](app-service-web-tutorial-custom-domain.md).

Чтобы просмотреть доменные имена, щелкните **Все параметры** > **Личные домены и SSL**.

### <a name="deployments"></a>Развернутые приложения
* Настройка непрерывного развертывания. Ознакомьтесь со статьей [Использование Git для развертывания веб-приложений в службе приложений Azure](app-service-deploy-local-git.md).
* Слоты развертывания. См. статью [Настройка промежуточных сред для веб-приложений в службе приложений Azure].

Чтобы просмотреть слоты развертывания, щелкните **Все параметры** > **Слоты развертывания**.

### <a name="monitoring"></a>Мониторинг
В режимах "Базовый" и "Стандартный" можно проверить доступность конечных точек HTTP и HTTPS из трех географических распределенных расположений. Тест мониторинга завершается с ошибкой, если код ответа HTTP говорит об ошибке (4xx или 5xx) или на ответ требуется более 30 секунд. Конечная точка считается доступной, если тесты мониторинга завершились для нее успешно для всех указанных расположений. 

Дополнительные сведения см. в статье [Практическое руководство: мониторинг состояния конечной веб-точки].

> [!NOTE]
> Чтобы приступить к работе со службой приложений Azure до создания учетной записи Azure, перейдите к разделу [Пробное использование службы приложений], где вы можете быстро создать кратковременное веб-приложение начального уровня в службе приложений. Никаких кредитных карт и обязательств.
> 
> 

## <a name="next-steps"></a>Дополнительная информация
* [Настройка личного доменного имени в службе приложений Azure]
* [Включение протокола HTTPS для приложения в службе приложений Azure]
* [Масштабирование веб-приложения в службе приложений Azure]
* [Основы мониторинга для веб-приложений в службе приложений Azure]

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[портал Azure]: https://portal.azure.com/
[Настройка личного доменного имени в службе приложений Azure]: ./app-service-web-tutorial-custom-domain.md
[Настройка промежуточных сред для веб-приложений в службе приложений Azure]: ./web-sites-staged-publishing.md
[Включение протокола HTTPS для приложения в службе приложений Azure]: ./app-service-web-tutorial-custom-ssl.md
[Практическое руководство: мониторинг состояния конечной веб-точки]: http://go.microsoft.com/fwLink/?LinkID=279906
[Основы мониторинга для веб-приложений в службе приложений Azure]: ./web-sites-monitor.md
[режим конвейера]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Масштабирование веб-приложения в службе приложений Azure]: ./web-sites-scale.md
[Пробное использование службы приложений]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
