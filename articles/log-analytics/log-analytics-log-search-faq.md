---
title: "Вопросы и ответы по новым возможностям поиска по журналам Log Analytics | Документация Майкрософт"
description: "Эта статья содержит часто задаваемые вопросы о переходе Log Analytics на новый язык запросов."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: eeed445631885093a8e1799a8a5e1bcc69214fe6
ms.openlocfilehash: 507136beef9718dc6a7f42a4b84f8030d4a60563
ms.contentlocale: ru-ru
ms.lasthandoff: 09/07/2017

---

# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Вопросы и ответы по новым возможностям поиска по журналам Log Analytics и известные проблемы

Эта статья содержит часто задаваемые вопросы о [переходе Log Analytics на новый язык запросов](log-analytics-log-search-upgrade.md), а также связанные с этим известные проблемы.  Перед обновлением рабочей области мы рекомендуем ознакомиться с содержанием этой статьи.


## <a name="alerts"></a>Оповещения

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Вопрос. У меня много правил генерации оповещений. Придется ли мне после обновления создать их заново на новом языке?  
Нет, все правила оповещений автоматически преобразуются на новый язык поиска в процессе обновления.  


## <a name="computer-groups"></a>Группы компьютеров

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Вопрос. Попытки использовать группы компьютеров завершаются ошибкой.  Их синтаксис изменился?
Да. После обновления рабочей области изменяется синтаксис использования групп компьютеров.  Дополнительные сведения см. в статье [Использование групп компьютеров при поиске по журналам Log Analytics](log-analytics-computer-groups.md).

### <a name="known-issue-groups-imported-from-active-directory"></a>Известная проблема. Группы, импортированные из Active Directory
В настоящее время вы не можете создать запрос, использующий группы компьютеров, импортированные из Active Directory.  В качестве обходного решения до устранения этой проблемы вы можете создать группу компьютеров с помощью импортированной группы Active Directory, а затем использовать эту группу в своем запросе.

Пример запроса на создание группы компьютеров, включающей импортированную группу Active Directory, выглядит следующим образом:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Панели мониторинга

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Вопрос. Можно ли по-прежнему использовать панели мониторинга в обновленной рабочей области?
Вы можете и далее использовать любые элементы, добавленные на **панель мониторинга** до обновления рабочей области, но после обновления вы не сможете их изменить или добавлять новые.  Вы можете по-прежнему создавать и изменять представления с помощью [конструктора представлений](log-analytics-view-designer.md), а также создавать панели мониторинга на портале Azure.


## <a name="log-searches"></a>Поиск по журналам

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Вопрос. До обновления я сохранял поисковые запросы вне рабочей области. Могу ли я теперь автоматически преобразовать их на новый язык поиска?
Вы можете преобразовать каждый запрос отдельно с помощью средства преобразования, которое доступно на странице поиска по журналам.  Невозможно автоматически преобразовать несколько поисковых запросов вне процесса обновления рабочей области.

