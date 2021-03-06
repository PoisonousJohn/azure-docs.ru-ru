---
title: "Общие сведения об обмене сообщениями в Центре Интернета вещей Azure | Документация Майкрософт"
description: "Руководство разработчика для Центра Интернета вещей. Обмен сообщениями между устройством и облаком. Содержит сведения о форматах сообщений и поддерживаемых протоколах связи."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: f54398d7ac46bf178d2bb603669b399d25370736
ms.contentlocale: ru-ru
ms.lasthandoff: 05/31/2017


---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Обмен сообщениями между устройством и облаком с помощью Центра Интернета вещей.

Сообщения Центра Интернета вещей служат для связи с устройством путем:

* отправки сообщений [с устройства в облако][lnk-d2c] с ваших устройств в серверную часть решения;
* отправки сообщений [из облака на устройство][lnk-c2d] из серверной части решения на ваши устройства.

Основные свойства функции обмена сообщениями в центре IoT — надежность и устойчивость сообщений. Эти свойства сохраняются, если на стороне устройства прерывается подключение и если наступает момент пиковой загрузки на стороне облака при обработке событий. Центр IoT гарантирует *как минимум однократную* доставку при двухстороннем обмене сообщениями между устройством и облаком.

Общие сведения о возможностях Центра Интернета вещей см. в статьях [Azure и Интернет вещей][lnk-azure-iot] и [Общие сведения о службе Центра Интернета вещей Azure][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Для чего используются сообщения Центра Интернета вещей

Для отправки телеметрии и оповещений с учетом временных рядов из приложения для устройства используйте сообщения, отправляемые с устройства в облако, а для отправки односторонних уведомлений в приложение для устройства — сообщения, отправляемые из облака на устройство.

* Дополнительные сведения см. в [руководстве по обмену данными между устройством и облаком][lnk-d2c-guidance], которое поможет вам выбрать оптимальный вариант: сообщения, отправляемые с устройства в облако, переданные свойства или отправку файла.
* Чтобы принять решение об использовании сообщений, отправляемых из облака на устройство, требуемых свойств или прямых методов см. сведения в [руководстве по обмену данными между облаком и устройством][lnk-c2d-guidance].

## <a name="next-steps"></a>Дальнейшие действия

* Дополнительные сведения о [сообщениях, передаваемых с устройства в облако][lnk-d2c], Центра Интернета вещей.
* Дополнительные сведения о [сообщениях, передаваемых из облака на устройство][lnk-c2d], Центра Интернета вещей.

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
