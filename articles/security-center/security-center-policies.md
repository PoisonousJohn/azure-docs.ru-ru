---
title: "Настройка политик безопасности в центре безопасности Azure | Документация Майкрософт"
description: "В этом документе вы ознакомились с процедурой настройки политик безопасности в Центре безопасности Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: f4e3f74ce3f342eecf633cd748e2b7b21b2ccdd2
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="set-security-policies-in-azure-security-center"></a>Настройка политик безопасности в центре безопасности Azure
В этом пошаговом руководстве описывается, как настроить политики безопасности в центре безопасности.

>[!NOTE] 
>С начала июня 2017 г. в центре безопасности используется Microsoft Monitoring Agent для сбора и хранения данных. См. дополнительные сведения о [миграции платформы центра безопасности Azure](security-center-platform-migration.md). В этой статье описывается, как будет работать центр безопасности после перехода к использованию Microsoft Monitoring Agent.
>

## <a name="what-are-security-policies"></a>Что такое политики безопасности?
Политика безопасности определяет набор элементов управления, которые рекомендуются для ресурсов в указанной подписке. В центре безопасности можно настраивать политики для подписок Azure в соответствии с потребностями безопасности вашей компании, типом приложений и уровнем конфиденциальности данных в каждой подписке.

Например, требования к безопасности ресурсов, используемых при разработке и тестировании, могут отличаться от требований, выдвигаемых к рабочим приложениям. Аналогично приложения с контролируемыми данными, такими как персональные данные, могут требовать более высокого уровня безопасности. Политики безопасности, включенные в центре безопасности Azure, обеспечивают рекомендации по безопасности и мониторинг, позволяя выявлять потенциальные уязвимости и бороться с угрозами. Дополнительные сведения об определении наиболее подходящего для вас варианта см. в статье [Руководство по планированию использования центра безопасности Azure и работе в нем](security-center-planning-and-operations-guide.md).

## <a name="set-security-policies"></a>Установка политик безопасности
Политики безопасности можно настроить для каждой подписки. Изменить политику безопасности может пользователь с правами владельца или участника этой подписки. Чтобы настроить политики безопасности в центре безопасности, войдите на портал Azure и сделайте следующее:

