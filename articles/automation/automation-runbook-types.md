---
title: "Типы модулей Runbook в службе автоматизации Azure | Документация Майкрософт"
description: "Описание различных типов модулей Runbook, которые можно использовать в службе автоматизации Azure, и рекомендации по выбору типа модуля. "
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9265c975-4281-4819-a84f-d86641277f36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/01/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: e859aef473b433fbf4efb639962f3a3ce0a23d7b
ms.contentlocale: ru-ru
ms.lasthandoff: 06/03/2017


---
# <a name="azure-automation-runbook-types"></a>Типы модулей Runbook в службе автоматизации Azure
Служба автоматизации Azure поддерживает четыре типа модулей Runbook, кратко описанных в приведенной ниже таблице.  Следующие разделы содержат дополнительную информацию о каждом типе, включая рекомендацию по использованию.

| Тип | Описание |
|:--- |:--- |
| [Графический](#graphical-runbooks) |Опирается на Windows PowerShell, а полностью создается и редактируется в графическом редакторе на портале Azure. |
| [Графический модуль рабочего процесса PowerShell](#graphical-runbooks) |Опирается на рабочий процесс Windows PowerShell, а полностью создается и редактируется в графическом редакторе на портале Azure. |
| [PowerShell](#powershell-runbooks) |Текстовый модуль Runbook, основанный на сценарии Windows PowerShell. |
| [Рабочий процесс PowerShell](#powershell-workflow-runbooks) |Текстовый модуль Runbook, основанный на рабочем процессе Windows PowerShell. |

## <a name="graphical-runbooks"></a>Графические модули Runbook
[Графические](automation-runbook-types.md#graphical-runbooks) модули Runbook и графические модули Runbook рабочего процесса PowerShell создаются и редактируются в графическом редакторе на портале Azure.  Их можно экспортировать в файл и затем импортировать в другую учетную запись автоматизации, но создавать и редактировать их в других инструментах нельзя.  Графические модули Runbook генерируют код PowerShell, но просматривать и изменять код напрямую нельзя. Графические модули Runbook нельзя конвертировать ни в один из [текстовых форматов](automation-runbook-types.md), а текстовые нельзя превратить в графические. Во время импорта графические модули Runbook можно преобразовывать в графические модули Runbook рабочего процесса PowerShell и наоборот.

### <a name="advantages"></a>Преимущества
* Визуальная модель создания insert-link-configure.  
* Фокусировка на том, как происходит поток данных.  
* Визуальное представление процессов управления.  
* Включение других модулей Runbook в качестве дочерних Runbook для создания рабочих процессов высокого уровня.  
* Способствует модульному программированию.  


### <a name="limitations"></a>Ограничения
* Редактировать модуль Runbook за пределами портала Azure нельзя.
* Для выполнения сложной логики может потребоваться действие Code, содержащее код PowerShell.
* Вам не удастся отобразить или напрямую изменить код PowerShell, созданный графическим рабочим процессом. Обратите внимание, что вы можете просматривать код в любых действиях Code.

## <a name="powershell-runbooks"></a>Модули Runbook PowerShell
Модули Runbook PowerShell используют Windows PowerShell.  Код модуля Runbook можно редактировать в текстовом редакторе на портале Azure.  Кроме того, можно использовать любой автономный текстовый редактор и [импортировать Runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) в службу автоматизации Azure.

### <a name="advantages"></a>Преимущества
* Реализация сложной логики с помощью кода рабочего процесса PowerShell без дополнительного усложнения рабочего процесса PowerShell. 
* Модуль Runbook запускается быстрее, чем модули рабочего процесса PowerShell, поскольку не требуют предварительной компиляции.

### <a name="limitations"></a>Ограничения
* Необходимо знание сценариев PowerShell.
* Вам не удастся использовать [параллельную обработку](automation-powershell-workflow.md#parallel-processing) для одновременного выполнения нескольких действий.
* Вам не удастся использовать [контрольные точки](automation-powershell-workflow.md#checkpoints) для возобновления Runbook в случае ошибки.
* Runbook рабочих процессов PowerShell и графические Runbook можно включать в качестве дочерних Runbook с помощью командлета Start-AzureAutomationRunbook, который создает задание.

### <a name="known-issues"></a>Известные проблемы
Ниже перечислены проблемы с модулями Runbook PowerShell, известные на данный момент.

* Runbook PowerShell не могут извлекать незашифрованный [переменный ресурс-контейнер](automation-variables.md) с пустым значением.
* Runbook PowerShell не имеют возможности извлекать [переменный ресурс-контейнер](automation-variables.md) , в имени которого есть символ *~* .
* Модуль Get-Process в цикле в модуле Runbook PowerShell может аварийно завершить работу после примерно 80 итераций. 
* Модуль Runbook PowerShell может завершиться ошибкой, если попытается записать слишком большой объем данных в поток вывода за один раз.   Обычно эту проблему можно обойти, выводя при работе с большими объектами только необходимые данные.  Например, вместо использования метода *Get-Process* можно вывести только требуемые поля, указав *Get-Process | Select ProcessName, CPU*.

## <a name="powershell-workflow-runbooks"></a>Модули Runbook рабочих процессов PowerShell
Runbook рабочих процессов PowerShell представляют собой текстовые Runbook, основанные на [рабочем процессе Windows PowerShell](automation-powershell-workflow.md).  Код модуля Runbook можно редактировать в текстовом редакторе на портале Azure.  Кроме того, можно использовать любой автономный текстовый редактор и [импортировать Runbook](http://msdn.microsoft.com/library/azure/dn643637.aspx) в службу автоматизации Azure.

### <a name="advantages"></a>Преимущества
* Реализация сложной логики с помощью кода рабочего процесса PowerShell.
* Использование [контрольных точек](automation-powershell-workflow.md#checkpoints) для возобновления модулей Runbook в случае ошибки.
* Использование [параллельной обработки](automation-powershell-workflow.md#parallel-processing) для одновременного выполнения нескольких действий.
* Графические Runbook могут включать другие графические Runbook и Runbook рабочего процесса PowerShell в качестве дочерних Runbook для создания рабочих процессов высокого уровня.

### <a name="limitations"></a>Ограничения
* Автор должен знать рабочий процесс PowerShell.
* Runbook должен справляться с усложнениями рабочего процесса PowerShell, например с [десериализованными объектами](automation-powershell-workflow.md#code-changes).
* Модуль Runbook запускается дольше, чем модули Runbook PowerShell, поскольку требует предварительной компиляции.
* Runbook PowerShell можно включать только как дочерние Runbook с помощью командлета Start-AzureAutomationRunbook, который создает задание.

## <a name="considerations"></a>Рекомендации
Выбирая тип модуля Runbook, принимайте во внимание следующие факторы:

* Нельзя преобразовать модули Runbook из графического типа в текстовый или наоборот.
* При использовании других типов дочерних модулей Runbook могут действовать некоторые ограничения.  Дополнительные сведения см. в статье [Дочерние модули Runbook в службе автоматизации Azure](automation-child-runbooks.md).

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о графической разработке модулей Runbook см. в статье [Графическая разработка в службе автоматизации Azure](automation-graphical-authoring-intro.md).
* Чтобы понять различия между PowerShell и рабочими процессами PowerShell для модулей Runbook, см. статью [Изучение рабочего процесса Windows PowerShell](automation-powershell-workflow.md).
* Дополнительные сведения о создании и импорте модуля Runbook см. в статье [Создание или импорт модуля Runbook в службе автоматизации Azure](automation-creating-importing-runbook.md).


