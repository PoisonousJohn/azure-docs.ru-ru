---
title: "Настройка единого входа Azure AD для приложений | Документация Майкрософт"
description: "Узнайте, как самостоятельно подключить приложения к Azure Active Directory с использованием SAML и единого входа на основе пароля"
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18415c92d50a00c14823685857ab7e2624334ec7
ms.openlocfilehash: b4a1bb3211da8c02d48ebad69d5e1cbb4de2c45d
ms.contentlocale: ru-ru
ms.lasthandoff: 03/01/2017

---
# <a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Настройка единого входа для приложений, которых нет в коллекции приложений Azure Active Directory
Эта статья о функциях, с помощью которых администраторы могут настроить единый вход для приложений, не принадлежащих к коллекции приложений Azure Active Directory, *без написания кода*. Эта возможность добавлена 18 ноября 2015 г. в составе технической предварительной версии и включена в [Azure Active Directory Premium](active-directory-editions.md). Если вам все же необходимо руководство для разработчиков по интеграции пользовательских приложений в Azure AD с использованием кода, см. статью [Сценарии аутентификации в Azure Active Directory](active-directory-authentication-scenarios.md).

Коллекция приложений Azure Active Directory содержит список приложений, которые поддерживают единый вход с помощью Azure Active Directory. Эта функция описана в [этой статье](active-directory-appssoaccess-whatis.md). Когда вы (как специалист ИТ-отдела или системный интегратор) найдете приложение, которое нужно подключить, сначала выполните пошаговые инструкции по включению единого входа, которые предложены на портале управления Azure.

Клиенты, у которых есть лицензии [Azure Active Directory Premium](active-directory-editions.md) , также получают дополнительные возможности:

