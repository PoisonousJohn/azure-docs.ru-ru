---
title: "Настройка сетевого сопоставления для репликации виртуальных машин Hyper-V в облаках VMM в Azure с помощью Azure Site Recovery | Документация Майкрософт"
description: "Здесь описывается, как настроить сетевое сопоставление при репликации виртуальных машин Hyper-V в облаках VMM в Azure с помощью Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.contentlocale: ru-ru
ms.lasthandoff: 07/26/2017

---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a>Шаг 9. Настройка сетевого сопоставления для репликации Hyper-V (с VMM) в Azure

После настройки [исходных и целевых параметров репликации](vmm-to-azure-walkthrough-source-target.md) воспользуйтесь сведениями в этой статье, чтобы настроить сетевое сопоставление между локальными сетями виртуальной машины VMM и сетями Azure.

Комментарии или вопросы можно добавить в конце этой статьи или на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Перед началом работы

- Узнайте о [сопоставлении сети](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).
- [Подготовьте VMM к сопоставлению сети](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Проверьте, подключены ли виртуальные машины на сервере VMM к сети виртуальной машины и создана ли по крайней мере одна виртуальная сеть Azure. Несколько сетей виртуальных машин можно сопоставить с одной сетью Azure.

## <a name="configure-mapping"></a>Настройка сопоставления

Настройте сопоставление следующим образом:

1. Выберите **Site Recovery Infrastructure** (Инфраструктура Site Recovery) > **Сетевые сопоставления** > **Сетевое сопоставление** и щелкните значок **+Network Mapping** (+Сетевое сопоставление).

    ![Сетевое сопоставление](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. В разделе **Добавление сетевого сопоставления** выберите исходный сервер VMM, а в качестве целевого сервера — **Azure**.
3. Проверьте подписку и модель развертывания после отработки отказа.
4. На странице **Исходная сеть**в списке, связанном с сервером VMM, выберите исходную сеть локальной виртуальной машины, которую нужно сопоставить.
5. На странице **Целевая сеть**выберите сеть Azure, в которой будут размещены реплицированные виртуальные машины Azure после создания. Нажмите кнопку **ОК**.

    ![Сетевое сопоставление](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Когда начинается сопоставление сетей, происходит следующее:

* При запуске сопоставления существующие виртуальные машины в исходной сети виртуальных машин подключаются к целевой сети. Новые виртуальные машины, подключенные к исходной сети виртуальных машин, будут подключены к сопоставленной сети Azure после репликации.
* Если изменить имеющееся сопоставление сетей, реплики виртуальных машин подключаются с новыми параметрами.
* Если целевая сеть включает несколько подсетей и одна из этих подсетей имеет то же имя, что и подсеть, в которой размещается исходная виртуальная машина, то реплика виртуальной машины подключается к этой целевой подсети после отработки отказа.
* Если подсети с таким же именем отсутствуют, виртуальная машина подключается к первой подсети в сети.



## <a name="next-steps"></a>Дальнейшие действия

Перейдите к статье [Шаг 10. Настройка политики репликации для репликации виртуальной машины Hyper-V (с VMM) в Azure](vmm-to-azure-walkthrough-replication.md).

