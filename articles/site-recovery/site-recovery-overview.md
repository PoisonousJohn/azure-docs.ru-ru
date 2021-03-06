---
title: "Что такое Azure Site Recovery? | Документация Майкрософт"
description: "Обзор службы Azure Site Recovery, а также сведения о ее развертывании."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: aa657c92f347f7529affee78ad1842e5e066b74d
ms.contentlocale: ru-ru
ms.lasthandoff: 08/01/2017

---
# <a name="what-is-site-recovery"></a>Что такое Site Recovery?

Вас приветствует служба Azure Site Recovery. В этой статье содержится общий обзор этой службы.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Непрерывность бизнес-процессов и аварийное восстановление (BCDR) с помощью служб восстановления Azure

Каждая организация стремится обеспечить безопасное хранение данных, а также выполнение приложений и рабочих нагрузок при запланированных и незапланированных простоях.

Службы Azure Site Recovery помогают реализовать стратегию BCDR.

- **Служба Site Recovery**: Site Recovery гарантирует непрерывность бизнес-процессов, обеспечивая доступ к приложениям, выполняющимся на виртуальных машинах и физических серверах, даже если сайт вышел из строя. Site Recovery реплицирует рабочие нагрузки, выполняющиеся на виртуальных машинах и физических серверах, чтобы они были доступны во вторичном расположении, если основной сайт недоступен. Служба восстанавливает рабочие нагрузки на основном сайте, когда его работа возобновляется.
- **Служба Backup**: кроме того, служба [Azure Backup](https://docs.microsoft.com/azure/backup/) обеспечивает безопасное хранение данных и их восстановление с помощью резервного копирования в Azure.

Site Recovery управляет репликацией для:

- виртуальных машин Azure между регионами;
- локальных виртуальных машин и физических серверов в Azure или на дополнительный сайт.


## <a name="what-does-site-recovery-provide"></a>Какие функции предоставляет служба Site Recovery?

**Компонент** | **Дополнительные сведения**
--- | ---
**Развертывание простого решения BCDR** | С помощью Site Recovery можно настраивать и администрировать репликацию, отработку отказа, а также восстановление размещения из одного расположения на портале Azure.
**Репликация виртуальных машин Azure** | Стратегию BCDR можно настроить таким образом, чтобы виртуальные машины Azure реплицировались между регионами Azure.
**Репликация локальных виртуальных машин в стороннее расположение** | Можно реплицировать локальные виртуальные машины и физические серверы в Azure или вторичное локальное расположение. Репликация в Azure позволит сократить затраты и отказаться от обременительного обслуживания дополнительного центра обработки данных.
**Репликация любых рабочих нагрузок** | Реплицируйте любые рабочие нагрузки, выполняющиеся на поддерживаемых виртуальных машинах Azure, локальных виртуальных машинах Hyper-V, виртуальных машинах VMware и физических серверах под управлением Windows и Linux.
**Надежное и безопасное хранение данных** | Site Recovery управляет репликацией, не мешая передаче данных приложений. Реплицированные данные хранятся в отказоустойчивой службе хранилища Azure. При отработке отказа виртуальные машины Azure создаются на основе реплицированных данных.
**Соответствие RTO и RPO** | Поддерживайте целевое время восстановления (RTO) и целевые точки восстановления (RPO) в пределах, установленных организацией. Site Recovery обеспечивает непрерывную репликацию для виртуальных машин Azure и виртуальных машин VMware. Для Hyper-V частота репликации составляет всего 30 секунд. Можно сократить целевое время восстановления (RTO) с помощью интеграции с [диспетчером трафика Azure](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Согласованность приложений при отработке отказа** | Точки восстановления можно настроить с помощью моментальных снимков, согласованных на уровне приложений. В моментальных снимках, согласованных на уровне приложений, сохраняются расположенные на диске данные, а также вся информация в памяти и все выполняемые транзакции.
**Бесперебойное тестирование** | Вы можете легко осуществить тестовую отработку отказа в рамках тренировочного аварийного восстановления. Это никак не отразится на выполняющейся репликации.
**Выполнение гибкой отработки отказа** | Вы можете запускать запланированные процедуры отработки отказа без потери данных при ожидаемых простоях, а также незапланированные процедуры отработки отказа с минимальной потерей данных (в зависимости от частоты репликации) на случай непредвиденных сбоев. Вы можете легко восстановить размещение на первичном сайте, как только он снова станет доступен.
**Создание планов восстановления** | Можно настроить отработку отказа и восстановление, а также определить их последовательность для многоуровневых приложений на нескольких виртуальных машинах с планами восстановления. В планах можно группировать машины, а также добавлять сценарии и действия, выполняемые вручную. Планы восстановления можно интегрировать с модулями Runbook службы автоматизации Azure.
**Интеграция с существующими технологиями BCDR** | Site Recovery интегрируется с другими технологиями BCDR. Например, можно использовать Site Recovery для защиты серверной части SQL Server корпоративных рабочих нагрузок, включая встроенную поддержку для SQL Server AlwaysOn для управления отработкой отказа групп доступности.
**Интеграция с библиотекой для автоматизации** | Обширная библиотека службы автоматизации Azure содержит готовые к работе и учитывающие особенности приложений скрипты, которые можно скачать и интегрировать с помощью Site Recovery.
**Управление параметрами сети** | Site Recovery интегрируется с Azure, обеспечивая простое сетевое управление приложениями, включая резервирование IP-адресов, настройку подсистем балансировки нагрузки и интеграцию диспетчера трафика Azure для эффективного переключения сетей.


## <a name="what-can-i-replicate"></a>Что можно реплицировать?

**Поддерживаются** | **Дополнительные сведения**
--- | ---
**Что можно реплицировать?** | Виртуальные машины Azure между регионами Azure (в предварительной версии).<br/><br/>  Локальные виртуальные машины VMware, виртуальные машины Hyper-V и физические серверы (Windows и Linux) в Azure<br/.<br/> Локальные виртуальные машины VMware, виртуальные машины Hyper-V и физические серверы во вторичное расположение. Для виртуальных машин Hyper-V репликация во вторичное расположение поддерживается, только если узлы Hyper-V находятся под управлением System Center VMM.
**Какие регионы поддерживаются для Site Recovery?** | [Поддерживаемые регионы](https://azure.microsoft.com/regions/services/) |
**Какие операционные системы можно использовать на реплицируемых компьютерах?** | [Требования для виртуальных машин Azure](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)<br></br>[Требования для виртуальных машин VMware](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> Для виртуальных машин Hyper-V поддерживается любая [гостевая ОС](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows), совместимая с Azure и Hyper-V.<br/><br/> [Требования для физических серверов](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Какие серверы или узлы VMware требуются?** | Виртуальные машины VMware можно разместить на [поддерживаемых узлах vSphere и серверах v-Center](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)
**Какие рабочие нагрузки можно реплицировать?** | Можно реплицировать любую рабочую нагрузку, выполняемую на поддерживающем репликацию компьютере. Кроме того, команда Site Recovery выполнила специальную проверку [ряда приложений](site-recovery-workload.md#workload-summary).


## <a name="azure-portal-considerations"></a>Рекомендации для портала Azure

* Службу Site Recovery можно развернуть на [портале Azure](https://portal.azure.com).
* На классическом портале Azure поддерживается классическая модель управления службой Site Recovery.
- Классический портал следует использовать только для поддержки существующих развернутых служб Site Recovery. Создать новые хранилища с помощью классического портала невозможно.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения см. в статье [Какие рабочие нагрузки можно защитить с помощью службы Azure Site Recovery?](site-recovery-workload.md)
* Начало работы с [репликацией виртуальных машин Azure между регионами](site-recovery-azure-to-azure.md), [репликацией VMware в Azure](vmware-walkthrough-overview.md) или [репликацией Hyper-V в Azure](hyper-v-site-walkthrough-overview.md).

