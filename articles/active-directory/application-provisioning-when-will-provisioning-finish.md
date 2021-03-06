---
title: "Подготовка пользователей для приложения в коллекции Azure AD выполняется час и более | Документация Майкрософт"
description: "Как узнать, почему подготовка для приложения может выполняться дольше, чем ожидалось"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: cc9e81de9bf8a3312da834502fa6ca25e2b5834a
ms.openlocfilehash: a0d2909ca69f06b6d08deaa25f35d55d9f9828fe
ms.contentlocale: ru-ru
ms.lasthandoff: 04/11/2017

---

<a id="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more" class="xliff"></a>

# Подготовка пользователей для приложения в коллекции Azure AD выполняется час и более

При первом включении автоматической подготовки для приложения начальная синхронизация может выполняться от 20 минут до нескольких часов в зависимости от размера каталога Azure AD и количества подготавливаемых пользователей. 

Последующие операции синхронизации занимают меньше времени, так как служба подготовки сохраняет "водяные знаки", которые представляют состояние обеих систем после начальной синхронизации. Это позволяет повысить скорость последующих операций синхронизации.

<a id="how-to-improve-provisioning-performance" class="xliff"></a>

## Как улучшить производительность подготовки

Если начальная синхронизация занимает несколько часов, повысить производительность можно только одним способом:

-   **Фильтры области.** Фильтры области позволяют точно настроить данные, которые служба подготовки получает из Azure AD, за счет фильтрации пользователей по значению конкретного атрибута. Дополнительные сведения о фильтрах области см. в статье [Подготовка приложений на основе атрибутов с использованием фильтров области](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

<a id="next-steps" class="xliff"></a>

## Дальнейшие действия
[Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](active-directory-saas-app-provisioning.md)


