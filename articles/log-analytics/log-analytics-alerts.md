---
title: "Общие сведения об оповещениях в Azure Log Analytics | Документация Майкрософт"
description: "Оповещения в Log Analytics идентифицируют важную информацию в репозитории OMS и могут заранее уведомлять вас о возникших проблемах или выполнять действия в попытке устранить их.  В этой статье описываются различные типы правил генерации оповещений и их определение."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 951e76d3fb18d9e433b148e82d4d6cee9417ce6d
ms.contentlocale: ru-ru
ms.lasthandoff: 07/28/2017

---
# <a name="understanding-alerts-in-log-analytics"></a>Общие сведения об оповещениях в Log Analytics

Оповещения в Log Analytics содержат важную информацию о вашем репозитории Log Analytics.  В этой статье содержатся сведения о работе правил генерации оповещений в Log Analytics, а также описываются различия между разными типами этих правил.

Сведения о создании правил генерации оповещений см. по следующим ссылкам:

- создание правил генерации оповещений на [портале Azure](log-analytics-alerts-creating.md);
- создание правил генерации оповещений с помощью [шаблона Resource Manager](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md);
- создание правил генерации оповещений с помощью [REST API](log-analytics-api-alerts.md).


## <a name="alert-rules"></a>Правила оповещения

Оповещения создаются с помощью правил оповещения, которые автоматически выполняют поиск по журналам через регулярные интервалы.  Если результаты поиска по журналам соответствуют определенным условиям, создается запись оповещения.  После этого правило может автоматически запустить одно или несколько действий, чтобы заблаговременно уведомить вас об оповещении либо вызвать другой процесс.  В каждом типе правил генерации оповещений используется определенная логика выполнения этого анализа.

![Оповещения Log Analytics](media/log-analytics-alerts/overview.png)

Правила генерации оповещений определяются следующими сведениями:

- **Поиск по журналам**.  Это запрос, который будет запускаться каждый раз при срабатывании правила генерации оповещений.  С помощью записей, возвращаемых этим запросом, можно определить, нужно ли создавать оповещение.
- **Временное окно**.  Указывает диапазон времени для запроса.  Запрос возвращает только те записи, которые были созданы в течение этого периода, предшествующего настоящему времени.  Это значение может составлять от 5 минут до 24 часов. Например, если временное окно равно 60 минутам, а запрос выполняется в 13:15, то возвращаются только записи, созданные между 12:15 и 13:15.
- **Частота**.  Указывает, как часто должен выполняться запрос. Это значение может составлять от 5 минут до 24 часов. Оно должно быть меньше или равно временному окну.  Если значение больше, чем временное окно, появляется риск пропуска записей.<br>Например, рассмотрим временное окно в 30 минут и частоту в 60 минут.  Если запрос выполняется в 13:00, он возвращает записи между 12:30 и 13:00.  В следующий раз этот запрос будет выполнен в 14:00, а записи возвратятся между 13:30 и 14:00.  Записи, созданные между 13:00 и 13:30, не будут учитываться.
- **Пороговое значение**.  Чтобы определить, следует ли создавать оповещение, оцениваются результаты поиска по журналам.  Для каждого типа правил генерации оповещений определяется собственное пороговое значение.

Каждое правило генерации оповещений в Log Analytics относится к одному из двух типов.  Каждый из этих типов подробно описан в последующих разделах.

