---
title: "Сетка событий Azure: безопасность и проверка подлинности"
description: "В статье описываются сетка событий Azure и ее основные понятия."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/14/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: ccef224ef1c2919a3e5469c1bbe0980c6963705b
ms.contentlocale: ru-ru
ms.lasthandoff: 08/30/2017

---

# <a name="event-grid-security-and-authentication"></a>Сетка событий: безопасность и проверка подлинности 

В сетке событий Azure предусмотрено три типа проверки подлинности:

* Подписки на события
* Публикация событий
* Доставка событий веб-перехватчика

## <a name="webhook-event-delivery"></a>Доставка событий веб-перехватчика

Веб-перехватчики — это один из многих способов получения событий в режиме реального времени из сетки событий Azure.

Каждый раз, когда имеется новое событие, готовое к доставке, сетка событий отправляет в веб-перехватчик HTTP-запрос, в теле которого находится событие.

При регистрации собственной конечной точки веб-перехватчика с сеткой событий он отправляет запрос POST с кодом простой проверки для подтверждения владения конечной точкой. В ответ приложение должно вернуть код проверки. Сетка событий не доставляет события на конечные точки веб-перехватчика, которые не прошли проверку.
 
### <a name="validation-details"></a>Сведения о проверке:

* Во время создания или обновления подписки на событие сетка событий публикует на целевой конечной точке событие "SubscriptionValidationEvent".
* В качестве заголовка событие содержит значение "Event-Type: Validation".
* Текст события имеет ту же схему, что и другие события сетки событий.
* В данных события содержится свойство ValidationCode со строкой, сгенерированной случайным образом (например, ValidationCode: acb13…).

Пример SubscriptionValidationEvent показан ниже.
```json
[{
  "Id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "Topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "Subject": "",
  "Data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "EventType": "Microsoft.EventGrid/SubscriptionValidationEvent",
  "EventTime": "2017-08-06T22:09:30.740323Z"
}]
```

Чтобы подтвердить владение конечной точкой, возвращается код проверки (например, "validation_response: acb13…"), пример которого показан ниже.

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Чтобы подтвердить владение конечной точкой, возвращается код проверки (например, validation_response: acb13…).

Наконец, необходимо отметить, что сетка событий Azure поддерживает только конечные точки веб-перехватчиков HTTPS.
## <a name="event-subscription"></a>Подписка на события

Чтобы подписаться на событие, необходимо иметь разрешение **Microsoft.EventGrid/EventSubscriptions/Write** для требуемого ресурса. Это разрешение необходимо, так как вы записываете новую подписку в области действия ресурса. Требуемый ресурс зависит от того, оформляется ли подписка на системный или пользовательский раздел. В этом разделе описываются оба типа подписки.

### <a name="system-topics-azure-service-publishers"></a>Системные разделы (издатели служб Azure)

Для системных разделов необходимо разрешение на запись новой подписки на события в области действия ресурса, публикующего событие. Ресурс имеет следующий формат: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Например, чтобы подписаться на событие в учетной записи хранения с именем **myacct**, требуется разрешение Microsoft.EventGrid/EventSubscriptions/Write для следующего ресурса: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Пользовательские разделы

Для пользовательских разделов необходимо разрешение на запись новой подписки на события в области действия сетки событий. Ресурс имеет следующий формат: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Например, чтобы подписаться на пользовательский раздел с именем **mytopic**, требуется разрешение Microsoft.EventGrid/EventSubscriptions/Write для следующего ресурса: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="topic-publishing"></a>Публикация разделов

Для разделов используется либо подписанный URL-адрес (SAS), либо проверка подлинности с использованием ключа. Рекомендуется использовать SAS, но проверка подлинности с использованием ключа обеспечивает простое программирование, а также совместима со множеством существующих издателей веб-перехватчиков. 

Значение проверки подлинности следует включить в заголовок HTTP. Для SAS в качестве значения заголовка используйте **aeg-sas-token**. Для подлинности с использованием ключа в качестве значения заголовка следует использовать **aeg-sas-key**.

### <a name="key-authentication"></a>Проверка подлинности с использованием ключа

Проверка подлинности с использованием ключа — самая простая форма проверки подлинности. Используйте следующий формат: `aeg-sas-key: <your key>`

Например, для передачи ключа можно использовать следующую команду: 

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Маркеры SAS

Маркеры SAS для сетки события включают ресурс, срок его действия и подпись. Маркер SAS имеет следующий формат: `r={resource}&e={expiration}&s={signature}`.

