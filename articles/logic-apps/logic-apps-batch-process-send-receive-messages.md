---
title: "Пакетная обработка сообщений как группы или коллекции в Azure Logic Apps | Документация Майкрософт"
description: "Отправка и получение сообщений для пакетной обработки в приложениях логики"
keywords: "пакет, пакетная обработка"
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 480ffce5dbe7c25181bb0ba5639de884e98ff4e6
ms.contentlocale: ru-ru
ms.lasthandoff: 08/10/2017

---

# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Отправка, получение и пакетная обработка сообщений в приложениях логики

Для обработки сообщений группами можно отправить элементы данных (или сообщения) в *пакет*, а затем обработать эти элементы как пакет. Этот подход полезен, когда необходимо быть уверенным, что элементы данных сгруппированы определенным образом и обрабатываются вместе. 

Можно создать приложения логики, которые получают элементы как пакет, используя триггер **Пакет**. Затем можно создать приложения логики, которые отправляют элементы в пакет, используя действие **Пакет**.

В этом разделе показано, как создать решение пакетной обработки, выполнив следующие задачи: 

* [Создание приложения логики, которое получает и собирает элементы как пакет](#batch-receiver). Это приложение логики для "получения пакета" указывает имя пакета и условия отправки пакета, которые должны выполняться, прежде чем получающее приложение отправит и обработает элементы. 

* [Создание приложения логики, которое отправляет элементы в пакет](#batch-sender). Это приложение логики для "отправки пакета" указывает, куда отправлять элементы. Точкой назначения должно быть существующее приложение логики для получения пакета. Вы также можете указать уникальный ключ, например номер клиента, чтобы "секционировать" (разделить) целевой пакет на подмножества на основе этого ключа. Таким образом все элементы с этим ключом будут собираться и обрабатываться вместе. 

## <a name="requirements"></a>Требования

Для выполнения этого примера необходимы следующие компоненты:

* Подписка Azure. Если у вас нет подписки, вы можете [создать бесплатную пробную версию учетной записи Azure](https://azure.microsoft.com/free/). В противном случае вы можете [зарегистрироваться на получение подписки с оплатой по мере использования](https://azure.microsoft.com/pricing/purchase-options/).

* Базовые знания [создания приложений логики](../logic-apps/logic-apps-create-a-logic-app.md). 

* Учетная запись электронной почты любого [поставщика электронной почты, поддерживаемого Azure Logic Apps](../connectors/apis-list.md).

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Создание приложения логики, которое получает сообщения как пакет

Прежде чем отправлять сообщения в пакет, необходимо сначала создать приложение логики для "получения пакета" с помощью триггера **Пакет**. Тогда вы сможете выбрать это получающее приложение логики при создании отправляющего приложения. В настройках получения укажите имя пакета, условия его отправки и другие параметры. 

Отправляющее приложение логики должно знать, куда отправлять элементы, в то время как получающее приложение логики не нуждается в сведениях об отправителях.

1. На [портале Azure](https://portal.azure.com) создайте приложение логики с именем BatchReceiver. 

2. В конструкторе Logic Apps добавьте триггер **Пакет**, который запускает рабочий процесс приложения логики. В поле поиска введите слово "пакет" в качестве фильтра. Выберите триггер **Пакет — Пакетные сообщения**.

   ![Добавление триггера "Пакет"](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Задайте имя пакета и укажите критерии для его отправки, например:

   * **Имя пакета**: имя, используемое для идентификации пакета. В данном примере — TestBatch.
   * **Число сообщений**: количество сообщений, которое хранится как пакет, а затем отправляется в обработку. В данном примере — 5.

   ![Ввод сведений для триггера "Пакет"](./media/logic-apps-batch-process-send-receive-messages/receive-batch-trigger-details.png)

4. Добавьте другое действие, которое отправляет сообщение электронной почты, когда срабатывает триггер "Пакет". Каждый раз, когда в пакете собирается пять элементов, приложение логики отправляет сообщение.

   1. Под триггером "Пакет" нажмите кнопку **+ Новый шаг** > **Добавить действие**.

   2. В поле поиска введите слово "почта" в качестве фильтра.
   Выберите соединитель электронной почты в зависимости от своего поставщика электронной почты.
   
      Например, если у вас рабочая или учебная учетная запись, то выберите соединитель Office 365 Outlook. 
      Если у вас учетная запись Gmail, то выберите соединитель Gmail.

   3. Выберите это действие для соединителя: **{*поставщик электронной почты*} - Отправить электронное письмо**

      Например:

      ![Выбор действия "Отправить электронное письмо" для поставщика электронной почты](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Если вы еще не создали подключение для поставщика электронной почты, то укажите учетные данные электронной почты для аутентификации при появлении соответствующего запроса. Дополнительные сведения об аутентификации учетных данных электронной почты см. в [этой статье](../logic-apps/logic-apps-create-a-logic-app.md).

6. Задайте свойства для только что добавленного действия.

   * В поле **Кому** введите адрес электронной почты получателя. 
   Для тестировании можете использовать свой собственный адрес.

   * В поле **Тема**, когда отобразится список **Динамическое содержимое**, выберите **Имя секции**.

     ![Выбор значения "Имя секции" в списке "Динамическое содержимое"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     В следующем разделе можно указать уникальный ключ секции, который разделит целевой пакет на логические наборы, в которые затем можно отправлять сообщения. 
     Каждый набор имеет уникальный номер, который создается отправляющим приложением логики. 
     Эта функция позволяет использовать один пакет с несколькими подмножествами и определить каждое подмножество с помощью задаваемого имени.

   * В поле **Текст**, когда отобразится список **Динамическое содержимое**, выберите **Идентификатор сообщения**.

     ![Выбор значения "Идентификатор сообщения" для поля "Текст"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Так как входные данные для действия **Отправить электронное письмо** являются массивом, конструктор автоматически добавляет для этого действия цикл **For each** (Для каждого). 
     Этот цикл выполняет действие в отношении каждого элемента внутри пакета. 
     То есть если в триггере "Пакет" задано пять элементов, то вы получите пять сообщений при каждом срабатывании триггера.

7.  Когда приложение логики для получения пакета создано, сохраните это приложение.

    ![Сохранение приложения логики](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a>Создание приложения логики, которое отправляет сообщения в пакет

Теперь создайте одно или несколько приложений логики, которые отправляют элементы в пакет, заданный получающим приложением логики. В настройках отправки укажите получающее приложение логики, имя пакета, содержимое сообщения и другие параметры. При необходимости можно указать уникальный ключ секции, чтобы разделить пакет на подмножества для сбора элементов с помощью этого ключа.

Отправляющее приложение логики должно знать, куда отправлять элементы, в то время как получающее приложение логики не нуждается в сведениях об отправителях.

1. Создайте второе приложение логики с именем BatchSender.

   1. В поле поиска введите слово "повторение" в качестве фильтра. 
   Выберите триггер **Расписание — повторение**.

      ![Добавление триггера "Расписание — повторение"](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Задайте частоту и интервал, чтобы отправляющее приложение логики запускалось каждую минуту.

      ![Настройка частоты и интервала триггера повторения](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Добавьте новый шаг для отправки сообщений в пакет.

   1. Под триггером "Повторение" нажмите кнопку **+ Новый шаг** > **Добавить действие**.

   2. В поле поиска введите слово "пакет" в качестве фильтра. 

   3. Выберите действие **Отправить сообщения в пакет — Выберите рабочий процесс Logic Apps с триггером пакета**.

      ![Выбор действия "Отправить сообщения в пакет"](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Теперь выберите приложение логики BatchReceiver, которое вы создали ранее и которое теперь доступно как действие.

      ![Выбор приложения логики для "получения пакета"](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > В этом списке также отображаются все прочие приложения логики, которые содержат триггеры пакетной обработки.

3. Задайте свойства пакета.

   * **Имя пакета**: имя пакета, определяемое принимающим приложением логики. В данном примере — TestBatch. Это имя проверяется во время выполнения.

     > [!IMPORTANT]
     > Убедитесь, что имя пакета не было изменено. Оно должно совпадать с именем пакета, указанным в принимающем приложении логики.
     > Изменение имени пакета приводит к сбою в работе отправляющего приложения логики.

   * **Содержимое сообщения**: текст отправляемого сообщения. 
   В данном примере добавьте следующее выражение, которое вставляет текущую дату и время в содержимое сообщения, отправляемого в пакет:

     1. Когда отобразится список **Динамическое содержимое**, выберите **Выражение**. 
     2. Введите выражение **utcnow()** и нажмите кнопку **ОК**. 

        ![В поле "Содержимое сообщения" выберите "Выражение". Введите utcnow().](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Теперь настройте для пакета секцию. В действии BatchReceiver выберите **Показать расширенные параметры**.

   * **Имя секции**: здесь можно указать необязательный уникальный ключ секции для разделения целевого пакета. Например, добавьте выражение, которое генерирует случайное число от 1 до 5.
   
     1. Когда отобразится список **Динамическое содержимое**, выберите **Выражение**.
     2. Введите следующее выражение: **rand(1,6)**

        ![Настройка секции для целевого пакета](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Эта функция **rand** генерирует число от 1 до 5. 
        То есть пакет разделяется на пять нумерованных секций, которые динамически задаются этим выражением.

   * **Идентификатор сообщения**: необязательный идентификатор сообщения. Если это поле оставить пустым, то генерируется идентификатор GUID. 
   Для данного примера оставьте это поле пустым.

5. Сохраните приложение логики. Теперь ваше отправляющее приложение логики выглядит примерно так:

   ![Сохранение отправляющего приложения логики](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Тестирование приложений логики

Чтобы проверить решение пакетной обработки, запустите приложения логики и дайте им поработать несколько минут. Скоро вы начнете получать сообщения электронной почты группами по пять сообщений. Все они будут иметь один ключ секции.

Приложение логики BatchSender запускается каждую минуту, генерирует случайное число в диапазоне от 1 до 5 и использует это число в качестве ключа секции для целевого пакета, в который отправляются сообщения. Каждый раз пакет содержит пять элементов с одинаковым ключом секции. Приложение логики BatchReceiver срабатывает и отправляет сообщение для каждого элемента.

> [!IMPORTANT]
> После выполнения проверки убедитесь, что приложение логики BatchSender отключено, чтобы оно прекратило отправлять сообщения и не перегружало папку "Входящие".

## <a name="next-steps"></a>Дальнейшие действия

* [Создание определений рабочих процессов для приложений логики с помощью JSON](../logic-apps/logic-apps-author-definitions.md)
* [Создание бессерверного приложения в Visual Studio с использованием приложений логики и функций](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Сценарий обработки исключений и ведения журнала ошибок для приложений логики](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

