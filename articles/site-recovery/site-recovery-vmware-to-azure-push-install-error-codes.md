---
title: "Устранение неполадок при репликации из VMware в Azure с помощью Azure Site Recovery | Документация Майкрософт"
description: "Устранение ошибок при репликации виртуальных машин Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: srinathv
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/24/2017
ms.author: asgang
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 1e2452ccc6d3f1859a39e1ee09cdde32f53767ba
ms.contentlocale: ru-ru
ms.lasthandoff: 08/28/2017

---
# <a name="troubleshooting-mobility-service-push-install-issues"></a>Устранение неполадок с принудительной установкой службы Mobility Service

Эта статья содержит сведения о распространенных проблемах при установке службы Mobility Service на исходном сервере для включения защиты.

## <a name="error-code-95107-protection-could-not-be-enabled"></a>Не удалось включить защиту (код ошибки 95107)
**Код ошибки** | **Возможные причины** | **Рекомендации по устранению ошибки**
--- | --- | ---
95107 </br>***Сообщение.*** Ошибка ***EP0858*** при принудительной установке службы Mobility Service на исходный компьютер. <br> Для установки службы Mobility Service указаны неверные учетные данные или учетная запись пользователя имеет недостаточно привилегий. | Для установки службы Mobility Service на исходный компьютер указаны неверные учетные данные пользователя. | Убедитесь, что для исходного компьютера указаны верные учетные данные. <br> Чтобы добавить или изменить учетные данные пользователя, перейдите на сервер конфигурации, щелкните значок Cspsconfigtool и выберите "Управление учетной записью". </br> Кроме того, для успешного выполнения принудительной установки убедитесь, что выполнены необходимые условия, приведенные ниже.

## <a name="error-code-95015-protection-could-not-be-enabled"></a>Не удалось включить защиту (код ошибки 95015)
**Код ошибки** | **Возможные причины** | **Рекомендации по устранению ошибки**
--- | --- | ---
95105 </br>***Сообщение.*** При принудительной установке службы мобильности на исходный компьютер произошла ошибка с кодом ***EP0856***. <br> Общий доступ к файлам и принтерам запрещен на исходном компьютере, или возникли проблемы с сетевым соединением между сервером обработки и исходным компьютером.| Общий доступ к файлам и принтерам выключен. | Разрешите общий доступ к файлам и принтерам на исходном компьютере в брандмауэре Windows, перейдите на исходный компьютер и в разделе параметров брандмауэра Windows разрешите запуск программы или компонента через брандмауэр Windows, затем выберите File and Printer Sharing for all profiles (Общий доступ к файлам и принтерам для всех профилей). </br> Кроме того, для успешного выполнения принудительной установки убедитесь, что выполнены необходимые условия, приведенные ниже.

## <a name="error-code-95117-protection-could-not-be-enabled"></a>Не удалось включить защиту (код ошибки 95117)
**Код ошибки** | **Возможные причины** | **Рекомендации по устранению ошибки**
--- | --- | ---
95117 </br>***Сообщение.*** Ошибка ***EP0865*** при принудительной установке службы Mobility Service на исходный компьютер. <br> Исходный компьютер не запущен, или возникли проблемы с сетевым соединением между сервером обработки и исходным компьютером. | Проблемы с сетевым соединением между сервером обработки и исходным компьютером. | Проверьте сетевое соединение между сервером обработки и исходным компьютером. </br> Кроме того, для успешного выполнения принудительной установки убедитесь, что выполнены необходимые условия, приведенные ниже.

## <a name="error-code-95103-protection-could-not-be-enabled"></a>Не удалось включить защиту (код ошибки 95103)
**Код ошибки** | **Возможные причины** | **Рекомендации по устранению ошибки**
--- | --- | ---
95103 </br>***Сообщение.*** При принудительной установке службы мобильности на исходный компьютер произошла ошибка с кодом ***EP0854***. <br> Использование инструментария управления Windows (WMI) запрещено на исходном компьютере, или возникли проблемы с сетевым соединением между сервером обработки и исходным компьютером.| Использование инструментария управления Windows (WMI) заблокировано в брандмауэре Windows. | Разрешите использование инструментария управления Windows (WMI) в брандмауэре Windows. В группе параметров брандмауэра Windows выберите параметр Allow an app or feature through Firewall (Разрешить запуск программы или компонента через брандмауэр) и выберите WMI для всех профилей. </br> Кроме того, для успешного выполнения принудительной установки убедитесь, что выполнены необходимые условия, приведенные ниже.

## <a name="check-push-install-logs-for-errors"></a>Проверка журналов принудительных установок на наличие ошибок

На сервере конфигурации или обработки перейдите к файлу PushinstallService, расположенному по адресу <Microsoft Azure Site Recovery Install Location>\home\svsystems\pushinstallsvc\, чтобы определить источник проблемы. Выполните указанные ниже действия по устранению неполадок, чтобы решить эту проблему.</br>
![pushiinstalllogs](./media/site-recovery-protection-common-errors/pushinstalllogs.png)

