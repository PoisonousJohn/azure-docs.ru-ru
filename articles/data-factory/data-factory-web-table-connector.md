---
title: "Перемещение данных из веб-таблицы с помощью фабрики данных Azure | Документация Майкрософт"
description: "Узнайте, как перемещать данные из таблицы на веб-странице с помощью фабрики данных Azure."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 08298608c2dfbe8b7226ae13bb0fe0f9cd5cec65
ms.contentlocale: ru-ru
ms.lasthandoff: 03/29/2017

---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Перемещение данных из источника веб-таблицы с помощью фабрики данных Azure
В этой статье описано, как с помощью действия копирования в фабрике данных Azure переместить данные из таблицы на веб-странице в хранилище данных, поддерживаемое в качестве приемника. В этой статье в продолжение темы о [действиях перемещения данных](data-factory-data-movement-activities.md) приведены общие сведения о перемещении данных с помощью действия копирования, а также список поддерживаемых хранилищ данных, используемых в качестве источников и приемников.

Сейчас фабрика данных поддерживает только перемещение данных из веб-таблицы в другие хранилища данных, но не наоборот.

> [!IMPORTANT]
> Сейчас этот веб-соединитель поддерживает только извлечение содержимого таблицы из HTML-страницы. Чтобы извлечь данные из конечной точки HTTP (HTTPS), используйте [соединитель HTTP](data-factory-http-connector.md).

## <a name="getting-started"></a>Приступая к работе
Создать конвейер с действием копирования, который перемещает данные из локального хранилища данных Cassandra, можно с помощью различных инструментов и интерфейсов API. 

