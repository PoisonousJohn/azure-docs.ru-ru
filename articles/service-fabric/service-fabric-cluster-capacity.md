---
title: "Планирование загрузки кластера Service Fabric | Документация Майкрософт"
description: "Рекомендации по планированию загрузки кластера Service Fabric. NodeTypes, устойчивость и уровни надежности"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 4c584f4a-cb1f-400c-b61f-1f797f11c982
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: chackdan
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 36b96360fabdcc64ffd2356540c580594637d48e
ms.contentlocale: ru-ru
ms.lasthandoff: 08/23/2017

---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Рекомендации по планированию загрузки кластера Service Fabric
Для любой рабочей развернутой службы важным шагом является планирование загрузки. Ниже приведено несколько факторов, которые необходимо учитывать при этом.

* Количество типов узлов, с которыми создается кластер
* Свойства каждого типа узла (размер, первичный узел, доступ к Интернету, количество виртуальных машин и т. п.).
* Надежность и устойчивость характеристик кластера.

Кратко рассмотрим каждый из пунктов.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Количество типов узлов, с которыми создается кластер
Во-первых, необходимо выяснить, для чего будет использоваться создаваемый кластер и какие виды приложений планируется в нем развернуть. Если назначение кластера не ясно, скорее всего, вы еще не готовы перейти к планированию загрузки.

Определите количество типов узлов, с которыми нужно создать кластер.  Каждый тип узла соответствует набору масштабирования виртуальных машин. Каждый тип узла поддерживает возможность независимого масштабирования, имеет разные наборы открытых портов и собственные метрики емкости. Поэтому решение о числе типов узлов, в сущности, сводится к следующему.

* Есть ли у вашего приложения несколько служб, среди которых есть службы, которые должны быть общедоступными или доступными из Интернета? Типичные приложения содержат интерфейсную службу шлюза, которая получает входные данные от клиента, и одну или несколько внутренних служб, взаимодействующих с интерфейсными службами. Поэтому в этом случае требуется как минимум два типа узлов.
* У служб (составляющих приложение) различные требования к инфраструктуре, например больше ОЗУ или тактов центрального процессора? Предположим, что приложение, которое вам необходимо развернуть, содержит интерфейсную и внутреннюю службы. Интерфейсная служба может выполняться на виртуальных машинах меньшего размера (например, D2) с портами, открытыми для доступа через Интернет.  Однако внутренняя служба, которая будет выполнять вычисления, должна размещаться на более крупных виртуальных машинах (D4, D6, D15) без выхода в Интернет.
  
  Хотя вы можете поместить все эти службы на узлы одного типа, в этом примере их лучше всего поместить в кластер с двумя типами узлов.  Так у каждого типа узла будут свои собственные свойства, например возможность подключения к Интернету или размер виртуальной машины. Кроме того, можно будет независимо менять число виртуальных машин.  
* Так как нельзя предсказать будущее, исходите из известных вам фактов и решите, какое число типов узлов требуется, чтобы начать использовать приложения. Вы всегда сможете добавить или удалить типы узлов позже. У кластера Service Fabric должен быть по крайней мере один тип узла.

## <a name="the-properties-of-each-node-type"></a>Свойства каждого типа узла
**Тип узла** можно считать эквивалентом роли в облачных службах. Он определяет размер виртуальных машин, их количество и свойства. Каждый тип узла, определенный в кластере Service Fabric, настроен как отдельный набор масштабирования виртуальных машин. Масштабируемые наборы виртуальных машин относятся к вычислительным ресурсам Azure. Их можно использовать для развертывания коллекции виртуальных машин и управления ею как набором. Будучи определенным как отдельный набор масштабирования виртуальных машин, каждый тип узла поддерживает возможность независимого масштабирования, имеет разные наборы открытых портов и собственные метрики загрузки.

Прочтите [этот документ](service-fabric-cluster-nodetypes.md), чтобы больше узнать о взаимосвязи типов узлов и набора масштабирования виртуальных машин, а также о том, как подключиться к одному из экземпляров с помощью протокола RDP, открыть новые порты и т. д.

Кластер может содержать узлы нескольких типов, однако тип первичного узла (тот, который вы задали на портале) должен включать в себя не менее пяти виртуальных машин для кластеров, используемых для рабочих нагрузок в рабочей среде (или не менее трех виртуальных машин для тестовых кластеров). Если вы создаете кластер с помощью шаблона Resource Manager, то ищите в определении типа узла атрибут **is Primary**. Тип первичного узла — это тип узла, на котором размещены системные службы Service Fabric.  

