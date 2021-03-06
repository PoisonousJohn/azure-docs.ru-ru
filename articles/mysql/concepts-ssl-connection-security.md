---
title: "SSL-соединения в базе данных Azure для MySQL | Документация Майкрософт"
description: "Сведения о настройке базы данных Azure для MySQL и связанных приложений для правильного использования SSL-соединений."
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 6dc147c9dd3983c2c599108162fa285d87ffc7a3
ms.contentlocale: ru-ru
ms.lasthandoff: 05/10/2017

---

# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-соединения в базе данных Azure для MySQL
База данных Azure для MySQL поддерживает подключение сервера базы данных к клиентским приложениям с помощью SSL (Secure Sockets Layer). Применение SSL-соединений между сервером базы данных и клиентскими приложениями обеспечивает защиту от атак "злоумышленник в середине" за счет шифрования потока данных между сервером и приложением.

## <a name="default-settings"></a>Параметры по умолчанию
По умолчанию в службе базы данных должно быть настроено обязательное использование SSL-соединений при подключении к MySQL.  Рекомендуется не отключать параметр SSL без необходимости. 

При подготовке нового сервера базы данных Azure для MySQL с помощью портала Azure и интерфейса командной строки применение SSL-соединений включается по умолчанию. 

Аналогичным образом предопределенные строки подключения в разделе "Строки подключения" в настройках сервера на портале Azure содержат необходимые параметры для распространенных языков, что позволяет подключиться к серверу базы данных с помощью SSL. Значение параметра SSL зависит от соединителя. Например, может использоваться "ssl=true", "sslmode=require", "sslmode=required" или другой вариант.

Чтобы узнать, как включить или отключить SSL-соединение при разработке приложения, ознакомьтесь со [статьей, посвященной настройке SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Дальнейшие действия
[Библиотеки подключений для базы данных Azure для MySQL](concepts-connection-libraries.md)