- Проще всего создать конвейер с помощью **мастера копирования**. В статье [Руководство. Создание конвейера с действием копирования с помощью мастера копирования фабрики данных](data-factory-copy-data-wizard-tutorial.md) приведены краткие пошаговые указания по созданию конвейера с помощью мастера копирования данных. 
- Также для создания конвейера можно использовать следующие инструменты: **портал Azure**, **Visual Studio**, **Azure PowerShell**, **шаблон Azure Resource Manager**, **API .NET** и **REST API**. Пошаговые инструкции по созданию конвейера с действием копирования см. в [руководстве по действию копирования](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Независимо от того, используются инструменты или интерфейсы API, для создания конвейера, который перемещает данные из источника данных в приемник, выполняются следующие шаги.

1. Создайте **связанные службы**, чтобы связать входные и выходные данные с фабрикой данных.
2. Создайте **наборы данных**, которые представляют входные и выходные данные для операции копирования. 
3. Создайте **конвейер** с действием копирования, который принимает входной набор данных и возвращает выходной набор данных. 

Если вы используете мастер, то он автоматически создает определения JSON для сущностей фабрики данных (связанных служб, наборов данных и конвейера). При использовании инструментов и интерфейсов API (за исключением API .NET) вы самостоятельно определяете эти сущности фабрики данных в формате JSON.  Пример определений JSON для сущностей фабрики данных, которые используются для копирования данных из веб-таблицы, доступен в разделе [Пример JSON. Копирование данных из веб-таблицы в большой двоичный объект Azure](#json-example-copy-data-from-web-table-to-azure-blob) этой статьи. 

Следующие разделы содержат сведения о свойствах JSON, которые используются для определения сущностей фабрики данных, относящихся к веб-таблице.

## <a name="linked-service-properties"></a>Свойства связанной службы
В приведенной далее таблице содержится описание элементов JSON, которые относятся к связанной веб-службе.

| Свойство | Описание | Обязательно |
| --- | --- | --- |
| type |Для свойства type необходимо задать значение **Web** |Да |
| URL-адрес |URL-адрес источника Web |Да |
| authenticationType |Анонимная. |Да |

### <a name="using-anonymous-authentication"></a>Использовать анонимную проверку подлинности

```json
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

## <a name="dataset-properties"></a>Свойства набора данных
Полный список разделов и свойств, используемых для определения наборов данных, см. в статье [Наборы данных](data-factory-create-datasets.md). Разделы структуры, доступности и политики JSON набора данных одинаковы для всех типов наборов данных (SQL Azure, большие двоичные объекты Azure, таблицы Azure и т. д.).

Раздел **typeProperties** во всех типах наборов данных разный. В нем содержатся сведения о расположении данных в хранилище данных. Раздел typeProperties набора данных типа **WebTable** содержит следующие свойства.

| Свойство | Описание | Обязательно |
|:--- |:--- |:--- |
| type |Тип набора данных. Необходимо задать значение **WebTable** |Да |
| path |Относительный URL-адрес ресурса, который содержит таблицу. |Нет. Если путь не задан, используется только URL-адрес, указанный в определении связанной службы. |
| index |Индекс таблицы в ресурсе. Дополнительные сведения см. в разделе [Получение индекса таблицы на HTML-странице](#get-index-of-a-table-in-an-html-page). |Да |

**Пример**

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="copy-activity-properties"></a>Свойства действия копирования
Полный список разделов и свойств, используемых для определения действий, см. в статье [Создание конвейеров](data-factory-create-pipelines.md). Свойства (включая имя, описание, входные и выходные таблицы, политику и т. д.) доступны для всех типов действий.

В свою очередь свойства, доступные в разделе typeProperties действия, зависят от конкретного типа действия. Для действия копирования они различаются в зависимости от типов источников и приемников.

В настоящее время, если источник в действии копирования относится к типу **WebSource**, дополнительные свойства не поддерживается.


## <a name="json-example-copy-data-from-web-table-to-azure-blob"></a>Пример JSON. Копирование данных из веб-таблицы в большой двоичный объект Azure
В примере ниже используется следующее:

1. Связанная служба типа [Web](#linked-service-properties).
2. Связанная служба типа [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Входной [набор данных](data-factory-create-datasets.md) типа [WebTable](#dataset-properties).
4. Выходной [набор данных](data-factory-create-datasets.md) типа [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. [Конвейер](data-factory-create-pipelines.md) с действием копирования, в котором используются [WebSource](#copy-activity-properties) и [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

В этом примере данные из веб-таблицы каждый час копируются в BLOB-объект Azure. Используемые в этих примерах свойства JSON описаны в разделах, следующих за примерами.

В следующем примере показано, как скопировать данные из веб-таблицы в большой двоичный объект Azure. Тем не менее данные можно напрямую копировать в любые приемники, указанные в статье [Действия перемещения данных](data-factory-data-movement-activities.md). Для этого применяется действие копирования в фабрике данных Azure.

**Связанная веб-служба**. В этом примере используется связанная веб-служба с анонимным доступом. Сведения об используемых типах аутентификации см. в разделе [Связанная веб-служба](#linked-service-properties).

```json
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

**Связанная служба хранения Azure**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Входной набор данных WebTable**. Если для параметра **external** задать значение **true**, то фабрика данных воспримет этот набор данных как внешний, не созданный в результате какого-либо действия в этой службе.

> [!NOTE]
> Дополнительные сведения см. в разделе [Получение индекса таблицы на HTML-странице](#get-index-of-a-table-in-an-html-page).  
>
>

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Выходной набор данных BLOB-объекта Azure**

Данные записываются в новый BLOB-объект каждый час (frequency: hour, interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Конвейер с действием копирования**

Конвейер содержит действие копирования, которое использует входной и выходной наборы данных и выполняется каждый час. В определении конвейера JSON для типа **source** установлено значение **WebSource**, а для типа **sink** — значение **BlobSink**.

Список свойств, поддерживаемых WebSource, см. в разделе [Свойства типа WebSource](#copy-activity-type-properties).

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="get-index-of-a-table-in-an-html-page"></a>Получение индекса таблицы на HTML-странице
1. Запустите **Excel 2016** и перейдите на вкладку **Данные**.  
2. На панели инструментов щелкните **Создать запрос**, выберите **Из других источников** и щелкните **Из Интернета**.

    ![Меню Power Query](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. В диалоговом окне **Из Интернета** введите **URL-адрес**, который будет использоваться в JSON связанной службы (например, https://en.wikipedia.org/wiki/), вместе с указанным для набора данных путем (например, AFI%27s_100_Years…100_Movies), а затем нажмите кнопку **ОК**.

    ![Диалоговое окно "Из Интернета"](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    В этом примере используется URL-адрес https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Если отображается диалоговое окно **Доступ к веб-содержимому**, выберите соответствующий **URL-адрес** и **тип аутентификации**, а затем нажмите кнопку **Подключить**.

   ![Диалоговое окно "Доступ к веб-содержимому"](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. В представлении дерева щелкните элемент **table**, чтобы просмотреть содержимое таблицы, а затем в нижней части экрана нажмите кнопку **Изменить**.  

   ![Диалоговое окно навигатора](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. В окне **Редактор запросов** на панели инструментов нажмите кнопку **Расширенный редактор**.

    ![Кнопка "Расширенный редактор"](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. В диалоговом окне "Расширенный редактор" число, отображаемое рядом с полем "Источник", является индексом.

    ![Расширенный редактор — индекс](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Если вы работаете с Excel 2013, используйте [Microsoft Power Query для Excel](https://www.microsoft.com/download/details.aspx?id=39379), чтобы получить индекс. Дополнительные сведения см. в статье [Подключение к веб-странице](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8). Точно так же можно использовать [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/).

> [!NOTE]
> Сведения о сопоставлении столбцов в наборе данных, используемом в качестве источника, со столбцами в приемнике см. в разделе [Сопоставление столбцов исходного набора данных со столбцами целевого набора данных](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Производительность и настройка
Ознакомьтесь со статьей [Руководство по настройке производительности действия копирования](data-factory-copy-activity-performance.md), в которой описываются ключевые факторы, влияющие на производительность перемещения данных (действие копирования) в фабрике данных Azure, и различные способы оптимизации этого процесса.

