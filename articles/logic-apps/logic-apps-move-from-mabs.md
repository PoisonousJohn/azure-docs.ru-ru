---
title: "Перемещение приложений из служб BizTalk в Azure Logic Apps | Документы Майкрософт"
description: "Перемещение или миграция служб BizTalk Azure (MABS) в Logic Apps"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: e58c6950d1d9420f32fc98ca917216dc5fae4fc3
ms.contentlocale: ru-ru
ms.lasthandoff: 05/31/2017


---

# <a name="move-from-biztalk-services-to-logic-apps"></a>Переход от служб BizTalk к службе Logic Apps

Службы Microsoft Azure BizTalk Services (MABS) находятся на этапе прекращения использования. Эта статья поможет вам переместить решения интеграции MABS в службу Azure Logic Apps. 

## <a name="overview"></a>Обзор

Службы BizTalk состоят из двух вложенных служб:

1.    гибридные подключения служб Microsoft BizTalk;
2.    интеграция на основе моста EAI и EDI.

Если вам требуется переместить гибридные подключения, в статье [Гибридные подключения к службе приложений Azure](../app-service/app-service-hybrid-connections.md) приводится описание изменений и компонентов этой службы. Гибридные подключения Azure заменяют гибридные подключения служб BizTalk. Служба гибридных подключений Azure доступна со службой приложений Azure и предлагается на портале Azure. Служба гибридных подключений Azure также предоставляет новый диспетчер гибридных подключений для управления существующими гибридными подключениями служб BizTalk и новыми гибридными подключениями, созданными на портале. Выпущена общедоступная версия гибридных подключений службы приложений Azure.

Служба Logic Apps заменяет интеграцию на основе моста EAI и EDI. Logic Apps обеспечивает те же возможности, что и службы BizTalk, а также дополнительные функции. Logic Apps предоставляет рабочий процесс масштабирования облака на основе потребления и возможности оркестрации, которые позволяют быстро создавать сложные решения интеграции с помощью браузера или инструментов Visual Studio.

В следующей таблице приведено сопоставление возможностей служб BizTalk и Logic Apps.

| Службы BizTalk   | Приложения логики            | Назначение                  |
| ------------------ | --------------------- | ---------------------------- |
| Соединитель          | Соединитель             | Отправка и получение данных   |
| Мост             | приложение логики;             | Обработчик конвейера           |
| Стадия проверки     | Действие проверки XML      | Проверка соответствия XML-документа схеме             |
| Стадия добавления       | Маркеры данных      | Передача свойств в сообщения или в механизм принятия решений о маршрутизации             |
| Стадия преобразования    | Действие преобразования      | Преобразование сообщений XML из одного формата в другой             |
| Стадия декодирования       | Действие декодирования неструктурированного файла      | Преобразование из неструктурированного файла в XML             |
| Стадия кодирования       |  Действие кодирования неструктурированного файла      | Преобразование из XML в неструктурированный файл             |
| Инспектор сообщений       |  Функции Azure или приложения API      | Запуск пользовательского кода в интеграции             |
| Действия маршрутизации      |  Условие или переключатель      | Маршрутизация сообщений к одному из указанных соединителей             |

В службах BizTalk имеются различные виды артефактов.

## <a name="connectors"></a>Соединители
Соединители в службах BizTalk позволяют мостам отправлять и получать данные, включая двусторонние мосты, поддерживающие взаимодействие типа "запрос-ответ" на основе HTTP. В службе Logic Apps используется та же терминология. Соединители в приложениях логики имеют то же назначение, а также включают более 140 мостов, которые могут подключаться к широкому ряду технологий и служб, как локально с помощью локального шлюза данных (вместо службы адаптера BizTalk), так и в облаке с помощью облачных служб SaaS и PaaS, таких как OneDrive, Office 365, Dynamics CRM и т. д.

Источники в службах BizTalk ограничены FTP, SFTP и подпиской на очереди или разделы служебной шины.

![](media/logic-apps-move-from-mabs/sources.png)

Каждому мосту по умолчанию назначена конечная точка HTTP, для которой настроены адрес времени выполнения и свойства относительного адреса моста. Те же возможности доступны через действие [запроса и ответа](../connectors/connectors-native-reqres.md) Logic Apps.

