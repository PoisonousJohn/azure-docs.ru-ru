---
title: "Соединитель управления ИТ-службами в OMS | Документация Майкрософт"
description: "Использование соединителя управления ИТ-службами для централизованного отслеживания и администрирования рабочих элементов ITSM в OMS, а также для быстрого решения возникающих проблем."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.contentlocale: ru-ru
ms.lasthandoff: 06/20/2017


---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Централизованное управление рабочими элементами ITSM с помощью соединителя управления ИТ-службами (предварительная версия)

![Символ соединителя управления ИТ-службами](./media/log-analytics-itsmc/itsmc-symbol.png)

Используйте соединитель управления ИТ-службами (ITSMC) в OMS Log Analytics, чтобы централизованно отслеживать и администрировать рабочие элементы между продуктами и службами ITSM.

Соединитель управления ИТ-службами интегрирует имеющиеся продукты и службы управления ИТ-службами (ITSM) с OMS Log Analytics.  Решение имеет двунаправленную интеграцию со службами и продуктами ITSM, предоставляя пользователям OMS возможности создавать инциденты, оповещения или события в решении ITSM. Соединитель также импортирует данные (например, инциденты и запросы на изменения) из решений ITSM в OMS Log Analytics.

С помощью соединителя управления ИТ-службами вы можете:

  - Централизованно отслеживать и администрировать рабочие элементы для продуктов и служб ITSM, используемых вашей организацией.
  - Создавать рабочие элементы ITSM (например, оповещения, события и инциденты) в ITSM из оповещений OMS или с помощью поиска по журналам.
  - Считывать инциденты и запросы на изменения из решений ITSM и сопоставлять с соответствующими данными журнала в рабочей области Log Analytics.
  - Находить любые неожиданные и необычные события и устранять связанные с ними проблемы еще до того, как пользователи сообщат о них в службу технической поддержки.
  - Импортировать данные рабочих элементов в Log Analytics и создавать отчеты о ключевых показателях эффективности.  С помощью этих отчетов можно выявлять и оценивать несколько важных элементов, например оценка вредоносных программ, и предпринимать действия над ними.
  - Просматривать выбранные панели мониторинга, чтобы подробнее ознакомиться с инцидентами, запросами на изменения и затронутыми системами.
  - Быстро устранять неполадки путем сопоставления с другими решениями по управлению в рабочей области Log Analytics.


## <a name="prerequisites"></a>Предварительные требования

Чтобы импортировать рабочие элементы ITSM в OMS Log Analytics, для решения требуется наличие связи между соединителем управления ИТ-службами в OMS и продуктом или службой ITSM, из которой импортируются рабочие элементы.


## <a name="configuration"></a>Конфигурация

Решение "Соединитель управления ИТ-службами" необходимо добавить в рабочую область OMS, как описано в статье [Добавление решений для управления Azure Log Analytics в рабочую область](log-analytics-add-solutions.md).

Плитка соединителя управления ИТ-службами в коллекции решений:

