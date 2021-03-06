---
title: "Лицензирование: Azure AD SSPR | Документация Майкрософт"
description: "Требования к лицензированию самостоятельного сброса пароля в Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: joflore
ms.custom: it-pro
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 5445a479fd6893048eb8ff356fa829a3dcd5f7d3
ms.contentlocale: ru-ru
ms.lasthandoff: 08/31/2017

---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Требования к лицензированию самостоятельного сброса пароля в Azure AD

Для работы функции сброса пароля Azure AD **в вашей организации должна быть назначена по крайней мере одна лицензия**. Лицензирование каждого пользователя при сбросе пароля необязательно. Для соблюдения лицензионного соглашения Майкрософт вам необходимо назначить лицензии для всех пользователей, которые используют функции уровня "Премиум".

* **Полностью облачные пользователи.** Любой платный номер SKU Office 365 (O365) или Azure AD Basic.
* **Облачные пользователи** или **локальные пользователи.** Azure AD Premium P1 или P2, Enterprise Mobility + Security (EMS) или Secure Productive Enterprise (SPE).

## <a name="licenses-required-for-password-writeback"></a>Лицензии, необходимые для компонента обратной записи паролей

Чтобы использовать компонент обратной записи паролей, в клиенте должна быть назначена одна из приведенных ниже лицензий.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!NOTE]
> Автономные планы лицензирования Office 365 **не поддерживают компонент обратной записи паролей**. Для работы этой функции необходим один из указанных выше планов.

Дополнительные сведения о лицензировании, включая расходы, можно найти на следующих страницах:

* [Сайт с ценами на Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
* [Функции и возможности Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security).
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Включение группового и пользовательского лицензирования

Теперь Azure AD поддерживает групповое лицензирование, позволяющее администратору назначать лицензии группе пользователей в пакетном режиме, вместо назначения их по одной. [Назначение лицензий группе пользователей в Azure Active Directory](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Некоторые службы Майкрософт недоступны во всех расположениях. Прежде чем назначать лицензию, администратор должен указать для пользователя свойство "Место использования". Назначить лицензию можно на портале Azure в разделе "Пользователь > Профиль > Параметры". **Если лицензии назначаются группам, все пользователи, для которых не указано расположение, наследуют расположение каталога.**

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о сбросе пароля с помощью Azure AD см. в следующих источниках:

* [**Приступая к работе с компонентами управления паролями**](active-directory-passwords-getting-started.md). Запуск и выполнение службы самостоятельного управления паролями Azure AD. 
* [**Deploy password reset without requiring end-user registration**](active-directory-passwords-data.md) (Развертывание сброса пароля без регистрации пользователя). Сведения о необходимых данных и их использовании для управления паролями.
* [**Развертывание компонентов управления паролями и обучение пользователей их использованию**](active-directory-passwords-best-practices.md). Рекомендации по планированию и развертыванию SSPR для пользователей.
* [**Настройка компонентов управления паролями в соответствии с требованиями организации**](active-directory-passwords-customize.md). Сведения о настройке интерфейса и параметров использования SSPR для организации.
* [**Параметры отчетов для управления паролями Azure AD**](active-directory-passwords-reporting.md). Определяйте, кто и когда использовал функцию SSPR.
* [**Как работает управление паролями в Azure Active Directory**](active-directory-passwords-how-it-works.md). Сведения о принципе работы управления паролями.
* [**Вопросы и ответы об управлении паролями**](active-directory-passwords-faq.md). Как? Почему? Что? Где? Кто? Когда? Здесь приведены ответы на интересующие вас вопросы.
* [**Устранение неполадок, связанных с управлением паролями**](active-directory-passwords-troubleshoot.md). Сведения об устранении распространенных проблем с SSPR.

