---
title: Включение поддержки Avere vFXT для Azure
description: Как включить отправку данных для службы поддержки из Avere vFXT для Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 0f5eee20b0487fb5fce82047f40d137effb87ead
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164436"
---
# <a name="enable-support-uploads"></a>Включение отправки данных для службы поддержки

Avere vFXT для Azure может автоматически отправлять данные о вашем кластере для службы поддержки. Благодаря наличию этих данных сотрудник службы поддержки может предоставлять наилучшее обслуживание клиентов.

## <a name="steps-to-enable-uploads"></a>Действия для включения отправок

Чтобы активировать поддержку, выполните следующие действия на панели управления Avere: Сведения об открытии панели управления Avere см. в [этой статье](avere-vfxt-cluster-gui.md).

1. Перейдите на вкладку **Settings** (Параметры) на верхней панели.
1. Щелкните ссылку **Support** (Поддержка) в левой области и примите условия политики конфиденциальности.

   ![Снимок экрана, на котором показана панель управления Avere и всплывающее окно с кнопкой Confirm (Подтверждение) для принятия политики конфиденциальности](media/avere-vfxt-privacy-policy.png)

1. Щелкните треугольник слева от заголовка **Customer Info** (Сведения о клиентах), чтобы развернуть раздел.
1. Нажмите кнопку **Revalidate upload information** (Повторно проверить информацию для отправки).
1. Укажите имя кластера для поддержки в поле **Unique Cluster Name** (Уникальное имя кластера). Убедитесь,что она уникально идентифицирует кластер для специалистов службы поддержки.
1. Установите флажки **Statistics Monitoring** (Мониторинг статистики), **General Information Upload** (Отправка общей информации) и **Crash Information Upload** (Отправка информации о сбоях).
1. Нажмите кнопку **Submit**(Отправить).

   ![Снимок экрана, на котором показана страница поддерживаемых параметров с разделом, заполненным сведениями о клиенте](media/avere-vfxt-support-info.png)

1. Щелкните треугольник слева от заголовка **Secure Proactive Support (SPS)** (Безопасная проактивная поддержка), чтобы развернуть раздел.
1. Установите флажок **Enable SPS Link** (Включить ссылку SPS).
1. Нажмите кнопку **Submit**(Отправить).

   ![Снимок экрана, на котором показана страница поддерживаемых параметров с заполненным разделом Secure Proactive Support (SPS) (Безопасная проактивная поддержка)](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Дополнительная информация

Если вам нужно добавить в кластер локальную систему хранения, следуйте инструкциям в статье о [настройке хранилища](avere-vfxt-add-storage.md). 

Если вы готовы начать подключение клиентов к кластеру, перейдите к статье [Подключение кластера Avere vFXT](avere-vfxt-mount-clients.md).