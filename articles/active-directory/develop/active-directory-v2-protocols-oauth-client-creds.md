
---
title: "Использование Azure AD версии 2.0 для доступа к защищенным ресурсам без участия пользователя | Документация Майкрософт"
description: "Построение веб-приложений с помощью реализации протокола проверки подлинности OAuth 2.0 в Azure AD."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 93b54c3fc4397573f77b2e157c6f1866786690da
ms.contentlocale: ru-ru
ms.lasthandoff: 06/08/2017


---
# Azure Active Directory версии 2.0 и поток учетных данных клиента OAuth 2.0
[Предоставление учетных данных клиента OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-4.4), которое иногда называют также *двусторонним OAuth*, можно использовать для доступа к интернет-ресурсам с помощью удостоверения приложения. Как правило, подобное предоставление используется для взаимодействия между серверами, которое должно выполняться в фоновом режиме без немедленного вмешательства пользователя. Приложения такого типа часто называют *управляющими программами* или *учетными записями служб*.

> [!NOTE]
> Не все сценарии и компоненты Azure Active Directory поддерживаются конечной точкой версии 2.0. Чтобы определить, стоит ли вам использовать конечную точку версии 2.0, ознакомьтесь с [ограничениями версии 2.0](active-directory-v2-limitations.md).
>
>

В более распространенном сценарии *трехстороннего OAuth* клиентскому приложению предоставляется разрешение на доступ к ресурсу от имени определенного пользователя. Обычно разрешение делегируется приложению пользователем во время [предоставления разрешений](active-directory-v2-scopes.md). Однако в потоке учетных данных клиента разрешения предоставляются приложению напрямую. Когда приложение предоставляет маркер для ресурса, то для выполнения действия этот ресурс требует авторизацию приложения, а не пользователя.

## Схема протокола
Весь поток учетных данных клиента аналогичен приведенному на следующей схеме. Описание каждого шага приведено далее в этой статье.

