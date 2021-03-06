---
title: "Мониторинг производительности Azure Service Fabric | Документация Майкрософт"
description: "Узнайте о счетчиках производительности для мониторинга и диагностики кластеров Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 9d63148c182c705b6b49733c59ed8fdd13872d72
ms.contentlocale: ru-ru
ms.lasthandoff: 07/20/2017

---

# <a name="performance-metrics"></a>Метрики производительности

Метрики следует собирать для анализа производительности кластера, а также приложений, выполняющихся в нем. Для кластеров Service Fabric рекомендуется собирать данные следующих счетчиков производительности.

## <a name="nodes"></a>Nodes

Для компьютеров в кластере рассмотрите возможность сбора приведенных ниже счетчиков производительности, чтобы лучше понимать нагрузку на каждом компьютере и принимать соответствующие решения о масштабировании кластера.

| Категория счетчика | Имя счетчика |
| --- | --- |
| Физический диск (а диск) | Среднее Длина очереди чтения с диска |
| Физический диск (а диск) | Среднее Длина очереди записи на диск |
| Физический диск (а диск) | Среднее время чтения с диска (с) |
| Физический диск (а диск) | Среднее время записи на диск (с) |
| Физический диск (а диск) | Операций чтения с диска в секунду  |
| Физический диск (а диск) | Скорость чтения с диска (байт/с)  |
| Физический диск (а диск) | Операций записи на диск в секунду |
| Физический диск (а диск) | Скорость записи на диск (байт/с) |
| Память | Доступный объем в МБ |
| Файл подкачки | % использования |
| Обработка (всего) | % загруженности процессора |
| Обработка (на службу) | % загруженности процессора |
| Обработка (на службу) | Идентификатор процесса |
| Обработка (на службу) | Байты исключительного пользования |
| Обработка (на службу) | Счетчик потоков |
| Обработка (на службу) | Байт виртуальной памяти |
| Обработка (на службу) | Рабочий набор |
| Обработка (на службу) | Рабочий набор (частный) |

## <a name="net-applications-and-services"></a>Приложения и службы .NET

Собирайте приведенные ниже счетчики, если вы развертываете службы .NET в кластере. 

| Категория счетчика | Имя счетчика |
| --- | --- |
| Память CLR .NET (на службу) | ИД процесса |
| Память CLR .NET (на службу) | Всего фиксировано байт |
| Память CLR .NET (на службу) | Всего зарезервировано байт |
| Память CLR .NET (на службу) | Байт во всех кучах |
| Память CLR .NET (на службу) | Сборов мусора для поколения 0 |
| Память CLR .NET (на службу) | Сборов мусора для поколения 1 |
| Память CLR .NET (на службу) | Сборов мусора для поколения 2 |
| Память CLR .NET (на службу) | Время на сборку мусора, % |

### <a name="service-fabrics-custom-performance-counters"></a>Настраиваемые счетчики производительности Service Fabric

Service Fabric создает достаточное число настраиваемых счетчиков производительности. Если у вас установлен пакет SDK, то полный список счетчиков можно просмотреть на компьютере Windows в приложении системного монитора ("Пуск" > "Системный монитор"). 

Если используется Reliable Actors, то для приложений, которые вы развертываете в кластере, добавьте счетчики из категорий `Service Fabric Actor` и `Service Fabric Actor Method` (см. раздел [Диагностика и мониторинг производительности в Reliable Actors](service-fabric-reliable-actors-diagnostics.md)).

Для Reliable Services имеются аналогичные категории счетчиков `Service Fabric Service` и `Service Fabric Service Method`, данные которых следует собирать. 

При использовании Reliable Collections рекомендуется добавить `Avg. Transaction ms/Commit` из `Service Fabric Transactional Replicator` для сбора метрики средней задержки при фиксации транзакции.


## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о [создании событий на уровне платформы](service-fabric-diagnostics-event-generation-infra.md) в Service Fabric.
* Собирайте метрики производительности с помощью [системы диагностики Azure](service-fabric-diagnostics-event-aggregation-wad.md).