![Плитка соединителя](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

После успешного добавления в разделе **OMS** > **Параметры** > **Подключенные источники** отобразится соединитель управления ИТ-службами.

![ITSMC подключен](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> По умолчанию соединитель управления ИТ-службами обновляет данные подключения каждые 24 часа. Чтобы обновить данные после любого изменения или обновления шаблона, нажмите кнопку "Обновить" рядом с подключением.

 ![Обновление ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Пакеты управления
Для этого решения не требуются какие-либо пакеты управления.

## <a name="connected-sources"></a>Подключенные источники

Соединитель управления ИТ-службами поддерживает следующие службы и продукты ITSM:

- [System Center Service Manager](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms);

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms);  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Использование решения

После подключения соединителя управления ИТ-службами в OMS к службе ITSM службы Log Analytics начинают сбор данных с подключенных продуктов и служб ITSM.

> [!NOTE]
> - Данные, импортированные соединителем управления ИТ-службами, отображаются в Log Analytics в качестве событий **ServiceDesk_CL**.
- Событие содержит поле с именем **ServiceDeskWorkItemType_s**, которое может принимать значение в качестве инцидента или запроса на изменение в зависимости от данных рабочего элемента, содержащихся в событии **ServiceDesk_CL**.

## <a name="input-data"></a>Входные данные
Рабочие элементы импортируются из продуктов и служб ITSM.

Ниже приведены примеры данных, собранных соединителем управления ИТ-службами.

> [!NOTE]
> В зависимости от типа рабочего элемента, импортированного в Log Analytics, **ServiceDesk_CL** содержит указанные ниже поля.

**Рабочий элемент:** **инциденты**  
ServiceDeskWorkItemType_s="Incident"

**Поля**

- ServiceDeskConnectionName;
- "Идентификатор службы поддержки";
- Состояние
- "Срочность";
- Влияние
- Приоритет
- Escalation (Эскалация);
- "Кем создано";
- "Кем разрешено";
- Closed By (Кем закрыто);
- Источник
- Кому назначено
- Категория
- Название
- Описание
- "Дата создания";
- Closed Date (Дата закрытия);
- Resolved Date (Дата разрешения);
- "Дата последнего изменения";
- Компьютер


**Рабочий элемент:** **запросы на изменение**

ServiceDeskWorkItemType_s="ChangeRequest"

**Поля**
- ServiceDeskConnectionName;
- "Идентификатор службы поддержки";
- "Кем создано";
- Closed By (Кем закрыто);
- Источник
- Кому назначено
- Название
- Тип
- Категория
- Состояние
- Escalation (Эскалация);
- Conflict Status (Состояние конфликта);
- "Срочность";
- Приоритет
- "Риск";
- Влияние
- Кому назначено
- "Дата создания";
- Closed Date (Дата закрытия);
- "Дата последнего изменения";
- Requested Date (Запрошенная дата);
- Planned Start Date (Планируемая дата начала);
- Planned End Date (Планируемая дата окончания);
- Work Start Date (Дата начала работы);
- Work End Date (Дата окончания работы);
- Описание
- Компьютер

## <a name="output-data-for-a-servicenow-incident"></a>Выходные данные инцидента ServiceNow

| Поле OMS | Поле ITSM |
|:--- |:--- |
| ServiceDeskId_s| Number |
| IncidentState_s | Состояние |
| Urgency_s |"Срочность"; |
| Impact_s |Влияние|
| Priority_s | Приоритет |
| CreatedBy_s | Opened by (Кем открыто) |
| ResolvedBy_s | "Кем разрешено"|
| ClosedBy_s  | Closed By (Кем закрыто) |
| Source_s| Contact type (Тип контакта) |
| AssignedTo_s | Кому назначено  |
| Category_s | Категория |
| Title_s|  Краткое описание |
| Description_s|  Примечания |
| CreatedDate_t|  Opened (Открыто) |
| ClosedDate_t| closed|
| ResolvedDate_t|"Разрешено"|
| Компьютер  | Элемент конфигурации |

## <a name="output-data-for-a-servicenow-change-request"></a>Выходные данные запроса на изменение ServiceNow

| Поле OMS | Поле ITSM |
|:--- |:--- |
| ServiceDeskId_s| Number |
| CreatedBy_s | "Кем запрошено" |
| ClosedBy_s | Closed By (Кем закрыто) |
| AssignedTo_s | Кому назначено  |
| Title_s|  Краткое описание |
| Type_s|  Тип |
| Category_s|  Catgory |
| CRState_s|  Состояние|
| Urgency_s|  "Срочность"; |
| Priority_s| Приоритет|
| Risk_s| "Риск";|
| Impact_s| Влияние|
| RequestedDate_t  | Requested by date |
| ClosedDate_t | Closed Date (Дата закрытия) |
| PlannedStartDate_t  |     Planned Start Date (Планируемая дата начала) |
| PlannedEndDate_t  |   Planned End Date (Планируемая дата окончания) |
| WorkStartDate_t  | Actual start date (Фактическая дата начала) |
| WorkEndDate_t | Actual end date (Фактическая дата окончания)|
| Description_s | Описание |
| Компьютер  | Элемент конфигурации |

**Снимок экрана с примером Log Analytics для данных ITSM:**

![Снимок экрана с Log Analytics](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>Интеграция соединителя управления ИТ-службами с другими решениями OMS

Сейчас соединитель управления ИТ-службами поддерживает интеграцию с решением схемы услуги.

Служба схемы услуги автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. Это решение позволяет рассматривать серверы как взаимосвязанные системы, предоставляющие важные службы. Схема услуги отображает сведения о подключениях между серверами, процессами и портами в любой подключенной по протоколу TCP архитектуре без дополнительной настройки. Пользователям требуется только установить агент. Дополнительные сведения см. в статье [Использование решения схемы услуги в Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-service-map.md).

Благодаря этой интеграции вы можете просматривать элементы службы поддержки, созданные в решениях ITSM, как показано ниже.

![Интегрированное решение ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Создание рабочих элементов ITSM для оповещений OMS

Для оповещений OMS можно создать связанные рабочие элементы в подключенных источниках ITSM.  Для этого сделайте следующее:

1. Запустите запрос поиска по журналам в окне **Поиск по журналу**, чтобы просмотреть данные. Результаты запроса служат источником для рабочих элементов.
2. В разделе **Поиск по журналу** щелкните **Оповещение**, чтобы открыть страницу **Добавить правило оповещения**.

    ![Снимок экрана с Log Analytics](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. В окне **Добавить правило оповещения** предоставьте необходимые сведения в полях **Имя**, **Серьезность**, **Поисковый запрос**, а также укажите **критерии оповещения** (временное окно и единицы измерения).
4. Для **Действия: ITSM** выберите значение **Да**.
5. Выберите подключение ITSM из списка **Выбрать подключение**.
6. Предоставьте необходимые сведения.
7. Чтобы создать отдельный рабочий элемент для каждой записи журнала для этого оповещения, установите флажок **Создать отдельные рабочие элементы для каждой записи журнала**.

    Или

    Не устанавливайте этот флажок, если вы хотите создать только один рабочий элемент для любого количества записей журнала для этого оповещения.

7. Щелкните **Сохранить**.

Оповещение OMS будет создано в разделе **Оповещения**. Соответствующие рабочие элементы подключения ITSM создаются, когда выполняются указанные условия оповещений.

## <a name="create-itsm-work-items-from-oms-logs"></a>Создание рабочих элементов ITSM из журналов OMS

Вы можете создать рабочие элементы в подключенных источниках ITSM с помощью OMS Log Search. Для этого сделайте следующее:

1. Выполните поиск необходимых данных в разделе **Поиск по журналу**, выберите данные и щелкните **Create work item** (Создать рабочий элемент).

    Отобразится окно **Create ITSM Work item** (Создание рабочего элемента ITSM):

    ![Снимок экрана с Log Analytics](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Добавьте следующие сведения:

  - **Название рабочего элемента.** Имя рабочего элемента.
  - **Описание рабочего элемента.** Описание нового рабочего элемента.
  - **Задействованный компьютер.** Имя компьютера, на котором находятся данные журналов.
  - **Выбрать подключение.** Подключение ITSM, в котором требуется создать рабочий элемент.
  - **Рабочий элемент.** Тип рабочего элемента.

3. Чтобы использовать имеющейся шаблон рабочего элемента для инцидента, установите для параметра **Generate work item based on the template** (Создать рабочий элемент на основе шаблона) значение **Да** и нажмите кнопку **Создать**.

    или

    Если вы хотите предоставить собственные настраиваемые значения, щелкните **Нет**.

4. Предоставьте соответствующие значения в текстовых полях **Тип контакта**, **Воздействие**, **Срочность**, **Категория** и **Подкатегория**, а затем нажмите кнопку **Создать**.

В ITSM будет создан рабочий элемент, который вы также можете просмотреть в OMS.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Устранение неполадок с подключением ITSM в OMS
1.  Если сбой подключения происходит из пользовательского интерфейса подключенного источника и отображается сообщение **Ошибка при сохранении подключения**, сделайте следующее:
 - При использовании подключений ServiceNow, Cherwell и Provance проверьте имя пользователя и пароль, а также идентификатор и секрет клиента каждого подключения. Если ошибка не исчезнет, проверьте наличие необходимых прав в соответствующем продукте ITSM, чтобы установить подключение.
 - При использовании Service Manager убедитесь, что веб-приложение успешно развернуто и создано гибридное подключение. Чтобы проверить подключение к локальному компьютеру Service Manager, перейдите по URL-адресу веб-приложения, как описано в документации по установке [гибридного подключения](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Если данные из ServiceNow не синхронизируются с OMS, убедитесь, что экземпляр ServiceNow не находится в спящем режиме. Иногда эта ошибка может происходить в экземплярах ServiceNow во время простоя. В противном случае сообщите о проблеме.
3.  Если оповещения поступают из OMS, но рабочие элементы не создаются в продукте ITSM или элементы конфигурации не создаются и (или) связываются с рабочими элементами, сделайте следующее:
 -  С помощью решения "Соединитель управления ИТ-службами" на портале OMS соберите сводные данные о подключениях, рабочих элементах, компьютерах и т. д. Щелкните сообщение об ошибке в колонке состояния, перейдите на страницу **Поиск по журналам** и просмотрите подключение с ошибкой, используя сведения в сообщении.
 - Сведения об ошибках и другие связанные сведения можно просмотреть непосредственно на странице **Поиск по журналам** с помощью *Type=ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Устранение неполадок развертывания веб-приложения Service Manager
1.  При наличии неполадок с развертыванием веб-приложения проверьте наличие всех необходимых разрешений в подписке, используемой для создания или развертывания ресурсов.
2.  Если во время выполнения [скрипта](log-analytics-itsmc-service-manager-script.md) отображается сообщение **Ссылка на объект не указывает на экземпляр объекта**, проверьте значения в разделе **Конфигурация пользователя**.
3.  Если вам не удается создать пространство имен ретранслятора шины обслуживания, убедитесь, в подписке зарегистрирован требуемый поставщик ресурсов. Если не зарегистрирован, создайте его на портале Azure вручную. Его также можно создать на портале Azure во время создания [гибридного подключения](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).


## <a name="contact-us"></a>Свяжитесь с нами

Свяжитесь с нами по адресу [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com), чтобы оставить отзывы или запросы касательно соединителя управления ИТ-службами.

## <a name="next-steps"></a>Дальнейшие действия
[Подключение продуктов и служб ITSM с помощью соединителя управления ИТ-службами (предварительная версия)](log-analytics-itsmc-connections.md)

