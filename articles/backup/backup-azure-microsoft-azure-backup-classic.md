---
title: "Архивация рабочих нагрузок DPM на классический портал Azure с помощью Сервера резервного копирования Azure | Документация Майкрософт"
description: "Правильная подготовка среды к резервному копированию рабочих нагрузок с помощью сервера службы архивации Azure."
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "сервер службы архивации Azure; хранилище службы архивации"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: ffef289e154986e4b08a072d3a95f77818fb9c35
ms.contentlocale: ru-ru
ms.lasthandoff: 08/03/2017

---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Подготовка к резервному копированию рабочих нагрузок с использованием сервера службы архивации Azure
> [!div class="op_single_selector"]
> * [Сервер службы архивации Azure](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Сервер службы архивации Azure (классическая модель)](backup-azure-microsoft-azure-backup-classic.md)
> * [Диспетчер SCDPM (классическая модель)](backup-azure-dpm-introduction-classic.md)
>
>

В этой статье рассматривается подготовка среды к резервному копированию рабочих нагрузок с помощью сервера службы архивации Azure. Используя сервер службы архивации Azure, вы можете создавать резервные копии рабочих нагрузок приложения, таких как виртуальные машины Hyper-V, Microsoft SQL Server, SharePoint Server, Microsoft Exchange и клиенты Windows, с помощью единой консоли.

> [!WARNING]
> Сервер службы архивации Azure наследует функциональные возможности Data Protection Manager (DPM) для резервного копирования рабочей нагрузки. Вы найдете ссылки на документацию по DPM для некоторых из этих возможностей. Однако сервер службы архивации Azure не обеспечивает защиту данных с помощью ленточных накопителей и не интегрируется с System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Компьютер Windows Server
![Шаг 1](./media/backup-azure-microsoft-azure-backup/step1.png)

Для начала работы с сервером службы архивации Azure нужно иметь компьютер Windows Server.

