---
title: "Руководство. Интеграция Azure Active Directory с AnswerHub | Документация Майкрософт"
description: "Узнайте, как настроить единый вход Azure Active Directory в приложении AnswerHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 3a1c9cc5d7a2ebe28e9fb7e0e6ed8e3d393873ae
ms.contentlocale: ru-ru
ms.lasthandoff: 06/20/2017


---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Руководство. Интеграция Azure Active Directory с AnswerHub

В этом руководстве описано, как интегрировать AnswerHub с Azure Active Directory (Azure AD).

Интеграция Azure AD с AnswerHub обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к AnswerHub.
- Вы можете включить автоматический вход пользователей в AnswerHub (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с AnswerHub, вам потребуется:

- подписка Azure AD;
- подписка AnswerHub с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление AnswerHub из коллекции
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-answerhub-from-the-gallery"></a>Добавление AnswerHub из коллекции
Чтобы настроить интеграцию AnswerHub с Azure AD, необходимо добавить AnswerHub из коллекции в список управляемых приложений SaaS.

**Чтобы добавить AnswerHub из коллекции, сделайте следующее.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Приложения][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Приложения][3]

4. В поле поиска введите **AnswerHub**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. На панели результатов выберите **AnswerHub** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в AnswerHub с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в AnswerHub соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в AnswerHub.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в AnswerHub.

Чтобы настроить и проверить единый вход Azure AD в AnswerHub, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя AnswerHub](#creating-an-answerhub-test-user)** нужно для того, чтобы в AnswerHub также существовал пользователь Britta Simon, связанный с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход в Azure AD на портале Azure и настроить его в приложении AnswerHub.

**Чтобы настроить единый вход Azure AD в AnswerHub, сделайте следующее.**

1. На портале Azure на странице интеграции с приложением **AnswerHub** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. В разделе **Домены и URL-адреса приложения AnswerHub** сделайте следующее.

    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `https://<company>.answerhub.com`

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<company>.answerhub.com`

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Замените эти значения фактическим URL-адресом для входа и идентификатором. Чтобы получить эти значения, обратитесь к [группе поддержки AnswerHub](mailto:success@answerhub.com). 
 
4. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. В разделе **Настройка AnswerHub** щелкните **Настроить AnswerHub**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода и URL-адрес службы единого входа SAML** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. В другом окне веб-браузера войдите на свой корпоративный веб-сайт AnswerHub в качестве администратора.
   
    >[!NOTE]
    >Если вам нужна помощь в настройке AnswerHub, обратитесь в [службу поддержки AnswerHub](mailto:success@answerhub.com.).
   
8. Перейдите в раздел **Administration**(Администрирование).

9. Откройте вкладку **User and Group** (Пользователь и группа).

10. В разделе **Social Settings** (Параметры социальных сетей) в области навигации слева выберите пункт **SAML Setup** (Настройка SAML).

11. Откройте вкладку **IDP Config** (Настройка IDP).

12. На вкладке **IDP Config** (Настройка IDP) выполните следующие действия.

     ![Настройка SAML](./media/active-directory-saas-answerhub-tutorial/ic785172.png "Настройка SAML")  
  
     а. В текстовое поле **IDP Login URL** (URL-адрес входа IdP) вставьте значение **URL-адрес службы единого входа SAML**, скопированное на портале Azure.
  
     b. В текстовое поле **IDP Logout URL** (URL-адрес выхода IdP) вставьте значение **URL-адрес выхода**, скопированное на портале Azure.
     
     c. В текстовое поле **IDP Name Identifier Format** (Формат идентификатора имени IdP) введите значение идентификатора имени пользователя, выбранное на портале Azure в разделе **Атрибуты пользователя**.
  
     г) Щелкните **Ключи и сертификаты**.

13. На вкладке «Ключи и сертификаты» выполните следующие действия.
    
     ![Ключи и сертификаты](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Ключи и сертификаты")  
 
     а. Откройте в Блокноте сертификат в кодировке Base-64, скачанный с портала Azure, скопируйте его содержимое в буфер обмена, а затем вставьте его в текстовое поле **IDP Public Key (x509 Format)** (Открытый ключ IdP (формат x509)).
  
     b. Щелкните **Сохранить**.

14. На вкладке **IDP Config** (Настройка IDP) нажмите кнопку **Сохранить**.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    а. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Щелкните **Создать**.
 
### <a name="creating-an-answerhub-test-user"></a>Создание тестового пользователя AnswerHub

Чтобы пользователи Azure AD могли выполнять вход в AnswerHub, они должны быть подготовлены для AnswerHub.  
В случае с AnswerHub подготовка выполняется вручную.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

1. Выполните вход на корпоративном веб-сайте **AnswerHub** в качестве администратора.

2. Перейдите в раздел **Administration**(Администрирование).

3. Откройте вкладку **Users & Groups** (Пользователи и группы).

4. В разделе **Manage Users** (Управление пользователями) в области навигации слева выберите пункт **Create or import users** (Создать или импортировать пользователей).
   
   ![Пользователи и группы](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Пользователи и группы")

5. Введите в текстовые поля **Email address** (Адрес электронной почты), **Username** (Имя пользователя) и **Password** (Пароль) соответствующие данные действующей учетной записи Azure Active Directory, которую нужно подготовить, и нажмите кнопку **Save** (Сохранить).

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя AnswerHub или API, предоставляемые AnswerHub для подготовки учетных записей пользователя AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив этому пользователю доступ к AnswerHub.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в AnswerHub, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. Из списка приложений выберите **AnswerHub**.

    ![Настройка единого входа](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент "AnswerHub" на панели доступа, вы автоматически войдете в приложение Anaplan.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png


