---
title: "Перенос схемы в хранилище данных SQL | Документация Майкрософт"
description: "Советы по переносу схемы в хранилище данных SQL Azure для разработки решений."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.contentlocale: ru-ru
ms.lasthandoff: 06/30/2017


---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Перенос схем в хранилище данных SQL
Рекомендации по переносу схем SQL в хранилище данных SQL. 

## <a name="plan-your-schema-migration"></a>Планирование переноса схемы

При планировании переноса прочитайте [обзор таблиц][table overview], чтобы ознакомиться с вопросами проектирования таблиц, такими как статистика, распределение, секционирование и индексирование.  В нем также рассматриваются некоторые [неподдерживаемые функции таблиц][unsupported table features] и способы их обойти.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Применение пользовательских схем для консолидации баз данных

Ваша текущая рабочая нагрузка, вероятно, использует более одной базы данных. Например, хранилище данных SQL Server может включать в себя промежуточную базу данных, базу данных хранилища данных и базы данных киоска данных. В этой топологии на каждой базе данных выполняется отдельная рабочая нагрузка с собственными политиками безопасности.

Напротив, хранилище данных SQL выполняет всю рабочую нагрузку хранилища данных в одной базе данных. Кроссплатформенные соединения баз данных не допускаются. Поэтому хранилище данных SQL ожидает, что все таблицы, используемые в хранилище данных, хранятся в одной базе данных.

Мы рекомендуем применять пользовательские схемы для консолидации имеющейся рабочей нагрузки в одной базе данных. Примеры см. в разделе [Пользовательские схемы](sql-data-warehouse-develop-user-defined-schemas.md).

## <a name="use-compatible-data-types"></a>Использование совместимых типов данных
Измените типы данных, чтобы обеспечить совместимость с хранилищем данных SQL. Список поддерживаемых и неподдерживаемых типов данных см. в разделе [о типах данных][data types]. В нем представлены обходные пути для неподдерживаемых типов. Кроме того, в нем приводится запрос для определения имеющихся типов, которые не поддерживаются в хранилище данных SQL.

## <a name="minimize-row-size"></a>Сведение к минимуму длины строки
Для повышения производительности рекомендуется свести к минимуму длину строк таблицы. Чем короче строки, тем выше производительность. Поэтому используйте наименьшие типы данных, которые вам подходят. 

Для ширины строки таблицы в PolyBase действует ограничение в 1 МБ.  Если планируется загружать данные в хранилище данных SQL с помощью PolyBase, обновите таблицы, обеспечив максимальную ширину строки меньше 1 МБ. 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a>Указание параметра распределения
Хранилище данных SQL — это система управления распределенными базами данных. Каждая таблица распределяется или реплицируется на вычислительные узлы. Доступен параметр таблицы, позволяющий указать способ распределения данных. Можно выбрать циклический перебор, репликацию или хэш-распределение. У каждого варианта есть преимущества и недостатки. Если не указать параметр распределения, по умолчанию хранилищем данных SQL будет использоваться циклический перебор.

- Циклический перебор используется по умолчанию. Он проще всего в использовании и загружает данные быстрее остальных методов, однако для операций соединения потребуется перемещение данных, что снизит производительность запросов.
- Копия реплицированной таблицы хранится на каждом вычислительном узле. Реплицированные таблицы являются высокопроизводительных, так как они не требуют перемещения данных для операций соединения и агрегирования. Для них требуется дополнительное место в хранилище, поэтому этот тип распределения лучше всего подходит для небольших таблиц.
- При хэш-распределении строки распределяются по всем узлам с помощью хэш-функции. Хэш-распределенные таблицы являются основой хранилища данных SQL, так как они предназначены обеспечить высокую производительность запросов в больших таблицах. Для данного типа распределения требуется выполнить планирование, чтобы выбрать наиболее подходящий столбец для распределения данных. Тем не менее, если вы не выбрали наиболее подходящий столбец с первого раза, то сможете легко повторно распределить данные по другому столбцу. 

Чтобы выбрать оптимальный вариант распределения для каждой таблицы, ознакомьтесь с разделом [Распределение таблиц в хранилище данных SQL](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Дальнейшие действия
После успешного переноса схемы базы данных в хранилище данных SQL вы можете перейти к одной из приведенных ниже статей.

* [Перенос данных][Migrate your data]
* [Перенос кода][Migrate your code]

Дополнительные рекомендации по работе с хранилищем данных SQL см. в статье [Рекомендации по использованию хранилища данных SQL Azure][best practices].

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->

