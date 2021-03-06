---
title: "Устранение неполадок при регистрации в Azure | Документация Майкрософт"
description: "Описание процесса устранения неполадок для некоторых распространенных проблем с регистрацией."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 763bc597bdfc40395511cdd9d797e5c7aaad0fdf
ms.openlocfilehash: af8a7bbc4bf007dfa5bef7ceb9cf940ad752239a
ms.contentlocale: ru-ru
ms.lasthandoff: 09/06/2017

---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Устранение неполадок при регистрации в Azure
Если не удается зарегистрироваться для работы с Azure, то воспользуетесь подсказками в этой статье для устранения распространенных неполадок. При возникновении проблем с кредитной картой во время регистрации см. статью [Банковская или кредитная карта отклонена при регистрации в Azure](billing-credit-card-fails-during-azure-sign-up.md). Если у вас есть учетная запись Azure, но не удается выполнить вход, то см. статью [Не удается выполнить вход в подписку Azure](billing-cannot-login-subscription.md).

## <a name="error-we-cannot-proceed-with-signup-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Ошибка "Невозможно продолжить регистрацию из-за проблемы с вашей учетной записью. Обратитесь в службу поддержки по вопросам выставления счетов" 

Устранить проблему можно так:

1. Войдите в [центр учетных записей Azure](https://account.azure.com) в качестве администратора учетной записи. 
2. Щелкните **Профиль**, а затем выберите **Изменить сведения**.
3. Убедитесь, что все поля адресов заполнены допустимыми значениями. 
4. При регистрации подписки Azure убедитесь, что адрес для выставления счетов, введенный во время регистрации кредитной карты, соответствует записям в вашем банке.

Если ошибка продолжает возникать, попробуйте зарегистрироваться в другом браузере.

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Индикатор выполнения зависает в разделе "Проверка личности с помощью карты"

Чтобы выполнить проверку личности с помощью карты, необходимо разрешить для браузера сторонние файлы cookie.

![Снимок экрана, на котором показано, что индикатор выполнения завис в разделе "Проверка личности с помощью карты" при регистрации](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Выполните следующие действия, чтобы обновить в браузере параметры файлов cookie.

1. Если используется браузер Chrome, то перейдите в меню **Настройки** > **Показать дополнительные настройки** > **Личные данные** > **Настройки контента**. Снимите флажок **Блокировать данные и файлы cookie сторонних сайтов**.
2. Если используется браузер Edge, то перейдите в меню **Параметры** > **Просмотреть доп. параметры** > **Файлы "cookie"**. Выберите **Не блокировать файлы "cookie"**.
3. Обновите страницу регистрации Azure и проверьте, устранена ли проблема.
4. Если это не помогло, закройте и перезапустите браузер, а затем повторите попытку.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Форма кредитной карты не поддерживает мой адрес для выставления счетов
Адрес для выставления счетов должен находиться в той же стране, которая выбрана в разделе **О вас**. Убедитесь, что вы выбрали правильную страну.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Отсутствие текстовых сообщений или вызовов при проверке учетной записи во время регистрации
Хотя это, как правило, происходит намного быстрее, доставка кода проверки может занять до четырех минут. Номер телефона, указанный для проверки, не сохраняется как контактный номер для учетной записи.

Ниже приведены некоторые дополнительные советы:
* Для проверки номера телефона невозможно использовать номер VoIP-телефона.
* Проверьте введенный номер телефона, включая код страны, выбранный в раскрывающемся меню.
* Если ваш телефон не получает текстовые сообщения (SMS), то попробуйте метод **Позвоните мне**.
* Убедитесь, что ваш телефон может получать звонки или SMS-сообщения с номера в США.

При получении текстового сообщения или входящего вызова введите полученный код в текстовое поле.

## <a name="credit-card-declined-or-not-accepted"></a>Кредитная карта отклонена или не принята
Оплата с использованием таких карт не поддерживается для подписок Azure. Другие причины, по которым карта может отклоняться, приведены в статье [Банковская или кредитная карта отклонена при регистрации в Azure](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>"Бесплатная пробная версия недоступна"
В прошлом вы уже использовали подписку Azure? В соответствии с условиями соглашения Azure любой новый пользователь Azure может выполнить только одну активацию бесплатной пробной версии. Если у вас уже была подписка Azure любого типа, вы не сможете активировать бесплатную пробную версию. Попробуйте зарегистрироваться, чтобы использовать [подписку с оплатой по мере использования](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>В учетной записи бесплатной пробной версии появился платеж
После регистрации в вашей учетной записи кредитной карты может быть выставлен небольшой счет для проверки, который будет удален в течение 3–5 дней. Если вы беспокоитесь об управлении расходами, узнайте больше о [предотвращении непредвиденных затрат](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Не удается активировать льготный план Azure, например MSDN, BizSpark, BizSparkPlus или MPN
Убедитесь, что используются правильные учетные данные для входа. Затем проверьте, имеете ли вы право на участие в программе вознаграждений. 

* MSDN
  * Проверьте свой статус соответствия требованиям на своей [странице учетной записи MSDN](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Если вам не удается проверить свой статус, обратитесь в [центры обслуживания клиентов с подписками MSDN](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * войдите на [портал BizSpark](https://www.microsoft.com/bizspark/default.aspx#start-two) и проверьте свой статус соответствия требованиям BizSpark и BizSpark Plus.
  * Если вам не удается проверить состояние, то [вы можете получить помощь на форумах BizSpark](http://aka.ms/bzforums).
* MPN
  * Войдите на [портал MPN](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) и проверьте свой статус соответствия требованиям. Дополнительные преимущества предоставляются при наличии соответствующих [компетенций облачной платформы](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx).
  * Если вам не удается проверить свой статус, обратитесь в [службу поддержки MPN](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Не удается активировать новую подписку Azure с открытой лицензией
Чтобы создать подписку Azure с открытой лицензией, вам потребуется действующий ключ OSA, с которым связан как минимум один соответствующий маркер Azure с открытой лицензией. Если у вас нет ключа OSA, свяжитесь с любым из партнеров корпорации Майкрософт, которые перечислены на странице [Партнеры Майкрософт](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Требуется помощь? Обратитесь в службу поддержки.
Если вам все еще нужна помощь, [обратитесь в службу поддержки](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), которая поможет быстро устранить проблему.

