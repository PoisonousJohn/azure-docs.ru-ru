---
title: "Настройка политики репликации для репликации виртуальных машин Hyper-V (без System Center VMM) в Azure с помощью Azure Site Recovery | Документация Майкрософт"
description: "В этой статье кратко описаны шаги для настройки политики репликации при выполнении репликации виртуальных машин Hyper-V в службу хранилища Azure."
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: ca5bec5cf1152e6259b9fe7a869edd2d62b88e1a
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---

# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a>Шаг 9. Настройка политики репликации для репликации виртуальных машин Hyper-V в Azure

В этой статье описывается настройка политики репликации при выполнении репликации виртуальных машин Hyper-V в Azure (без System Center VMM) с помощью службы [Azure Site Recovery](site-recovery-overview.md) на портале Azure.


Комментарии или вопросы можно добавить в конце этой статьи или на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="about-snapshots"></a>Сведения о моментальных снимках

Hyper-V использует два типа резервного копирования: стандартное резервное копирование, обеспечивающее создание добавочных резервных копий всей виртуальной машины, и соответствующие приложению моментальные снимки, обеспечивающие создание моментального снимка данных приложений в виртуальной машине на момент времени.
    - Функция моментальных снимков, соответствующих приложению, использует службу теневого копирования томов (VSS), чтобы гарантировать согласованное состояние приложений на момент создания снимка.
    - Использование моментальных снимков, согласованных на уровне приложений, влияет на производительность приложений, выполняющихся на исходных виртуальных машинах. Заданное значение должно быть меньше числа настроенных дополнительных точек восстановления.

## <a name="set-up-a-replication-policy"></a>Настройка политики репликации

1. Чтобы создать политику репликации, выберите **Подготовка инфраструктуры** > **Параметры репликации** > **Создание и связывание**.

    ![Сеть](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. На странице **Создать и связать политику**укажите имя политики.
3. В раскрывающемся списке **Частота копирования**выберите периодичность репликации изменений данных после начальной репликации (каждые 30 секунд, 5 или 15 минут).

    > [!NOTE]
    > Частота 30 секунд не поддерживается при репликации в хранилище класса Premium. Это ограничение связано с ограничением в 100 моментальных снимков на большой двоичный объект, которые можно сохранить в хранилище класса Premium. [Подробнее](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob).

4. В поле **Хранение точки восстановления** укажите продолжительность периода хранения для каждой точки восстановления (в часах). Виртуальные машины можно будет восстановить до любой точки в пределах этого периода.
5. В поле **Периодичность создания моментальных снимков с согласованием приложений** укажите, как часто (от 1-12 до часов) нужно создавать точки восстановления, содержащие согласованные на уровне приложений моментальные снимки.
6. В поле **Время запуска начальной репликации** укажите, когда должна запускаться начальная репликация. При репликации используется полоса пропускания Интернета. Поэтому проведение репликации нужно запланировать на свободное от работы время. Нажмите кнопку **ОК**.

    ![Политика репликации](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

При создании политики она автоматически сопоставляется с сайтом Hyper-V. Сайт Hyper-V (и входящие в него виртуальные машины) можно связать с несколькими политиками репликации, последовательно выбрав **Репликация** > имя_политики > **Связать сайт Hyper-V**.



## <a name="next-steps"></a>Дальнейшие действия

Перейдите к статье [Шаг 10. Включение репликации для физических серверов в Azure](hyper-v-site-walkthrough-enable-replication.md).