## <a name="xml-processing-and-bridges"></a>Обработка XML и мосты
Мост в службах BizTalk является аналогом конвейера обработки. Мост может принимать данные, полученные из соединителя, и выполнять некоторые действия с ними, а затем отправлять их в другую систему. Служба Logic Apps обеспечивает те же функции, поддерживая те же шаблоны взаимодействия на основе конвейера, что и службы BizTalk, а также предоставляя ряд других шаблонов интеграции. [Мост типа "запрос-ответ" XML](https://msdn.microsoft.com/library/azure/hh689781.aspx) в службах BizTalk называется конвейером VETER и состоит из стадий:

* (V) — проверка,
* (E) — добавление,
* (T) — преобразование,
* (E) — добавление,
* (R) — маршрутизация.

Как показано на следующем рисунке, обработка разбивается на запрос и ответ и позволяет контролировать пути запроса и ответа по отдельности (например, с помощью разных карт):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Кроме того, односторонний мост XML добавляет стадии декодирования и кодирования в начале и конце обработки, а транзитный мост содержит одну стадию добавления.

### <a name="message-processing-and-decodingencoding"></a>Обработка и декодирование/кодирование сообщений
В службах BizTalk для полученных XML-сообщений разных типов выполняется определение соответствующей схемы. Это происходит на стадии **Типы сообщений** конвейера обработки получения. Затем на стадии декодирования обнаруженный тип сообщения используется для его декодирования с помощью заданной схемы. Если схема является схемой неструктурированного файла, выполняется преобразование неструктурированного файла в XML. 

Служба Logic Apps обеспечивает аналогичные возможности. Служба получает неструктурированный файл через широкий ряд различных протоколов с помощью триггеров разных соединителей (файловая система, FTP, HTTP и т. д.) и использует действие [декодирования неструктурированного файла](../logic-apps/logic-apps-enterprise-integration-flatfile.md) для преобразования входных данных в XML. Вы можете переместить имеющиеся схемы неструктурированных файлов непосредственно в приложения логики (без необходимости вносить какие-либо изменения), а затем отправить схемы в вашу учетную запись интеграции.

### <a name="validation"></a>Проверка
После преобразования входных данных в XML (или при получении сообщения в формате XML) выполняется проверка на соответствие сообщения XSD-схеме. Для этого в службе Logic Apps используется действие [Проверка XML](../logic-apps/logic-apps-enterprise-integration-xml-validation.md). И в этом случае можно использовать те же схемы служб BizTalk без изменений.

### <a name="transform-messages"></a>Преобразование сообщений
В службах BizTalk стадия преобразования преобразует один формат XML-сообщения в другой путем применения карты с помощью модуля сопоставления на основе TRFM. В Logic Apps используется аналогичный процесс. Действие преобразования выполняет карту из учетной записи интеграции. Основным различием является формат карт — в Logic Apps используется формат XSLT. Формат XSLT включает возможность повторного использования уже имеющегося XSLT-файла, включая карты, созданные для BizTalk Server, которые содержат функтоиды. 

### <a name="routing-rules"></a>Правила маршрутизации
Службы BizTalk принимают решение о маршрутизации входящих сообщений и данных в ту или иную конечную точку или соединитель. Вы можете выбрать одну из ряда предварительно настроенных конечных точек с помощью параметра фильтра маршрутизации:

![](media/logic-apps-move-from-mabs/route-filter.png)

Служба Logic Apps предоставляет более сложные возможности логики благодаря [условию](../logic-apps/logic-apps-use-logic-app-features.md) и [переключателю](../logic-apps/logic-apps-switch-case.md), обеспечивая расширенные функции управления потоком и маршрутизации. Эффективное преобразование фильтров маршрутизации в службах BizTalk достигается с помощью **условия**, *если* существует только два варианта. Если существует больше двух вариантов, используется **переключатель**.

### <a name="enrich"></a>Добавление
На стадии добавления службы BizTalk добавляют свойства в контекст сообщения, связанный с полученными данными. Например, повышая уровень свойства, используемого для маршрутизации (см. ниже) из поиска по базе данных, или извлекая значение с помощью выражения XPath. Logic Apps предоставляет доступ ко всем выходным контекстным данным из предшествующих действий, что упрощает репликацию аналогичного поведения. Например, с помощью действия подключения SQL `Get Row` можно вернуть данные из базы данных SQL Server и использовать эти данные в действии принятия решений для маршрутизации. Аналогично, свойства входящих сообщений в очереди служебной шины, запускаемых триггером, являются адресуемыми, также как и XPath с помощью выражений языка определений рабочего процесса xpath.

### <a name="use-custom-code"></a>Использование пользовательского кода
Службы BizTalk позволяют [выполнять пользовательский код](https://msdn.microsoft.com/library/azure/dn232389.aspx), отправленный в собственные сборки. Эта возможность реализуется с помощью интерфейса [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx). Каждая стадия моста содержит два свойства (On Enter Inspector и On Exit Inspector), предоставляющие созданный вами тип .Net, который реализует этот интерфейс. Пользовательский код позволяет осуществлять более сложную обработку данных, а также повторно использовать существующий код в сборках, которые выполняют общую бизнес-логику. 

Служба Logic Apps предоставляет два основных способа выполнения пользовательского кода: Функции Azure и приложения API. Функции Azure можно создавать и вызывать из приложений логики. См. статью [Добавление и выполнение пользовательского кода для приложений логики с помощью Функций Azure](../logic-apps/logic-apps-azure-functions.md). Приложения API, часть службы приложений Azure, можно использовать для создания собственных триггеров и действий. Подробнее см. в статье, посвященной [созданию пользовательского API для Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Если у вас есть пользовательский код в сборках, вызываемый из служб BizTalk, вы можете переместить этот код в Функции Azure или создать настраиваемые API с приложениями API — в зависимости от того, что требуется реализовать. Например, если у вас есть код, служащий оболочкой для другой службы, для которой отсутствует соединитель Logic Apps, создайте приложение API и используйте действия, предоставляемые приложением API в приложении логики. При наличии вспомогательных функций или библиотек можно рекомендовать использование Функций Azure.

### <a name="edi-processing-and-trading-partner-management"></a>Управление торговыми партнерами и обработка EDI
Службы BizTalk включают обработку данных EDI и B2B с поддержкой AS2 (Applicability Statement 2), X12 и EDIFACT. Служба Logic Apps также поддерживает эти возможности. В службах BizTalk вы создаете мосты EDI и торговых партнеров/соглашения и управляете ими на выделенном портале отслеживания и управления.

В Logic Apps эта функция входит в состав [пакета интеграции Enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md). Он состоит из учетной записи интеграции и действий B2B для обработки EDI и B2B. [Учетная запись интеграции](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) используется для создания [торговых партнеров](../logic-apps/logic-apps-enterprise-integration-partners.md) и [соглашений](../logic-apps/logic-apps-enterprise-integration-agreements.md) и управления ими. Создав учетную запись интеграции, ее можно связать с одним или несколькими приложениями логики. После связывания вы можете использовать B2B-действия для доступа к информации о торговых партнерах в приложении логики. Поддерживаются следующие действия:

* Кодирование AS2
* Декодирование AS2
* Кодирование X12
* Декодирование X12
* Кодирование EDIFACT
* Декодирование EDIFACT

В отличие от служб BizTalk эти действия не связаны с транспортными протоколами. Поэтому при создании приложений логики вам доступно больше вариантов соединителей, которые используются для отправки и получения данных. Например, можно получать X12 файлы как вложения электронной почты, а затем обрабатывать эти файлы в приложении логики. 

## <a name="manage-and-monitor"></a>Управление и мониторинг
Аналогично управлению торговыми партнерами, выделенный портал служб BizTalk предоставлял возможности отслеживания для мониторинга и устранения неполадок. 

Служба Logic Apps обеспечивает более широкие возможности отслеживания и мониторинга на [портале Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) и с помощью [решения B2B Operations Management Suite](../logic-apps/logic-apps-monitor-b2b-message.md), включая мобильное приложение для отслеживания системы независимо от вашего местонахождения.

## <a name="high-availability"></a>Высокая доступность
Для достижения высокой доступности (HA) в службах BizTalk используется более одного экземпляра в данном регионе для распределения нагрузки обработки. В приложениях логики высокая доступность в регионе обеспечивается как встроенная функция (и не требует отдельной оплаты). Для аварийного восстановления за пределами региона для обработки B2B в службах BizTalk необходимо реализовать процессы резервного копирования и восстановления. В службе Logic Apps предоставляется [возможность активного/пассивного аварийного восстановления](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) в разных регионах; что позволяет выполнять синхронизацию данных B2B в учетных записях интеграции в разных регионах для обеспечения непрерывности бизнеса.

## <a name="next"></a>Далее
* Прочтите статью [Что такое приложения логики?](logic-apps-what-are-logic-apps.md)
* [Создайте свое первое приложение логики](logic-apps-create-a-logic-app.md) или приступите к работе, используя [готовый шаблон](logic-apps-use-logic-app-templates.md).  
* [Просмотрите список всех доступных соединителей](../connectors/apis-list.md), которые можно использовать в приложении логики.

