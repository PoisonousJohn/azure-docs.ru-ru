---
title: "Действия перед началом репликации виртуальных машин Azure в другой регион | Документация Майкрософт"
description: "В этой статье кратко описаны шаги, которые нужно выполнить до начала репликации виртуальных машин Azure между регионами Azure с помощью службы Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: d38fc766d5226be7161433555da9622e006c80e9
ms.contentlocale: ru-ru
ms.lasthandoff: 08/31/2017

---

# <a name="step-2-before-you-start"></a>Шаг 2. Перед началом работы

Когда вы закончите изучение [архитектуры](azure-to-azure-walkthrough-architecture.md) для репликации виртуальных машин Azure между регионами Azure с помощью [Azure Site Recovery](site-recovery-overview.md), переходите к этой статье для проверки обязательных предварительных условий.

- Изучив эту статью, вы узнаете, что вам нужно для создания такого развертывания, и выполните все обязательные шаги.
- Все комментарии можно добавить в конце этой статьи. Вопросы можно задать на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Репликация виртуальных машин Azure сейчас доступна в режиме предварительной версии.



## <a name="support-recommendations"></a>Рекомендации по поддержке

См. таблицу ниже. Полный список требований поддержки см. в [матрице поддержки](site-recovery-support-matrix-azure-to-azure.md).

**Компонент** | **Требование**
--- | ---
**Хранилище служб восстановления** | Мы рекомендуем создать хранилище служб восстановления в целевом регионе Azure, который вы будете использовать для аварийного восстановления. Например, если вы хотите реплицировать виртуальные машины из региона "Восточная часть США" в регион "Центральная часть США", создайте хранилище в регионе "Центральная часть США".
**Подписка Azure.** | Ваша подписка должна включать возможность создавать виртуальные машины Azure в целевом расположении, которое вы будете использовать в качестве региона аварийного восстановления. Свяжитесь со службой поддержки, чтобы включить необходимые квоты.
**Емкость в целевом регионе** | Ваша подписка должна иметь достаточную емкость в целевом регионе Azure для всех виртуальных машин, учетных записей хранения и сетевых компонентов.
**Хранилище** | Примените к виртуальным машинам Azure, которые будут выполнять роль источника, все [рекомендации по организации хранилища](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks), чтобы избежать проблем с производительностью.<br/><br/> Учетные записи хранилища должны находиться в том же регионе, что и хранилище.<br/><br/> Нельзя выполнить репликацию в учетные записи класса Premium в регионах "Центральная Индия" и "Южная Индия".<br/><br/> Когда вы развертываете репликацию с параметрами по умолчанию, Site Recovery создает все необходимые учетные записи хранения на основе исходной конфигурации. Если вы решите изменить эти параметры, соблюдайте [целевые показатели масштабируемости для дисков виртуальных машин](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Сеть** | Вам нужно разрешить исходящие подключения от виртуальных машин Azure к определенным URL-адресам или диапазонам IP-адресов.<br/><br/> Учетные записи сети должны находиться в том же регионе, что и хранилище.
**Azure** | Убедитесь, что на виртуальной машине Azure (Windows или Linux) установлены все свежие корневые сертификаты. Если это не так, из-за ограничений безопасности вы не сможете зарегистрировать виртуальную машину в Site Recovery.
**Учетная запись Azure** | Учетная запись пользователя Azure должна содержать определенные [разрешения](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) для включения репликации виртуальной машины Azure.


## <a name="set-permissions-on-the-account"></a>Установка разрешений для учетной записи

1. Узнайте, какие [разрешения](site-recovery-role-based-linked-access-control.md) необходимы для репликации.
2. Чтобы добавить нужные разрешения, воспользуйтесь [этими инструкциями](../active-directory/role-based-access-control-configure.md#add-access).


## <a name="verify-target-resources"></a>Проверка целевых ресурсов

1. Предоставьте используемой подписке Azure возможность создавать виртуальные машины в целевом регионе, который вы будете использовать для аварийного восстановления. Свяжитесь со службой поддержки, чтобы включить необходимые квоты.
2. Убедитесь, что подписка располагает достаточными ресурсами для поддержки виртуальных машин таких же размеров, которые установлены для исходных виртуальных машин. По умолчанию при настройке репликации Site Recovery выбирает для целевой виртуальной машины такой же размер или похожий. Дополнительные сведения об [устранении неполадок](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) с целевыми ресурсами.

## <a name="verify-azure-vm-certificates"></a>Проверка сертификатов виртуальной машины Azure

1. Убедитесь, что на виртуальной машине Windows или Linux, которую вы будете реплицировать, установлены все свежие корневые сертификаты. Если новых корневых сертификатов нет, виртуальную машину нельзя регистрировать в Site Recovery из-за ограничений безопасности.
2. На виртуальных машинах Windows выполните следующее:

    - Установите на виртуальной машине все последние обновления Windows, чтобы гарантировать присутствие всех доверенных корневых сертификатов.
    - При работе в отключенной среде выполните стандартную процедуру обновления Windows и (или) обновления сертификатов, которая принята в вашей организации, чтобы установить на виртуальные машины все свежие корневые сертификаты и обновленный список отзыва сертификатов.
3. На виртуальных машин Linux выполните инструкции распространителя Linux, чтобы установить на виртуальные машины все свежие доверенные корневые сертификаты и обновленный список отзыва сертификатов. Дополнительные сведения об [устранении неполадок](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) с доверенными корневыми сертификатами.


## <a name="next-steps"></a>Дальнейшие действия

Перейдите к статье, посвященной [планированию сети](azure-to-azure-walkthrough-network.md), чтобы настроить исходящую связь.

