---
title: "Обзор службы \"Сетка событий Azure\""
description: "В статье описывается служба \"Сетка событий Azure\" и связанные с ней основные понятия."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/18/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 59a834f32793e349d5639baf3c80dbcba274dfa8
ms.contentlocale: ru-ru
ms.lasthandoff: 08/19/2017

---

# <a name="an-introduction-to-azure-event-grid"></a>Общие сведения о службе "Сетка событий Azure"

Служба "Сетка событий Azure" позволяет легко создавать приложения с архитектурой на основе событий. Достаточно выбрать ресурс Azure, на которой вы хотите подписаться, и указать обработчик событий или конечную точку веб-перехватчика для отправки события. Служба "Сетка событий" обеспечивает встроенную поддержку событий, поступающих из таких служб Azure, как хранилища BLOB-объектов и группы ресурсов. Кроме того, служба предоставляет настраиваемую поддержку для приложения и сторонних событий, используя настраиваемые разделы и веб-перехватчики. 

Вы можете применять фильтры для маршрутизации определенных событий в различные конечные точки, использовать многоадресную рассылку в несколько конечных точек, а также проверять надежность доставки. Служба "Сетка событий" обеспечивает встроенную поддержку для пользовательских и сторонних событий.

Предварительная версия службы "Сетка событий Azure" поддерживает расположения **westus2** и **westcentralus**. Будут добавлены другие регионы.

В статье представлен обзор службы "Сетка событий Azure". Чтобы приступить к использованию службы, см. раздел [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md).

![Функциональная модель сетки событий](./media/overview/event-grid-functional-model.png)

Сейчас хранилище BLOB-объектов Azure не является общедоступным в качестве издателя.

## <a name="concepts"></a>Основные понятия

Есть пять основных понятий, с которыми нужно ознакомиться перед началом работы со службой "Сетка событий Azure":

* **События** — это то, что произошло.
* **Источники или издатели событий** — это расположение, в котором произошло событие.
* **Разделы** — это конечная точка, в которую издатель отправляет событие.
* **Подписки на события** — это конечная точка или встроенный механизм для маршрутизации события, иногда в несколько обработчиков. Кроме того, подписки используются обработчиками для интеллектуальной фильтрации входящих событий.
* **Обработчики событий** — это приложения или службы, реагирующие на события.

Дополнительные сведения об этих понятиях см. в разделе [Concepts in Azure Event Grid](concepts.md) (Основные понятия службы "Сетка событий Azure").

## <a name="capabilities"></a>Возможности

Вот несколько ключевых преимуществ службы "Сетка событий Azure":

* **Простота** — интерактивная поддержка целевых событий в ресурсе Azure для любого обработчика событий или конечной точки.
* **Расширенная фильтрация** — фильтрация по типам или пути публикации событий, гарантирующая, что обработчики получают только релевантные события.
* **Размноженная отправка** — подписывайтесь на несколько конечных точек для одного и того же события, чтобы копии событий отправлялись в любое требуемое число расположений.
* **Надежность** — выполнение повторных попыток в течение 24 часов с экспоненциальным откладыванием гарантирует доставку событий.
* **Оплата за событие** — вы платите только за те ресурсы службы "Сетка событий", которые используете.
* **Высокая пропускная способность** — в службе "Сетка событий" можно создавать высокие рабочие нагрузки с поддержкой миллионов событий в секунду.
* **Встроенные события** — быстрое возобновление работы благодаря встроенным событиям с определяемыми ресурсами.
* **Пользовательские события** — используйте маршрутизацию и фильтр службы "Сетка событий", чтобы обеспечить надежную доставку пользовательских событий в приложение.

## <a name="built-in-publisher-and-handler-integration"></a>Встроенная интеграция издателя и обработчика

Azure обеспечивает встроенную поддержку событий, а также их издателей и обработчиков с помощью многочисленных служб.

### <a name="publishers"></a>Издатели

Сейчас встроенная поддержка издателей для службы "Сетка событий" добавлена в следующие службы Azure:

* Группы ресурсов (операции управления)
* Подписки Azure (операции управления)
* Концентраторы событий
* Пользовательские разделы

Другие службы Azure будут добавлены в этом году.

### <a name="handlers"></a>Обработчики

Сейчас встроенная поддержка обработчиков для службы "Сетка событий" добавлена в следующие службы Azure: 

