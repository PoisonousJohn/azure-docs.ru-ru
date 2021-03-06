---
title: "Защита мобильных приложений и веб-приложений PaaS с помощью служб приложений Azure | Документация Майкрософт"
description: " Ознакомьтесь с рекомендациями по безопасности служб приложений Azure, чтобы защитить свои веб-приложения и мобильные приложения PaaS. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: terrylan
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: 0738522250f1863c9584936a2d2b2c7a0a823c8c
ms.contentlocale: ru-ru
ms.lasthandoff: 07/18/2017

---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-services"></a>Защита мобильных приложений и веб-приложений PaaS с помощью служб приложений Azure

В этой статье рассматривается набор рекомендаций по безопасности [служб приложений Azure](https://azure.microsoft.com/services/app-service/), предназначенных для защиты веб-приложений и мобильных приложений PaaS. Эти рекомендации основаны на нашем опыте, полученном в процессе использования Azure AD, и на отзывах других пользователей.

## <a name="azure-app-services"></a>Службы приложений Azure
[Службы приложений Azure](../app-service/app-service-value-prop-what-is.md) — это предложение PaaS, которое позволяет создавать веб-приложения и мобильные приложения для любой платформы или устройства и подключаться к данным откуда угодно, как в облаке, так и в локальной среде. Служба приложений обладает возможностями для работы в Интернете и мобильной среде, которые ранее предлагались в составе отдельных решений Веб-сайты Azure и мобильные службы Azure. Она также предусматривает новые возможности для автоматизации бизнес-процессов и размещения облачных API. Службы приложений — это отдельная интегрируемая служба, которая привносит широкий спектр возможностей в сценарии с использованием Интернета, мобильных устройств и интеграции.

Чтобы узнать больше, ознакомьтесь с обзорами [мобильных приложений](../app-service-mobile/app-service-mobile-value-prop.md) и [веб-приложений](../app-service-web/app-service-web-overview.md).

## <a name="best-practices"></a>Рекомендации

При использовании служб приложений придерживайтесь следующих рекомендаций.

- [Используйте аутентификацию с помощью Azure Active Directory (AD)](../app-service-web/web-sites-authentication-authorization.md#authenticate-through-azure-active-directory). Службы приложений предоставляют службу OAuth 2.0 для поставщика удостоверений. Служба OAuth 2.0 предназначена упростить разработку клиентов, так ка предоставляет определенные последовательности авторизации для веб-приложений, классических приложений и мобильных телефонов. Azure AD использует OAuth 2.0 для авторизации доступа к мобильным приложениям и веб-приложениям.
- Доступ следует ограничивать с учетом возможности использования необходимой информации и минимальных полномочий. Ограничение доступа крайне важно для организаций, которые планируют применять политики безопасности для доступа к данным. Контроль доступа на основе ролей (RBAC) может использоваться для назначения пользователям, группам и приложениям разрешений, действующих в рамках определенной области. Чтобы узнать больше о предоставлении пользователям доступа к приложениям, ознакомьтесь с разделом [Начало работы с управлением доступом на портале Azure](../active-directory/role-based-access-control-what-is.md).
- Обеспечьте защиту ключей. Неважно, насколько надежна ваша система безопасности, если ключи подписки будут утеряны. Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. С помощью хранилища ключей можно шифровать ключи и секреты (например, ключи аутентификации, ключи учетных записей хранения, ключи шифрования данных, PFX-файлы и пароли), используя ключи, защищенные аппаратными модулями безопасности. Чтобы обеспечить более высокий уровень защиты, ключи можно импортировать или создать в аппаратных модулях безопасности. Чтобы узнать больше, ознакомьтесь с разделом [то такое хранилище ключей Azure?](../key-vault/key-vault-whatis.md) Key Vault можно также использовать для управления TLS-сертификатами с автоматическим продлением.
- Ограничьте исходные IP-адреса входящего трафика. В [среде служб приложений](../app-service-web/app-service-app-service-environment-intro.md) есть функция интеграции виртуальных сетей, которая помогает ограничить IP-адреса входящего трафика с помощью групп безопасности сети (NSG). Если вы не знакомы с виртуальными сетями Azure, вот краткое описание. Эта функция позволяет размещать множество ресурсов Azure в сети, недоступной из Интернета, а также самостоятельно управлять доступом к ним. Дополнительные сведения см. в статье [Интеграция приложения с виртуальной сетью Azure](../app-service-web/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Дальнейшие действия
В этой статье был представлен набор рекомендаций по безопасности служб приложений, предназначенных для защиты веб-приложений и мобильных приложений PaaS. Дополнительные сведения о безопасности развернутых служб PaaS см. в следующих статьях:

- [Защита развернутых приложений PaaS](security-paas-deployments.md)
- [Защита мобильных приложений и веб-приложений PaaS с помощью Базы данных SQL и хранилища данных SQL](security-paas-applications-using-sql.md)

