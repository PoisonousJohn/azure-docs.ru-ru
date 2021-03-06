---
title: "Выделенные группы в Azure Active Directory | Документация Майкрософт"
description: "Обзор работы выделенных групп в Azure Active Directory и способов их создания."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;oldportal
ms.translationtype: Human Translation
ms.sourcegitcommit: a4a78b92c8bb9e8aff25fd724ed78283de8f2fd8
ms.openlocfilehash: 92b9c88ec49424c96c3bd21bc5c4ce390352c17b
ms.contentlocale: ru-ru
ms.lasthandoff: 02/13/2017

---
# <a name="dedicated-groups-in-azure-active-directory"></a>Выделенные группы в Azure Active Directory
В Azure Active Directory (Azure AD) функция выделенных групп автоматически создает и заполняет предварительно определенные группы Azure AD. Нельзя добавлять или удалять участников выделенных групп с помощью классического портала Azure, командлетов Windows PowerShell или программно.

> [!NOTE]
> Выделенные группы требуют назначения лицензии Azure AD Premium:
>
> * администратору, который управляет правилом в группе;
> * всем пользователям, которых правило выбрало в качестве участников группы.
>
>

**Включение выделенных групп**

1. На [классическом портале Azure](https://manage.windowsazure.com)щелкните **Active Directory**, а затем откройте каталог своей организации.
2. Перейдите на вкладку **Группы** , а затем откройте группу, которую нужно изменить.
3. Выберите вкладку **Настройка**, а затем для параметра **Включить выделенные группы** выберите значение **Да**.

После включения выделенных групп, т. е. выбора значения **Да**, можно дополнительно включить автоматическое создание выделенной группы "Все пользователи" в каталоге, задав для параметра **Включить группу "Все пользователи"** значение **Да**. Затем можно также изменить имя этой выделенной группы, введя его в поле **Отображаемое имя для группы "Все пользователи"**.

Группу "Все пользователи" можно использовать для назначения одинаковых разрешений всем пользователям в каталоге. Например, можно предоставить всем пользователям в каталоге доступ к приложению SaaS, назначив выделенной группе «Все пользователи» доступ к этому приложению.

Выделенная группа "Все пользователи" включает в себя всех пользователей в каталоге, в том числе гостей и внешних пользователей. Если требуется группа без внешних пользователей, вы можете создать группу с помощью динамического правила на основе атрибутов, например:

                (user.userPrincipalName -notContains "#EXT#@")

Для группы, не включающей в себя гостей, используйте такое правило:

                (user.userType -ne "Guest")

Сведения о том, как создавать *более сложные* правила (которые содержат несколько сравнений), регулирующие членство в динамических группах, см. в статье [Использование атрибутов для создания расширенных правил](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Дальнейшие действия
В следующих статьях содержатся дополнительные сведения об Azure Active Directory.

* [Управление доступом к ресурсам с помощью групп Azure Active Directory](active-directory-manage-groups.md)
* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
* [Что такое Microsoft Azure Active Directory](active-directory-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)

