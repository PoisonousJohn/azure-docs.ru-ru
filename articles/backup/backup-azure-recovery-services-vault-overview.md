---
title: "Обзор хранилищ служб восстановления | Документация Майкрософт"
description: "Обзор и сравнение хранилищ служб восстановления и резервных хранилищ Azure."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/15/2017
ms.author: markgal;arunak
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 19e2aafe3de106be32f3d90c63c0ea03c626f272
ms.contentlocale: ru-ru
ms.lasthandoff: 05/16/2017


---
# <a name="recovery-services-vaults-overview"></a>Обзор хранилищ служб восстановления

В этой статье описываются возможности хранилища служб восстановления. Хранилище службы восстановления — это сущность хранилища в Azure, содержащая данные. Этими данными обычно являются копии данных или сведений о конфигурации для виртуальных машин, рабочих нагрузок, серверов или рабочих станций. Хранилище служб восстановления является версией резервного хранилища, но только на основе модели Resource Manager. Корпорация Майкрософт рекомендует использовать хранилища служб восстановления и преобразовать любые имеющиеся резервные хранилища в хранилища служб восстановления.

## <a name="what-is-a-recovery-services-vault"></a>Что такое хранилище служб восстановления?

Хранилище служб восстановления — это интернет-хранилище в Azure, предназначенное для хранения таких данных, как резервные копии, точки восстановления и политики архивации. Хранилища служб восстановления можно использовать для хранения архивных данных для различных служб Azure, в том числе виртуальных машин IaaS (Windows или Linux) и баз данных SQL Azure. Хранилища служб восстановления поддерживают работу с System Center DPM, Windows Server, сервером резервного копирования Azure и многими другими решениями. Хранилища служб восстановления упрощают организацию архивных данных и одновременно снижают затраты на управление.

В рамках одной подписки Azure можно создать неограниченное число хранилищ служб восстановления.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Сравнение хранилищ служб восстановления и резервных хранилищ

В основе хранилищ служб восстановления лежит модель Azure Resource Manager, а резервные хранилища основаны на модели диспетчера служб Azure. При обновлении резервного хранилища до хранилища служб восстановления архивные данные не изменяются как во время, так и после процесса обновления. Хранилища служб восстановления предоставляют функции, недоступные для резервных хранилищ, в том числе:

- **Расширенные возможности для защиты архивных данных**: с помощью хранилищ служб восстановления служба архивации Azure предоставляет функции безопасности для защиты облачных резервных копий. Эти функции безопасности гарантируют защиту резервных копий и безопасное восстановление данных из облачных резервных копий даже в случае компрометации рабочих и резервных серверов. [Подробнее](backup-azure-security-feature.md)

- **Централизованный мониторинг гибридной ИТ-среды**: с помощью хранилищ служб восстановления на центральном портале можно отслеживать не только [виртуальные машины Azure IaaS](backup-azure-manage-vms.md), но и [локальные ресурсы](backup-azure-manage-windows-server.md#manage-backup-items). [Подробнее](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Управление доступом на основе ролей (RBAC)**: обеспечивает точное управление доступом в Azure. [Azure предоставляет различные встроенные роли](../active-directory/role-based-access-built-in-roles.md), кроме того, в службе архивации Azure доступны три [встроенные роли для управления точками восстановления](backup-rbac-rs-vault.md). Хранилища служб восстановления совместимы с RBAC, что позволяет ограничить доступ к архивации и восстановлению, предоставив его определенному набору ролей пользователей. [Подробнее](backup-rbac-rs-vault.md)

- **Защита всех конфигураций виртуальных машин Azure**: хранилища служб восстановления позволяют защитить виртуальные машины на основе Resource Manager, включая диски уровня "Премиум", управляемые диски и зашифрованные виртуальные машины. Обновление резервного хранилища до хранилища служб восстановления дает возможность обновить виртуальные машины на основе диспетчера служб до виртуальных машин на основе Resource Manager. При обновлении хранилища можно сохранить точки восстановления виртуальных машин на основе диспетчера служб и настроить защиту для обновленных виртуальных машин (на основе Resource Manager). [Подробнее](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Мгновенное восстановление виртуальных машин IaaS**: с помощью хранилищ служб восстановления можно восстановить файлы и папки виртуальной машины IaaS, не восстанавливая всю виртуальную машину, что позволяет сократить время восстановления. Мгновенное восстановление виртуальных машин IaaS доступно для виртуальных машин Linux и Windows. [Подробнее](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Управление хранилищами служб восстановления на портале
Создавать хранилища служб восстановления и управлять ими на портале Azure просто, так как служба архивации интегрирована в колонку "Параметры" Azure. Подобная интеграция означает, что можно создать хранилище служб восстановления и управлять им *в контексте целевой службы*. Например, чтобы просмотреть точки восстановления для виртуальной машины, выберите ее и щелкните **Резервное копирование** в колонке "Параметры". Отобразится информация о резервных копиях к этой виртуальной машины. В этом примере **ContosoVM** — это имя виртуальной машины. **ContosoVM-demovault** — имя хранилища служб восстановления. Не нужно запоминать имя хранилища служб восстановления, содержащего точки восстановления. Его можно узнать из информации о виртуальной машине.  

![Сведения хранилища служб восстановления о виртуальной машине](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Если с помощью одного хранилища служб восстановления защищено несколько серверов, может быть логичнее просмотреть сведения о хранилище служб восстановления. Можно найти все хранилища служб восстановления в подписке и выбрать нужное из списка.

В следующих разделах содержатся ссылки на статьи, в которых объясняется, как использовать хранилище служб восстановления для каждого типа действия.

### <a name="back-up-data"></a>Архивация данных
- [Архивация виртуальной машины Azure](backup-azure-vms-first-look-arm.md)
- [Архивация Windows Server или рабочей станции Windows](backup-try-azure-backup-in-10-mins.md)
- [Архивация рабочих нагрузок DPM в Azure](backup-azure-dpm-introduction.md)
- [Подготовка к архивации рабочих нагрузок с помощью сервера резервного копирования Azure](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Управление точками восстановления
- [Управление архивацией виртуальных машин Azure](backup-azure-manage-vms.md)
- [Управление файлами и папками](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Восстановление данных из хранилища
- [Восстановление отдельных файлов виртуальной машины Azure](backup-azure-restore-files-from-vm.md)
- [Восстановление виртуальной машины Azure](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Защита хранилища
- [Защита облачных архивных данных в хранилищах служб восстановления](backup-azure-security-feature.md)



## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими темами, прочитав соответствующие статьи:</br>
[Архивация виртуальной машины IaaS](backup-azure-arm-vms-prepare.md)</br>
[Архивация сервера резервного копирования Azure](backup-azure-microsoft-azure-backup.md)</br>
[Архивация Windows Server](backup-configure-vault.md)

