---
title: "Синхронизация Azure AD Connect: включение корзины AD | Документация Майкрософт"
description: "В этом разделе приведены рекомендации по использованию функции корзины AD с Azure AD Connect."
services: active-directory
keywords: "корзина AD, случайное удаление, привязка к источнику"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9778db69e94e9f1d033cc8c16fdb9554df3eddcc
ms.contentlocale: ru-ru
ms.lasthandoff: 04/03/2017

---
# Синхронизация Azure AD Connect: включение корзины AD
<a id="azure-ad-connect-sync-enable-ad-recycle-bin" class="xliff"></a>
Для локальных каталогов Active Directory, которые синхронизируются с Azure AD, рекомендуется включить функцию корзины AD. 

Если вы случайно удалите объект пользователя в локальной службе AD, то при восстановлении с помощью этой функции Azure AD восстановит соответствующий объект пользователя Azure AD.  Сведения о функции корзины AD см. в статье [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx) (Обзор сценария восстановления удаленных объектов Active Directory).

## Преимущества включения корзины AD
<a id="benefits-of-enabling-the-ad-recycle-bin" class="xliff"></a>
Эта функция помогает восстановить объекты пользователей Azure AD следующим образом:

* Если вы случайно удалите объект пользователя в локальной службе AD, то соответствующий объект пользователя Azure AD будет удален при следующем цикле синхронизации. По умолчанию Azure AD хранит удаленный объект пользователя Azure AD в состоянии обратимого удаления в течение 30 дней.

* Если у вас включена функция корзины в локальной службе AD, то вы можете восстановить удаленный объект пользователя в локальной службе AD, не изменяя его значения привязки к источнику. Когда восстановленный объект пользователя в локальной службе AD синхронизируется с Azure AD, служба Azure AD восстановит соответствующий объект пользователя Azure AD, находящийся с состоянии обратимого удаления. Сведения об атрибуте привязки к источнику см. в статье [Azure AD Connect: принципы проектирования](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Если функция корзины в локальной службе AD не включена, то может потребоваться создать объект пользователя AD, чтобы заменить удаленный объект. Если в службе синхронизации Azure AD Connect для атрибута привязки к источнику настроено использование создаваемого системой атрибута AD (такого как ObjectGuid), то вновь создаваемый объект пользователя AD не будет иметь то же значение привязки к источнику, что и удаленный объект пользователя AD. Когда вновь созданный объект пользователя AD синхронизируется с Azure AD, служба Azure AD создаст объект пользователя Azure AD, а не восстановит соответствующий объект, находящийся с состоянии обратимого удаления.

> [!NOTE]
> По умолчанию Azure AD хранит удаленные объекты пользователя Azure AD в состоянии обратимого удаления в течение 30 дней, прежде чем удалить их без возможности восстановления. Тем не менее, администраторы могут ускорить удаление таких объектов. После окончательного удаления объектов их больше невозможно восстановить, даже если в локальной службе AD включена функция корзины.



## Дальнейшие действия
<a id="next-steps" class="xliff"></a>
**Обзорные статьи**

* [Службы синхронизации Azure AD Connect: общие сведений о синхронизации и ее настройка](active-directory-aadconnectsync-whatis.md)

* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)

