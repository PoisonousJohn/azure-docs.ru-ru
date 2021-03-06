---
title: "Отчеты о доступе и использовании для Azure MFA | Документация Майкрософт"
description: "Эта статья содержит информацию о том, как использовать функцию отчетов службы Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: f76e726c6a67de4b0472c0e97f9e72c31c14c4f2
ms.contentlocale: ru-ru
ms.lasthandoff: 05/15/2017


---
# <a name="reports-in-azure-multi-factor-authentication"></a>Отчеты в службе Azure Multi-Factor Authentication
Azure Multi-Factor Authentication предоставляет несколько типов отчетов, которые могут быть полезны для вас и вашей организации. Доступ к этим отчетам можно получить через портал управления Многофакторной идентификации. Ниже приведен список доступных отчетов.

| Отчет | Описание |
|:--- |:--- |
| Использование |В отчетах об использовании содержится информация об общем использовании, сводный отчет по пользователям и сведения о пользователях. |
| Состояние серверов |В этом отчете отображается состояние серверов Multi-Factor Authentication, с которыми связана ваша учетная запись. |
| Журнал заблокированных пользователей |В этих отчетах отображается журнал запросов о блокировании и разблокировании пользователей. |
| Журнал обойденных пользователей |Отображается журнал запросов об обходе службы Multi-Factor Authentication в случае использования того или иного номера телефона. |
| Предупреждение о мошенничестве |Отображается история оповещений о мошенничестве, отправленных в рамках указанного диапазона дат. |
| Поставлено в очередь |Отображается список отчетов, поставленных в очередь для обработки, и их состояние. Ссылка на загрузку или просмотр отчета предоставляется по завершении его составления. |

## <a name="view-reports"></a>Просмотр отчетов
1. Войдите на [классический портал Azure](https://manage.windowsazure.com).
2. Выберите слева элемент Active Directory.
3. Выберите один из описанных ниже вариантов в зависимости от того, используются ли поставщики аутентификации.
   * **Вариант 1**: перейдите на вкладку "Поставщики Multi-Factor Authentication". Выберите поставщика MFA и нажмите кнопку **Управление** в нижней части окна.
   * **Вариант 2**: выберите свой каталог и перейдите на вкладку **Настройка**. В разделе многофакторной проверки подлинности щелкните **Управление параметрами службы**. В нижней части страницы "Параметры службы MFA" щелкните "Перейти на портал".
4. На портале управления Многофакторной идентификации Azure выберите необходимый тип отчета в области навигации слева в разделе **Просмотреть отчет**.

<center>![Облако](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Дополнительные ресурсы**

* [Для пользователей](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication в MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)

