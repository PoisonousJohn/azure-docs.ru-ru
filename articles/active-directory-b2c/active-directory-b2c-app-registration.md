---
title: "Azure Active Directory B2C: регистрация приложения | Документация Майкрософт"
description: "Как зарегистрировать приложение для использования Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 3d4fe2fa10d848c8b29e4d22d284c0d378f07ae0
ms.contentlocale: ru-ru
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: регистрация приложения

Из этого краткого руководства вы узнаете, как зарегистрировать приложение в клиенте Microsoft Azure Active Directory (Azure AD) B2C за несколько минут. Ознакомившись с этим руководством, вы зарегистрируете приложение для использования в клиенте Azure AD B2C.

## <a name="prerequisites"></a>Предварительные требования

Чтобы создать приложение с поддержкой регистрации и входа пользователей, его сначала нужно зарегистрировать с использованием клиента Azure Active Directory B2C. Получите собственный клиент, следуя указаниям в статье о [создании клиента Azure AD B2C](active-directory-b2c-get-started.md).

Управление приложениями, созданными в колонке Azure AD B2C на портале Azure, должно осуществляться в том же расположении. Если вы измените приложения B2C с помощью PowerShell или другого портала, их поддержка прекратится и они не будут работать с Azure AD B2C. Дополнительные сведения см. в разделе [Неисправные приложения](#faulted-apps). 

## <a name="navigate-to-b2c-settings"></a>Переход к параметрам B2C

Войдите на [портал Azure](https://portal.azure.com/) как глобальный администратор клиента B2C. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

Выберите следующие действия в зависимости от типа приложения, которое регистрируется:

* [регистрация веб-приложения;](#register-a-web-app)
* [регистрация веб-API;](#register-a-web-api)
* [регистрация мобильного или собственного приложения.](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a>Регистрация веб-приложения

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

Если веб-приложение вызовет веб-API, защищенный Azure AD B2C, выполните следующие действия.
   1. Создайте секрет приложения. Для этого перейдите в колонку **Ключи** и нажмите кнопку **Создать ключ**. Запишите значение параметра **Ключ приложения**. Это значение используется в качестве секрета приложения в коде приложения.
   2. Щелкните **Доступ через API**, **Добавить** и выберите свой веб-API и области (разрешения).

> [!NOTE]
> **Секрет приложения** — это важные учетные данные безопасности, которые должны быть надежно защищены.
> 

[Перейдите к **дальнейшим действиям**](#next-steps).

## <a name="register-a-web-api"></a>Регистрация веб-API

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Щелкните **Опубликованные области**, если необходимо добавить дополнительные области. По умолчанию определена область user_impersonation. Она позволяет предоставлять другим приложениям доступ к этому API от имени пользователя, выполнившего вход. При необходимости можно удалить эту область.

[Перейдите к **дальнейшим действиям**](#next-steps).

## <a name="register-a-mobile-or-native-app"></a>Регистрация мобильного или собственного приложения

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Перейдите к **дальнейшим действиям**](#next-steps).

## <a name="limitations"></a>Ограничения

### <a name="choosing-a-web-app-or-api-reply-url"></a>Выбор URL-адреса ответа для веб-приложения или API

Сейчас приложения, зарегистрированные с помощью Azure AD B2C, ограничены определенным набором значений URL-адресов ответа. URL-адрес ответа для веб-приложений и служб должен начинаться со схемы `https`. Все его значения должны совместно использовать один домен DNS. Например, невозможно зарегистрировать веб-приложение с одним из следующих URL-адресов ответа:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Система регистрации сравнивает полное DNS-имя существующего URL-адреса ответа и DNS-имя добавляемого URL-адреса ответа. Запрос на добавление DNS-имени завершится ошибкой при выполнении любого из следующих условий.

* Если DNS-имя нового URL-адреса ответа не соответствует DNS-имени существующего URL-адреса ответа.
* Если DNS-имя нового URL-адреса ответа не является поддоменом существующего URL-адреса ответа.

Например, в приложении используется этот URL-адрес ответа:

`https://login.contoso.com`

Можно добавить его следующим образом.

`https://login.contoso.com/new`

В этом случае DNS-имя полностью совпадает. Или же можно сделать следующее.

`https://new.login.contoso.com`

В этом случае используется ссылка на поддомен DNS, login.contoso.com. Если требуется создать приложение с URL-адресами ответа login-east.contoso.com и login-west.contoso.com, то нужно добавить эти URL-адреса ответа в таком порядке:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Два последних значения являются поддоменами первого URL-адреса ответа (contoso.com), поэтому их можно добавить.

### <a name="choosing-a-native-app-redirect-uri"></a>Выбор URI перенаправления для собственного приложения

При выборе URI перенаправления для мобильных и собственных приложений нужно учитывать два важных момента:

* **Уникальность**. Схема URI перенаправления должна быть уникальной для каждого приложения. В нашем примере (com.onmicrosoft.contoso.appname://redirect/path) мы используем com.onmicrosoft.contoso.appname как схему. Рекомендуется следовать этому шаблону. Если два приложения используют одну и ту же схему, пользователь увидит диалоговое окно выбора приложения. Если пользователь сделает неправильный выбор, произойдет ошибка входа.
* **Полнота**. URI перенаправления должен иметь схему и путь. Путь должен содержать по крайней мере одну косую черту после имени домена (например, //contoso/ будет работать, а при использовании //contoso произойдет ошибка).

Убедитесь, что URI перенаправления не содержит специальных знаков, например символов подчеркивания.

### <a name="faulted-apps"></a>Неисправные приложения

Не следует вносить изменения в приложения B2C:

* с помощью других порталов для управления приложениями, таких как [классический портал Azure](https://manage.windowsazure.com/) и [портал регистрации приложений](https://apps.dev.microsoft.com/);
* с помощью API Graph или PowerShell.

Если изменить приложение B2C, используя средства выше, а затем попытаться изменить его в колонке функций Azure AD B2C портала Azure, приложение станет неисправным и вы не сможете использовать его в Azure AD B2C. Вам придется удалить приложение и создать его заново.

Чтобы удалить приложение, перейдите на [портал регистрации приложений](https://apps.dev.microsoft.com/). Приложения отображаются только для владельцев. Прав администратора клиента недостаточно.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда у вас есть приложение, зарегистрированное для использования Azure AD B2C, вы можете изучить одно из [кратких руководств](active-directory-b2c-overview.md#get-started), чтобы подготовить его к работе и запустить.

> [!div class="nextstepaction"]
> [Azure AD B2C: регистрация и вход в систему в веб-приложении ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
