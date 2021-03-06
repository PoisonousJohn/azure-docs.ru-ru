---
title: "Создание представлений для анализа данных в OMS Log Analytics | Документация Майкрософт"
description: "Конструктор представлений Log Analytics позволяет создавать пользовательские представления, содержащие различные визуальные представления данных репозитория OMS, которые отображаются в OMS или на портале Azure. Эта статья содержит обзор конструктора представлений и процедур по созданию и изменению пользовательских представлений."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: e3c463d749dc4179df58286b9bb75584880a6bc6
ms.contentlocale: ru-ru
ms.lasthandoff: 08/28/2017

---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Использование конструктора представлений для создания пользовательских представлений Log Analytics
Конструктор представлений [Log Analytics](log-analytics-overview.md) позволяет создавать в консоли OMS пользовательские представления, содержащие различные визуальные представления данных репозитория OMS. Эта статья содержит обзор конструктора представлений и процедур по созданию и изменению пользовательских представлений.

Ниже перечислены другие статьи о конструкторе представлений:

* [Справка по плиткам](log-analytics-view-designer-tiles.md) — справочник по параметрам для каждой плитки, доступной в пользовательских представлениях.
* [Log Analytics View Designer visualization part reference](log-analytics-view-designer-parts.md) (Справка по элементам визуализации в конструкторе представлений Log Analytics). Справочник по параметрам всех плиток, которые можно использовать при создании пользовательских представлений.

>[!NOTE]
> Если ваша рабочая область переведена на [новый язык запросов Log Analytics](log-analytics-log-search-upgrade.md), запросы во всех представлениях должны создаваться на [новом языке запросов](https://go.microsoft.com/fwlink/?linkid=856078).  Все представления, созданные до обновления рабочей области, будут автоматически преобразованы.

## <a name="concepts"></a>Основные понятия
В следующей таблице перечислены элементы, которые входят в представления, созданные с помощью конструктора представлений.

| Часть | Описание |
|:--- |:--- |
| Плитка |Отображается на главной обзорной панели мониторинга Log Analytics.  Содержит визуальный обзор сведений, содержащихся в пользовательском представлении.  На плитках различных типов по-разному визуализируются записи, хранящиеся в репозитории OMS.  Щелкните плитку, чтобы открыть пользовательское представление. |
| Пользовательское представление |Отображается при нажатии плитки.  Содержит один или несколько элементов визуализации. |
| Элемент визуализации |Визуальное отображение данных, хранящихся в репозитории OMS, на основе результатов одного или нескольких [поисков по журналам](log-analytics-log-searches.md).  Обычно элемент содержит заголовок с обобщенным визуальным представлением данных и список лучших результатов.  Различные типы элементов обеспечивают разную визуализацию записей, хранящихся в репозитории OMS.  Щелкните содержимое элемента, чтобы выполнить поиск по журналам и увидеть подробные результаты. |

![Обзор конструктора представлений](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Добавление конструктора представлений в рабочую область
Если конструктор представлений доступен в режиме предварительного просмотра, его необходимо добавить в рабочую область, щелкнув **Предварительный просмотр компонентов** в разделе **Параметры** на портале OMS.

![Включение предварительной версии](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Создание и изменение представлений
### <a name="create-a-new-view"></a>Создание представления
Откройте новое представление в **конструкторе представлений**, щелкнув плитку "Конструктор представлений" на главной панели мониторинга OMS.

![Плитка "Конструктор представлений"](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Изменение существующего представления
Чтобы изменить существующее представление в конструкторе, откройте представление, щелкнув его плитку на главной панели мониторинга OMS.  Затем нажмите кнопку **Изменить**, чтобы открыть представление в конструкторе.

![Изменение представления](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Клонирование существующего представления
При клонировании представления создается другое представление, которое открывается в конструкторе.  Для нового представления используется такое же имя, как и у исходного, с добавлением в конце слова "копия" (Copy).  Чтобы клонировать представление, щелкните его плитку на главной панели мониторинга OMS.  Затем нажмите кнопку **Клонировать**, чтобы открыть представление в конструкторе представлений.

![Клонирование представления](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Удаление существующего представления
Чтобы удалить существующее представление, откройте его, щелкнув соответствующую плитку на главной панели мониторинга OMS.  Затем нажмите кнопку **Изменить**, чтобы открыть представление в конструкторе, и кнопку **Удалить представление**.

![Удаление представления](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Экспорт существующего представления
Представление можно экспортировать в JSON-файл, который затем можно импортировать в другую рабочую область или использовать в [шаблоне Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).  Чтобы экспортировать существующее представление, откройте его, щелкнув плитку нужного представления на главной панели мониторинга OMS.  Нажмите кнопку **Экспорт**, чтобы создать файл экспорта в папке скачивания браузера.  У файла будет такое же имя, как и у исходного представления, и расширение *omsview*.

![Экспорт представления](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Импорт существующего представления
Вы можете импортировать файл *omsview*, ранее экспортированный из другой группы управления.  Чтобы импортировать существующее представление, сначала создайте другое представление.  Затем нажмите кнопку **Импорт** и выберите файл *omsview*.  В существующее представление будет скопирована конфигурация, сохраненная в этом файле.

![Экспорт представления](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Работа с конструктором представлений
В окне конструктора есть три области.  Область **разработки** содержит пользовательское представление.  Чтобы добавить в него плитки или элементы, скопируйте их из области **управления** в область **разработки**.  В области **свойств** будут отображаться свойства плитки или выбранного элемента.

![«Просмотреть конструктор»](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Настройка плитки представления
В пользовательском представлении может быть только одна плитка.  Выберите вкладку **Плитка** в области **управления**, чтобы просмотреть текущую плитку представления или выбрать другую.  В области **свойств** отображаются свойства для текущей плитки.  Настройте свойства плитки, используя подробные сведения из [справки по плиткам](log-analytics-view-designer-tiles.md), а затем сохраните изменения, нажав кнопку **Применить**.

### <a name="configure-visualization-parts"></a>Настройка элементов визуализации
Представление может содержать неограниченное число элементов визуализации.  Выберите вкладку **Представление**, а затем элемент визуализации, который хотите добавить в представление.  В области **свойств** будут отображаться свойства выбранного элемента.  Настройте свойства представления, используя подробные сведения из [справки по элементам визуализации](log-analytics-view-designer-parts.md), а затем сохраните изменения, нажав кнопку **Применить**.

### <a name="delete-a-visualization-part"></a>Удаление элемента визуализации
Вы можете удалить из представления любой элемент визуализации, щелкнув кнопку **X** в правом верхнем углу этого элемента.

### <a name="rearrange-visualization-parts"></a>Изменение порядка элементов визуализации
Представления могут содержать только одну строку элементов визуализации.  Чтобы изменить порядок размещения элементов в представлении, перетащите нужные элементы на новое место.

## <a name="next-steps"></a>Дальнейшие действия
* Добавление [плиток](log-analytics-view-designer-tiles.md) к пользовательскому представлению.
* Добавление [элементов визуализации](log-analytics-view-designer-parts.md) к пользовательскому представлению.

