---
title: "Использование Azure AD Connect при наличии существующего клиента Azure AD | Документация Майкрософт"
description: "В этом разделе описывается использование Connect при наличии существующего клиента Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: f59028a2f909914222236f3b3575afd0949b4277
ms.openlocfilehash: c89e206462856d25a81729e7028065ac1cd13ef3
ms.contentlocale: ru-ru
ms.lasthandoff: 02/23/2017

---

# Azure AD Connect: если имеется существующий клиент
<a id="azure-ad-connect-when-you-have-an-existent-tenant" class="xliff"></a>
В большинстве разделов об использовании Azure AD Connect предполагается, что вы начинаете с нового клиента Azure AD, в котором отсутствуют какие-либо пользователи или другие объекты. Но если вы начали с клиента Azure AD, заполнили его пользователями и другими объектами, а теперь хотите использовать Connect, то этот раздел предназначен именно для вас.

## Основные сведения
<a id="the-basics" class="xliff"></a>
Объект в Azure AD управляется в облаке (Azure AD) или в локальной среде. Невозможно управлять одними атрибутами отдельного объекта в локальной среде, а другими — в Azure AD. У каждого объекта имеется флаг, указывающий, где осуществляется управление этим объектом.

Можно управлять одними пользователями в локальной среде, а другими — в облаке. Распространенный сценарий для этой конфигурации — организация, состоящая из сотрудников отдела бухгалтерии и отдела продаж. Сотрудники отдела бухгалтерии используют локальную учетную запись AD, а сотрудники отдела продаж  — учетную запись в Azure AD. Будет удобно управлять одними пользователями в локальной среде, а другими — в Azure AD.

Если вы начали управлять пользователями в Azure AD, и некоторые пользователи также имеются в локальной среде AD, и вам потребовалось использовать Connect, то следует учесть некоторые дополнительные моменты.

## Синхронизация с существующими пользователями в Azure AD
<a id="sync-with-existing-users-in-azure-ad" class="xliff"></a>
Когда вы установите Azure AD Connect и начнете синхронизацию, служба синхронизации Azure AD (в Azure AD) будет проверять каждый новый объект и пытаться найти существующий объект для сопоставления. Для этого используются три атрибута: **userPrincipalName**, **proxyAddresses** и **sourceAnchor**/**immutableID**. Сопоставление по **userPrincipalName** и **proxyAddresses** называется **нестрогим сопоставлением**. Сопоставление по**sourceAnchor** называется **строгим сопоставлением**. Для оценки по атрибуту **proxyAddresses** используются только значения с **SMTP:**, представляющие собой основной электронный адрес.

Соответствие оценивается только для новых объектов, поступающих из Connect. Если изменить существующий объект, чтобы он соответствовал какому-либо из этих атрибутов, то появится сообщение об ошибке.

Если Azure AD находит объект, значения атрибутов которого совпадают с объектом, поступившим из Connect, и он уже существует в Azure AD, то предпочтение отдается объекту из Connect. Объект, ранее управляемый облаком, помечается как управляемый локально. Все атрибуты в Azure AD, значения которых есть в локальной среде AD, перезаписываются локальными значениями. Исключением является ситуация, когда атрибут имеет значение **NULL** в локальной среде. В этом случае значение в Azure AD остается, но вы можете изменить его в локальной среде на другое.

> [!WARNING]
> Так как все атрибуты в Azure AD будут перезаписаны значениями из локальной среды, убедитесь, что в ней хранятся корректные данные. Например, если вы только использовали управляемый электронный адрес в Office 365 и не обновляли его в локальной среде AD DS, то будут потеряны все значения в Azure AD и Office 365, которые отсутствуют в AD DS.

> [!IMPORTANT]
> Если используется синхронизация паролей, которая всегда включена при стандартных параметрах, то пароль в Azure AD перезаписывается паролем в локальной среде AD. Если пользователи управляли разными паролями, то после установки Connect им необходимо сообщить о том, что следует использовать локальный пароль.

При планировании необходимо учитывать предыдущий раздел и приведенные предупреждения. В случае внесения множества изменений в Azure AD, которые не отражены в локальной среде AD DS, нужно запланировать заполнение AD DS обновленными значениями перед синхронизацией объектов с помощью Azure AD Connect.

Если объекты были сопоставлены нестрого, то в объект в Azure AD добавляется атрибут **sourceAnchor**, позволяющий в дальнейшем использовать строгое сопоставление.

### Строгое и нестрогое сопоставление
<a id="hard-match-vs-soft-match" class="xliff"></a>
Для нового установленного экземпляра Connect нет практической разницы между строгим и нестрогим сопоставлением. Различие имеется в случае аварийного восстановления. В случае потери сервера с Azure AD Connect можно повторно установить новый экземпляр без потери данных. Во время первоначальной установки в Connect передается объект с атрибутом sourceAnchor. Затем клиент (Azure AD Connect) может оценить соответствие, что будет гораздо быстрее, чем оценка соответствия в Azure AD. Строгое сопоставление оценивается Connect и Azure AD. Нестрогое сопоставление оценивается только Azure AD.

### Объекты, отличные от пользователей
<a id="other-objects-than-users" class="xliff"></a>
Как правило, у пользователей имеются атрибуты userPrincipalName и proxyAddresses, что упрощает сопоставление. Но у других объектов, таких как группы безопасности, их нет. В этом случае возможна только строгое сопоставление по атрибуту sourceAnchor. В локальной среде атрибут SourceAnchor всегда представляет собой **objectGUID**, преобразованный в Base64. Поэтому необходимо обновить значение в Azure AD, если требуется сопоставить два объекта. Атрибут SourceAnchor/immutableID можно обновить только с помощью PowerShell, а не через порталы.

## Создание локальной службы Active Directory на основе данных в Azure AD
<a id="create-a-new-on-premises-active-directory-from-data-in-azure-ad" class="xliff"></a>
Некоторые клиенты начинают с облачного решения на основе Azure AD и не используют локальную среду AD. Позднее возникает необходимость использовать локальные ресурсы, и они создают локальную среду AD на основании данных Azure AD. В этом случае Azure AD Connect ничем не поможет. Это решение не позволяет создать пользователей в локальной среде и не дает возможности установить локальный пароль так же, как и в Azure AD.

Если единственная причина, по которой планируется добавить локальную среду AD, это поддержка бизнес-приложений, то вам следует рассмотреть возможность использования [доменных служб Azure AD](../../active-directory-domain-services/index.md).

## Дальнейшие действия
<a id="next-steps" class="xliff"></a>
Узнайте больше об [интеграции локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).