### <a name="primary-node-type"></a>Тип первичного узла
Для кластера с несколькими типами узлов необходимо выбрать, какой из них будет первичным. Ниже приведены характеристики типа первичного узла.

* **Минимальный размер виртуальных машин** для типа первичного узла зависит от выбранного **уровня устойчивости**. Значение по умолчанию для уровня устойчивости — Bronze. Прокрутите вниз, чтобы прочитать об уровне устойчивости и его значениях.  
* **Минимальное количество виртуальных машин** для типа первичного узла зависит от выбранного **уровня надежности**. Значение по умолчанию для уровня надежности — Silver. Прокрутите вниз, чтобы прочитать об уровне надежности и его значениях. 


* Системные службы Service Fabric (например, служба диспетчера кластера или служба хранилища образов) размещаются на первичных узлах, поэтому надежность и устойчивость кластера определяется значениями уровней надежности и устойчивости, выбранными для типа первичного узла.

![Снимок экрана с двумя типами узлов ][SystemServices]

### <a name="non-primary-node-type"></a>Тип вторичного узла
В кластере с несколькими типами узлов имеется один тип первичного узла, а остальные — типы вторичных узлов. Ниже приведены характеристики типа вторичного узла.

* Минимальный размер виртуальных машин для этого типа узла зависит от выбранного уровня устойчивости. Значение по умолчанию для уровня устойчивости — Bronze. Прокрутите вниз, чтобы прочитать об уровне устойчивости и его значениях.  
* Минимальным количеством виртуальных машин для этого типа узла может быть одна виртуальная машина. Тем не менее это число следует выбирать, руководствуясь количеством реплик приложения или служб, которые вы хотите запустить на узле этого типа. После развертывания кластера число виртуальных машин для типа узла можно увеличить.

## <a name="the-durability-characteristics-of-the-cluster"></a>Характеристики устойчивости кластера
Уровень устойчивости используется, чтобы указать системе привилегии, имеющиеся у виртуальных машин в базовой инфраструктуре Azure. Для типа первичного узла эта привилегия позволяет Service Fabric приостановить любой запрос инфраструктуры на уровне виртуальной машины (например, перезагрузку, переустановку из образа или перенос виртуальной машины), который влияет на требования к кворуму, установленные для системных служб и ваших служб с отслеживанием состояния. Для типов вторичных узлов эта привилегия позволяет Service Fabric приостановить любой запрос инфраструктуры на уровне виртуальной машины, например перезагрузку, переустановку из образа или перенос виртуальной машины и т. д., который влияет на требования к кворуму, установленные для ваших служб с отслеживанием состояния, которые в ней запущены.

Эта привилегия обозначается следующими значениями.

* Gold: задания инфраструктуры могут быть приостановлены в течение двух часов для UD. Устойчивость Gold можно включить только для номеров SKU виртуальных машин полных узлов, например D15_V2, G5 и т. д.
* Silver. Задания инфраструктуры могут быть приостановлены на 10 минут для UD. Они доступны на всех стандартных виртуальных машинах, содержащих одно или больше ядер.
* Bronze: привилегии отсутствуют. Это уровень по умолчанию. Используйте этот уровень устойчивости только для типов узлов, на которых выполняются _только_ рабочие нагрузки без учета состояния. 

> [!WARNING]
> Типы узлов, выполняющиеся с устойчивостью Bronze, _не получают привилегий_. Это означает, что задания инфраструктуры, которые влияют рабочие нагрузки без отслеживания состояния, не будут остановлены или отложены. Возможно, такие задания все же будут влиять на ваши рабочие нагрузки, приводя к простою или другим проблемам. Для выполнения любого вида производственной рабочей нагрузки рекомендуется использовать по крайней мере устойчивость Silver. 
> 

Вы можете выбрать уровень устойчивости для каждого типа узла. В одном кластере можно выбрать уровень устойчивости Gold или Silver для одного типа узла и Bronze — для другого. **Необходимо не менее 5 узлов любого типа с устойчивостью Gold или Silver**. 

**Преимущества использования уровней устойчивости Silver или Gold**
 