* Функции Azure
* Logic Apps
* Служба автоматизации Azure
* Веб-перехватчики

Другие службы Azure будут добавлены в этом году.

## <a name="what-can-i-do-with-event-grid"></a>Что можно сделать с помощью службы "Сетка событий"?

Служба "Сетка событий Azure" включает несколько функций, которые значительно расширяют возможности бессерверных операций, автоматизации операций и интеграции. 

### <a name="serverless-application-architectures"></a>Архитектура бессерверных приложений

![Бессерверное приложение](./media/overview/serverless_web_app.png)

Служба "Сетка событий Azure" связывает источники данных и обработчики событий. Например, служба "Сетка событий Azure" может активировать функции для бессерверной обработки данных, запуская анализ изображений при каждом добавлении новой фотографии в контейнер больших двоичных объектов. 

### <a name="ops-automation"></a>Автоматизация операций

![Автоматизация операций](./media/overview/Ops_automation.png)

Служба "Сетка событий Azure" ускоряет автоматизацию процессов и упрощает применение политик. Например, служба "Сетка событий" может уведомлять службу автоматизации Azure о создании виртуальных машин или базы данных SQL. Эти события можно использовать для автоматической проверки соответствия конфигураций служб, отправки метаданных в рабочие средства, присвоения тегов виртуальным машинам или регистрации рабочих элементов.

### <a name="application-integration"></a>Интеграция приложений

![Интеграция приложений](./media/overview/app_integration.png)

Служба "Сетка событий Azure" связывает приложения с другими службами. Например, вы можете создавать пользовательские разделы для отправки событий приложения в службу "Сетка событий", используя преимущества надежной доставки, расширенных возможностей маршрутизации и непосредственной интеграции с Azure. Кроме того, службу "Сетка событий Azure" можно использовать с Logic Apps для обработки данных в любой точке мира без необходимости писать код. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Чем служба "Сетка событий" отличается от других служб интеграции Azure?

Служба "Сетка событий Azure" — это объединяющее решение для обработки событий, обеспечивающее реактивное программирование на основе событий. Она тесно интегрирована со службами Azure и может интегрироваться со сторонними службами. Сообщение о событии содержит сведения, необходимые для реагирования на изменения в службах и приложениях. Служба "Сетка событий" — это не конвейер данных, она не выполняет доставку фактического объекта, который был обновлен.

Идеальным вариантом для стандартных корпоративных приложений, которые требуют выполнения транзакций, упорядочивания, поиска повторяющихся сообщений и мгновенного обеспечения согласованности, является служебная шина. Служба "Сетка событий" предназначена для использования с реактивной моделью, обеспечивая высокую скорость, масштабируемость, экономичность и широкий диапазон применения. Она хорошо подходит для бессерверной архитектуры.

Служба "Сетка событий" дополняет другие службы Azure, такие как Logic Apps и концентраторы событий. Чтобы начать рабочий процесс, служба запускает приложение логики. Концентраторы событий работают со службой "Сетка событий", позволяя реагировать на события, зафиксированные функцией записи концентраторов событий, а также создавать входящий трафик данных и конвейеры преобразования.

## <a name="how-much-does-event-grid-cost"></a>Сколько стоит служба "Сетка событий"?

Для службы "Сетка событий Azure" применяется модель оплаты за событие, поэтому счет выставляются только за те ресурсы, которые вы используете.

Тариф составляет 0,60 долл. США за миллион операций (0,30 долл. США для предварительной версии). За первые 100 000 операций в месяц плата не взимается. Операциями считаются прием событий, расширенное сопоставление, попытки доставки и вызовы управления.  Дополнительные сведения см. на странице [с ценами](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Дальнейшие действия

* [Создание пользовательских событий и подписка на них](custom-event-quickstart.md) — перейдите сразу к делу. Отправляйте пользовательские события в любую конечную точку, воспользовавшись инструкциями в кратком руководстве по службе "Сетка событий Azure".
* [Использование Logic Apps в качестве обработчика событий](monitor-virtual-machine-changes-event-grid-logic-app.md) — ознакомьтесь с руководством по созданию приложения с использованием Logic Apps для реагирования на события, отправляемые службой "Сетка событий".
* [Справочник по REST API службы "Сетка событий Azure"](/rest/api/eventgrid)  
  Дополнительные технические сведения о службе "Сетка событий Azure" и руководство по управлению подписками на события, маршрутизацией и фильтрацией.
