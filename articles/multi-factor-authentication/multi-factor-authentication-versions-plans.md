---
title: "Версии и планы потребления многофакторной идентификации Azure | Документация Майкрософт"
description: "Информация о клиенте многофакторной идентификации и ее доступных методах и версиях. Сведения обо всех планах потребления"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: kgremban
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: c47ca818bd77291338de6e90e5dd27465475e868
ms.contentlocale: ru-ru
ms.lasthandoff: 08/29/2017

---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Как получить службу Azure Multi-Factor Authentication

Когда речь идет о защите учетных записей, двухшаговая проверка подлинности должна быть общепринятым стандартом в организации. Эта функция особенно важна для административных учетных записей, которые имеют привилегированный доступ к ресурсам. Поэтому корпорация Майкрософт поддерживает базовую двухфакторную проверку подлинности для администраторов Office 365 и Azure без дополнительной оплаты. Если вы хотите расширить эти возможности для администраторов или включить двухфакторную проверку подлинности для остальных пользователей, вы можете приобрести службу "Многофакторная идентификация Azure" (Azure MFA). 

В этой статье объясняются различия между версиями, предоставленными для администраторов, и полной версией Azure MFA. Если вы уже готовы развернуть полную систему предложений Azure MFA, в дальнейшем разделе вы найдете варианты реализации и правила расчета потребления, используемые корпорацией Майкрософт.

