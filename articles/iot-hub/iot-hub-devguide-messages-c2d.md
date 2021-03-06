---
title: "Принципы отправки сообщений Центра Интернета вещей Azure из облака на устройство | Документы Майкрософт"
description: "Руководство разработчика — обмен сообщениями между облаком и устройством с помощью Центра Интернета вещей. Статья содержит сведения о жизненном цикле сообщения и параметрах конфигурации."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 763bc597bdfc40395511cdd9d797e5c7aaad0fdf
ms.openlocfilehash: 706c9650a8deef941f9b39956021456053369e5e
ms.contentlocale: ru-ru
ms.lasthandoff: 09/06/2017

---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Отправка сообщений, пересылаемых из облака на устройство, из Центра Интернета вещей

Для отправки односторонних уведомлений из серверной части вашего решения в приложение на устройстве используются сообщения "из облака на устройство", отправляемые из Центра Интернета вещей на ваше устройство. Обсуждение других вариантов передачи данных из облака на устройство, поддерживаемых Центром Интернета вещей, см. в [руководстве по обмену данными между облаком и устройством][lnk-c2d-guidance].

Сообщения из облака на устройство можно отправлять через конечную точку, обращенную к службе (**/messages/devicebound**). Устройства затем получают эти сообщения через конечную точку конкретного устройства (**/devices/{ИД_устройства}/messages/devicebound**).

Чтобы адресовать каждое сообщение, отправляемое из облака на устройство, отдельному устройству, Центр Интернета вещей задает для свойства **to** значение **/devices/{ИД_устройства}/messages/devicebound**.

Одна очередь устройства содержит не более 50 сообщений, отправляемых из облака на устройство. Попытка отправить на устройство большее количество сообщений приведет к ошибке.

## <a name="the-cloud-to-device-message-lifecycle"></a>Жизненный цикл сообщений, отправляемых из облака на устройство

Чтобы гарантировать как минимум однократную доставку, Центр Интернета вещей помещает сообщения, отправляемые из облака на устройство, в очередь устройства. Сами устройства должны явно подтвердить *завершение* доставки, чтобы центр IoT мог удалить сообщения из очереди. Этот способ гарантирует устойчивость к сбоям подключения и ошибкам устройств.

На следующей схеме показан жизненный цикл сообщений, отправляемых с облака в устройство, в Центре Интернета вещей.

![Жизненный цикл сообщений, отправляемых из облака на устройство][img-lifecycle]

Когда служба Центра Интернета вещей отправляет сообщение на устройство, она задает для него состояние **В очереди**. Когда устройству нужно *получить* сообщение, Центр Интернета вещей *блокирует* сообщение (задавая состояние **Невидимо**), чтобы другие потоки на том же устройстве начали получать другие сообщения. Когда поток устройства завершает обработку сообщения, он уведомляет центр IoT путем *завершения* сообщения. Затем Центр Интернета вещей задает состояние **Завершено**.

Устройство также может:

* *отклонить* сообщение, в результате чего центр IoT переводит его в состояние **Deadlettered** (Не доставлено); Устройства, подключаемые по протоколу MQTT, не могут отклонять сообщения, передаваемые из облака на устройство.
* *прервать* сообщение, в результате чего центр IoT помещает его в очередь с состоянием **Enqueued**(Поставлено в очередь).

При обработке сообщения потоком может произойти ошибка без уведомления центра IoT. В этом случае состояние сообщения автоматически меняется с **Невидимо** на **Enqueued** (Поставлено в очередь) после *истечения срока видимости (или блокировки)*. Значение по умолчанию — одна минута.

Свойство **Максимальное число доставок** Центра Интернета вещей определяет, сколько раз состояние сообщения может изменяться с **Enqueued** (Поставлено в очередь) на **Невидимо**. После выполнения такого количества изменений Центр Интернета вещей устанавливает для сообщения состояние **Deadlettered** (Не доставлено). Таким же образом Центр Интернета вещей устанавливает для сообщения состояние **Deadlettered** (Не доставлено) после истечения срока действия сообщения (см. раздел [Срок действия сообщения (срок жизни)][lnk-ttl]).

В руководстве [Отправка сообщений из облака на устройство с помощью Центра Интернета вещей][lnk-c2d-tutorial] демонстрируется отправка сообщений "из облака на устройство" из облака и их получение на устройстве.

Обычно, если потеря сообщения, отправляемого с облака в устройство, никак не влияет на логику приложения, это сообщение завершается устройством. Например, если устройство сохраняет содержимое сообщения локально или успешно выполняет операцию. Сообщение может также содержать временные сведения, потеря которых не влияет на функциональность приложения. Иногда при работе с длительно выполняемыми задачами можно сделать следующее:

* Завершить сообщение, отправляемое с облака в устройство, после того, как описание задачи сохранится в локальном хранилище.
* Отправить одно или несколько сообщений (с устройства в облако) в серверную часть решения на различных этапах выполнения задачи.

## <a name="message-expiration-time-to-live"></a>Срок действия сообщения (срок жизни)

Каждое сообщение из облака на устройство имеет срок действия. Его может задать:

