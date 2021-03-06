---
title: "Управление каталогом архивов StorSimple | Документация Майкрософт"
description: "В этой статье описываются способы использования страницы каталога резервного копирования службы диспетчера StorSimple для перечисления, выбора и удаления резервных наборов данных для тома."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 5ee9855e1428c7a2d871d9c215d302c5c3b7101a
ms.contentlocale: ru-ru
ms.lasthandoff: 07/06/2017


---
# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Использование службы диспетчера StorSimple для управления каталогом резервного копирования
## <a name="overview"></a>Обзор
На странице **Каталог резервного копирования** службы StorSimple Manager находятся все наборы резервных копий, созданных во время ручной или запланированной архивации. Эта страница позволяет просмотреть все резервные копии для определенной политики резервного копирования или определенного тома, выбрать или удалить резервные копии или использовать резервную копию для восстановления или клонирования тома.

В этом учебнике описывается, как выбирать и удалять резервный набор данных. Сведения о восстановлении устройства из архива см. в разделе [Восстановление тома StorSimple из резервного набора данных](storsimple-restore-from-backup-set.md). Сведения о клонировании тома см. в разделе [Клонирование тома с помощью службы диспетчера StorSimple](storsimple-clone-volume.md).

![Каталог резервного копирования](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

На странице **Каталог резервного копирования** можно создать запрос, который поможет сузить спектр выбранных резервных наборов данных. Вы можете фильтровать полученные резервные наборы данных по следующим параметрам:

* **Устройство** — устройство, на котором был создан резервный набор данных.
* **Политика или том резервного копирования** — политика или том резервного копирования, связанные с этим резервным набором данных.
* **От и до** — диапазон дат и времени создания резервного набора данных.

Затем отфильтрованные резервные наборы данных будут представлены в табличной форме на основе следующих атрибутов:

* **Имя** — имя политики резервного копирования или тома, связанное с резервным набором данных.
* **Размер** — фактический размер резервного набора данных.
* **Создано** — дата и время создания резервных копий. 
* **Тип** — наборы резервного копирования могут представлять собой локальные моментальные снимки или облачные моментальные снимки. Локальный моментальный снимок — это резервная копия всех данных тома, которая хранится локально на устройстве, а облачный моментальный снимок — это резервная копия данных тома, хранящаяся в облаке. Локальные моментальные снимки обеспечивают более быстрый доступ, а облачные моментальные снимки выбираются для обеспечения устойчивости данных.
* **Инициировано** — резервные копии могут инициироваться автоматически по расписанию или вручную пользователем. Для планирования резервного копирования можно использовать политику резервного копирования. Кроме того, можно использовать параметр **Создать резервную копию** для резервного копирования вручную.

## <a name="list-backup-sets-for-a-volume"></a>Создание списка резервных наборов данных для тома
Выполните следующие действия, чтобы создать список всех резервных копий для тома.

#### <a name="to-list-backup-sets"></a>Для создания списка резервных наборов данных
1. На странице службы диспетчера StorSimple щелкните вкладку **Каталог резервных копий** .
2. Отфильтруйте выбранные элементы следующим образом:
   
   1. Выберите подходящее устройство.
   2. В раскрывающемся списке выберите том для просмотра соответствующих резервных копий.
   3. Укажите интервал времени.
   4. Щелкните значок с изображением флажка  ![значок с изображением флажка](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , чтобы выполнить этот запрос.
      
      В списке резервных наборов данных должны отобразиться резервные копии, связанные с выбранным томом.

## <a name="select-a-backup-set"></a>Выбор резервного набора данных
Чтобы выбрать резервный набор данных для тома или политики резервного копирования, выполните следующие действия.

#### <a name="to-select-a-backup-set"></a>Для выбора резервного набора данных
1. На странице службы диспетчера StorSimple щелкните вкладку **Каталог резервных копий** .
2. Отфильтруйте выбранные элементы следующим образом:
   
   1. Выберите подходящее устройство.
   2. В раскрывающемся списке выберите том или политику резервного копирования для той резервной копии, которую нужно выбрать.
   3. Укажите интервал времени.
   4. Щелкните значок с изображением флажка  ![значок с изображением флажка](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , чтобы выполнить этот запрос.
      
      В списке резервных наборов данных должны отобразиться резервные копии, связанные с выбранным томом или политикой резервного копирования.
3. Выберите и разверните резервный набор данных. Параметры **Восстановить** и **Удалить** отображаются в нижней части страницы. С выбранным резервным набором данных можно выполнить одно из следующих действий.

## <a name="delete-a-backup-set"></a>Удаление резервного набора данных
Удаление резервной копии, если больше не требуется хранить связанные с ней данные. Чтобы удалить резервный набор данных, выполните указанные ниже действия.

#### <a name="to-delete-a-backup-set"></a>Чтобы удалить резервный набор данных
1. На странице службы диспетчера StorSimple откройте вкладку **Каталог резервного копирования**.
2. Отфильтруйте выбранные элементы следующим образом:
   
   1. Выберите подходящее устройство.
   2. В раскрывающемся списке выберите том или политику резервного копирования для той резервной копии, которую нужно выбрать.
   3. Укажите интервал времени.
   4. Щелкните значок с изображением флажка  ![значок с изображением флажка](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) , чтобы выполнить этот запрос.
      
      В списке резервных наборов данных должны отобразиться резервные копии, связанные с выбранным томом или политикой резервного копирования.
3. Выберите и разверните резервный набор данных. Параметры **Восстановить** и **Удалить** отображаются в нижней части страницы. Нажмите кнопку **Delete**(Удалить).
4. Во время выполнения удаления и его успешного завершения вы получите уведомление. После завершения удаления обновите запрос на этой странице. Удаленный резервный набор данных больше не будет отображаться в списке резервных наборов данных.

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте об использовании каталога резервного копирования [для восстановления устройства с помощью набора архивации](storsimple-restore-from-backup-set.md).
* Узнайте об [использовании службы диспетчера StorSimple для администрирования устройства StorSimple](storsimple-manager-service-administration.md).


