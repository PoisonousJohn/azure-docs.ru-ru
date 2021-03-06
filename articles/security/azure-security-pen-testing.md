---
title: "Тестирование на проникновение | Документация Майкрософт"
description: "В этой статье представлен обзор тестирования на проникновение и способах его проведения для ваших приложений, которые работают в инфраструктуре Azure."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 070e848f753452953b9e5dfe94799e7c0a314530
ms.contentlocale: ru-ru
ms.lasthandoff: 08/25/2017

---
# <a name="pen-testing"></a>Тестирование на проникновение
Одним из основных преимуществ платформы Microsoft Azure при развертывании и тестировании приложений является тот факт, что вам не нужно собирать локальную инфраструктуру для разработки, тестирования и развертывания приложений. За инфраструктуру целиком отвечают службы платформы Microsoft Azure. Вам не нужно беспокоиться о подаче заявок, приобретении и настройке собственного локального оборудования.

Это прекрасно, но вам по-прежнему нужно самостоятельно проводить стандартную экспертизу безопасности. Одной из таких задач является тестирование на проникновение (тестирование на защищенность от несанкционированного доступа), которое выполняется для приложений, развертываемых в Azure.

Возможно, вы уже знаете, что корпорация Майкрософт выполняет [тестирование среды Azure на проникновение](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Это помогает нам улучшать платформу и принимать меры по усилению контроля безопасности, внедрять новые элементы обеспечения безопасности и повышать эффективность процессов обеспечения безопасности.

Мы не тестируем ваши приложения на проникновение, однако понимаем, что вам потребуется такая проверка. Это очень важно, так как, повышая защищенность своих приложений, вы вносите свой вклад в повышение уровня безопасности всей экосистемы Azure.

Когда вы будете тестировать свои приложения на проникновение, мы можем воспринять это как атаку на систему. Мы [постоянно отслеживаем](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) различные виды атак и можем запустить процесс реагирования на инцидент, если возникнет такая необходимость. Запуск реагирования на инцидент при тестировании приложения на проникновение не нужен ни вам, ни нам.

Что делать?

Когда вы будете готовы выполнить для своих приложений, размещенных в Azure, тестирование на проникновение, [сообщите нам](https://portal.msrc.microsoft.com/en-us/engage/pentest). Поставив нас в известность о проведении определенных видов тестирования, вы предотвратите возможность случайной блокировки (например, блокировки IP-адреса, используемого при тестировании). При этом сами тесты должны соответствовать условиям Azure по проведению тестирования на проникновение. См. [унифицированные правила вмешательства Microsoft Cloud для тестирования на проникновение](https://technet.microsoft.com/en-us/mt784683).
Стандартные тесты, которые вы можете проводить:

* тестирование конечных точек для выявления [основных 10 уязвимостей в рамках проекта Open Web Application Security Project (открытый проект обеспечения безопасности веб-приложений, OWASP)](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* [нечеткое тестирование](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) конечных точек;
* [сканирования портов](https://en.wikipedia.org/wiki/Port_scanner) конечных точек;

К тестам, которые запрещено выполнять, относятся атаки типа ["отказ в обслуживании" (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . В эту категорию включены как запуск DoS-атаки, так и все сопутствующие тесты, которые могут определять, демонстрировать или имитировать DoS-атаку любого типа.

Готовы начать тестирование приложений, размещенных в Microsoft Azure, на проникновение? Если да, перейдите на страницу [Penetration testing overview](https://technet.microsoft.com/library/mt784683.aspx) (Обзор тестирования на проникновение). В нижней части страницы нажмите кнопку "Create a Testing Request" (Создать запрос на тестирование). Кроме того, вы найдете дополнительные сведения об условиях тестировании на проникновение, а также полезные ссылки, с помощью которых можно сообщать об уязвимостях, связанных с платформой Azure или другими службами Microsoft.