1. Щелкните плитку **Политика** на панели мониторинга в центре безопасности.
2. В открывшейся колонке "Политика безопасности" выберите подписку, для которой необходимо включить политику безопасности.

    ![Определение политики](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Для выбранной подписки откроется колонка **Политика безопасности** с набором параметров. В этой колонке доступны следующие параметры:

   * **Политика предотвращения.** Используйте этот параметр для настройки политик для каждой подписки.  
   * **Уведомление по электронной почте** — используйте этот параметр, чтобы настроить отправку уведомлений для первых за день оповещений и для оповещений с высоким уровнем серьезности. Параметры электронной почты можно настроить только для политик подписки. Дополнительные сведения о настройке получения уведомлений по электронной почте см. в статье [Предоставление сведений о контактных лицах по вопросам безопасности в центре безопасности Azure](security-center-provide-security-contact-details.md).
   * **Ценовая категория** — используйте этот параметр для обновления ценовой категории. Дополнительные сведения о вариантах оплаты см. на странице с [ценами на использование центра безопасности](security-center-pricing.md).
4. Убедитесь, что для параметра **Сбор данных с виртуальных машин** задано значение **Вкл**. Этот параметр позволяет включить автоматический сбор журналов для существующих и новых ресурсов с помощью Microsoft Monitoring Agent. Агент Microsoft Monitoring Agent также используется в Operations Management Suite и службе Log Analytics. Данные, собранные из этого агента, сохраняются в имеющихся рабочих областях Log Analytics, связанных с подпиской Azure, или новых рабочих областях с учетом расположения виртуальной машины.

5. В колонке **Политика безопасности** щелкните **Политика предотвращения**, чтобы просмотреть доступные параметры. Нажмите кнопку **Вкл.**, чтобы включить рекомендации по безопасности, соответствующие этой подписке.

    ![Выбор политик безопасности](./media/security-center-policies/security-center-policies-fig7.png)

В следующей таблице содержатся справочные сведения о различных параметрах.

| Политика | Если включена |
| --- | --- |
| Обновление системы |Ежедневно получает список доступных критических обновлений и обновлений для системы безопасности из Центра обновления Windows или служб Windows Server Update Services. Полученный список зависит от настроенной для соответствующей виртуальной машины службы и рекомендаций по установлению отсутствующих обновлений. Эта политика определяет доступные для систем Linux пакеты обновлений с помощью предоставленной дистрибутивами системы управления пакетами. Кроме того, она проверяет наличие критических обновлений и обновлений системы безопасности на виртуальных машинах [облачных служб Azure](../cloud-services/cloud-services-how-to-configure.md). |
| Уязвимости ОС |Ежедневно анализирует конфигурации операционной системы, чтобы определить проблемы, делающие виртуальную машину более уязвимой к атакам. Политика также рекомендует изменения конфигурации для устранения таких уязвимостей. Дополнительные сведения о конкретных отслеживаемых конфигурациях см. в [списке рекомендуемых базовых шаблонов](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (На данный момент Windows Server 2016 поддерживается не полностью.) |
| Защита конечных точек |Рекомендует подготовить решения для защиты конечных точек для всех виртуальных машин Windows, чтобы обнаруживать и удалять вирусы, программы-шпионы и другие вредоносные программы. |
| Шифрование дисков |Рекомендует включить на всех виртуальных машинах шифрование дисков, чтобы усилить защиту хранящихся данных. |
| Группы безопасности сети |Рекомендует настроить [группы безопасности сети](../virtual-network/virtual-networks-nsg.md) для контроля входящего и исходящего трафика виртуальных машин с общедоступными конечными точками. Группы безопасности сети, настроенные для подсети, наследуются всеми сетевыми интерфейсами виртуальных машин, если не указано иначе. В дополнение к проверке настройки группы безопасности сети эта политика получает доступ к входящим правилам безопасности, чтобы выявить правила, разрешающие входящий трафик. |
| Брандмауэр веб-приложения |Рекомендует подготовить брандмауэр веб-приложения на виртуальных машинах, если выполнено одно из следующих требований. </br></br>Используется [общедоступный IP-адрес уровня экземпляра](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP), а в правилах безопасности для входящего трафика для связанной группы безопасности сети разрешен доступ к порту 80 или 443.</br></br>Используется IP-адрес с балансировкой нагрузки, а в связанных правилах балансировки нагрузки и преобразования сетевых адресов (NAT) для входящего трафика разрешен доступ к порту 80 или 443. Дополнительные сведения см. в статье, посвященной [поддержке Azure Resource Manager для балансировщика нагрузки](../load-balancer/load-balancer-arm.md). |
| Брандмауэр следующего поколения |Расширяет защиту сети за пределы групп безопасности сети, встроенных в Azure. Центр обеспечения безопасности обнаруживает развертывания, для которых рекомендуется использовать брандмауэр следующего поколения, и позволяет подготовить виртуальное устройство. |
| Аудит SQL и обнаружение угроз |Рекомендует включить аудит доступа к базам данных Azure для соответствия бизнес-требованиям и расширенное обнаружение угроз в целях контроля. |
| Шифрование SQL |Рекомендует включить шифрование в неактивном состоянии для баз данных SQL Azure, связанных резервных копий и файлов журналов транзакций. Это позволит защитить данные от считывания, даже если безопасность будет нарушена. |
| Оценка уязвимостей |Рекомендует установить решение для оценки уязвимостей на виртуальную машину. |
| Шифрование хранилища |Сейчас эта функция доступна для файлов и больших двоичных объектов Azure. После включения шифрования службы хранилища будут шифроваться только новые данные, а все имеющиеся файлы в этой учетной записи хранения останутся незашифрованными. |
| Доступ к сети JIT |Если JIT включен, центр безопасности блокирует входящий трафик для виртуальной машины Azure путем создания правила группы безопасности сети. Вы выбираете порты на виртуальной машине, для которых должен быть заблокирован входящий трафик. Дополнительные сведения см. в статье [Управление доступом к виртуальным машинам с помощью JIT](https://docs.microsoft.com/azure/security-center/security-center-just-in-time). |

Завершив настройку всех параметров, нажмите кнопку **ОК** в колонке **Политика безопасности**, где содержатся рекомендации, и **Сохранить** в колонке **Политика безопасности** с начальными настройками.

> [!NOTE]
> Ценовая категория по-прежнему применяется на уровне группы ресурсов. Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/security-center/).
>
>

## <a name="see-also"></a>См. также
В этом документе вы ознакомились с подробными сведениями о настройке политик безопасности в Центре безопасности Azure. Дополнительные сведения о Центре безопасности Azure см. в следующих статьях:

* [Руководство по планированию использования центра безопасности Azure и работе в нем](security-center-planning-and-operations-guide.md). Узнайте, как спланировать работу в центре безопасности Azure, и получите рекомендации по переходу к его использованию.
* [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md). Узнайте, как отслеживать работоспособность ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md). Узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md). Узнайте, как отслеживать работоспособность партнерских решений.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md). Часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/). Записи блога, посвященные безопасности и соответствию требованиям в Azure.