- **[Число результатов](#number-of-results-alert-rules)**. Если число записей, возвращенных в результатах поиска по журналам, превышает указанное количество, создается оповещение.
- **[Измерение метрик](#metric-measurement-alert-rules)**.  Оповещение, созданное для каждого объекта в результатах поиска по журналам со значением, превышающим указанное пороговое значение.

Ниже приведены различия между типами правил генерации оповещений.

- Правило генерации оповещений **Число результатов** всегда создает одно оповещение, а **Измерение метрик** — по одному для каждого объекта, для которого превышено пороговое значение.
- Правило генерации оповещений **Число результатов** создает оповещение при одноразовом превышении порогового значения. Правило генерации оповещений **Измерение метрик** может создавать оповещение при превышении порогового значения определенное количество раз за указанный интервал времени.

## <a name="number-of-results-alert-rules"></a>Правило генерации оповещений "Число результатов"
Правила генерации оповещений **Число результатов** создают одно оповещение, если количество записей, возвращенных в результатах поискового запроса, превышает указанное пороговое значение.

### <a name="threshold"></a>Threshold (Пороговое значение)
Под пороговым значением правила генерации оповещений **Число результатов** подразумевается значение, большее или меньшее чем определенное значение.  Если число записей, возвращенных в результатах поиска по журналам, соответствует этому критерию, создается оповещение.

### <a name="scenarios"></a>Сценарии

#### <a name="events"></a>События
Этот тип правила генерации оповещений идеально подходит для работы с такими событиями, как журналы событий Windows, системный журнал и настраиваемые журналы.  Вам может потребоваться создать оповещение, когда создается конкретное событие ошибки или при создании нескольких событий ошибки в определенном временном окне.

Для оповещения об одном событии задайте количество результатов больше 0, а частоту и временное окно установите равными 5 минутам.  При этом запрос выполняется каждые 5 минут и проверяет наличие одного события, созданного с момента последнего выполнения запроса.  Уменьшение частоты может привести к увеличению интервала между сбором данных о событии и созданием оповещения.

Некоторые приложения могут вносить в журнал нерегулярную ошибку, которая не обязательно должна вызывать оповещение.  Например, приложение может повторно запустить процесс, который создал событие ошибки и затем успешно завершил работу при следующем выполнении.  В этом случае вам может потребоваться создавать оповещение только в случае возникновения нескольких событий за определенное временное окно.  

В некоторых случаях может потребоваться создать оповещение в случае отсутствия события.  Например, процесс может регистрировать регулярные события, чтобы указать, что он работает правильно.  Если он не регистрирует одно из таких событий в пределах определенного временного окна, следует создать оповещение.  В этом случае задайте пороговое значение **Меньше 1**.

#### <a name="performance-alerts"></a>Оповещения производительности
[Данные о производительности](log-analytics-data-sources-performance-counters.md) хранятся в виде записей в репозитории OMS по аналогии с событиями.  Если вы хотите создать оповещение, когда значение счетчика производительности превышает определенный порог, это пороговое значение следует включить в запрос.

Например, если бы вам было нужно получать оповещения, когда процессор нагружен более чем на 90 %, то вы могли бы использовать запрос с пороговым значением для правила генерации оповещений **Больше 0**. Пример такого запроса приведен ниже.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Если бы вам было нужно получать оповещения, когда средняя нагрузка процессора превышает 90 % в определенном временном окне, то вы могли бы использовать запрос с [командой measure](log-analytics-search-reference.md#commands) и пороговым значением для правила генерации оповещений **Больше 0**. Ниже приведен пример такого запроса.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90

>[!NOTE]
> Если для рабочей области обновлен [язык запросов Log Analytics](log-analytics-log-search-upgrade.md), указанные выше запросы будут изменены следующим образом: `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90`
> `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90`.


## <a name="metric-measurement-alert-rules"></a>Metric measurement alert rules (Измерение метрики для правила генерации оповещений)

>[!NOTE]
> Сейчас правила генерации оповещений "Измерение метрик" находятся на этапе общедоступной предварительной версии.

Правила генерации оповещений **Измерение метрик** создают оповещение для каждого объекта в запросе со значением, превышающим указанное пороговое значение.  Ниже приведены их четко выраженные отличия от правил генерации оповещений **Число результатов**.

#### <a name="log-search"></a>Поиск по журналам
Правило генерации оповещений **Число результатов** поддерживает все типы запросов, но в отношении запросов для правила "Измерение метрик" применяются определенные требования.  Например, этот запрос должен содержать [команду measure](log-analytics-search-reference.md#commands), которая позволяет сгруппировать результаты по определенному полю. Эта команда должна содержать следующие элементы.

- **Агрегатная функция**.  Указывает выполняемые вычисления и (необязательно) числовое поле для статистических вычислений.  Например, **count()** возвращает число записей в запросе, а **avg(CounterValue)** — среднее значение поля CounterValue в течение указанного интервала.
- **Группировка по полю**.  Для каждого экземпляра этого поля будет создана запись с агрегированным значением, и для каждого из них можно создать оповещение.  Например, если вы хотите создать оповещение для каждого компьютера, используйте группировку **по компьютерам**.   
- **Интервал**.  Определяет интервал времени, в течение которого выполняется статистическое вычисление данных.  Например, если вы указали значение **5minutes**, запись будет создана для каждого экземпляра поля группировки, вычисленного с интервалом в 5 минут, за период времени, указанный для оповещения.

#### <a name="threshold"></a>Threshold (Пороговое значение)
Пороговое значение для правил генерации оповещений "Измерение метрик" определяется на основе статического значения и числа нарушений.  Если любая точка данных в результатах поиска по журналам превышает это значение, она рассматривается как нарушение.  Когда число нарушений в результатах для любого объекта превышает указанное значение, для этого объекта создается оповещение.

#### <a name="example"></a>Пример
Рассмотрим ситуацию, где оповещение должно создаваться, когда использование процессора на компьютере превышает 90 % три раза в течение 30 минут.  Вы должны создать правило генерации оповещений с приведенными ниже сведениями.  

**Запрос:** Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer Interval 5minute.<br>
**Временное окно:** 30 минут.<br>
**Периодичность предупреждений:** 5 минут.<br>
**Объединенное значение:** больше 90.<br>
**Активировать оповещение на основе:** общее число нарушений превышает 5.<br>

Запрос вернет среднее значение для каждого компьютера с интервалом в 5 минут.  Этот запрос выполняется каждые 5 минут для данных, собранных в течение предыдущих 30 минут.  Ниже приведен пример данных для трех компьютеров.

![Результаты выполнения примера запроса](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

В этом примере отдельные оповещения создаются для компьютеров srv02 и srv03, так как за указанное временное окно они 3 раза превысили пороговое значение в 90 %.  Если значение параметра **Активировать оповещение на основе:** изменить на **Consecutive** (Последовательные нарушения), тогда оповещение будет создано только для компьютера srv03, так как он превысил пороговое значение 3 раза подряд.

## <a name="alert-records"></a>Записи оповещений
Записи оповещений, созданные правилами генерации оповещений в Log Analytics, имеют значение **Alert** для **Type** и значение **OMS** для **SourceSystem**.  У них есть свойства, приведенные в таблице ниже.

| Свойство | Описание |
|:--- |:--- |
| Тип |*Предупреждение* |
| SourceSystem |*OMS* |
| *Объект*  | Оповещения [Измерение метрик](#metric-measurement-alert-rules) будут содержать свойство поля группировки.  Например, если результаты поиска по журналам группируются по компьютерам, запись оповещения будет содержать поле Computer с именем компьютера в качестве значения.
| AlertName |Имя оповещения. |
| AlertSeverity |Серьезность оповещения. |
| LinkToSearchResults |Ссылки на поиск журналов Log Analytics, возвращающая записи из запроса, создавшего оповещение. |
| Запрос |Текст запроса, который был запущен. |
| QueryExecutionEndTime |Конец диапазона времени для запроса. |
| QueryExecutionStartTime |Начало диапазона времени для запроса. |
| ThresholdOperator | Оператор, используемый правилом оповещения. |
| ThresholdValue | Значение, используемое правилом оповещения. |
| TimeGenerated |Дата и время создания оповещения. |

Существуют и другие виды записей оповещений, создаваемые [решением для управления оповещениями](log-analytics-solution-alert-management.md) и [экспортами Power BI](log-analytics-powerbi.md).  Все они имеют значение **Alert** для **Type**, но различаются по **SourceSystem**.


## <a name="next-steps"></a>Дальнейшие действия
* Установите [решение для управления оповещениями](log-analytics-solution-alert-management.md), чтобы анализировать предупреждения, создаваемые в Log Analytics, вместе с оповещениями, полученными от System Center Operations Manager.
* Вы можете ознакомиться с дополнительными сведениями о [поисках журналов](log-analytics-log-searches.md) , которые могут создавать оповещения.
* Вы можете выполнить пошаговое руководство по [настройке webook](log-analytics-alerts-webhooks.md) с использованием правила генерации оповещений.  
* Вы можете научиться писать [модули Runbook в службе автоматизации Azure](https://azure.microsoft.com/documentation/services/automation) для устранения проблем, обозначенных в оповещениях.