| Расположение | Минимальные требования | Дополнительные инструкции |
| --- | --- | --- |
| Таблицы Azure |Виртуальная машина IaaS Azure<br><br>Standard A2: 2 ядра, 3,5 ГБ ОЗУ |Можно начать с простого образа коллекции центра обработки данных Windows Server 2012 R2. [Защита рабочих нагрузок IaaS с помощью сервера службы архивации Azure (DPM)](https://technet.microsoft.com/library/jj852163.aspx) имеет свои особенности. Прежде чем развертывать виртуальную машину, полностью ознакомьтесь со статьей. |
| Локальная система |Виртуальная машина Hyper-V,<br> виртуальная машина VMware<br> или физический узел<br><br>2 ядра и 4 ГБ ОЗУ |С помощью дедупликации Windows Server можно выполнить дедупликацию хранилища DPM. См. дополнительные сведения об использовании [DPM и дедупликации](https://technet.microsoft.com/library/dn891438.aspx) при развертывании на виртуальных машинах Hyper-V. |

> [!NOTE]
> Рекомендуется установить сервер службы архивации Azure на компьютере с центром обработки данных Windows Server 2012 R2. В последнюю версию операционной системы Windows автоматически включено множество необходимых компонентов.
>
>

Если вы планируете присоединить к домену Сервер резервного копирования Azure, рекомендуется присоединить к домену физический сервер или виртуальную машину перед установкой программного обеспечения Сервера резервного копирования Azure. Перемещение имеющегося Сервера резервного копирования Azure в новый домен после развертывания *не поддерживается*.

## <a name="2-backup-vault"></a>2. Хранилище службы архивации
![Шаг 2](./media/backup-azure-microsoft-azure-backup/step2.png)

Независимо от того, отправляются архивные данные в Azure или сохраняются в локальной среде, Сервер резервного копирования Azure должен быть зарегистрирован в хранилище. Если вы только приступили к работе со службой Azure Backup и хотите использовать Azure Backup Server, ознакомьтесь со статьей [Подготовка к резервному копированию рабочих нагрузок с использованием сервера Azure Backup Server](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Начиная с марта 2017 года классический портал больше нельзя использовать для создания хранилищ службы архивации.
> Теперь вы можете обновить свои резервные хранилища до хранилищ служб восстановления. См. дополнительные сведения об [обновлении резервного хранилища до хранилища служб восстановления](backup-azure-upgrade-backup-to-recovery-services.md). Корпорация Майкрософт рекомендует обновить резервные хранилища до хранилищ служб восстановления.<br/> После 15 октября 2017 г. вы не сможете использовать PowerShell для создания резервных хранилищ. **До 1 ноября 2017 г.**:
>- Все оставшиеся резервные хранилища будут автоматически обновлены до хранилищ служб восстановления.
>- Вы не сможете получить доступ к данным резервных копий на классическом портале. Вместо этого для доступа к данным резервных копий в хранилищах служб восстановления используйте портал Azure.
>



## <a name="3-software-package"></a>3. Программный пакет
![Шаг 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-the-software-package"></a>Скачивание программного пакета
Как и в случае с учетными данными хранилища, вы можете загрузить службу архивации Microsoft Azure для рабочих нагрузок приложения на **странице быстрого запуска** хранилища архивации.

1. Щелкните **Для рабочих нагрузок приложения (диск — диск — облако)**. В результате откроется страница Центра загрузки, где можно скачать программный пакет.

    ![Резервное копирование Microsoft Azure: экран приветствия](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Щелкните элемент **Загрузить**.

    ![Центр загрузки 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Выберите все файлы и нажмите кнопку **Далее**. Скачайте все файлы со страницы скачивания службы архивации Microsoft Azure и поместите их в одну папку.
   ![Центр загрузки 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Поскольку общий размер файлов превышает 3 ГБ, при скорости подключения 10 Мбит/с загрузка может занять до одного часа.

### <a name="extracting-the-software-package"></a>Извлечение программного пакета
После загрузки всех файлов щелкните **MicrosoftAzureBackupInstaller.exe**. Будет запущен **мастер установки службы архивации Microsoft Azure** , с помощью которого можно извлечь файлы установки в указанное расположение. Выполните указания мастера и нажмите кнопку **Извлечь** , чтобы начать процесс извлечения.

> [!WARNING]
> Для извлечения файлов установки требуется как минимум 4 ГБ свободного места.
>
>

![мастер установки службы архивации Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Когда извлечение будет завершено, установите флажок, чтобы запустить только что извлеченный файл *setup.exe* и начать установку сервера службы архивации Microsoft Azure, а затем нажмите кнопку **Готово** .

### <a name="installing-the-software-package"></a>Установка программного пакета
1. Щелкните **Сервер архивации Microsoft Azure** , чтобы запустить мастер установки.

    ![мастер установки службы архивации Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. На экране приветствия нажмите кнопку **Далее** . Вы перейдете в раздел *Проверки готовности* . На этом экране нажмите кнопку **Проверить** , чтобы определить, выполнены ли предварительные требования к оборудованию и программному обеспечению для сервера службы архивации Azure. Если все обязательные условия выполняются, появится соответствующее сообщение. Нажмите кнопку **Далее** .

    ![Сервер службы архивации Azure — приветствие и проверка готовности](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Для сервера службы архивации Microsoft Azure необходимо, чтобы в SQL Server Standard и пакет установки сервера службы архивации Azure были включены соответствующие двоичные файлы SQL Server. При запуске новой установки сервера резервного копирования Azure выберите параметр **Установить новый экземпляр SQL Server во время этой установки** и нажмите кнопку **Проверить и установить**. После успешной установки необходимых компонентов нажмите кнопку **Далее**.

    ![Сервер службы архивации Azure — проверка SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    В случае сбоя и появления рекомендации перезагрузить компьютер, выполните инструкцию на экране и нажмите кнопку **Проверить снова**.

   > [!NOTE]
   > Сервер службы архивации Azure не поддерживает использование удаленного экземпляра SQL Server. Экземпляр, который использует сервер службы архивации Azure, должен быть локальным.
   >
   >

4. Укажите расположение для установки файлов сервера архивации Microsoft Azure и нажмите кнопку **Далее**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Папка для временных файлов обязательна для резервного копирования в Azure. Убедитесь, что размер папки для временных файлов составляет по крайней мере 5 % объема данных, для которых планируется создать резервную копию в облаке. После завершения установки необходимо настроить защиту отдельных дисков. Дополнительные сведения о пулах носителей см. в статье [Настройка пулов носителей и дискового хранилища](https://technet.microsoft.com/library/hh758075.aspx).
5. Введите надежный пароль для локальных учетных записей пользователей с ограниченными правами и нажмите кнопку **Далее**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Укажите, следует ли проверять обновления с помощью *Центра обновления Майкрософт* , и нажмите кнопку **Далее**.

   > [!NOTE]
   > Рекомендуется выполнять перенаправление из Центра обновления Windows в Центр обновления Майкрософт, который предлагает обновления безопасности и другие важные обновления для Windows и других продуктов, таких как сервер службы архивации Microsoft Azure.
   >
   >

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Просмотрите *Сводку параметров* и нажмите кнопку **Установить**.

    ![Microsoft Azure Backup PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Установка выполняется поэтапно. На первом этапе выполняется установка агента служб восстановления Microsoft Azure на сервер. В это время мастер также проверяет наличие подключения к Интернету. При наличии подключения к Интернету можно продолжить установку. В противном случае потребуется указать сведения о прокси-сервере.

    Следующий шаг — настроить агент служб восстановления Microsoft Azure. В рамках настройки необходимо предоставить учетные данные хранилища, чтобы зарегистрировать компьютер в хранилище службы архивации. Вам также потребуется предоставить парольную фразу для шифрования и расшифровки данных, передаваемых между Azure и локальной средой. Вы можете сгенерировать парольную фразу автоматически или ввести ее вручную. Она должна включать не менее 16 символов. Продолжайте работу мастера, пока агент не будет настроен.

    ![PreReq2 сервера службы архивации Azure](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. После успешного завершения регистрации сервера службы архивации Microsoft Azure работа мастера установки продолжается: выполняется установка и настройка SQL Server и компонентов сервера службы архивации Azure. C установкой компонента SQL Server установка компонентов сервера службы архивации Microsoft Azure завершается.

    ![Сервер службы архивации Azure](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

По завершению установки на рабочем столе должны быть созданы соответствующие значки. Дважды щелкните значок для запуска продукта.

### <a name="add-backup-storage"></a>Добавление хранилища службы архивации
Первая резервная копия хранится в хранилище, подключенном к компьютеру сервера службы архивации Azure. Дополнительные сведения о добавлении дисков см. в статье [Настройка пулов носителей и дискового хранилища](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Хранилище службы архивации необходимо добавить, даже если вы планируете отправлять данные в Azure. В текущей архитектуре сервера службы архивации Azure хранилище службы архивации Azure содержит *вторую* копию данных, а локальное хранилище — первую (и обязательную) резервную копию.  
>
>

## <a name="4-network-connectivity"></a>4. Сетевое подключение
![Шаг 4](./media/backup-azure-microsoft-azure-backup/step4.png)

Для успешной работы серверу службы архивации Azure необходимо подключение к службе архивации Azure. Чтобы проверить, подключен ли компьютер к Azure, используйте командлет ```Get-DPMCloudConnection``` в консоли PowerShell сервера службы архивации Azure. Если выходные данные командлета имеют значение TRUE, то подключение существует. В противном случае оно отсутствует.

При этом подписка Azure должна быть активна. Чтобы определить состояние подписки и управлять им, войдите на [портал подписки](https://account.windowsazure.com/Subscriptions).

Узнав состояние подключения и подписки Azure, можно использовать следующую таблицу, чтобы определить влияние на предоставляемые возможности резервного копирования и восстановления.

| Состояние подключения | Подписка Azure | Резервное копирование в Azure | Резервное копирование на диск | Восстановление из Azure | Восстановление с диска |
| --- | --- | --- | --- | --- | --- |
| Подключено |Активна |Разрешено |Разрешено |Разрешено |Разрешено |
| Подключено |Срок действия истек |Остановлено |Остановлено |Разрешено |Разрешено |
| Подключено |Отозвана |Остановлено |Остановлено |Остановлено; точки восстановления Azure удалены |Остановлено |
| Подключение потеряно на 15 и более дней |Активна |Остановлено |Остановлено |Разрешено |Разрешено |
| Подключение потеряно на 15 и более дней |Срок действия истек |Остановлено |Остановлено |Разрешено |Разрешено |
| Подключение потеряно на 15 и более дней |Отозвана |Остановлено |Остановлено |Остановлено; точки восстановления Azure удалены |Остановлено |

### <a name="recovering-from-loss-of-connectivity"></a>Восстановление после потери подключения
При наличии брандмауэра или прокси-сервера, который препятствует доступу к Azure, необходимо добавить следующие адреса доменов в белый список в профиле брандмауэра или прокси-сервера:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

После восстановления подключения компьютера сервера службы архивации к Azure доступные операции зависят от состояния подписки Azure. В таблице выше содержатся подробные сведения об операциях, которые можно выполнять после того, как состояние компьютера изменится на «Подключено».

### <a name="handling-subscription-states"></a>Сведения о состояниях подписки
Состояние подписки Azure можно изменить со *Срок действия истек* или *Отозвана* на *Активная*. Если подписка находится в состоянии, отличном от *Активная*, это может повлиять на поведение продукта:

* Если подписка находится в состоянии *Отозвана* , во время пребывания в этом состоянии основные возможности становятся недоступными. После изменения состояния подписки на *Активная*возможности архивации и восстановления снова становятся доступными. Кроме того, можно извлечь данные резервного копирования, хранящиеся на локальном диске, если период их удержания достаточно большой. Но как только состояние подписки меняется на *Отозвана* , данные архивации в Azure безвозвратно теряются.
* Если подписка находится в состоянии *Срок действия истек*, функциональные возможности теряются только до восстановления состояния *Активная*. Если подписка пребывает в состоянии *Срок действия истек* и на этот период времени запланирована архивация, она не будет выполнена.

## <a name="troubleshooting"></a>Устранение неполадок
Если на этапе установки, резервного копирования или восстановления происходит сбой сервера резервного копирования Microsoft Azure с указанием кода ошибки, см. информацию о кодах ошибок [в этом документе](https://support.microsoft.com/kb/3041338).
Вы также можете изучить [часто задаваемые вопросы, связанные с резервным копированием в Azure](backup-azure-backup-faq.md).

## <a name="next-steps"></a>Дальнейшие действия
См. дополнительные сведения о [подготовке среды для DPM](https://technet.microsoft.com/library/hh758176.aspx) на веб-сайте Microsoft TechNet. На этом сайте также содержатся сведения о поддерживаемых конфигурациях, при которых можно развернуть и использовать сервер службы архивации Azure.

Вы можете использовать эти статьи для более глубокого изучения вариантов защиты рабочих нагрузок с помощью сервера архивации Microsoft Azure.

* [Резервное копирование сервера SQL Server](backup-azure-backup-sql.md)
* [Резервное копирование сервера SharePoint](backup-azure-backup-sharepoint.md)
* [Резервное копирование альтернативного сервера](backup-azure-alternate-dpm-server.md)

