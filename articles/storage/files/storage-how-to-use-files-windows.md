---
title: "Подключение общей папки Azure и получение доступа к этой папке в Windows | Документация Майкрософт"
description: "Подключение общей папки Azure и получение доступа к этой папке в Windows."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 67b8e2e0039c8bc63f50f177e3c0d18b07df45e6
ms.contentlocale: ru-ru
ms.lasthandoff: 08/21/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Подключение общей папки Azure и получение доступа к этой папке в Windows
[Хранилище файлов Azure](../storage-dotnet-how-to-use-files.md) — это простая в использовании облачная файловая система Майкрософт. Файловые ресурсы Azure можно подключить в Windows и Windows Server. В этой статье описывается три разных способа подключения общей папки Azure в Windows: с помощью пользовательского интерфейса проводника, PowerShell, а также командной строки. 

Чтобы подключить общую папку Azure за пределами региона Azure, в котором она размещается, например локально или в другом регионе Azure, операционная система должна поддерживать протокол SMB 3.0. 

В зависимости от версии операционной системы общую папку Azure можно подключить локально на компьютере под управлением Windows или на виртуальной машине Azure. Для иллюстрации воспользуйтесь следующей таблицей. 

| Версия Windows        | Версия SMB |Возможность подключения на виртуальной машине Azure|Возможность подключения в локальной среде|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Да                 | Нет                  |
| Windows Server 2008 R2 | SMB 2.1     | Да                 | Нет                  |
| Windows 8              | SMB 3.0     | Да                 | Да                 |
| Windows Server 2012    | SMB 3.0     | Да                 | Да                 |
| Windows Server 2012 R2 | SMB 3.0     | Да                 | Да                 |
| Windows 10             | SMB 3.0     | Да                 | Да                 |

> [!Note]  
> Мы всегда рекомендуем использовать последнюю версию KB для своей версии Windows.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Предварительные требования для подключения общей папки Azure с помощью Windows 
* **Имя учетной записи хранения**: чтобы подключить общую папку Azure, вам потребуется имя учетной записи хранения.

* **Ключ учетной записи хранения**: чтобы подключить общую папку Azure, вам потребуется первичный (или вторичный) ключ учетной записи хранения. В настоящее время ключи SAS для подключения не поддерживаются.

* **Откройте порт 445**: хранилище файлов Azure использует протокол SMB. Взаимодействие SMB выполняется через TCP-порт 445. Проверьте, чтобы брандмауэр не блокировал TCP-порты 445 с клиентского компьютера.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>Подключение общей папки Azure с помощью проводника
> [!Note]  
> Имейте в виду, что следующие инструкции приведены для Windows 10 и могут немного отличаться для более поздних версий. 

1. **Откройте проводник**: это можно сделать, открыв меню "Пуск" или с помощью клавиш WIN+E.

2. **Перейдите к элементу "Этот компьютер" в левой области окна. Меню, доступные на ленте, изменятся. В меню "Компьютер" выберите раздел "Подключение сетевого диска"**.
    
    ![Снимок экрана раскрывающегося меню "Подключение сетевого диска"](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Скопируйте UNC-путь из области подключения на портале Azure**: подробные сведения об этом см. [здесь](storage-how-to-use-files-portal.md#connect-to-file-share).

    ![UNC-путь области подключения хранилища файлов Azure](./media/storage-how-to-use-files-windows/portal_netuse_connect.png)

4. **Выберите букву диска и введите UNC-путь.** 
    
    ![Снимок экрана диалогового окна "Подключение сетевого диска"](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Используйте имя учетной записи хранения с префиксом `Azure\` как имя пользователя и ключ учетной записи хранения в качестве пароля.**
    
    ![Снимок экрана диалогового окна сетевых учетных данных](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Выберите общую папку Azure**.
    
    ![Общая папка Azure теперь подключена](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Когда вы будете готовы отключить общую папку Azure, это можно сделать, щелкнув правой кнопкой мыши запись для общей папки в разделе проводника "Сетевые расположения" и выбрав параметр "Отключить"**.

## <a name="mount-the-azure-file-share-with-powershell"></a>Подключение общей папки Azure с помощью PowerShell
1. **Используйте следующую команду для подключения общей папки Azure**. Не забудьте заменить значения `<storage-account-name>`, `<share-name>`, `<storage-account-key>` и `<desired-drive-letter>` соответствующим образом.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Выберите общую папку Azure**.

3. **Когда вы закончите, отключите общую папку Azure, используя следующую команду**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Вы можете использовать параметр `-Persist` для `New-PSDrive`, чтобы во время подключения сделать общую папку Azure видимой для остальной части операционной системы.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>Подключение общей папки Azure с помощью командной строки
1. **Используйте следующую команду для подключения общей папки Azure**. Не забудьте заменить значения `<storage-account-name>`, `<share-name>`, `<storage-account-key>` и `<desired-drive-letter>` соответствующим образом.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Выберите общую папку Azure**.

3. **Когда вы закончите, отключите общую папку Azure, используя следующую команду**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Вы можете настроить общую папку Azure для автоматического повторного подключения при перезагрузке. Для этого сохраните учетные данные в Windows. Следующая команда сохранит учетные данные:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Дальнейшие действия
Дополнительную информацию о хранилище файлов Azure см. по этим ссылкам.

* [Часто задаваемые вопросы](../storage-files-faq.md)
* [Troubleshoot Azure File storage problems in Windows](storage-troubleshoot-windows-file-connection-problems.md) (Устранение неполадок хранилища файлов Azure в Windows)      

### <a name="conceptual-articles-and-videos"></a>Тематические статьи и видео
* [Azure Files Storage: a frictionless cloud SMB file system for Windows and Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/) (Хранилище файлов Azure: удобная облачная файловая система SMB для Windows и Linux)
* [Использование хранилища файлов Azure в Linux](../storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Поддержка средств для работы с хранилищем файлов Azure
* [Использование AzCopy со службой хранилища Microsoft Azure](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Использование интерфейса командной строки (CLI) Azure со службой хранилища Azure](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Troubleshoot Azure File storage problems in Windows](storage-troubleshoot-windows-file-connection-problems.md) (Устранение неполадок хранилища файлов Azure в Windows)
* [Troubleshoot Azure File storage problems in Linux](storage-troubleshoot-linux-file-connection-problems.md) (Устранение неполадок хранилища файлов Azure в Linux)

### <a name="blog-posts"></a>Записи блога
* [Хранилище файлов Azure стало общедоступным](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Хранилище файлов Azure: взгляд изнутри)
* [Введение в службы файлов Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrating Data to Microsoft Azure Files](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/) (Перемещение данных в файлы Microsoft Azure)

### <a name="reference"></a>Справочные материалы
* [Справочник по клиентской библиотеке хранилища для .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Справочник по REST API службы файлов](http://msdn.microsoft.com/library/azure/dn167006.aspx)

