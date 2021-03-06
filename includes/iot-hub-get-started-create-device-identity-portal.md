## <a name="create-a-device-identity"></a>Создание удостоверения устройства

В этом разделе объясняется, как создать удостоверение устройства в реестре удостоверений в Центре Интернета вещей с помощью [портала Azure][lnk-azure-portal]. Устройство может подключиться к Центру Интернета вещей, только если в реестре удостоверений есть соответствующая запись. Дополнительные сведения см. в разделе, посвященном реестру удостоверений, в [руководстве разработчика по Центру Интернета вещей Azure][lnk-devguide-identity]. **Обозреватель устройств** на портале поможет вам создать уникальный идентификатор устройства и ключ, с помощью которых выполняется идентификация во время подключения к Центру Интернета вещей. Идентификаторы устройств чувствительны к регистру.

1. Убедитесь, что вы вошли на [портал Azure][lnk-azure-portal].

1. На навигационной панели щелкните **Все ресурсы** и найдите ресурс Центра Интернета вещей.

    ![Переход к Центру Интернета вещей][img-find-iothub]

1. После того как ресурс Центра Интернета вещей откроется, щелкните средство **Обозреватель устройств**, а затем вверху выберите **Добавить**. Укажите имя для нового устройства, например **myDeviceId**, и щелкните **Сохранить**.

    ![Создание удостоверения устройства на портале][img-create-device]

   После этого будет создано удостоверение устройства для вашего Центра Интернета вещей.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. В списке **обозревателя устройств** щелкните недавно созданное устройство и запишите значение в поле **Connection string---primary key** (Строка подключения — первичный ключ). 

    ![Строка подключения к устройству.][img-connection-string]

> [!NOTE]
> В реестре удостоверений в Центре Интернета вещей хранятся только идентификаторы устройств, необходимые для безопасного доступа к Центру Интернета вещей. В этом реестре хранятся идентификаторы и ключи устройств, которые используются в качестве учетных данных безопасности, и флажок включения или выключения, который позволяет вам отключить доступ для отдельного устройства. Если в приложении необходимо хранить другие метаданные для конкретного устройства, следует использовать хранилище конкретного приложения. Дополнительные сведения см. в [руководстве разработчика по Центру Интернета вещей][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