## <a name="push-install-pre-requisites-for-windows"></a>Необходимые компоненты для принудительной установки в Windows
### <a name="ensure-file-and-printer-sharing-is-enabled"></a>Обеспечение общего доступа к файлам и принтерам
Предоставьте общий доступ к файлам и принтерам и инструментарию управления Windows на исходном компьютере в брандмауэре Windows. </br>
#### <a name="if-source-machine-is-domain-joined-br"></a>Если исходный компьютер присоединен к домену, сделайте следующее: </br>
Настройте параметры брандмауэра с помощью консоли управления групповыми политиками (GPMC).
1. Войдите на компьютер домена Active Directory c правами администратора и откройте консоль управления групповыми политиками (для запуска GPMC.MSC выберите "Пуск > Выполнить").</br>
3. Если консоль управления групповыми политиками не установлена, перейдите по ссылке, чтобы [установить ее](https://technet.microsoft.com/library/cc725932.aspx). </br>
4. В дереве консоли управления групповыми политиками дважды щелкните объекты групповой политики в лесу и домене, а затем перейдите к политике домена по умолчанию. </br>
![gpmc1](./media/site-recovery-protection-common-errors/gpmc1.png) </br>
</br>
5. Щелкните правой кнопкой мыши политику домена по умолчанию и выберите "Изменить". Откроется новое окно "Редактор управления групповыми политиками". </br>
![gpmc2](./media/site-recovery-protection-common-errors/gpmc2.png) </br>
</br>
6. В редакторе "Управление групповыми политиками" выберите "Конфигурация компьютера > Политики > Административные шаблоны > Сеть > Сетевые подключения > Брандмауэр Windows". </br>
![gpmc3](./media/site-recovery-protection-common-errors/gpmc3.png) </br>
</br>
7. Для профиля домена и стандартного профиля включите следующие параметры: </br>
а) Дважды щелкните исключение "Брандмауэр Windows: разрешить исключение для входящего общего доступа к файлам и принтерам". Выберите "Включено" и нажмите кнопку ОК. </br>
б) Дважды щелкните исключение "Брандмауэр Windows: разрешить исключение для входящих сообщений удаленного администрирования". Выберите "Включено" и нажмите кнопку ОК. </br>
![gpmc4](./media/site-recovery-protection-common-errors/gpmc4.png) </br>
</br>

###### <a name="if-source-machine-is-not-domain-joined-and-part-of-workgroup-br"></a>Если исходный компьютер не присоединен к домену и не является частью рабочей группы, выполните указанные ниже действия. </br>
Настройте параметры брандмауэра на удаленном компьютере (для рабочей группы).
1. Перейдите к исходному компьютеру,</br>
2. в группе параметров брандмауэра Windows выберите параметр Allow an app or feature through Firewall (Разрешить запуск программы или компонента через брандмауэр) и File and Printer Sharing for all profiles (Общий доступ к файлам и принтерам для всех профилей). </br>
3. В группе параметров брандмауэра Windows выберите параметр Allow an app or feature through Firewall (Разрешить запуск программы или компонента через брандмауэр) и выберите WMI для всех профилей. </br>

#### <a name="disable-remote-user-account-control-uac"></a>Отключение функции удаленного контроля учетных записей (UAC)
Отключите UAC с помощью раздела реестра, чтобы принудительно установить службу Mobility Service.
1. Нажмите кнопку "Пуск > Выполнить", введите regedit и нажмите клавишу ВВОД.
2. Найдите и щелкните следующий подраздел реестра: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System.
3. Если реестр записи LocalAccountTokenFilterPolicy не существует, сделайте следующее:
4. В меню "Правка" выберите "Создать" и щелкните значение DWORD.
5. Введите LocalAccountTokenFilterPolicy и нажмите клавишу ВВОД.
6. Щелкните правой кнопкой мыши LocalAccountTokenFilterPolicy и выберите "Изменить".
7. В поле "Значение" введите 1 и нажмите кнопку ОК.
8. Закройте редактор реестра.


## <a name="push-install-pre-requisites-for-linux"></a>Необходимые компоненты для принудительной установки в Linux

1. Создайте привилегированного пользователя на исходном сервере Linux. (Используйте эту учетную запись только для принудительной установки и обновлений.)</br>
2. Убедитесь, что файл /etc/hosts на исходном сервере Linux содержит записи, которые связывают имя локального узла с IP-адресами всех сетевых адаптеров. </br>
3. Установите последние версии пакетов openssh, openssh-server и openssl на исходном сервере Linux. </br>
Убедитесь, что SSH-порт 22 включен и работает. </br>
4. Проверьте наличие устаревших записей агентов на исходном сервере, удалите предыдущие версии агентов, а затем перезагрузите сервер и переустановите агент. </br>

#### <a name="enable-sftp-subsystem-and-password-authentication-in-the-sshdconfig-file"></a>Включение подсистемы SFTP и проверки пароля в файле sshd_config
1. Выполните вход в качестве привилегированного пользователя на исходном сервере. </br>
2. В файле /etc/ssh/sshd_config</br> найдите строку, начинающуюся с PasswordAuthentication. </br>
3. d.   Раскомментируйте ее и измените значение с No на Yes. </br>
![Linux1](./media/site-recovery-protection-common-errors/linux1.png)
4. Найдите строку, которая начинается с Subsystem, и раскомментируйте ее. </br>
![Linux2](./media/site-recovery-protection-common-errors/linux2.png)
5. Сохраните изменения и перезапустите службу SSHD. </br>

