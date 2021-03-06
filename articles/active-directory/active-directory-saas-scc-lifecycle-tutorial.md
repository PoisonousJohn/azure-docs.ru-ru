---
title: "Руководство по интеграции Azure Active Directory с SCC LifeCycle | Документация Майкрософт"
description: "Узнайте, как использовать SCC LifeCycle с Azure Active Directory для реализации единого входа, автоматической подготовки к работе и многого другого."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Учебник. Интеграция Azure Active Directory с SCC LifeCycle
Цель данного учебника — показать интеграцию Azure и SCC LifeCycle.  

Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

* Действующая подписка на Azure
* Подписка SCC LifeCycle с поддержкой единого входа.

После завершения этого руководства пользователи Azure AD, назначенные SCC LifeCycle, будут иметь возможность единого входа в приложение на веб-сайте компании SCC LifeCycle (вход, инициированный поставщиком услуг) или с помощью инструкций из статьи [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1. Включение интеграции приложений для SCC LifeCycle
2. Настройка единого входа.
3. Настройка подготовки учетных записей пользователей
4. Назначение пользователей

![Сценарий](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Сценарий")

## <a name="enable-the-application-integration-for-scc-lifecycle"></a>Включение интеграции приложений для SCC LifeCycle
В этом разделе показано, как включить интеграцию приложений для SCC LifeCycle.

**Чтобы включить интеграцию приложений для SCC LifeCycle, сделайте следующее:**

1. На классическом портале Azure в области навигации слева щелкните **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.
3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
   
    ![Приложения](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Приложения")
4. В нижней части страницы нажмите кнопку **Добавить** .
   
    ![Добавление приложения](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Добавление приложения")
5. В диалоговом окне **Что необходимо сделать?** щелкните **Добавить приложение из коллекции**.
   
    ![Добавление приложения из коллекции](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Добавление приложения из коллекции")
6. В **поле поиска** введите **SCC LifeCycle**.
   
    ![Коллекция приложений](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Коллекция приложений")
7. В области результатов выберите **SCC LifeCycle** и нажмите кнопку **Завершить**, чтобы добавить приложение.
   
    ![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
   
## <a name="configure-single-sign-on"></a>Настройка единого входа

В этом разделе показано, как разрешить пользователям проходить аутентификацию в SCC LifeCycle со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

**Чтобы настроить единый вход, выполните следующие действия:**

1. На классическом портале Azure на странице интеграции с приложением **SCC LifeCycle** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Настройка единого входа")
2. На странице **Как пользователи должны входить в SCC LifeCycle?** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.
   
    ![Настройка единого входа](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Настройка единого входа")
3. На странице **Настроить URL-адрес приложения** в текстовом поле **URL-адрес входа** введите URL-адрес, используемый пользователями для входа в приложение SCC LifeCycle, в формате *https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*, после чего и нажмите кнопку **Далее**.
   
    ![Настройка URL-адреса приложения](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Настройка URL-адреса приложения")
4. На странице **Настройка единого входа в SCC LifeCycle** щелкните **Скачать метаданные**, а затем сохраните файл метаданных на локальном компьютере.
   
   ![Настройка единого входа](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Настройка единого входа")
5. Передайте этот файл метаданных в группу поддержки SCC LifeCycle.
   
   >[!NOTE]
   >Единый вход должна включить группа поддержки SCC LifeCycle.
   > 
   > 

6. На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.
   
    ![Настройка единого входа](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Настройка единого входа")
   
## <a name="configure-user-provisioning"></a>Настроить подготовку учетных записей пользователей

Чтобы пользователи Azure AD могли выполнять вход в SCC LifeCycle, они должны быть подготовлены для SCC LifeCycle. Действие для настройки подготовки пользователей в SCC LifeCycle отсутствует.

Когда назначенный пользователь пытается войти в SCC LifeCycle, учетная запись SCC LifeCycle создается автоматически (при необходимости).

>[!NOTE]
>Вы можете использовать любые другие инструменты создания учетной записи пользователя SCC LifeCycle или API, предоставляемые SCC LifeCycle для подготовки учетных записей пользователя AAD.
> 
> 

## <a name="assign-users"></a>Назначить пользователей
Чтобы проверить свою конфигурацию, предоставьте пользователям Azure AD, которые должны использовать приложение, доступ путем их назначения.

**Чтобы назначить пользователей SCC LifeCycle, сделайте следующее:**

1. На классическом портале Azure создайте тестовую учетную запись.
2. На странице интеграции с приложением **SCC LifeCycle** щелкните **Назначить пользователей**.
   
    ![Назначение пользователей](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Назначение пользователей")
3. Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.
   
    ![Да](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Да")

Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа можно найти в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).


