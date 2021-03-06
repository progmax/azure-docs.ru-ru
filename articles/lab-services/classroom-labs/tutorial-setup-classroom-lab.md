---
title: Настройка аудиторной лаборатории с помощью Служб лабораторий Azure | Документация Майкрософт
description: В этом руководстве вы настроите лабораторию для использования в аудитории.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: babff55d6684feb1f0414970616260be96b994f4
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706013"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Руководство. Настройка аудиторной лаборатории 
В этом руководстве вы настроите аудиторную лабораторию с виртуальными машинами, используемыми учащимися в аудитории.  

Вот какие действия выполняются в этом руководстве:

> [!div class="checklist"]
> * Создание аудиторной лаборатории.
> * Настройка аудиторной лаборатории.
> * Отправка ссылки для регистрации учащимся

## <a name="prerequisites"></a>Предварительные требования
Чтобы настроить лабораторию аудитории в учетной записи лаборатории, вы должны быть членом роли **Создатель лаборатории** в учетной записи лаборатории. Учетная запись, которую вы использовали для создания учетной записи лаборатории, автоматически добавляется в эту роль. См. дополнительные сведения о том, как [назначить владельцу лаборатории роль, позволяющую добавлять других пользователей в роль создателя лаборатории](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Создание лаборатории для аудитории

1. Перейдите на [веб-сайт Служб лабораторий Azure](https://labs.azure.com). 
2. Выберите **Вход** и введите свои учетные данные. Службы лабораторий Azure поддерживают учетные записи организации и учетные записи Майкрософт. 
3. В окне **New Lab** (Создание лаборатории) выполните следующие действия. 
    1. Укажите **имя** своей лаборатории. 
    2. Укажите максимальное **число пользователей**, которые могут работать в лаборатории. 
    6. Щелкните **Сохранить**.

        ![Создание лаборатории для аудитории](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. На странице **выбора характеристик виртуальных машин** выполните следующие действия:
    1. Выберите **размер** для виртуальных машин, созданных в лаборатории. 
    2. Выберите **регион**, в котором требуется создать виртуальные машины. 
    3. Выберите **образ виртуальной машины**, который будет использоваться для создания виртуальных машин в лаборатории. 
    4. Щелкните **Далее**.

        ![Указание характеристик виртуальной машины](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. На странице **Задание учетных данных** укажите учетные данные по умолчанию для всех виртуальных машин в лаборатории. 
    1. Укажите **имя пользователя** для всех виртуальных машин в лаборатории.
    2. Укажите **пароль** для пользователя. 

        > [!IMPORTANT]
        > Запишите имя пользователя и пароль. Они больше не будут отображаться.
    3. Нажмите кнопку **Создать**. 

        ![Настройка учетных данных](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. На странице **настройки шаблона** отображаются сведения о состоянии создания лаборатории. Создание шаблона лаборатории занимает до 20 минут. 

    ![Настройка шаблона](../media/tutorial-setup-classroom-lab/configure-template.png)
7. По завершении настройки шаблона отобразится следующая страница: 

    ![Страница настройки шаблона по завершении процесса](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. В рамках этого руководства можно выполнить следующие необязательные действия: 
    1. Нажмите кнопку **Запуск**, чтобы запустить шаблон виртуальной машины.
    2. Подключитесь к шаблону виртуальной машины. Для этого выберите **Подключиться**. 
    3. Установите и настройте программное обеспечение в шаблоне виртуальной машины. 
    4. **Остановите** работу виртуальной машины.  
    5. Введите **описание** шаблона.

        ![Кнопка "Далее" на странице настройки шаблона](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Выберите **Далее** на странице шаблона. 
10. На странице **публикации шаблона** выполните описанные ниже действия. 
    1. Для немедленной публикации шаблона установите флажок *I understand I can't modify the template after publishing. This process can only be done once and can take up to an hour* (Я понимаю, что не смогу изменить шаблон после публикации. Это выполняется однократно и может занять до одного часа) и выберите **Опубликовать**.  

        > [!WARNING]
        > После публикации отменить это действие будет нельзя. 
    2. Чтобы опубликовать шаблон позже, выберите **Сохранить для использования в будущем**. Вы можете опубликовать шаблон виртуальной машины по завершении работы мастера. Дополнительные сведения о том, как настроить и опубликовать шаблон после завершения работы мастера, см. в [этом разделе](how-to-create-manage-template.md#publish-the-template-vm) статьи [Управление лабораторией для аудитории в решении "Службы лабораторий Azure"](how-to-manage-classroom-labs.md).

        ![Публикация шаблона](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Отобразятся сведения о **ходе публикации** шаблона. Этот процесс может занять до одного часа. 

    ![Публикация шаблона — ход выполнения](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Если шаблон опубликован успешно, отобразится показанная ниже страница. Нажмите кнопку **Готово**.

    ![Публикация шаблона — успешное выполнение](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Откроется **панель мониторинга** лаборатории. 
    
    ![Панель мониторинга лаборатории для аудитории](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Перейдите на страницу **Виртуальные машины** и проверьте, что на ней есть виртуальные машины в состоянии **Не назначено**. Эти виртуальные машины еще не назначены учащимся. Они должны находиться в состоянии **Остановлено**. На этой странице вы можете запустить виртуальную машину для учащегося, подключиться к ней, а также остановить или удалить ее. Виртуальные машины на этой странице можете запускать как вы, так и ваши учащиеся. 

    ![Виртуальные машины в остановленном состоянии](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Добавление пользователей в лабораторию

1. В меню слева выберите **Пользователи**. По умолчанию включен параметр **Ограничить доступ**. Когда этот параметр включен, даже если пользователь имеет ссылку на регистрацию, он не сможет зарегистрироваться в лаборатории, пока не будет включен в список пользователей. С помощью отправляемой ссылки для регистрации могут зарегистрироваться только пользователи из списка. Ниже описано, как добавить пользователей в список. Параметр **Ограничить доступ** можно отключить, что позволит пользователям регистрироваться в лаборатории, используя ссылку для регистрации. 
2. На панели инструментов выберите **Добавить пользователей**. 
3. На странице **Добавление пользователей** введите адреса электронной почты пользователей в отдельных строках или одной строке, разделяя их точкой с запятой. 

    ![Добавление адресов электронной почты пользователей](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Щелкните **Сохранить**. В списке отобразятся адреса электронной почты и состояние пользователей (зарегистрирован или нет). 

    ![Список пользователей](../media/how-to-configure-student-usage/users-list-new.png)


## <a name="send-registration-link-to-students"></a>Отправка ссылки для регистрации учащимся

1. Перейдите на страницу **Пользователи**. 
2. Щелкните плитку **Получить ссылку для регистрации**.

    ![Ссылка для регистрации учащихся](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. В диалоговом окне **User registration** (Регистрация пользователей) щелкните **Копировать**. Ссылка скопируется в буфер обмена. 

    ![Ссылка для регистрации учащихся](../media/tutorial-setup-classroom-lab/registration-link.png)
2. В диалоговом окне **Регистрация пользователей** нажмите кнопку **Закрыть**. 
4. Отправьте учащемуся ссылку для регистрации, чтобы он мог зарегистрироваться для прохождения курса. Если включен параметр **Ограничить доступ** и в списке есть пользователи, выполните следующие действия:
    1. Щелкните **адрес электронной почты** пользователя в списке. 
    2. Запустится программа электронной почты по умолчанию и отобразится окно сообщения с заполненной строкой **Кому**. 
    3. Вставьте в сообщение **URL-адрес для регистрации**, скопированный ранее. 
    4. Отправьте **сообщение электронной почты**.


## <a name="next-steps"></a>Дополнительная информация
В этом руководстве вы создали и настроили аудиторную лабораторию. Чтобы узнать, как учащиеся могут получить доступ к виртуальной машине в лаборатории с помощью ссылки для регистрации, перейдите к следующему руководству:

> [!div class="nextstepaction"]
> [Руководство по получению доступа к аудиторной лаборатории в Службах лабораторий Azure](tutorial-connect-virtual-machine-classroom-lab.md)

