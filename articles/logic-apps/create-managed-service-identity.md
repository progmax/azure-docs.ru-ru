---
title: Проверка подлинности с помощью управляемых удостоверений в Azure Logic Apps | Документация Майкрософт
description: Чтобы выполнить проверку подлинности без входа, создайте управляемое удостоверение, ранее называвшееся "Управляемое удостоверение службы" (MSI), чтобы приложение логики могло получить доступ к ресурсам в других клиентах Azure Active Directory (Azure AD) без учетных данных или секретов.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 10/05/2018
ms.openlocfilehash: 84529e1097678ba7a039ffaeec57a9293c93dafd
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50229645"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Проверка подлинности и получение доступа к ресурсам с помощью управляемых удостоверений в Azure Logic Apps

Чтобы получить доступ к ресурсам в других клиентах Azure Active Directory (Azure AD) и выполнить аутентификацию идентификатора без входа, приложение логики может использовать [управляемое удостоверение](../active-directory/managed-identities-azure-resources/overview.md), известное ранее как "Управляемое удостоверение службы" (MSI), а не учетные данные или секреты. Azure управляет этим удостоверением за вас и помогает защитить учетные данные, потому что вам не нужно предоставлять или сменять секреты. В этой статье показано, как можно создать назначенное системой управляемое удостоверение для приложения логики и управлять им. Дополнительные сведения об управляемых удостоверениях см. в статье [Что такое управляемые удостоверения для ресурсов Azure?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> В настоящее время в каждой подписке Azure может быть не более 10 рабочих процессов приложения логики с назначенными системой управляемыми удостоверениями.

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще ее нет, <a href="https://azure.microsoft.com/free/" target="_blank">зарегистрируйтесь для получения бесплатной учетной записи Azure</a>.

* Приложение логики, в котором необходимо использовать управляемое удостоверение, назначенное системой. Если у вас нет приложения логики, ознакомьтесь с кратким руководством [Создание первого автоматизированного рабочего процесса с помощью Azure Logic Apps на портале Azure](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Создание управляемого удостоверения

Назначенное системой управляемое удостоверение для приложения логики можно включить или создать с помощью портала Azure, Azure PowerShell или шаблонов Azure Resource Manager. 

### <a name="azure-portal"></a>Портал Azure

Чтобы включить назначенное системой управляемое удостоверение для приложения логики на портале Azure, включите параметр **Зарегистрировать в Azure Active Directory** в параметрах рабочего процесса приложения логики.

1. На [портале Azure](https://portal.azure.com) откройте приложение логики в конструкторе приложений логики.

1. Выполните следующие действия. 

   1. В меню приложения логики в разделе **Параметры** выберите **Параметры рабочего процесса**. 

   1. В разделе **Управляемое удостоверение службы** > 
   **Зарегистрировать в Azure Active Directory** выберите **Вкл.**

   1. Когда все будет готово, щелкните **Сохранить** на панели инструментов.

      ![Включение параметра управляемого удостоверения](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      У приложения логики теперь есть назначенное системой управляемое удостоверение, зарегистрированное в Azure Active Directory с такими свойствами и значениями:

      ![Уникальные идентификаторы GUID для идентификатора субъекта и идентификатора клиента](./media/create-managed-service-identity/principal-tenant-id.png)

      | Свойство | Значение | ОПИСАНИЕ | 
      |----------|-------|-------------| 
      | **Идентификатор субъекта** | <*ИД субъекта*> | Глобальный уникальный идентификатор (GUID), который представляет приложение логики в клиенте Azure AD | 
      | **Идентификатор клиента** | <*ИД клиента Azure AD*> | Глобальный уникальный идентификатор (GUID), представляющий клиент Azure AD, участником которого является приложение логики. В клиенте Azure AD субъект-служба имеет то же имя, что и экземпляр приложения логики. | 
      ||| 

### <a name="deployment-template"></a>Шаблон развертывания

Если вы хотите автоматизировать создание и развертывание таких ресурсов Azure, как приложения логики, можно настроить [шаблоны Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). Чтобы создать назначенное системой управляемое удостоверение для приложения логики с помощью шаблона, добавьте элемент `"identity"` и свойство `"type"` в определение рабочего процесса приложения логики в шаблоне развертывания. 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Например: 

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
         "actions": {}, 
         "parameters": {}, 
         "triggers": {}, 
         "contentVersion": "1.0.0.0", 
         "outputs": {} 
   }, 
   "parameters": {}, 
   "dependsOn": [] 
}
```

Когда Azure создает приложение логики, определение рабочего процесса приложения логики содержит такие дополнительные свойства:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Свойство | Значение | ОПИСАНИЕ | 
|----------|-------|-------------|
| **principalId** | <*ИД субъекта*> | Глобальный уникальный идентификатор (GUID), который представляет приложение логики в клиенте Azure AD | 
| **tenantId** | <*ИД клиента Azure AD*> | Глобальный уникальный идентификатор (GUID), представляющий клиент Azure AD, участником которого является приложение логики. В клиенте Azure AD субъект-служба имеет то же имя, что и экземпляр приложения логики. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Получение доступа к ресурсам с помощью управляемого удостоверения

После создания назначенного системой управляемого удостоверения для приложения логики [удостоверению можно предоставить доступ к другим ресурсам Azure](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Затем удостоверение можно использовать для аутентификации так же, как и любой другой [субъект-службу](../active-directory/develop/app-objects-and-service-principals.md). 

> [!NOTE]
> И назначенное системой управляемое удостоверение, и ресурс, к которому необходимо назначить доступ, должны находиться в одной подписке Azure.

### <a name="assign-access-to-managed-identity"></a>Назначение доступа к управляемому удостоверению

Чтобы предоставить доступ к другому ресурсу Azure для назначенного системой управляемого удостоверения приложения логики, выполните следующие действия:

1. На портале Azure перейдите к ресурсу Azure, к которому вы хотите назначить доступ для управляемого удостоверения. 

1. В меню ресурса выберите **Управление доступом (IAM)**, а затем щелкните **Добавить**. 

   ![Добавление разрешений](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. В разделе **Добавить разрешения** выберите **Роль**, которая требуется удостоверению. 

1. В свойстве **Назначение доступа** выберите значение **Пользователь, группа или приложение Azure AD**, если оно еще не выбрано.

1. В поле **Выбор** начните вводить имя приложения логики. Когда появится приложение логики, выберите его.

   ![Выбор приложения логики с управляемым удостоверением](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Когда все будет готово, нажмите **Сохранить**.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Проверка подлинности на основе управляемых удостоверений в приложении логики

После настройки приложения логики с назначенным системой управляемым удостоверением, которому предоставлен доступ к необходимому ресурсу, удостоверение можно использовать для проверки подлинности. Например, теперь можно использовать действие HTTP для того, чтобы приложение логики могло отправить HTTP-запрос или вызов к этому ресурсу. 

1. Добавьте действие **HTTP** в приложение логики. 

1. Предоставьте необходимые сведения для этого действия, такие как **метод** запроса и расположение **URI** ресурса, который необходимо вызвать.

1. В действии HTTP выберите **Показать расширенные параметры**. 

1. В списке **Проверка подлинности** выберите **Управляемое удостоверение службы**, после чего отобразится свойство **Аудитория**, которое необходимо задать:

   ![Выбор "Управляемое удостоверение службы"](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Продолжите создание приложения логики нужным образом.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Удаление управляемого удостоверения

Чтобы отключить назначенное системой управляемое удостоверение приложения логики, выполните шаги как при создании идентификатора с помощью портала Azure, Azure PowerShell или шаблонов развертывания Azure Resource Manager. 

При удалении приложения логики Azure автоматически удаляет присвоенный системой идентификатор приложения логики из Azure AD.

### <a name="azure-portal"></a>Портал Azure

1. Откройте приложение логики в конструкторе Logic App.

1. Выполните следующие действия. 

   1. В меню приложения логики в разделе **Параметры** выберите **Параметры рабочего процесса**. 
   
   1. В разделе **Управляемое удостоверение службы** выберите **Выкл.** для свойства **Зарегистрировать в Azure Active Directory**.

   1. Когда все будет готово, щелкните **Сохранить** на панели инструментов.

      ![Отключение параметра управляемого удостоверения](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Шаблон развертывания

Если вы создали назначенное системой управляемое удостоверение приложения логики с помощью шаблона развертывания Azure Resource Manager, задайте свойству `"type"` элемента `"identity"` значение `"None"`. Это действие также удаляет идентификатор участника из Azure AD. 

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Получение поддержки

* Если у вас возникли вопросы, то посетите [форум Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Отправить идею по поводу возможности или проголосовать за нее вы можете на [сайте отзывов пользователей Logic Apps](https://aka.ms/logicapps-wish).

