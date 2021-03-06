---
title: "Архивация сервера Exchange Server в службу архивации Azure с помощью Сервера резервного копирования Azure | Документация Майкрософт"
description: "Узнайте, как выполнить архивацию сервера Exchange Server в службу архивации Azure с помощью Сервера резервного копирования Azure."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.contentlocale: ru-ru
ms.lasthandoff: 03/27/2017


---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a>Архивация сервера Exchange Server в службу архивации Azure с помощью Сервера резервного копирования Azure
В этой статье описывается, как настроить Сервер резервного копирования Microsoft Azure (MABS) для архивации сервера Microsoft Exchange Server в Azure.  

## <a name="prerequisites"></a>Предварительные требования
Прежде чем продолжить, убедитесь, что Сервер резервного копирования Azure [установлен и подготовлен](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>Агент защиты MABS
Чтобы установить агент защиты MABS на сервер Exchange Server, выполните следующее.

1. Убедитесь, что брандмауэры настроены правильно. Ознакомьтесь со статьей [Настройка исключений брандмауэра для агента](https://technet.microsoft.com/library/Hh758204.aspx).
2. Установите агент на сервере Exchange Server, выбрав **Управление > Агенты > Установить** в консоли администратора MABS. Подробные инструкции см. в статье [Установка агента защиты DPM](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396).

## <a name="create-a-protection-group-for-the-exchange-server"></a>Создание группы защиты для сервера Exchange Server
1. В консоли администратора MABS щелкните **Защита**, затем на ленте инструментов нажмите кнопку **Создать**. Откроется **мастер создания групп защиты**.
2. На экране **приветствия** щелкните **Далее**.
3. На экране **Выбор типа группы защиты** выберите **Серверы** и щелкните **Далее**.
4. Выберите на сервере Exchange Server базу данных, которую требуется защитить, и нажмите кнопку **Далее**.

   > [!NOTE]
   > Если вы хотите защитить базу данных Exchange 2013, изучите [предварительные требования для Exchange 2013](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    В следующем примере выбрана база данных Exchange 2010.

    ![Выберите членов группы](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Выберите способ защиты данных.

    Укажите имя группы защиты и выберите оба следующих параметра:

   * «Мне нужна краткосрочная защита с использованием диска»;
   * «Мне нужна оперативная защита».
6. Нажмите кнопку **Далее**.
7. Выберите параметр **Запустить программу Eseutil для проверки целостности данных** , чтобы проверить целостность баз данных Exchange Server.

    После этого проверка согласованности резервных копий будет выполняться на сервере MABS, что позволит исключить операции ввода-вывода при выполнении команды **eseutil** на сервере Exchange Server.

   > [!NOTE]
   > Чтобы использовать этот параметр, скопируйте файлы Ese.dll и Eseutil.exe в каталог C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin на сервере MABS. В противном случае возникнет следующая ошибка:   
   > ![ошибка Eseutil](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Нажмите кнопку **Далее**.
9. Выберите базу данных для **копирующей архивации**, а затем нажмите кнопку **Далее**.

   > [!NOTE]
   > Если не выбрать «Полное резервное копирование» хотя бы для одной копии базы данных в группе обеспечения доступности баз данных, журналы не будут усечены.
   >
   >
10. Настройте цели **краткосрочного резервного копирования** и нажмите кнопку **Далее**.
11. Проверьте, есть ли на диске свободное место, и нажмите кнопку **Далее**.
12. Выберите время создания сервером MABS начальной репликации и нажмите кнопку **Далее**.
13. Выберите параметры проверки согласованности и нажмите кнопку **Далее**.
14. Выберите базу данных для резервного копирования в Azure и нажмите кнопку **Далее**. Например:

    ![Выбор оперативной защиты данных](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Определите расписание для **службы архивации Azure** и нажмите кнопку **Далее**. Например:

    ![Выбор расписания оперативного резервного копирования](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Обратите внимание, что точки оперативного восстановления создаются на основании точек быстрого полного восстановления. Таким образом, в расписании резервного копирования нужно указать, что точки оперативного восстановления должны создаваться после точек быстрого полного восстановления.
    >
    >
16. Настройте политику хранения для **службы архивации Azure** и нажмите кнопку **Далее**.
17. Выберите параметр оперативной репликации и нажмите кнопку **Далее**.

    Если база данных имеет большой размер, процедура создания первой резервной копии по сети может занять много времени. Чтобы избежать этого, можно создать автономную резервную копию.  

    ![Выбор политики оперативного хранения](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Проверьте параметры и щелкните **Создать группу**.
19. Нажмите кнопку **Закрыть**

## <a name="recover-the-exchange-database"></a>Восстановление базы данных Exchange
1. Чтобы восстановить базу данных Exchange, щелкните **Восстановление** в консоли администратора MABS.
2. Выберите базу данных Exchange, которую требуется восстановить.
3. Выберите точку оперативного восстановления из раскрывающегося списка *Время восстановления* .
4. Щелкните **Восстановить**, чтобы запустить **мастер восстановления**.

Для точек оперативного восстановления существует пять типов восстановления:

* **Восстановить в исходное расположение сервера Exchange.** Данные будут восстановлены на исходный сервер Exchange Server.
* **Восстановить в другую базу данных на сервере Exchange.** Данные будут восстановлены в другую базу данных на другом сервере Exchange Server.
* **Восстановить в базу данных восстановления.** Данные будут восстановлены в базу данных восстановления Exchange (RDB).
* **Копировать в сетевую папку.** Данные будут восстановлены в сетевую папку.
* **Копировать на ленту.** Если на сервере MABS подключена и настроена ленточная библиотека или изолированный ленточный накопитель, то точка восстановления будет скопирована на свободную ленту.

    ![Выбор оперативной репликации](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Дальнейшие действия
* [Часто задаваемые вопросы о службе архивации Azure](backup-azure-backup-faq.md)