### <a name="question-why-are-my-query-results-not-sorted"></a>Вопрос. Почему результаты запроса не отсортированы?
В новом языке запросов результаты не сортируются по умолчанию.  Чтобы отсортировать результаты по одному или нескольким свойствам, используйте [оператор SORT](https://go.microsoft.com/fwlink/?linkid=856079).

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Известная проблема. Результаты поиска в списке могут содержать свойства без данных
Результаты поиска по журналу в списке могут содержать свойства без данных.  До обновления эти свойства не отображались.  Мы работаем над устранением этой проблемы, после чего они перестанут отображаться.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Известная проблема. При выборе значения на диаграмме не отображаются подробные результаты
При выборе значения на диаграмме до обновления вы получали подробный список записей, соответствующих выбранному значению.  После обновления возвращается только одна строка со сводными данными.  В настоящее время мы работаем над решением этой проблемы.

## <a name="log-search-api"></a>API поиска по журналам

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Вопрос. Обновляется ли API поиска по журналам после обновления рабочей области?
Прежние версии [API поиска п журналам](log-analytics-log-search-api.md) не будут работать после обновления рабочей области.  Сведения о новых API. см. в руководстве по [API REST Log Analytics Azure](https://dev.loganalytics.io/).


## <a name="portals"></a>Порталы

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Вопрос. Следует ли использовать новый портал углубленной аналитики или продолжать использовать портал поиска по журналам?
Сравнение этих двух порталов см. в статье [Порталы для создания и изменения запросов к журналу в службе Azure Log Analytics](log-analytics-log-search-portals.md).  Каждый портал имеет свои преимущества, так что вы можете выбрать наиболее подходящий вариант в соответствии со своими требованиями.  Часто пользователи пишут запросы на портале углубленной аналитики, а затем вставляют их в другие расположения, например конструктор представлений.  При использовании этого варианта вам стоит ознакомиться с [потенциальными проблемами](log-analytics-log-search-portals.md#advanced-analytics-portal).


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Вопрос. Затронут ли эти изменения интеграцию с Power BI?
Да.  После обновления рабочей области процесс экспорта данных Log Analytics в Power BI работать не будет.  Все существующие расписания, созданные перед обновлением, будут отключены.  После обновления в Azure Log Analytics работает та же платформа, что и в Application Insights, и вы можете использовать для экспорта запросов Log Analytics в Power BI тот же процесс, что и для [экспорта запросов Application Insights в Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Известная проблема. Ограничение размера запроса Power BI
В настоящее время размер запроса Log Analytics, который можно экспортировать в Power BI, ограничивается 8 МБ.  Мы планируем увеличить это ограничение.


##<a name="powershell-cmdlets"></a>Командлеты PowerShell

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Вопрос. Обновляется ли командлет PowerShell поиска по журналам после обновления рабочей области?
Обновление языка поиска командлета [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) пока не предусмотрено.  Продолжайте использовать прежний язык запросов с этим командлетом даже после обновления рабочей области.  После обновления командлета мы обновим соответствующую документацию.


## <a name="resource-manager-templates"></a>Шаблоны диспетчера ресурсов

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Вопрос. Можно ли создать обновленную рабочую область с помощью шаблона Resource Manager?
Да.  Используйте API версии 2017-03-15-preview и добавить раздел **features**, как показано в примере ниже.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Решения

### <a name="question-will-my-solutions-continue-to-work"></a>Вопрос. Будут ли и дальше работать мои решения?
Все решения будут продолжать работать в обновленной рабочей области. Новый язык запросов позволяет увеличить их производительность.  В этом разделе описаны известные проблемы, возникающие с некоторыми имеющимися решениями.

### <a name="known-issue-capacity-and-performance-solution"></a>Известная проблема. Решение "Емкость и производительность"
Некоторые компоненты в представлении [Емкости и производительности](log-analytics-capacity.md) могут быть пустыми.  Вскоре мы предоставим исправление этой проблемы.

### <a name="known-issue-device-health-solution"></a>Известная проблема. Решение "Работоспособность устройств"
Решение [Работоспособность устройств ](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) не собирает данные в обновленной рабочей области.  Вскоре мы предоставим исправление этой проблемы.

### <a name="known-issue-application-insights-connector"></a>Известная проблема. Соединитель Application Insights
В настоящее время [Соединитель Application Insights](log-analytics-app-insights-connector.md) не поддерживается в обновленной рабочей области.  Мы работаем над решением этой проблемы.

## <a name="upgrade-process"></a>Процесс обновления

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Вопрос. У меня есть несколько рабочих областей. Можно ли обновить все рабочие области одновременно?  
Нет.  Каждое обновление применяется только к одной рабочей области. В настоящее время не существует возможности обновить сразу несколько рабочих областей. Обратите внимание, что обновление затронет и других пользователей обновленной рабочей области.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Вопрос. Изменятся ли после обновления имеющиеся данные в журналах, собранных в рабочей области?  
Нет. Данные журналов, доступные для поиска в рабочей области, не изменяются в процессе обновления. Сохраненные поисковые запросы, оповещения и представления автоматически преобразуются на новый язык поиска.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Вопрос. Что произойдет, если не обновлять рабочую область?  
Старая версия поиска по журналам будет считаться устаревшей через несколько месяцев. Рабочие области, которые вы не обновите к этому времени, будут обновлены автоматически.

### <a name="question-i-didnt-choose-to-upgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Вопрос. Рабочая область обновилась самостоятельно. Что произошло?  
Возможно, вашу рабочую область обновил другой администратор. Также обратите внимание, что после выхода общедоступной версии нового языка все рабочие области обновятся автоматически.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-to-cancel-it-and-restore-everything-back-what-should-i-do"></a>Вопрос: Я случайно обновил рабочую область, а теперь хочу отменить обновление. Что мне делать?  
Не беспокойтесь.  Мы создали моментальный снимок рабочей области перед обновлением, и вы можете ее восстановить. Но учтите, что при этом будут утрачены все поиски, оповещения и представления, которые вы успели сохранить после обновления.  Чтобы восстановить среду рабочей области, воспользуйтесь процедурой из раздела [Можно ли вернуться обратно после обновления?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)


## <a name="views"></a>Views

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Вопрос. Как создать представление с помощью конструктора представлений?
До обновления рабочей области вы могли создать представление с помощью конструктора представлений на плитке главной панели мониторинга.  После обновления эта плитка удаляется.  Вы можете создать представление с помощью конструктора представлений на портале OMS, нажав зеленую кнопку "+" в меню слева.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Известная проблема. После нажатия кнопки "Показать все" на графиках в представлениях не отображается соответствующий график
При нажатии кнопки *Показать все* в нижней области графика в представлении отображается таблица.  До обновления рабочей области отображался график.  Мы работаем над решением этой проблемы.


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [переключении рабочей области на новый язык запросов Log Analytics](log-analytics-log-search-upgrade.md).