* свойство **ExpiryTimeUtc** в службе;
* Центр Интернета вещей с использованием *срока жизни* по умолчанию, указанного в соответствующем свойстве Центра Интернета вещей.

Ознакомьтесь с разделом [Параметры конфигурации сообщений, отправляемых из облака на устройство][lnk-c2d-configuration].

Распространенный способ воспользоваться сроком действия, чтобы сообщения не отправлялись на отключенные устройства — задать короткий срок жизни. Этот подход позволяет добиться того же результата, что и поддержание состояния подключения устройства, но при этом обладает большей эффективностью. При запросе подтверждения сообщений Центр Интернета вещей сообщит, какие устройства:

* могут получать сообщения;
* находятся в автономном режиме или в состоянии сбоя.

## <a name="message-feedback"></a>Отзыв на сообщение

При отправке сообщений с облака в устройство служба может запросить отзыв на каждое сообщение, уведомляющий о его конечном состоянии.

| Свойство Ack | Поведение |
| ------------ | -------- |
| **positive** | Если сообщение, отправляемое из облака на устройство, достигает состояния **Completed** (Завершено), Центр Интернета вещей создает отзыв на сообщение. |
| **negative** | Если сообщение, отправляемое из облака на устройство, достигает состояния **Deadlettered** (Не доставлено), Центр Интернета вещей создает отзыв на сообщение. |
| **full**     | Центр Интернета вещей создает отзыв на сообщение в любом случае. |

Если свойство **Ack** имеет значение **full** и отзыв не получен, то это означает, что срок действия отзыва истек. Служба не знает, что случилось с исходным сообщением. На практике служба должна убедиться, что может обработать отзыв до истечения срока его действия. Максимальный срок действия составляет два дня, и этого времени достаточно для повторного запуска службы при сбое.

Как описано в статье [Endpoints][lnk-endpoints] (Конечные точки), Центр Интернета вещей предоставляет отзывы в виде сообщений через конечную точку, доступную для службы (**/messages/servicebound/feedback**). Семантика получения отзыва идентична семантике, используемой для сообщений, отправляемых из облака на устройство, с тем же [жизненным циклом сообщения][lnk-lifecycle]. По возможности отзывы на сообщения группируются в одно сообщение, имеющее приведенный ниже формат:

| Свойство     | Описание |
| ------------ | ----------- |
| EnqueuedTime | Метка времени, указывающая, когда было создано сообщение. |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

Основная часть — это сериализованный массив записей JSON, каждая из которых имеет следующие свойства:

| Свойство           | Описание |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Метка времени, указывающая, когда отобразился результат сообщения. Например, устройство завершило его или истек срок его действия. |
| OriginalMessageId  | Идентификатор **MessageId** сообщения, которое отправляется из облака на устройство и к которому относятся эти сведения отзыва. |
| StatusCode         | Обязательная строка. Используется в отзывах, созданных центром IoT. <br/> 'Success' <br/> 'Expired' <br/> 'DeliveryCountExceeded' <br/> 'Rejected' <br/> 'Purged' |
| Описание        | Строковые значения **StatusCode**. |
| deviceId           | Идентификатор **DeviceId** целевого устройства, которому отправляется сообщение из облака и к которому относится этот отзыв. |
| DeviceGenerationId | Идентификатор **DeviceGenerationId** целевого устройства, которому отправляется сообщение из облака и к которому относится этот отзыв. |

Служба должна указать идентификатор **MessageId** для сообщения, отправляемого из облака на устройство, чтобы соотнести свой отзыв с исходным сообщением.

Ниже приведен пример текста сообщения отзыва.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Параметры конфигурации сообщений, отправляемых из облака на устройство

Каждый Центр Интернета вещей предоставляет следующие параметры конфигурации для отправки сообщений из облака на устройство.

| Свойство                  | Описание | Диапазон и значение по умолчанию |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Заданный по умолчанию срок жизни для сообщений, отправляемых из облака на устройство. | Интервал ISO_8601 — до 2 устройств (минимум 1 минута). Значение по умолчанию — 1 час. |
| maxDeliveryCount          | Лимит доставки для очереди доставки сообщений, отправляемых из облака на устройство, для каждого отдельного устройства. | От 1 до 100. Значение по умолчанию — 10. |
| feedback.ttlAsIso8601     | Хранение отзывов, направленных службе. | Интервал ISO_8601 — до 2 устройств (минимум 1 минута). Значение по умолчанию — 1 час. |
| feedback.maxDeliveryCount |Лимит доставок для очереди отзывов. | От 1 до 100. Значение по умолчанию — 100. |

Дополнительные сведения о настройке этих параметров конфигурации см. в статье [Создание Центра Интернета вещей][lnk-portal].

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о пакетах SDK, которые можно использовать для приема сообщений, отправляемых с устройства в облако, см. в статье, посвященной [пакетам SDK для Azure IoT][lnk-sdks].

Инструкции по настройке получения сообщений, передаваемых из облака на устройство, см. в статье, посвященной [отправке сообщений из облака на устройство][lnk-c2d-tutorial].

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md

