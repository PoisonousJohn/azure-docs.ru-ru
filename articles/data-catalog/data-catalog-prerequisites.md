---
title: "Необходимые компоненты каталога данных Azure | Документация Майкрософт"
description: "Сведения о необходимых компонентах, которые нужны для начала работы с каталогом данных Azure."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.translationtype: Human Translation
ms.sourcegitcommit: f7479260c7c2e10f242b6d8e77170d4abe8634ac
ms.openlocfilehash: 6ae024e59d29d20c223243b71aceae1db7eefdf5
ms.contentlocale: ru-ru
ms.lasthandoff: 06/21/2017

---
# <a name="azure-data-catalog-prerequisites"></a>Предварительные условия для работы с каталогом данных Azure

Перед настройкой каталога данных Azure следует решить несколько вопросов. Не беспокойтесь, этот процесс не займет много времени.

## <a name="azure-subscription"></a>Подписка Azure.
Чтобы настроить каталог данных, необходимо быть владельцем или совладельцем подписки Azure.

Подписки Azure помогают организовать доступ к ресурсам облачных служб, таких как каталог данных. Они также позволяют управлять составлением отчетов об использовании ресурса, выставлением счетов за использование и их оплатой. Каждая подписка может иметь отдельные настройки для выставления счетов и их оплаты, поэтому у вас могут быть разные подписки и тарифные планы для разных отделов, проектов, региональных офисов и т. д. Каждая облачная служба привязана к подписке, поэтому необходимо иметь подписку перед настройкой каталога данных. Дополнительные сведения см. в статье [Связь между подписками Azure и службой Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Для настройки каталога данных необходимо выполнить вход с помощью учетной записи пользователя Azure Active Directory (Azure AD).

Azure AD предоставляет компаниям простой способ управления удостоверениями и доступом не только в облаке, но и локально. Пользователи могут использовать единую рабочую или учебную учетную запись для единого входа во все облачные и локальные веб-приложения. Каталог данных использует Azure AD для проверки подлинности входа. Дополнительные сведения см. в статье [Что такое Azure Active Directory](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Пользователи могут входить на [портал Azure](http://portal.azure.com/) с помощью личной учетной записи Майкрософт, а также рабочей или учебной учетной записи Azure Active Directory. Чтобы настроить каталог данных с помощью портала Azure или [портала каталога данных](http://www.azuredatacatalog.com), необходимо войти с помощью учетной записью Azure Active Directory, а не личной учетной записи.
>
>

## <a name="active-directory-policy-configuration"></a>Настройка политики Active Directory
Бывает, что пользователь может войти на портал каталога данных, но при попытке входа в средство регистрации источника данных отображается сообщение об ошибке, которая запрещает вход в систему. Эта ошибка может происходить только в том случае, если пользователь находится в локальной сети, или только в том случае, если пользователь подключается из-за пределов сети компании.

Средство регистрации источника данных использует проверку подлинности форм для проверки учетных данных пользователя в Active Directory. Для успешного входа в систему администратор Azure Active Directory должен включить проверку подлинности на основе форм в глобальной политике проверки подлинности.

Глобальная политика проверки подлинности позволяет включать методы проверки подлинности отдельно для подключений в интрасети и экстрасети, как показано на следующем снимке экрана. Ошибки входа в систему могут возникать, если не включена проверка подлинности на основе форм для сети, из которой подключается пользователь.

 ![Глобальная политика аутентификации AD FS](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Дальнейшие действия
Подробнее: [Настройка политик проверки подлинности](https://technet.microsoft.com/library/dn486781.aspx).

