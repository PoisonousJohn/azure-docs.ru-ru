---
title: "Коды ошибок отчета об активности входа на портале Azure Active Directory | Документация Майкрософт"
description: "Коды ошибок отчета об активности входа. Справочные материалы."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 2a1b7b87df2cd8fa2e98f217480b46f5f6334297
ms.contentlocale: ru-ru
ms.lasthandoff: 07/17/2017

---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Коды ошибок отчета об активности входа на портале Azure Active Directory

Информация, доступная в отчете о входе пользователя, поможет вам ответить на такие вопросы:

- Кто выполнил вход с помощью Azure Active Directory?
- В какие приложения выполнен вход?
- Какие входы завершились сбоем и почему?

В этой статье приведены коды ошибок и соответствующие описания. 

## <a name="how-can-i-display-failed-sign-ins"></a>Как отобразить входы, завершившиеся сбоем? 

Знакомство с данными о действиях входа следует начать с раздела **[События входа](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** в разделе **Действие** службы **Azure Active**.


![Действие входа](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Действие входа")


В отчете входа можно отобразить все попытки, завершившиеся сбоем, выбрав **Сбой** как **состояние входа**.


![Действие входа](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Действие входа")

Если вы щелкните элемент из отображаемого списка, откроется колонка **подробных сведений о входе**. Это представление предоставит подробные сведения, отслеживаемые Azure Active Directory о входах, включая **код ошибки входа** и **причину сбоя**.

![Действие входа](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Действие входа")


В качестве альтернативы использования портала Azure для доступа к данным входа можно также использовать [API отчетов](active-directory-reporting-api-getting-started-azure-portal.md).


В разделе ниже вы ознакомитесь с полным обзором всех возможных ошибок и соответствующими описаниями. 

## <a name="error-codes"></a>Коды ошибок

| Ошибка| Описание |
| --- | --- |
| 50001| Субъект-служба с именем X не найдена в клиенте с именем Y. Это может произойти, если приложение не было установлено администратором клиента. Или если основной ресурс не найден в каталоге или является недопустимым.|
| 50008| В маркере отсутствует утверждение SAML или же оно неправильно настроено.|
| 50011| Адрес ответа отсутствует, неправильно настроен или не соответствует адресам ответа, настроенным для приложения.|
| 50053| Учетная запись заблокирована из-за большого количества попыток входа с помощью неправильного идентификатора пользователя или пароля.|
| 50054| Для идентификации используется старый пароль.|
| 50055| Недопустимый пароль. Введен пароль с истекшим сроком действия.|
| 50057| Учетная запись пользователя отключена.|
| 50058| Нет сведений об удостоверении пользователя на основе предоставленных учетных данных, или пользователь не найден в клиенте, или был отправлен автоматический запрос на вход, но пользователь не выполнил вход, или службе не удалось выполнить аутентификацию пользователя.|
| 50074| Требуется строгая проверка подлинности (второго фактора).|
| 50079| Пользователь должен зарегистрироваться для двухфакторной проверки подлинности.|
| 50126| Недопустимое имя пользователя или пароль, или недопустимое локальное имя пользователя или пароль.|
| 50131| Используется в различных ошибках условного доступа. Например, при недопустимом состоянии устройства с Windows запрос блокируется из-за решений подозрительной активности, политики доступа и политики безопасности.|
| 50133| Сеанс является недопустимым из-за истечения срока действия или последней смены пароля.|
| 50144| Истек срок действия пароля пользователя Active Directory.|
| 65001| У приложения X нет разрешения на доступ к приложению Y или разрешение было отозвано. Или же пользователь или администратор не согласились использовать приложение с именем идентификатора X. Отправьте интерактивный запрос на проверку подлинности для этого пользователя и ресурса. Или же пользователь или администратор не согласились использовать приложение с именем X идентификатора. Отправьте запрос на проверку подлинности администратору клиента, чтобы действовать от имени приложения Y для ресурса Z.|
| 65005| Список доступа к обязательным ресурсам приложения не содержит приложения, обнаруживаемые ресурсом, или клиентское приложение запросило доступ к ресурсу, который не был указан в списке доступа к обязательным ресурсам, или служба Graph вернула недопустимый запрос или ресурс не был найден.|
| 70001| Приложение с именем X не найдено в клиенте с именем Y. Это может произойти, если приложение не было установлено администратором клиента или если ни один пользователь в клиенте не получил разрешение на установку. Возможно, вы отправили запрос на проверку подлинности в другой клиент.|
| 80001| Агент проверки подлинности недоступен.|
| 80002| Истекло время ожидания запроса на проверку пароля агента проверки подлинности.|
| 80003| Агент проверки подлинности получил недопустимый ответ.|
| 80004| В запросе на вход использовано неправильное имя участника-пользователя.|
| 80005| Агент проверки подлинности: произошла ошибка.|
| 80007| Агенту проверки подлинности не удалось подключиться к Active Directory.|
| 80010| Агенту проверки подлинности не удалось расшифровать пароль.|
| 81001| Билет Kerberos пользователя слишком большой.|
| 81002| Не удалось проверить билет Kerberos пользователя.|
| 81003| Не удалось проверить билет Kerberos пользователя.|
| 81004| Проверка подлинности Kerberos завершилась сбоем.|
| 81008| Не удалось проверить билет Kerberos пользователя.|
| 81009| Не удалось проверить билет Kerberos пользователя.|
| 81010| Не удалось выполнить простой единый вход, так как срок действия билета Kerberos пользователя истек или этот билет является недопустимым.|
| 81011| Не удалось найти объект пользователя на основе сведений в билете Kerberos пользователя.|
| 81012| Пользователь, пытающийся выполнить вход в Azure AD, и пользователь устройства отличаются.|
| 81013| Не удалось найти объект пользователя на основе сведений в билете Kerberos пользователя.|
| 90014| Используется в различных случаях, когда в учетных данных отсутствует ожидаемое поле.|
| 90093| Возвращен граф с запрещенным кодом ошибки для запроса.|



## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье [Отчеты о действиях входа на портале Azure Active Directory](active-directory-reporting-activity-sign-ins.md).


