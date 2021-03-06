---
title: "Сопоставлений сетей для репликации виртуальной машины Hyper-V на дополнительный сайт с использованием Azure Site Recovery | Документация Майкрософт"
description: "Здесь описывается, как сопоставить сети при репликации виртуальных машин Hyper-V на дополнительный сайт VMM с помощью Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.contentlocale: ru-ru
ms.lasthandoff: 08/01/2017

---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a>Шаг 7. Сопоставление сетей для репликации виртуальных машин Hyper-V на дополнительный сайт


После настройки [исходных и целевых параметров](vmm-to-vmm-walkthrough-source-target.md) для репликации виртуальных машин Hyper-V на дополнительный сайт System Center Virtual Machine Manager (VMM) воспользуйтесь сведениями в этой статье для настройки сопоставления сетей для репликации виртуальной машины Hyper-V на дополнительный сайт с помощью [Azure Site Recovery](site-recovery-overview.md).

Любые комментарии, которые возникнут после прочтения этой статьи, можно добавить в конце этой страницы или на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="before-you-start"></a>Перед началом работы

- Прежде чем начинать работу, изучите сведения о [сетевом сопоставлении](vmm-to-vmm-walkthrough-network.md#network-mapping-overview).
- Проверьте, подключены ли виртуальные машины на серверах VMM к сети виртуальной машины.

## <a name="configure-network-mapping"></a>Настройка сетевого сопоставления

1. В области **Сетевое сопоставление** > **Сетевые сопоставления** щелкните **+Network Mapping** (+Сетевое сопоставление).
2. На вкладке **Добавление сетевого сопоставления** выберите исходный и целевой серверы VMM. Будут получены сети виртуальных машин, связанные с серверами VMM.
3. В разделе **Исходная сеть**из списка сетей виртуальных машин, связанных с сервером-источником VMM, выберите нужную сеть.
4. В разделе **Целевая сеть** выберите сеть, которую вы хотите использовать для сервера-получателя VMM. Нажмите кнопку **ОК**.

    ![Сетевое сопоставление](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Когда начинается сопоставление сетей, происходит следующее:

* Все существующие виртуальные машины реплик, соответствующие исходной сети виртуальной машины, будут подключены к целевой сети виртуальной машины.
* Новые виртуальные машины, подключенные к исходной сети виртуальной машины, будут подключены к целевой сопоставленной сети после репликации.
* Если изменить существующее сопоставление, указав новую сеть, реплики виртуальных машин будут подключаться с новыми параметрами.
* Если целевая сеть включает несколько подсетей и одна из этих подсетей имеет то же имя, что и подсеть, в которой размещается исходная виртуальная машина, то реплика виртуальной машины будет подключена к этой целевой подсети после отработки отказа. Если нет подсетей с таким же именем, виртуальная машина будет подключена к первой подсети в сети.



## <a name="next-steps"></a>Дальнейшие действия

Перейдите к статье [Шаг 8. Настройка политики репликации](vmm-to-vmm-walkthrough-replication.md).