1. Сокращение обязательных действий в операции свертывания (т. е. деактивация узла и Remove-ServiceFabricNodeState вызываются автоматически).
2. Уменьшение риска потери данных из-за операций инфраструктуры Azure или инициированных клиентом внутренних операций изменения номера SKU виртуальной машины.
     
**Недостатки использования уровней устойчивости Silver или Gold**
 
1. Развертывания в масштабируемый набор виртуальных машин (и другие связанные ресурсы Azure) могут быть задержаны, просрочены или полностью заблокированы из-за проблем в кластере или на уровне инфраструктуры. 
2. Увеличивается число [событий жизненного цикла реплики](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle ) (например, переключений первичного узла) из-за автоматизированной деактивации узлов во время операций инфраструктуры Azure.

### <a name="recommendations-on-when-to-use-silver-or-gold-durability-levels"></a>Рекомендации по использованию уровней устойчивости Silver или Gold

Используйте уровни устойчивости Silver или Gold для всех типов узлов со службами с отслеживанием состояния, предполагающими частое свертывание (уменьшение числа экземпляров виртуальных машин), если вам предпочтительнее задержка операций развертывания за счет упрощения этих операций масштабирования. Сценарии развертывания (добавления экземпляров виртуальных машин) не влияют на выбор уровня устойчивости, только сценарии свертывания.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Рекомендации по эксплуатации для типа узла, для которого установлен уровень устойчивости Silver или Gold