![Поток учетных данных клиента](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## Получение прямой авторизации
Как правило, приложение получает прямую авторизацию доступа к ресурсу двумя способами: с помощью списка управления доступом (ACL) для ресурса или назначения разрешений приложению в Azure Active Directory (Azure AD). Эти два метода являются самыми распространенными в Azure AD, и их рекомендуется использовать для клиентов и ресурсов, применяющих поток учетных данных клиента. Тем не менее ресурс может выбрать другой способ авторизации клиентов. Каждый сервер ресурсов может выбрать метод, являющийся наиболее оптимальным для приложения.

### Доступ к спискам управления
Поставщик ресурсов может принудительно применять проверку авторизации на основе списка известных идентификаторов приложений, а также предоставлять определенный уровень доступа на основе этих идентификаторов. Когда ресурс получает маркер из конечной точки версии 2.0, он может расшифровать этот маркер и извлечь идентификатор приложения клиента из утверждений `appid` и `iss`. Затем он проверяет наличие приложения в действующем списке управления доступом. Детализация и методы использования списка управления доступом для разных ресурсов могут значительно отличаться.

Нередко список управления доступом используется для выполнения тестов веб-приложения или веб-API. Веб-API может предоставить определенному клиенту только ограниченный набор прав доступа. Для выполнения комплексных тестов API создайте тестовый клиент, получающий маркеры от конечной точки версии 2.0 и отправляющий их в API. Затем API может проверить идентификатор приложения тестового клиента с помощью списка управления доступом для предоставления доступа ко всем возможностям API. При использовании такого списка управления доступом обязательно проверяйте не только значение `appid` вызывающего объекта. Также убедитесь в надежности значения `iss` маркера.

Данный тип авторизации часто используется в управляющих программах и учетных записях служб, которым требуется доступ к данным пользователей, являющихся потребителями с личной учетной записью Майкрософт. Необходимую авторизацию для доступа к корпоративным данным рекомендуется получать с помощью разрешений приложения.

### Разрешения приложения
Вместо списков управления доступом можно использовать интерфейсы API, чтобы предоставлять набор разрешений приложения. Администратор организации предоставляет приложению разрешения, которые можно использовать только для доступа к данным, принадлежащим этой организации и ее сотрудникам. Например, Microsoft Graph предоставляет несколько разрешений приложений для следующих действий:

* чтение почты во всех почтовых ящиках;
* чтение и запись почты во всех почтовых ящиках;
* отправка почты любым пользователем;
* Прочитать данные каталога

Дополнительные сведения о разрешениях приложения см. на сайте [Microsoft Graph](https://graph.microsoft.io).

Чтобы использовать разрешения приложения в приложении, выполните указания в следующих разделах.

#### Запрос разрешений на портале регистрации приложения
1. Перейдите к приложению на [портале регистрации приложений](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) или [создайте приложение](active-directory-v2-app-registration.md), если это еще не сделано. При создании приложения потребуется использовать по крайней мере один секрет приложения.
2. Найдите раздел **Direct Application Permissions** (Непосредственные разрешения приложения) и добавьте разрешения, необходимые для приложения.
3. **Сохраните** регистрацию приложения.

#### Выполнение входа пользователя в приложение (рекомендуется)
Обычно при создании приложения, использующего разрешения, для него нужно настроить страницу или представление, в котором администратор может утвердить разрешения приложения. Эта страница может быть частью потока входа в приложение, параметров приложения или выделенного потока подключения. Во многих случаях разумно отображать это представление подключения в приложении только после того, как пользователь выполнил вход с помощью учебной или рабочей учетной записи Майкрософт.

Если используется вход пользователя в приложение, то можно определить организацию, к которой он принадлежит, прежде чем запрашивать у пользователя утверждение разрешений приложения. Хотя это не обязательно, таким образом можно сделать пользовательский интерфейс более интуитивно понятным. Чтобы выполнить вход пользователя, следуйте нашим [указаниям в учебниках по протоколу версии 2.0](active-directory-v2-protocols.md).

#### Запрос разрешений у администратора каталога
Когда вы будете готовы запросить разрешения у администратора организации, можно будет перенаправить пользователя в *конечную точку предоставления согласия администратора* версии 2.0.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Параметр | Условие | Описание |
| --- | --- | --- |
| tenant |Обязательно |Клиент каталога, из которого необходимо запросить разрешение. Его можно указать в виде GUID или понятного имени. Если неизвестно, какому клиенту относится пользователь, и нужно обеспечить его вход с помощью любого клиента, используйте `common`. |
| client_id |Обязательно |Идентификатор приложения, присвоенный приложению на [портале регистрации приложений](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). |
| redirect_uri |Обязательно |Универсальный код ресурса (URI) перенаправления, на который нужно направлять ответ для обработки в приложении. Он должен в точности соответствовать одному из универсальных кодов ресурсов (URI) перенаправления, зарегистрированных на портале, но иметь вид URL-адреса. Он может содержать дополнительные сегменты пути. |
| state |Рекомендуется |Значение, добавленное в запрос, которое также возвращается в ответе маркера. Это может быть строка любого контента. Параметр state используется для кодирования сведений о состоянии пользователя в приложении перед созданием запроса на аутентификацию, например сведений об открытой на тот момент странице или представлении. |

На этом этапе Azure AD обеспечивает вход только администратора клиента для выполнения запроса. Администратору будет предложено утвердить все непосредственные разрешения, запрошенные для приложения на портале регистрации приложений.

##### Успешный ответ
Если администратор утверждает разрешения для приложения, успешный ответ будет выглядеть следующим образом.

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Параметр | Описание |
| --- | --- | --- |
| tenant |Клиент каталога, предоставивший приложению запрошенные разрешения в формате GUID. |
| state |Значение, добавленное в запрос, которое также возвращается в ответе маркера. Это может быть строка любого контента. Параметр state используется для кодирования сведений о состоянии пользователя в приложении перед созданием запроса на аутентификацию, например сведений об открытой на тот момент странице или представлении. |
| admin_consent |Установите значение **True**. |

##### Сообщение об ошибке
Если администратор не утверждает разрешения для приложения, сообщение о неудачном выполнении будет выглядеть следующим образом.

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Параметр | Описание |
| --- | --- | --- |
| error |Строка кода ошибки, которую можно использовать для классификации типов ошибок и реагирования на ошибки. |
| error_description |Конкретное сообщение об ошибке, с помощью которого можно определить первопричину возникновения ошибки. |

После получения успешного ответа от конечной точки подготовки приложения приложение получило запрошенные непосредственные разрешения. Теперь вы можете запросить маркер для ресурса, который вам нужен.

## Получение маркера
Авторизовав приложение, можно переходить к получению маркеров доступа для интерфейсов API. Для получения маркера путем предоставления учетных данных клиента отправьте запрос POST к конечной точке версии 2.0 `/token`.

### Первый сценарий: запрос маркера доступа с помощью общего секрета

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Параметр | Условие | Описание |
| --- | --- | --- |
| client_id |Обязательно |Идентификатор приложения, присвоенный приложению на [портале регистрации приложений](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). |
| scope |Обязательно |Значение, передаваемое для параметра `scope` в этом запросе, должно быть идентификатором требуемого ресурса (универсальным кодом ресурса (URI) идентификатора приложения) с суффиксом `.default`. В примере Microsoft Graph используется значение `https://graph.microsoft.com/.default`. Это значение сообщает конечной точке версии 2.0, что среди всех непосредственных разрешений, настроенных для приложения, она должна выдать маркер для тех, которые связаны с требуемым ресурсом. |
| client_secret |Обязательно |Секрет приложения, созданный для приложения на портале регистрации приложений. |
| grant_type |Обязательно |Этот параметр должен содержать значение `client_credentials`. |

### Второй сценарий: запрос маркера доступа с помощью сертификата

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

| Параметр | Условие | Описание |
| --- | --- | --- |
| client_id |Обязательно |Идентификатор приложения, присвоенный приложению на [портале регистрации приложений](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). |
| scope |Обязательно |Значение, передаваемое для параметра `scope` в этом запросе, должно быть идентификатором требуемого ресурса (универсальным кодом ресурса (URI) идентификатора приложения) с суффиксом `.default`. В примере Microsoft Graph используется значение `https://graph.microsoft.com/.default`. Это значение сообщает конечной точке версии 2.0, что среди всех непосредственных разрешений, настроенных для приложения, она должна выдать маркер для тех, которые связаны с требуемым ресурсом. |
| client_assertion_type |обязательно |Значение должно быть `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`. |
| client_assertion |обязательно | Утверждение (JSON Web Token), которое необходимо создать и подписать с помощью сертификата, зарегистрированного как учетные данные для приложения. Ознакомьтесь с информацией об [учетных данных сертификата](active-directory-certificate-credentials.md), чтобы узнать, как зарегистрировать сертификат и задать формат утверждения.|
| grant_type |Обязательно |Этот параметр должен содержать значение `client_credentials`. |

Обратите внимание на то, что параметры являются почти такими же, как и при использовании запроса с помощью общего секрета, за исключением параметра client_secret, который заменяется двумя параметрами: client_assertion_type и client_assertion.

### Успешный ответ
Успешный ответ выглядит следующим образом.

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Параметр | Описание |
| --- | --- |
| access_token |Запрашиваемый маркер доступа. Приложение может использовать этот маркер для аутентификации в защищенном ресурсе, таком как веб-API. |
| token_type |Указывает значение типа маркера. Единственный тип, поддерживаемый Azure AD — `bearer` |
| expires_in |Срок действия маркера доступа (в секундах). |

### Сообщение об ошибке
Сообщение об ошибке выглядит следующим образом.

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Параметр | Описание |
| --- | --- |
| error |Строка кода ошибки, которую можно использовать для классификации типов возникших ошибок и реагирования на них. |
| error_description |Конкретное сообщение об ошибке, с помощью которого можно определить первопричину возникновения ошибки аутентификации. |
| error_codes |Список кодов ошибок, характерных для службы маркеров безопасности, которые могут помочь при диагностике. |
| Timestamp |Время возникновения ошибки. |
| trace_id |Уникальный идентификатор для запроса, который может помочь при диагностике. |
| correlation_id |Уникальный идентификатор для запроса, который может помочь при диагностике компонентов. |

## Использование маркера
После получения маркера его можно использовать для выполнения запросов к ресурсу. По истечении срока действия маркера повторно отправьте запрос к конечной точке `/token`, чтобы получить новый маркер доступа.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try the following command! (Replace the token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## Пример кода
Пример программной реализации предоставления учетных данных клиента с помощью конечной точки предоставления согласия администратора представлен в [примере кода управляющей программы версии 2.0](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

