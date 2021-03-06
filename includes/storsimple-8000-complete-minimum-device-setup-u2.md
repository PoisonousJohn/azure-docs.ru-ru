<!--author=alkohli last changed: 01/12/17-->

#### <a name="to-complete-the-minimum-storsimple-device-setup"></a>Минимальная настройка устройства StorSimple

   > [!NOTE]
   > Имя устройства нельзя изменить после завершения его минимальной настройки.
   
1. В колонке **Устройства** в таблице со списком устройств выберите нужное устройство. Устройство находится в состоянии **Готово к настройке**. Откроется колонка **Настройка устройства**.

     ![Сетевые интерфейсы для минимальной настройки устройства StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig1.png)

2. В колонке **Настройка устройства** выполните следующие действия:
   
   1. Укажите **понятное имя** для устройства. Имя устройства по умолчанию содержит сведения о модели устройства и серийный номер. Для управления устройством можно указать понятное имя длиной не более 64 символов.
   2. Задайте **часовой пояс** на основе географического расположения развертывания устройства. Устройство использует заданный часовой пояс для всех запланированных операций.
   3. В разделе **DATA 0 settings** (Параметры Data 0) выполните следующие действия:

       1. Сетевой интерфейс DATA 0 отображается как включенный с параметрами сети (IP-адрес, подсеть, шлюз), настроенными с помощью мастера установки. DATA 0 также автоматически включается для облака и iSCSI.

       2. Введите фиксированные IP-адреса для контроллеров 0 и 1. **Статические IP-адреса контроллера должны быть свободными внутри подсети, доступной по IP-адресу устройства.** Если интерфейс DATA 0 настроен для IPv4, статические IP-адреса должны быть указаны в формате IPv4. Если указан префикс для IPv6, статические IP-адреса будут заполнены автоматически в этих полях.

            ![Сетевые интерфейсы для минимальной настройки устройства StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig2.png)

            Фиксированные IP-адреса для контроллера используются для установки обновлений на устройство. Поэтому фиксированные IP-адреса должны маршрутизироваться и должны иметь доступ к Интернету. Чтобы проверить, являются ли фиксированные IP-адреса контроллера маршрутизируемыми, можно использовать командлет [Test-HcsmConnection][Test]. В следующем примере показано, что фиксированные IP-адреса маршрутизируются к Интернету и могут получать доступ к серверам Центра обновления Майкрософт.

            ![Test-HcsmConnection, показывающий маршрутизируемые IP-адреса](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig3.png)

1. Нажмите кнопку **ОК**. Запускается конфигурация устройства. После завершения конфигурации устройства вы получите уведомление. В колонке **устройств** состояние устройства изменится на **Подключено**.

    ![Сетевые интерфейсы для минимальной настройки устройства StorSimple](./media/storsimple-8000-complete-minimum-device-setup-u2/step4minconfig4.png)

<!--Link reference-->
[Test]: https://technet.microsoft.com/library/dn715782(v=wps.630).aspx