## <a name="push-installation-checks-on-configurationprocess-server"></a>Проверка принудительной установки на сервере конфигурации или сервере обработки
#### <a name="validate-credentials-for-discovery-and-installation"></a>Проверка учетных данных для обнаружения и установки

1. На сервере конфигурации запустите файл Cspsconfigtool.</br>
![WMItestConnect5](./media/site-recovery-protection-common-errors/wmitestconnect5.png) </br>

2. Убедитесь, что учетная запись, используемая для защиты, имеет права администратора на исходном компьютере. </br>

#### <a name="check-connectivity-between-process-server-and-source-server"></a>Проверка сетевого соединения между сервером обработки и исходным компьютером
1. Убедитесь, что сервер обработки имеет подключение к Интернету.
2. Проверьте подключение WMI с помощью wbemtest.exe. </br>
На сервере обработки нажмите кнопку "Пуск > Выполнить" и выберите файл wbemtest.exe. Откроется окно "Тестер инструментария управления Windows", как показано ниже.</br>
   ![WMItestConnect1](./media/site-recovery-protection-common-errors/wmitestconnect1.png) </br>
   </br>
Щелкните "Подключение" и введите IP-адрес исходного сервера в поле "Пространство имен". Укажите имя пользователя и пароль. (Если исходный компьютер присоединен к домену, укажите имя домена вместе с именем пользователя, например "имя_домена\имя_пользователя". Если исходный компьютер входит в рабочую группу, укажите только имя пользователя.) Выберите уровень проверки подлинности в качестве конфиденциальности пакетов. </br>
![WMItestConnect2](./media/site-recovery-protection-common-errors/wmitestconnect2.png) </br>
   </br>
   Щелкните "Подключение". Теперь подключение к WMI с использованием указанных данных должно быть успешным. Должно отобразиться окно "Тестер инструментария управления Windows", как показано ниже. </br>
   ![WMItestConnect3](./media/site-recovery-protection-common-errors/wmitestconnect3.png) </br>
</br>
   Если при подключении к WMI произойдет сбой, отобразится всплывающее окно сообщения об ошибке. На приведенном ниже снимке экрана показана неудачная попытка, когда администрирование WMI или удаленное администрирование не включено в допустимом приложении брандмауэра Windows. </br>
   ![WMItestConnect4](./media/site-recovery-protection-common-errors/wmitestconnect4.png) </br>
</br>

3. Проверьте состояние и подключение WMI.</br>
На сервере конфигурации или обработки </br>
нажмите кнопку "Пуск > Выполнить", выберите файл wmimgmt.msc, а затем — "Действия > Дополнительные действия" и подключитесь к другому (исходному) компьютеру. </br>
Введите учетные данные учетной записи, используемой для защиты и проверки работоспособности подключения. </br>

#### <a name="verify-network-shared-folders-of-source-machine-is-accessible-from-process-server-ps-remotely-using-specified-credentials"></a>Убедитесь, что вы можете получить удаленный доступ к общим сетевым папкам исходного компьютера с сервера обработки с помощью указанных учетных данных.
  1. Войдите на компьютер сервера обработки, откройте обозреватель файлов, в адресной строке введите \\\source-machine-ip\C$ и нажмите клавишу ВВОД. </br>
  ![Fileshare1](./media/site-recovery-protection-common-errors/fileshare1.png) </br>
  2. Обозреватель файлов запросит учетные данные. Введите имя пользователя и пароль, затем нажмите кнопку ОК.</br>
   Если исходный компьютер присоединен к домену, укажите имя домена, а также имя пользователя, например "имя_домена\имя_пользователя".</br>
   Если исходный компьютер входит в рабочую группу, укажите только имя пользователя. </br>
  ![Fileshare2](./media/site-recovery-protection-common-errors/fileshare2.png) </br>
  3. Если подключение установлено успешно, на сервере обработки можно удаленно просмотреть папки исходного компьютера. </br>
  ![Fileshare3](./media/site-recovery-protection-common-errors/fileshare3.png) </br>

> [!NOTE] 
> Если подключение завершилось сбоем, проверьте, соблюдены ли все предварительные требования.
>

Если вы не хотите открывать окно "Инструментарий управления Windows", можно также установить службу Mobility Service вручную на исходном компьютере.</br> [Установка службы Mobility Service вручную с помощью графического пользовательского интерфейса](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) </br>
[Автоматизация установки Mobility Service с использованием средств развертывания программного обеспечения](site-recovery-install-mobility-service-using-sccm.md) </br>

## <a name="next-steps"></a>Дальнейшие действия
- [Шаг 11. Включение репликации для виртуальных машин VMware, реплицируемых в Azure](vmware-walkthrough-enable-replication.md)

