---
title: "Знакомство с Azure Cosmos DB | Документация Майкрософт"
description: "Узнайте об Azure Cosmos DB. Эта глобально распределенная многомодельная база данных с низкой задержкой, гибкой масштабируемостью и высоким уровнем доступности."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: c9d04ae0bc11b99f893e5f003f136fbfe0dfccc9
ms.contentlocale: ru-ru
ms.lasthandoff: 08/11/2017

---

# <a name="welcome-to-azure-cosmos-db"></a>Добро пожаловать в базу данных Azure Cosmos DB

Azure Cosmos DB — это глобально распределенная многомодельная база данных Майкрософт. Azure Cosmos DB позволяет гибко и независимо масштабировать пропускную способность и ресурсы хранилища в любых регионах Azure, стоит лишь нажать кнопку. Она гарантирует пропускную способность, задержку, доступность и согласованность в соответствии с [соглашениями об уровне обслуживания](https://aka.ms/acdbsla) (SLA), которые зачастую не могут предложить другие службы баз данных.

![Cosmos DB — это глобально распределенная служба базы данных Майкрософт с гибкой масштабируемостью, гарантированно низкой задержкой, пятью моделями согласованности и универсальными соглашениями об уровне обслуживания.](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Преимущества использования Azure Cosmos DB с некоторыми решениями

Использование Azure Cosmos DB [гарантированно](https://azure.microsoft.com/support/legal/sla/cosmos-db/) обеспечит доступность, высокую пропускную способность, низкую задержку и настраиваемые уровни согласованности при использовании с любыми [веб-приложениями, мобильными и игровыми приложениями, а также приложениями Интернета вещей](use-cases.md), которым необходимо обрабатывать большое число операций чтения и записи разнообразных данных в [глобальном](distribute-data-globally.md) масштабе и с малым временем отклика.

## <a name="key-capabilities"></a>Ключевые возможности
В качестве глобально распределенной службы базы данных Azure Cosmos DB предоставляет следующие возможности для создания быстродействующих и масштабируемых приложений:

* **Интегрированные возможности глобального распределения**.
    * Вы можете [распределять данные](distribute-data-globally.md) в любом числе [регионов Azure](https://azure.microsoft.com/regions/) [одним щелчком мыши](tutorial-global-distribution-documentdb.md). Теперь вы можете размещать данные в непосредственной близости от пользователей, тем самым минимизируя возможные задержки. 
    * Благодаря API-интерфейсам Azure Cosmos DB со множественной адресацией приложение получает данные о расположении ближайшего региона, что позволяет ему отправлять запросы в ближайший центр обработки данных. И все это возможно без необходимости изменять конфигурацию. Вы просто определяете регион для записи и сколько угодно регионов для чтения, — а об остальном позаботится служба.

* **Разные модели данных и популярные API-интерфейсы для доступа к данным и создания запросов к ним**.
    * Azure Cosmos DB разработана по принципу модели данных на основе ARS. В нее встроена поддержка нескольких моделей данных, включая модели данных документов, пар "ключ-значение", графов, таблиц и столбцов.
    * API-интерфейсы поддерживаются пакетами SDK, доступными на нескольких языках, для следующих моделей данных:
        * [API DocumentDB](documentdb-introduction.md);
        * [API MongoDB](mongodb-introduction.md);
        * [API таблицы](table-introduction.md);
        * [API Graph (Gremlin)](graph-introduction.md);
        * в ближайшее время будет реализована поддержка для дополнительных моделей данных. 

* **Гибкое масштабирование пропускной способности и хранилища по требованию по всему миру**.
    * Легко масштабируйте пропускную способность баз данных с [посекундной](request-units.md) степенью детализации и изменяйте ее при необходимости. 
    * Масштабируйте объем хранилища [автоматически и прозрачно](partition-data.md), чтобы раз и навсегда удовлетворить соответствующие требования.

* **Создание высокопроизводительных и критически важных приложений**.
    * Azure Cosmos DB гарантирует пользователям низкую сквозную задержку на уровне 99-го процентиля. 
    * Для обычного элемента размером 1 КБ Cosmos DB обеспечивает сквозную задержку операций чтения менее 10 мс, а для индексированных операций записи — менее 15 мс на уровне 99-го процентиля в рамках одного и того же региона Azure. Среднее значение задержки значительно ниже (менее 5 мс).

* **Гарантированная доступность**.
    * Доступность на уровне 99,99 % в рамках одного региона.
    * Выполняйте развертывание в любом количестве [регионов Azure](https://azure.microsoft.com/regions) для повышения доступности.
    * [Моделируйте сбой](regional-failover.md) одного или нескольких регионов с гарантированной нулевой потерей данных. 

* **Лучший способ создания глобально распределенных приложений**.
    * [Пять моделей согласованности](consistency-levels.md) обеспечивают широкий спектр функций — от строгой согласованности SQL до окончательной согласованности NoSQL. 
  
* **Гарантированный возврат средств**.
    * Быстро получайте данные или верните деньги. 
    * [Соглашения об уровне обслуживания](https://aka.ms/acdbsla) гарантируют высокую доступность, низкую задержку, высокую пропускную способность и согласованность. 

* **Отсутствие необходимости управлять схемами и индексами базы данных**.
    * Перестаньте волноваться о синхронизации схемы базы данных и индексов со схемой вашего приложения. Azure Cosmos DB не использует схемы. 
    * Ядро СУБД Azure Cosmos DB вообще не зависит от схем. Оно автоматически индексирует все принимаемые данные, не нуждаясь в схемах и индексах и моментально выполняя запросы. 

* **Низкая стоимость владения**.
    * Это решение в пять-десять раз [экономичнее](https://aka.ms/cosmos-db-tco-paper), чем неуправляемые решения.
    * Cosmos DB в три раза дешевле, чем DynamoDB.

## <a name="capability-comparison"></a>Сравнение возможностей

Azure Cosmos DB предоставляет лучшие возможности реляционных и нереляционных баз данных.

| Возможности | Реляционные базы данных   | Нереляционные базы данных (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Глобальное распределение | Нет | Нет | Да, готовое распределение в более чем 30 регионах для API-интерфейсов множественной адресации|
| Горизонтальное масштабирование | Нет | Да | Да, масштабировать хранилище и пропускную способность можно независимо | 
| Гарантии относительно задержки | Нет | Да | Да, 99 % операций чтения менее чем за 10 мс и записи менее чем за 15 мс | 
| высокую доступность; | Нет | Да | Да, служба Cosmos DB всегда доступна, поддерживает компромиссы PACELC и позволяет выполнять автоматическую отработку отказа или отработку отказа в ручном режиме|
| Модель данных + API | Реляционная БД + SQL | Многомодельность + OSS API | Многомодельность + SQL + OSS API (в ближайшее время ожидается больше) |
| Соглашения об уровне обслуживания | Да | Нет | Да, комплексные соглашения об уровне обслуживания в отношении задержки, пропускной способности, целостности и доступности |


## <a name="next-steps"></a>Дальнейшие действия
Приступая к работе с Azure Cosmos DB, ознакомьтесь с одним из наших кратких руководств:

* [Azure Cosmos DB: Build a DocumentDB API web app with .NET and the Azure portal](create-documentdb-dotnet.md) (Azure Cosmos DB. Создание веб-приложения API DocumentDB с помощью .NET и портала Azure)
* [Azure Cosmos DB: Migrate an existing Node.js MongoDB web app](create-mongodb-nodejs.md) (Azure Cosmos DB. Миграция имеющегося веб-приложения Node.js MongoDB)
* [Azure Cosmos DB: Build a .NET application using the Graph API](create-graph-dotnet.md) (Azure Cosmos DB. Создание приложения .NET с помощью API Graph)
* [Azure Cosmos DB: Build a .NET application using the Table API](create-table-dotnet.md) (Azure Cosmos DB. Создание приложения .NET с помощью API таблицы)

