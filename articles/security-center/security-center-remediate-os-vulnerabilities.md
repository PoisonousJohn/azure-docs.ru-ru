---
title: "Исправление уязвимостей ОС в центре безопасности Azure | Документация Майкрософт"
description: "В этом документе объясняется, как выполнить рекомендацию центра безопасности Azure **Устранение уязвимостей ОС**."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 991d41f5-1d17-468d-a66d-83ec1308ab79
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: e6b251d5b97c57b3b6f79d14e53fbed5ca37ecb0
ms.contentlocale: ru-ru
ms.lasthandoff: 06/17/2017


---
# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Исправление уязвимостей ОС в центре безопасности Azure
Центр безопасности Azure ежедневно анализирует конфигурации операционной системы виртуальной машины, которые могут сделать ее более уязвимой для атак, и рекомендует изменения конфигурации для устранения таких уязвимостей. Центр безопасности рекомендует устранить уязвимости в случае, если конфигурация операционной системы виртуальной машины не будет соответствовать рекомендуемым правилам конфигурации.

> [!NOTE]
> Дополнительные сведения о конкретных отслеживаемых конфигурациях доступны в [списке рекомендуемых правил конфигурации](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
>
>

## <a name="implement-the-recommendation"></a>Выполнение рекомендаций

> [!NOTE]
> В документе приводится обзор службы с помощью примера развертывания.  Этот документ не является пошаговым руководством.
>
>

1. В колонке **Рекомендации** выберите **Устранение уязвимостей ОС**.
   ![Исправление уязвимостей ОС][1]

    Откроется колонка **Устранение уязвимостей ОС**. В ней перечислены виртуальные машины с конфигурацией операционной системы, которая не соответствуют рекомендуемым правилам конфигурации.  Для каждой виртуальной машины колонка содержит:

   * **Правила с ошибкой** — количество правил, не выполненных конфигурацией ОС виртуальной машины.
   * **Время последнего сканирования** — дата и время последней проверки конфигурации ОС виртуальной машины, выполненной центром безопасности.
   * **Состояние** — текущее состояние рекомендации уязвимости:

     * "Открыто" — уязвимость еще не исправлена;
     * "Выполняется" — уязвимость уже устраняется, вам ничего не нужно делать;
     * "Устранено" — уязвимость устранена (если проблема устранена, эта строка неактивна).
   * **Серьезность** — всем уязвимостям присваивается низкая серьезность, означающая, что уязвимость необходимо устранить, но она не требует немедленного внимания.

2. Выберите виртуальную машину. Откроется колонка для этой виртуальной машины, в которой отображены нарушенные правила.
   ![Правила конфигурации, которые не были выполнены][2]

3. Выберите правило. В этом примере выберем **Пароль должен отвечать требованиям сложности**. Откроется колонка, описывающая не выполненное правило и последствия этого. Просмотрите эту информацию и решите, как будут применяться конфигурации операционной системы.
  ![Описание правила, которое не выполнено][3]

  Центр безопасности использует перечисление общей конфигурации (CCE) для назначения уникальных идентификаторов для правил конфигурации. В этой колонке предоставляется следующая информация:

  - "Имя" — имя правила;
  - "Серьезность" — указывает значение серьезности CCE: критическая ошибка, важное уведомление или предупреждение;
  - "CCIED" — уникальный идентификатор CCE правила;
  - "Описание" — описание правила;
  - "Уязвимость" — описание уязвимости или риска в случае, если правило не применяется;
  - "Воздействие" — влияние на работу организации в случае применения правила;
  - "Ожидаемое значение" — ожидаемое значение при анализе центром безопасности конфигурации ОС виртуальной машины посредством правила;
  - "Операция правила" — операция правила, используемая центром безопасности во время анализа конфигурации ОС виртуальной машины посредством правила;
  - "Фактическое значение" — значение, возвращаемое после анализа конфигурации ОС виртуальной машины посредством правила;
  - "Результат вычисления" — результат анализа: проверка пройдена или нет.

## <a name="see-also"></a>Дополнительные материалы
В этой статье объясняется, как выполнить рекомендацию центра безопасности "Remediate OS vulnerabilities" (Исправить уязвимости ОС). Набор правил конфигурации можно просмотреть [здесь](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Центр безопасности использует CCE (перечисление общей конфигурации) для назначения уникальных идентификаторов для правил конфигурации. Посетите сайт [CCE](https://nvd.nist.gov/cce/index.cfm) , чтобы получить дополнительную информацию.

Дополнительные сведения о центре безопасности см. в следующих ресурсах:

* [Поддерживаемые платформы в центре безопасности Azure](security-center-os-coverage.md) — список поддерживаемых виртуальных машин Windows и Linux.
* [Настройка политик безопасности в центре безопасности Azure](security-center-policies.md). Узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление рекомендациями по безопасности в Центре безопасности Azure](security-center-recommendations.md). Узнайте, как рекомендации могут помочь вам защитить ресурсы Azure.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md). Узнайте, как отслеживать работоспособность ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md). Узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md) — узнайте, как отслеживать состояние работоспособности решений партнеров.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md). Часто задаваемые вопросы об использовании этой службы.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) (Блог по безопасности Azure). Публикации блога, посвященные безопасности и соответствию требованиям в Azure.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png

