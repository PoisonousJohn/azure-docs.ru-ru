---
title: "Необходимые условия для репликации физического сервера в Azure с использованием Azure Site Recovery | Документация Майкрософт"
description: "В этой статье перечислены предварительные требования для репликации в Azure рабочих нагрузок, запущенных на физических серверах Windows и Linux, с помощью службы Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 5f5cb4b9b6fcee6b56ccdcb6223afddd9de90fec
ms.contentlocale: ru-ru
ms.lasthandoff: 06/29/2017

---

# <a name="step-2-review-the-prerequisites-for-physical-server-to-azure-replication"></a>Шаг 2. Просмотр предварительных требований для репликации физического сервера в Azure

Ознакомьтесь с предварительными требованиями, перечисленными в таблице.


**Предварительные требования** | **Дополнительные сведения**
--- | ---
**Таблицы Azure** | Ознакомьтесь с [требованиями Azure](site-recovery-prereq.md#azure-requirements).
**Локальный сервер конфигурации** | Вам потребуется физический сервер или виртуальная машина VMware под управлением Windows Server 2012 R2 или более поздней версии. Настройте этот сервер при развертывании Site Recovery.<br/><br/> По умолчанию сервер обработки и главный целевой сервер также устанавливаются на этот компьютер. При увеличении масштаба может понадобиться отдельный сервер обработки. В таком случае для него будут те же требования, что и для сервера конфигурации.<br/><br/> [Подробнее](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Локальные виртуальные машины** | Компьютеры, которые нужно реплицировать, должны работать под управлением [поддерживаемой операционной системы](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) и соответствовать [предварительным требованиям Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**URL-адреса** | Серверу конфигурации требуется доступ к следующим URL-адресам.<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> При использовании правил брандмауэра на основе IP-адресов убедитесь, что эти правила разрешают обмен данными с Azure.<br/></br> Разрешите доступ для [диапазонов IP-адресов центра обработки данных Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) и использование порта HTTPS (443).<br/></br> Необходимо разрешить доступ для диапазонов IP-адресов региона Azure, в котором располагается ваша подписка, и региона "Западная часть США" (используется для контроля доступа и управления удостоверениями).<br/><br/> Разрешите доступ к URL-адресу http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi для скачивания MySQL.
**Служба Mobility Service** | Должна быть установлена на каждом реплицированном сервере.




## <a name="limitations"></a>Ограничения

Прежде чем начать развертывание, ознакомьтесь с ограничениями, описанными в таблице.

**Ограничения** | **Дополнительные сведения**
--- | ---
**Таблицы Azure** | Учетные записи хранилища и сети должны находиться в том же регионе, что и хранилище.<br/><br/> При использовании учетной записи хранилища класса Premium для хранения журналов репликации также понадобится учетная запись хранилища класса Standard.<br/><br/> Нельзя выполнить репликацию в учетные записи класса Premium в регионах "Центральная Индия" и "Южная Индия".
**Локальный сервер конфигурации** | Если в качестве компьютера сервера конфигурации используется виртуальная машина VMware, адаптер виртуальной машины VMware должен иметь тип VMXNET3. В обратном случае [установите это обновление](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1).<br/><br/> При использовании виртуальной машины VMware на ней нужно установить vSphere PowerCLI 6.0.<br/><br> Компьютер не должен быть контроллером домена.<br/><br/> Компьютеру должен быть присвоен статический IP-адрес.<br/><br/> Имя узла должно содержать 15 знаков или меньше, а операционная система должна быть на английском языке.
**Репликация компьютеров** | Проверьте [ограничения виртуальной машины Azure](site-recovery-prereq.md#azure-requirements).<br/><br/> Нельзя реплицировать виртуальные машины с зашифрованными дисками или дисками с типом загрузки UEFI или EFI.<br/><br> Кластеры общих дисков не поддерживаются. Если исходная виртуальная машина содержит объединение сетевых адаптеров, то после отработки отказа оно преобразуется в единый сетевой адаптер.<br/><br/> При использовании диска iSCSI на виртуальной машине после отработки отказа Site Recovery преобразует его в VHD-файл. Если виртуальная машина Azure может получить доступ к целевому объекту iSCSI, она подключится к нему и будет использовать и этот диск, и диск VHD. В таком случае отключите целевой объект iSCSI.<br/><br/> Если вы хотите поддерживать согласованность нескольких виртуальных машин, благодаря чему виртуальные машины с одинаковой рабочей нагрузкой могут совместно восстанавливаться до согласованной точки данных, откройте порт 20004 на виртуальной машине.<br/><br/> Windows необходимо установить на диск C. Диск операционной системы должен быть базовым, а не динамическим. Диск данных может быть динамическим.<br/><br/> Файлы Linux /etc/hosts на виртуальных машинах должны содержать записи, которые связывают имя локального узла с IP-адресами всех сетевых адаптеров. Имена узлов, точки подключения, имена устройств и системные пути и имена файлов (/etc, /usr) должны быть только на английском языке.<br/><br/> Поддерживаются определенные типы [хранилищ Linux](site-recovery-support-matrix-to-azure.md#support-for-storage).<br/><br/>Создайте или задайте в параметрах виртуальной машины **disk.enableUUID=true**. Это необходимо для обеспечения согласованного UUID для правильного подключения VMDK и гарантирует, что во время восстановления размещения на локальный компьютер будут переноситься только разностные изменения без полной репликации.


## <a name="next-steps"></a>Дальнейшие действия

- При выполнении полного развертывания перейдите к статье [Step 3: Plan capacity and scaling for physical server to Azure replication](physical-walkthrough-capacity.md) (Шаг 3. Планирование производительности и масштабирования для репликации физического сервера в Azure).
- При выполнении простого тестового развертывания перейдите к разделу [Шаг 4. Планирование сетей для репликации из Hyper-V в Azure](physical-walkthrough-network.md).