>[!IMPORTANT]
>Эта статья — руководство, которое поможет вам освоить разные способы приобретения службы многофакторной идентификации Azure. Сведения о ценах и выставлении счетов всегда можно найти [на странице цен на многофакторную идентификацию](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Доступные версии службы Azure Multi-Factor Authentication

В следующей таблице описываются различия между тремя версиями службы многофакторной идентификации.

| Version (версия) | Описание |
| --- | --- |
| Многофакторная проверка подлинности для Office 365 |Эта версия работает исключительно с приложениями Office 365, и ей можно управлять с портала Office 365. Администраторы могут [защитить свои ресурсы Office 365 с помощью двухшаговой проверки подлинности](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Эта версия является частью подписки Office 365. |
| Многофакторная проверка подлинности для администраторов Azure | Глобальные администраторы клиентов Azure могут включить двухфакторную проверку подлинности для своих учетных записей глобального администратора без дополнительных затрат.|
| Azure Multi-Factor Authentication | Многофакторная идентификация Azure, также называемая полной версией, предлагает широчайший набор возможностей. Доступны дополнительные варианты настройки с помощью [классического портала Azure](https://manage.windowsazure.com), расширенные возможности отчетности и поддержка большого количества локальных и облачных приложений. Служба "Многофакторная идентификация Azure" входит в планы [Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features) и [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing). Ее можно развернуть как в облаке, так и локально. |

## <a name="feature-comparison-of-versions"></a>Сравнение функций в разных версиях
Приведенная ниже таблица содержит список функций, доступных в различных версиях Многофакторной идентификации Azure.

> [!NOTE]
> В этой сравнительной таблице рассматриваются функции, которые являются частью каждой версии Многофакторной идентификации. При наличии полной службы Многофакторной идентификации Azure некоторые возможности могут быть недоступны в зависимости от того, используется ли [MFA в облаке или MFA в локальной системе](multi-factor-authentication-get-started.md).


| Функция | Многофакторная проверка подлинности для Office 365 | Многофакторная проверка подлинности для администраторов Azure | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Защита учетных записей администраторов с помощью MFA |● |● (только учетные записи глобального администратора) |● |
| Мобильное приложение в качестве второго фактора |● |● |● |
| Телефонный вызов в качестве второго фактора |● |● |● |
| SMS в качестве второго фактора |● |● |● |
| Пароли приложений для клиентов, которые не поддерживают Multi-Factor Authentication |● |● |● |
| Администраторский контроль над методами проверки |● |● |● |
| Режим ПИН-кода | | |● |
| Предупреждение о мошенничестве | | |● |
| Отчеты службы "Многофакторная идентификация Microsoft Azure" | | |● |
| Разовый обход | | |● |
| Настраиваемые приветствия для телефонных вызовов | | |● |
| Настраиваемый идентификатор вызывающей стороны для телефонных звонков | | |● |
| Надежные IP-адреса | | |● |
| Запоминание данных MFA для доверенных устройств |● |● |● |
| Пакет SDK службы Multi-Factor Authentication | | |● (Требуется поставщик многофакторной идентификации и полная подписка Azure) |
| MFA для локальных приложений | | |● |
| Политики условного доступа | | | ● |

## <a name="how-to-get-azure-multi-factor-authentication"></a>Как получить службу Azure Multi-Factor Authentication
Если требуется полная функциональность, предоставляемая многофакторной идентификацией Azure, существует несколько вариантов.

### <a name="option-1---mfa-licenses"></a>Вариант 1. Лицензии MFA

Приобретите лицензии многофакторной идентификации Azure и назначьте их пользователям в Azure Active Directory. 

При использовании этого варианта, в случае необходимости двухшаговой проверки для пользователей, у которых нет лицензии, нужно создать поставщика многофакторной идентификации Azure. В противном случае вам придется оплатить эту функцию дважды.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Вариант 2. Пакеты лицензий, которые включают MFA

Приобретите лицензии, которые уже включают многофакторную идентификацию Azure, например Azure Active Directory Premium, Enterprise Mobility + Security, и назначьте их пользователям в Azure Active Directory. 

Если вы выберете этот вариант, создавайте поставщика многофакторной идентификации Azure, только если двухшаговая проверка будет выполняться для пользователей, у которых нет лицензии. В противном случае вам придется оплатить эту функцию дважды. 

### <a name="option-3---mfa-consumption-based-model"></a>Вариант 3. MFA с оплатой по мере использования

Создать поставщика Azure Multi-Factor Authentication в рамках подписки Azure. Поставщики MFA Azure представляют собой ресурсы, за использование которых взимается плата в рамках Соглашения Enterprise или финансового обязательства Azure или списываются средства с кредитной карты, как и для всех остальных ресурсов Azure. Поставщики можно создавать только в полной версии подписки Azure, но не в ограниченной подписке Azure с предельной суммой расходов 0 долларов США. Ограниченные подписки создаются при активации лицензии, которые описаны в вариантах 1 и 2. 

При использовании поставщика Многофакторной идентификации Azure доступны две модели использования, которые тарифицируются по подписке Azure:  

1. **На пользователя**. Этот вариант предприятия обычно выбирают, чтобы применить двухшаговую проверку для известного числа определенных сотрудников, которым регулярно требуется выполнять аутентификацию. Оплата в этом варианте взимается в зависимости от количества пользователей, для которых активирована служба MFA в клиенте Microsoft Azure AD и на сервере Azure MFA. Если служба MFA включена для пользователей как в Azure AD, так и на сервере Azure MFA, и при этом включена синхронизация домена (Azure AD Connect), то учитывается та система, в которой пользователей MFA больше. Если синхронизация домена не используется, то учитывается сумма всех пользователей, для которых включена MFA — в Azure AD и на сервере Azure MFA. Сумма к оплате вычисляется пропорционально и передается в коммерческую систему ежедневно. 

  > [!NOTE]
  > Выставление счетов: пример 1. На сегодня MFA в вашей организации включена для 5000 пользователей. Система MFA делит это значение на 31 и фиксирует на этот день оплату за 161,29 пользователей. На следующий день вы добавляете еще 15 пользователей, и теперь система MFA фиксирует 161,77 пользователей за день. В конце цикла выставления счетов количество пользователей, за которые выставлены счета в вашей подписке Azure, составит в сумме примерно 5000. 
  >
  > Выставление счетов: пример 2. У вас есть определенное число пользователей с лицензиями. Для остальных вы используете поставщика MFA Azure с оплатой за количество пользователей. Для вашего клиента приобретено 4500 лицензий Enterprise Mobility + Security, а многофакторная идентификация включена для 5000 пользователей. В подписке Azure выставляется счет за 500 пользователей, что в пропорциональном распределении составляет 16,13 пользователя в день. 

2. **На аутентификацию подлинности**. Этот вариант предприятия обычно выбирают, чтобы использовать двухшаговую проверку подлинности для больших групп пользователей, которым выполнять аутентификацию нужно нечасто. Выставление счетов основано на количестве запросов на двухфакторную проверку подлинности, независимо от успешности выполнения этих проверок. Счета по этой позиции появляются в отчетах по использованию Azure пакетами по 10 проверок ежедневно. 

  > [!NOTE]
  > Выставление счетов: пример 3. На сегодня служба Azure MFA получила 3105 запросов на двухшаговую проверку. В подписке Azure выставляется счет за 310,5 пакетов аутентификации. 

Важно отметить, что даже при наличии лицензий Azure MFA вы можете получать счета за конфигурации с оплатой по мере использования. Например, если вы настроили поставщик Azure MFA, вы оплачиваете каждый запрос на двухшаговую проверку, даже от пользователей с лицензиями. Если вы настроили поставщик Azure MFA в домене, который не связан с вашим клиентом Azure AD, вы оплачиваете каждого включенного в нем пользователя, даже если у них есть лицензии в Azure AD. 

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о ценах [на многофакторную проверку подлинности Azure](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

- Развертывание Azure MFA [в облаке или локально](multi-factor-authentication-get-started.md)