* самостоятельная интеграция любого приложения, которое совместимо с поставщиками удостоверений SAML 2.0 (инициированная поставщиком услуг или поставщиком удостоверений);
* самостоятельная интеграция любого веб-приложения, где есть HTML-страница входа, с использованием функции [единого входа на основе пароля](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Самостоятельное подключение приложений, которые используют протокол SCIM для подготовки пользователей ([процесс описан здесь](active-directory-scim-provisioning.md))
* Возможность добавлять ссылки на любые приложения в [Средство запуска приложений Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) или [Панель доступа Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

Подключать можно не только приложения SaaS, которых еще нет в коллекции приложений Azure AD, но и любые сторонние веб-приложения, которые ваша организация развернула на локальных или облачных серверах, к управлению которыми вы имеете доступ.

Эти преимущества также известны как *шаблоны интеграции приложений*. Они предоставляют точки подключения на основе стандартов для приложений с поддержкой аутентификации на основе SAML, SCIM или форм и предусматривают гибкие параметры и настройки для совместимости с широким спектром приложений. 

## <a name="adding-an-unlisted-application"></a>Добавление приложений, которые отсутствуют в списке
Чтобы подключиться к приложению, используя шаблон интеграции приложений, войдите на портал управления Azure, используя данные учетной записи администратора Azure Active Directory. Последовательно выберите **Active Directory > [Каталог] > Приложения**, щелкните **Добавить**, а затем — **Добавить приложение из коллекции**. 

![][1]

В коллекции приложений вы можете добавлять приложения, которые отсутствуют в списке, в категорию **Пользовательские** слева или использовать для этого ссылку **Add an unlisted application** (Добавить приложение, отсутствующее в списке), которая отображается в результатах поиска, если нужное приложение не найдено. После ввода имени приложения можно настроить параметры и поведение функции единого входа. 

**Совет**. Сначала воспользуйтесь поиском. Возможно, нужное приложение уже существует в коллекции приложений. Если приложение найдено и в его описании указан единый вход, значит, федеративный единый вход для этого приложения уже поддерживается. 

![][2]

Использование приложения после добавления в список таким способом будет похожим на использование уже интегрированных приложений. Выберите элемент **Настройка единого входа**. На следующем экране представлены три варианта настройки единого входа. Их описание вы найдете в следующих разделах.

![][3]

## <a name="azure-ad-single-sign-on"></a>единого входа Azure AD
Этот метод нужен, чтобы настроить для приложения проверку подлинности на основе SAML. Для этого приложение должно поддерживать SAML 2.0. Прежде чем продолжить, узнайте, как использовать SAML для вашего приложения. Когда вы нажмете **Далее**, вам будет предложено ввести три различных URL-адреса конечных точек SAML для приложения. 

![][4]

а именно:

* **URL-адрес входа (только инициированный поставщиком услуг)** , по которому пользователь переходит для входа в это приложение. Если приложение настроено для выполнения единого входа, инициированного поставщиком услуг, то переходе пользователя на этот URL-адрес поставщик услуг выполнит необходимое перенаправление в Azure AD для проверки подлинности и входа пользователя. Если это поле заполнено, Azure AD будет использовать этот URL-адрес для запуска приложения из Office 365 и панели доступа Azure AD. Если это поле пропускается, Azure AD выполнит единый вход, инициированный поставщиком удостоверений, при запуске приложения из Office 365, с панели доступа Azure AD или URL-адреса единого входа Azure AD (отображается на вкладке "Панель мониторинга").
* **URL-адрес издателя**. URL-адрес издателя должен однозначно определять приложение, для которого выполняется настройка единого входа. Это значение, которое Azure AD отправляет назад в приложение в качестве параметра **Аудитория** токена SAML, а приложение должно его проверить. Это значение также отображается как **идентификатор сущности** во всех метаданных SAML, предоставленных приложением. Сведения о значениях параметров "Идентификатор сущности" и "Аудитория" см. в документации по SAML приложения.  Ниже приведен пример того, как отображается URL-адрес аудитории в токене SAML, возвращенном в приложение.

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **URL-адрес ответа**. URL-адрес ответа указывает место, где приложение ожидает токен SAML. Он также называется **URL-адресом службы обработчика утверждений (ACS)**. Сведения об URL-адресе ответа токена SAML или URL-адресе ACS см. в документации по SAML приложения.
  После ввода адресов нажмите кнопку **Далее**, чтобы перейти на следующий экран. Этот экран сообщает, какие параметры вам нужно настроить на стороне приложения, чтобы оно принимало токен SAML от Azure AD. 

![][5]

Разные приложения используют разные параметры. Выбрать нужные параметры поможет раздел по SAML в документации приложения. URL-адреса **входа** и **выхода** указывают на одну конечную точку, то есть на конечную точку обработки запроса SAML для вашего экземпляра Azure AD. URL-адрес поставщика указан в параметре Issuer токена SAML, который выдан вашему приложению. 

Когда вы завершите настройку приложения, нажмите кнопку **Далее**, а затем — **Завершить**, чтобы закрыть диалоговое окно. 

## <a name="assigning-users-and-groups-to-your-saml-application"></a>Назначение пользователей и групп для приложения SAML
Когда приложение будет готово к использованию Azure AD в качестве поставщика удостоверений на основе SAML, останется сделать совсем немного. Azure AD из соображений безопасности не выдает токен, который позволяет войти в приложение, если у пользователя нет права на доступ к приложению в Azure AD. Такие права можно выдать пользователю напрямую или через одну из групп, в которые он входит. 

Чтобы назначить для приложения пользователей или группы, нажмите кнопку **Назначить пользователей** . Выберите нужного пользователя или группу, а затем нажмите кнопку **Назначить** . 

![][6]

Когда вы назначите пользователя, Azure AD сможет выдавать ему токен для входа. Также на панели доступа для этого пользователя появится плитка этого приложения. Если пользователь работает в Office 365, плитка приложения появится и в средстве запуска приложений Office 365. 

Вы можете загрузить логотип, который будет отображаться на плитке приложения. Для этого нажмите кнопку **Загрузить логотип** на вкладке **Настройка** для этого приложения. 

### <a name="customizing-the-claims-issued-in-the-saml-token"></a>Настройка утверждений, которые передаются в токене SAML
Когда пользователь проходит проверку подлинности для приложения, Azure AD выдает токен SAML для приложения. В токене собрана информация (утверждения), которая однозначно идентифицирует пользователя. По умолчанию здесь указаны имя для входа, адрес электронной почты, имя и фамилия пользователя. 

На вкладке **атрибуты** вы можете просмотреть или изменить утверждения, которые передаются приложению в токене SAML. 

![][7]

Изменение утверждений, выпущенных в маркере SAML, может потребоваться по двум основным причинам: • приложение требует другого набора URI утверждений или значений утверждений; • приложение развернуто таким образом, что утверждение NameIdentifier должно содержать данные, отличные от имени пользователя (т. е. основного имени пользователя), которое хранится в Azure Active Directory. 

Сведения о том, как добавлять и редактировать утверждения, см. в этой [статье о настройке утверждений](active-directory-saml-claims-customization.md). 

### <a name="testing-the-saml-application"></a>Тестирование приложения SAML
Как только URL-адреса и сертификаты SAML будут настроены в Azure AD и в приложении, пользователи или группы будут назначены для приложения в Azure, а утверждения будут проверены и при необходимости изменены, пользователь сможет выполнять вход в приложение. 

Для проверки входа откройте панель доступа Azure AD по адресу https://myapps.microsoft.com с той учетной записью, которую вы назначили для приложения. На панели доступа щелкните элемент нужного приложения и убедитесь, что вход выполняется корректно. Кроме того, вы можете зайти на страницу входа в приложение и ввести учетные данные на ней. 

Некоторые советы по отладке приведены в [статье об отладке единого входа в приложения на основе SAML](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Единый вход с помощью пароля
Выберите этот вариант, чтобы настроить [единый вход с помощью пароля](active-directory-appssoaccess-whatis.md) для веб-приложения, у которого есть HTML-страница входа. Единый вход с помощью пароля (хранение пароля в хранилище) позволяет управлять доступом и паролями для веб-приложений, которые не поддерживают федерацию удостоверений. Также он полезен в ситуациях, когда несколько пользователей работают под одной учетной записью (например, с корпоративными страницами в социальных сетях). 

Когда вы нажмете кнопку **Далее**, вам будет предложено ввести URL-адрес веб-страницы входа в приложение. Обратите внимание, что здесь нужно указать страницу с полями ввода имени пользователя и пароля. После ввода информации Azure AD запускает процесс синтаксического анализа страницы входа, чтобы найти поля для ввода имени пользователя и пароля. Если поиск не будет успешным, вам будет предложен альтернативный процесс с установкой расширения браузера (поддерживаются Internet Explorer, Chrome и Firefox). Вы сможете вручную выбрать и указать поля ввода.

Когда страница входа будет определена, вы сможете назначить для приложения пользователей и группы, а также установить политики учетных данных, как и для обычных [приложений с единым входом по паролю](active-directory-appssoaccess-whatis.md).

Примечание. Вы можете загрузить логотип, который будет отображаться на элементе приложения. Для этого нажмите кнопку **Загрузить логотип** на вкладке **Настройка** для этого приложения. 

## <a name="existing-single-sign-on"></a>Существующий единый вход
Выберите этот вариант, чтобы добавить ссылку на ваше приложение в панель доступа Azure AD или на портал Office 365. Этот метод подходит для добавления ссылок на пользовательские веб-приложения, которые используют для проверки подлинности службы федерации Active Directory Azure (или другую службу федерации), а не Azure AD. Еще он позволяет просто добавить глубокие ссылки на определенные страницы SharePoint или другие веб-сайты. Они появятся на панели доступа пользователя. 

Когда вы нажмете кнопку **Далее**, вам будет предложено ввести URL-адрес приложения. После завершения процесса вы сможете назначить для приложения пользователей и группы. У этих пользователей появится элемент приложения в [средстве запуска приложений Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) или на [панели доступа Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

Примечание. Вы можете загрузить логотип, который будет отображаться на элементе приложения. Для этого нажмите кнопку **Загрузить логотип** на вкладке **Настройка** для этого приложения.

## <a name="related-articles"></a>Связанные статьи
* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
* [Настройка утверждений, выпущенных в маркере SAML для предварительно интегрированных приложений](active-directory-saml-claims-customization.md)
* [Устранение неполадок единого входа на основе SAML](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png