Ресурс — это путь для раздела, в который вы отправляете события. Вот пример допустимого пути к ресурсу: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

Вы создаете подпись на основе ключа.

Например, допустимое значение **aeg-sas-token** выглядит следующим образом:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
``` 

В следующем примере создается маркер SAS для использования с сеткой событий:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    string encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString());

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

## <a name="management-access-control"></a>Контроль доступа к управлению

Сетка событий Azure позволяет контролировать уровень доступа, предоставленного разным пользователям для выполнения различных операций управления, таких как создание списка подписок на события, создание новых подписок и генерирование ключей. Для этих целей сетка событий использует проверку доступа на основе ролей Azure (RBAC).

### <a name="operation-types"></a>Типы операций

Сетка событий Azure поддерживает следующие действия:

* Microsoft.EventGrid/*/read 
* Microsoft.EventGrid/*/write 
* Microsoft.EventGrid/*/delete 
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action 
* Microsoft.EventGrid/topics/listKeys/action 
* Microsoft.EventGrid/topics/regenerateKey/action

Последние три операции возвращают потенциально секретную информацию, которая отфильтровывается из обычных операций чтения. Это наилучший способ ограничить доступ к этим операциям. Настраиваемые роли можно создавать с помощью [Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md), [интерфейса командной строки (CLI) Azure](../active-directory/role-based-access-control-manage-access-azure-cli.md) и интерфейса [REST API](../active-directory/role-based-access-control-manage-access-rest.md).

### <a name="enforcing-role-based-access-check-rbac"></a>Применение проверки доступа на основе ролей (RBAC)

Чтобы принудительно применить RBAC для разных пользователей, выполните следующие действия:

#### <a name="create-a-custom-role-definition-file-json"></a>Создание файла определения пользовательской роли (.json)

Вот пример определений роли сетки событий, которые позволяют пользователям выполнять разные действия.

**EventGridReadOnlyRole.json**: разрешено только чтение.
```json
{ 
  "Name": "Event grid read only role", 
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856", 
  "IsCustom": true, 
  "Description": "Event grid read only role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/read" 
  ], 
  "NotActions": [ 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription Id>" 
  ] 
}
```

**EventGridNoDeleteListKeysRole.json**: разрешен ограниченный набор действий по публикации и запрещены действия удаления.
```json
{ 
  "Name": "Event grid No Delete Listkeys role", 
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174", 
  "IsCustom": true, 
  "Description": "Event grid No Delete Listkeys role", 
  "Actions": [     
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action" 
  ], 
  "NotActions": [ 
    "Microsoft.EventGrid/*/delete" 
  ], 
  "AssignableScopes": [ 
    "/subscriptions/<Subscription id>" 
  ] 
}
```

**EventGridContributorRole.json**: разрешено выполнять все действия сетки событий.  
```json
{ 
  "Name": "Event grid contributor role", 
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514", 
  "IsCustom": true, 
  "Description": "Event grid contributor role", 
  "Actions": [ 
    "Microsoft.EventGrid/*/write", 
    "Microsoft.EventGrid/*/delete", 
    "Microsoft.EventGrid/topics/listkeys/action", 
    "Microsoft.EventGrid/topics/regenerateKey/action", 
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action" 
  ], 
  "NotActions": [], 
  "AssignableScopes": [ 
    "/subscriptions/d48566a8-2428-4a6c-8347-9675d09fb851" 
  ] 
} 
```

#### <a name="install-and-login-to-azure-cli"></a>Установка интерфейса командной строки Azure и вход в него

* Интерфейс командной строки Azure версии 0.8.8 или более поздней. Чтобы установить последнюю версию и связать ее со своей подпиской Azure, см. статью [Установка Azure CLI](../cli-install-nodejs.md).
* Azure Resource Manager в Azure CLI. Дополнительные сведения см. в статье [Управление ресурсами и группами ресурсов Azure с помощью интерфейса командной строки Azure](../xplat-cli-azure-resource-manager.md).

#### <a name="create-a-custom-role"></a>Создание настраиваемой роли

Чтобы создать настраиваемую роль, используйте следующую команду:

    azure role create --inputfile <file path>

#### <a name="assign-the-role-to-a-user"></a>Назначение роли пользователю


    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>


## <a name="next-steps"></a>Дальнейшие действия

* Общие сведения о сетке событий см. в статье [Сведения о сетке событий](overview.md)