1. Поддерживайте постоянную работоспособность кластера и приложений и проверяйте, своевременно ли реагируют приложения на все [события жизненного цикла реплики службы](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (например, задержку сборки реплики).
2. Реализуйте более безопасные способы изменения номера SKU виртуальной машины (масштабирования). Изменение номера SKU виртуальной машины для масштабируемого набора виртуальных машин по своей сути является небезопасной операцией, поэтому рекомендуется по возможности ее избегать. Ниже описывается, как избежать распространенных проблем.
    - **Для типов узлов, не являющихся первичными:** рекомендуется создать масштабируемый набор виртуальных машин, изменить ограничение размещения служб, чтобы добавить новый тип узла для масштабируемого набора виртуальных машин, а затем сократить число экземпляров старого масштабируемого набора до 0, удаляя по одному узлу (так удаление узлов не повлияет на надежность кластера).
    - **Для типа первичного узла:** рекомендуется не изменять номер SKU виртуальной машины. Если причиной создания номера SKU является увеличение емкости, то мы рекомендуем добавить экземпляры или, если это возможно, создать новый кластер. Если у вас нет выбора, то внесите изменения в определение модели масштабируемого набора виртуальных машин, указав новую конфигурацию. Если в кластере используется только один тип узла, то убедитесь, что все приложения с отслеживанием состояния своевременно реагируют на все [события жизненного цикла реплики службы](service-fabric-reliable-services-advanced-usage.md#stateful-service-replica-lifecycle) (например, задержку сборки реплики) и длительность восстановления реплики службы не превышает пяти минут (для уровня устойчивости Silver). 


> [!WARNING]
> Изменять размер (номер SKU) виртуальной машины для масштабируемого набора виртуальных машин, для которого не используется по крайней мере устойчивость Silver, не рекомендуется. Изменение размера (номера SKU) виртуальной машины — внутренняя операция инфраструктуры, которая может привести к потере данных. Без хоть какой-либо возможности отложить или отследить это изменение вероятна ситуация, в которой операция приведет к потере данных служб с отслеживанием состояния или вызовет другие непредвиденные проблемы в работе, даже для рабочих нагрузок без отслеживания состояния. 
> 
    
3. Обеспечьте как минимум пять узлов для любого масштабируемого набора виртуальных машин с включенной функцией MR.
4. Не удаляйте случайные экземпляры виртуальных машин. Всегда используйте функцию уменьшения масштаба масштабируемого набора виртуальных машин. Удаление случайных экземпляров виртуальной машины может нарушить баланс в распределении экземпляров виртуальной машины в домене обновления и домене сбоя. Этот дисбаланс может негативно повлиять на возможность системы правильно распределять нагрузку между экземплярами служб или репликами служб.
6. При использовании автомасштабирования установите правила таким образом, чтобы свертывание (удаление экземпляров виртуальной машины) выполнялось одновременно только на одном узле. Одновременно уменьшать масштаб более одного экземпляра небезопасно.
7. При уменьшении масштаба типа первичного узла никогда не следует задавать значение ниже, чем допускается уровнем надежности.


## <a name="the-reliability-characteristics-of-the-cluster"></a>Характеристики надежности кластера
Уровень надежности используется для задания числа реплик системных служб, которые будут выполняться в этом кластере на первичных узлах. Чем больше реплик, тем надежнее системные службы в кластере.  

Уровень надежности может принимать следующие значения:

* Platinum: запуск системных служб с целевым набором из 9 реплик.
* Gold: запуск системных служб с целевым набором из 7 реплик.
* Silver: запуск системных служб с целевым набором из 5 реплик. 
* Bronze: запуск системных служб с целевым набором из 3 реплик.

> [!NOTE]
> Выбранный уровень надежности определяет минимальное количество узлов для типа первичного узла. 
> 
> 


### <a name="recommendations-for-the-reliability-tier"></a>Рекомендации по уровню надежности.

 При увеличении или уменьшении размера кластера (суммы экземпляров виртуальных машин во всех типах узлов) необходимо изменить уровень надежности кластера с одного на другой. Это активирует обновления кластера, необходимые для изменения числа реплик системных служб. Подождите, пока обновление не будет завершено, прежде чем вносить другие изменения в кластер, например добавлять узлы.  Ход выполнения обновления можно отслеживать в Service Fabric Explorer или с помощью командлета [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps).

Ниже приведены рекомендации по выбору уровня надежности.

| **Размер кластера** | **Уровень надежности** |
| --- | --- |
| 1 |Не указывайте параметр уровня надежности, система рассчитает его сама |
| 3 |Bronze |
| 5 или 6|Silver |
| 7 или 8 |Gold |
| 9 и более |Platinum |




## <a name="primary-node-type---capacity-guidance"></a>Рекомендации по загрузке для типа первичного узла

Ниже приведены рекомендации по планированию загрузки для типа первичного узла.

1. **Количество экземпляров виртуальных машин для выполнения любой рабочей нагрузки в Azure:** необходимо указать минимальный размер для типа первичного узла, равный 5. 
2. **Количество экземпляров виртуальных машин для запуска тестов рабочих нагрузок в Azure:** можно указать минимальный размер для типа первичного узла, равный 1 или 3. Одноузловой кластер работает со специальной конфигурацией, поэтому масштабирование этого кластера не поддерживается. Одноузловой кластер не является надежным, поэтому в шаблоне Resource Manager нужно удалить или не указывать эту конфигурацию (не задавать значение конфигурации будет недостаточно). Если одноузловой кластер настраивается через портал, то конфигурация задается автоматически. Одно- и трехузловые кластеры не поддерживаются для выполнения производственных задач. 
3. **Номер SKU виртуальной машины:** тип первичного узла используется для запуска системных служб, поэтому выбранный для него номер SKU виртуальной машины должен обеспечивать достаточно ресурсов для общей пиковой загрузки, которую вы планируете разместить в кластере. Чтобы пояснить, что имеется ввиду, приведу аналогию. Представьте, что тип первичного узла — ваши легкие, которые снабжают кислородом мозг. Поэтому, если мозг не получает достаточно кислорода, то тело страдает. 

Необходимая загрузка кластера определяется рабочей нагрузкой, которую планируется в нем выполнять. Поэтому мы не можем дать качественные рекомендации именно для вашей рабочей нагрузки, однако ниже приведены общие рекомендации, которые помогут приступить к работе.

Для производственных рабочих нагрузок 


- Рекомендуемый номер SKU виртуальной машины — Standard D3, Standard D3_V2 или их аналог, обеспечивающий не менее 14 ГБ на локальном SSD.
- Минимальный поддерживаемый номер SKU виртуальной машины — Standard D1, Standard D1_V2 или их аналог, обеспечивающий не менее 14 ГБ на локальном SSD. 
- Номера SKU виртуальных машин с неполным числом ядер (например, Standard A0) для производственных рабочих нагрузок не поддерживаются.
- Номер SKU Standard A1 не поддерживается для производственных рабочих нагрузок по соображениям производительности.


## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Рекомендации по производительности для рабочих нагрузок с отслеживанием состояния для прочих типов узлов

Эти рекомендации предназначены для рабочих нагрузок с отслеживанием состояния, использующих в Service Fabric интерфейсы [Reliable Collections или Reliable Actors](service-fabric-choose-framework.md), которые выполняются в типах узлов, не являющихся первичными.


**Количество экземпляров виртуальных машин:** рабочие нагрузки с отслеживанием состояния рекомендуется запускать как минимум на 5 репликах, задав это количество целевым. Это означает, что в стабильном состоянии в каждом домене сбоя и домене обновления будет по реплике (из набора реплик). Весь принцип уровня надежности для типа первичного узла состоит в том, чтобы просто задать этот параметр для системных служб. Та же рекомендация относится к службам с отслеживанием состояния.

Таким образом для производственных рабочих нагрузок минимальный рекомендуемый размер для типа узла, не являющегося первичным, равен 5, если этот тип используется для выполнения рабочих нагрузок с отслеживанием состояния.


**Номер SKU виртуальной машины:** этот тип узла используется для выполнения служб приложения, поэтому выбранный для него номер SKU виртуальной машины должен обеспечивать достаточно ресурсов для общей пиковой загрузки, которую вы планируете разместить в каждом узле. Необходимая загрузка для типа узла определяется рабочей нагрузкой, которую планируется выполнять в кластере. Поэтому мы не можем дать качественные рекомендации именно для вашей рабочей нагрузки, однако ниже приведены общие рекомендации, которые помогут приступить к работе.

Для производственных рабочих нагрузок 

- Рекомендуемый номер SKU виртуальной машины — Standard D3, Standard D3_V2 или их аналог, обеспечивающий не менее 14 ГБ на локальном SSD.
- Минимальный поддерживаемый номер SKU виртуальной машины — Standard D1, Standard D1_V2 или их аналог, обеспечивающий не менее 14 ГБ на локальном SSD. 
- Номера SKU виртуальных машин с неполным числом ядер (например, Standard A0) для производственных рабочих нагрузок не поддерживаются.
- В частности, номер SKU Standard A1 не поддерживается для производственных рабочих нагрузок по соображениям производительности.


## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Рекомендации по производительности для рабочих нагрузок без отслеживания состояния для прочих типов узлов

Эти рекомендации предназначены для рабочих нагрузок без отслеживания состояния, которые выполняются в типах узлов, не являющихся первичными.

**Количество экземпляров виртуальных машин:** для рабочих нагрузок без отслеживания состояния минимальное поддерживаемое количество для типа узла, не являющегося первичным, равно 2. Это дает возможность запустить два экземпляра приложения без отслеживания состояния и позволяет службе перенести потерю экземпляра виртуальной машины. 

> [!NOTE]
> Если кластер работает в Service Fabric версии ниже 5.6, то из-за дефекта в среде выполнения (который исправлен в версии 5.6) масштабирование узла, не являющегося первичным, до числа реплик меньше 5 приводит к утрате работоспособности кластера, которая не восстановится, пока вы не выполните команду [Remove ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/servicefabric/vlatest/Remove-ServiceFabricNodeState), указав имя соответствующего узла. Чтобы узнать больше, прочитайте раздел [Масштабирование кластера Service Fabric с помощью правил автомасштабирования](service-fabric-cluster-scale-up-down.md).
> 
>

**Номер SKU виртуальной машины:** этот тип узла используется для выполнения служб приложения, поэтому выбранный для него номер SKU виртуальной машины должен обеспечивать достаточно ресурсов для общей пиковой загрузки, которую вы планируете разместить в каждом узле. Необходимая загрузка для типа узла определяется рабочей нагрузкой, которую планируется выполнять в кластере. Поэтому мы не можем дать качественные рекомендации именно для вашей рабочей нагрузки, однако ниже приведены общие рекомендации, которые помогут приступить к работе.

Для производственных рабочих нагрузок 


- Рекомендуемый номер SKU виртуальной машины — Standard D3, Standard D3_V2 или их аналог. 
- Минимальный поддерживаемый номер SKU виртуальной машины — Standard D1, Standard D1_V2 или их аналог. 
- Номера SKU виртуальных машин с неполным числом ядер (например, Standard A0) для производственных рабочих нагрузок не поддерживаются.
- Номер SKU Standard A1 не поддерживается для производственных рабочих нагрузок по соображениям производительности.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Дальнейшие действия
Завершив планирование загрузки и настройку кластера, прочитайте следующие разделы:

* [Защита кластера Service Fabric](service-fabric-cluster-security.md)
* [Общие сведения о наблюдении за работоспособностью системы в Service Fabric](service-fabric-health-introduction.md)
* [Связь между типами узлов Service Fabric и масштабируемыми наборами виртуальных машин](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

