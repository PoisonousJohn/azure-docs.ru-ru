---
title: "Добавление SSL-сертификата в приложение службы приложений Azure | Документация Майкрософт"
description: "Сведения о добавлении SSL-сертификата в приложение службы приложений."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: fb38555f1f299352f06deae1ca231895163068e5
ms.contentlocale: ru-ru
ms.lasthandoff: 05/26/2017

---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Приобретение и настройка сертификата SSL для службы приложений Azure

В этом руководстве вы защитите свое веб-приложение, приобретя SSL-сертификат для **[службы приложений Azure](http://go.microsoft.com/fwlink/?LinkId=529714)**, безопасно сохранив его в [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) и сопоставив с личным доменом.

## <a name="step-1---log-in-to-azure"></a>Шаг 1. Вход в Azure

Войдите на портал Azure по адресу http://portal.azure.com.

## <a name="step-2---place-an-ssl-certificate-order"></a>Шаг 2. Размещение заказа на SSL-сертификат

Заказ на SSL-сертификат можно разместить, создав новый [сертификат службы приложения](https://portal.azure.com/#create/Microsoft.SSL) на **портале Azure**.

![Создание сертификата](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Введите понятное **имя** для SSL-сертификата, а также **доменное имя**.

> [!NOTE]
> Это один из наиболее важных этапов процесса покупки. Не забудьте ввести правильное имя узла (пользовательский домен), который необходимо защитить с помощью этого сертификата. **НЕ** добавляйте к имени узла префикс WWW. 
>

Выберите **подписку**, **группу ресурсов** и **SKU сертификата**.

> [!WARNING]
> Сертификаты службы приложений доступны лишь другим службам приложений в той же подписке.  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Шаг 3. Сохранение сертификата в Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) — это служба Azure, которая помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами.
>

После завершения покупки SSL-сертификата нужно открыть колонку ресурса [Сертификаты службы приложений](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders).

![вставить изображение для готовности к размещению в хранилище ключей](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Вы увидите, что состояние сертификата — **Ожидание выдачи**. Это связано с тем, что нужно выполнить еще некоторые действия, прежде чем вы сможете воспользоваться этим сертификатом.

Щелкните **Конфигурация сертификата** в колонке "Свойства сертификата" и затем **Шаг 1. Хранилище**, чтобы сохранить этот сертификат в Azure Key Vault.

В колонке **Состояние Key Vault** щелкните **Репозиторий Key Vault**, чтобы выбрать существующее Key Vault для сохранения этого сертификата, или щелкните **Создать новое Key Vault**, чтобы создать Key Vault в той же подписке и группе ресурсов.

> [!NOTE]
> Расходы на хранение этого сертификата в Azure Key Vault будут минимальными.
> Дополнительные сведения см. в разделе с **[ценами на Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

После выбора репозитория Key Vault для сохранения сертификата на элементе **Хранилище** должны появиться сведения об успешном выполнении.

![Вставить изображение с успешным сохранением в Key Vault](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Шаг 4. Проверка владельца домена

> [!NOTE]
> Сертификаты службы приложений поддерживают три типа проверки домена: проверка через домен, по почте и вручную. Подробнее они описаны в разделе [Дополнительно](#advanced).

В той же колонке **Конфигурация сертификата**, которую вы использовали в шаге 3, щелкните **Шаг 2. Проверка**.

**Проверка домена**. Этот метод наиболее удобен, **только если** вы **[приобрели личный домен в службе приложений Azure](custom-dns-web-site-buydomains-web-app.md)**.
Нажмите кнопку **Проверить**, чтобы завершить этот шаг.

![Вставить изображение с проверкой через домен](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

После нажатия кнопки **Проверить** нажимайте кнопку **Обновить** до тех пор, пока для операции **Проверить** не появятся сведения об успешном выполнении.

![Вставить изображение с успешной проверкой в Key Vault](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Шаг 5. Назначение сертификата приложению службы приложений

> [!NOTE]
> Прежде чем выполнять действия, описанные в этом разделе, необходимо связать личное доменное имя с приложением. Дополнительные сведения см. в разделе **[Сопоставление имени личного домена с приложением Azure](app-service-web-tutorial-custom-domain.md)**.
>

На **[портале Azure](https://portal.azure.com/)** щелкните **Служба приложений** в левой части страницы.

Щелкните имя приложения, которому вы хотите назначить этот сертификат.

В разделе **Параметры** щелкните **SSL-сертификаты**.

Щелкните **Импортировать сертификат службы приложений** и выберите сертификат, который вы приобрели.

![вставить изображение для импорта сертификата](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

В разделе **SSL-привязки** щелкните **Add bindings** (Добавить привязки), а затем в раскрывающихся списках выберите имя домена для защиты с применением SSL, а также используемый сертификат. Также можно выбрать SSL на основе **[указания имени сервера (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** или IP-адреса.

![вставить изображение для привязок SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Чтобы сохранить изменения и активировать SSL, щелкните **Добавить привязку** .

> [!NOTE]
> Если вы выбрали **SSL на основе IP-адреса** и личный домен настроен с помощью записи А, нужно выполнить приведенные ниже шаги. Подробнее они описаны в разделе [Дополнительно](#Advanced).

На этом этапе приложение должно открываться с использованием `HTTPS://`, а не `HTTP://`, что подтверждает правильность настройки сертификата.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Шаг 6. Задачи управления

### <a name="azure-cli"></a>Инфраструктура CLI Azure

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Привязка SSL-сертификата к веб-приложению")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Привязка SSL-сертификата к веб-приложению")]

## <a name="advanced"></a>Расширенная

### <a name="verifying-domain-ownership"></a>Проверка принадлежности домена

Сертификаты службы приложений поддерживают еще два типа проверки домена: проверка по почте и вручную.

#### <a name="mail-verification"></a>Проверка по почте

Проверочное сообщение уже было отправлено по адресам электронной почты, связанным с этим пользовательским доменом.
Чтобы завершить шаг проверки по электронной почте, откройте сообщение и щелкните ссылку для проверки.

![Вставить изображение с проверкой по почте](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Если необходимо повторно отправить проверочное сообщение, нажмите кнопку **Переотправить сообщение электронной почты**.

#### <a name="manual-verification"></a>Проверка вручную

> [!IMPORTANT]
> Проверка веб-страницы HTML (работает только с номером SKU сертификата уровня "Стандартный").
>

1. Создайте HTML-файл **starfield.html**.

1. Содержимое этого файла должно полностью совпадать с именем токена проверки домена. (Токен можно скопировать из колонки состояния проверки домена.)

1. Отправьте этот файл в корень веб-сервера, на котором размещается ваш домен `/.well-known/pki-validation/starfield.html`.

1. Нажмите кнопку **Обновить**, чтобы обновить состояние сертификата после завершения проверки. Проверка может занять несколько минут.

> [!TIP]
> Проверьте терминал с помощью `curl -G http://<domain>/.well-known/pki-validation/starfield.html`, ответ должен содержать `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Проверка записей типа TXT DNS

1. С помощью диспетчера DNS создайте запись типа TXT в поддомене `@` со значением, идентичным токену проверки домена.
1. Нажмите кнопку **Обновить**, чтобы обновить состояние сертификата после завершения проверки.

> [!TIP]
> Нужно создать запись типа TXT в `@.<domain>` со значением `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Назначение сертификата приложению службы приложений

Если выбран **вариант SSL-подключения на основе IP** и пользовательский домен настроен с помощью записи А, необходимо выполнить следующие дополнительные шаги:

После настройки привязки SSL-подключения на основе IP вашему веб-приложению будет предоставлен выделенный IP-адрес. Этот IP-адрес можно найти на странице **Личный домен** в разделе параметров приложения прямо над разделом **Имена узлов**. Он указывается как **Внешний IP-адрес**

![вставить изображение для SSL на основе IP](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Обратите внимание, что этот IP-адрес отличается от виртуального IP-адреса, который использовался ранее для настройки записи A для вашего домена. Если выбрано использование SSL-подключения на основе SNI или если не настроено использование SSL, адрес для этой записи не указывается.

С помощью средств, предоставляемых регистратором имени домена, измените запись A для имени пользовательского домена, указав IP-адрес из предыдущего шага.

## <a name="rekey-and-sync-the-certificate"></a>Повторное создание ключей и синхронизация сертификата

Если вам когда-либо придется повторно назначить ключ сертификата, выберите **Повторное создание ключей и синхронизация** в колонке **Свойства сертификата**.

Для запуска процесса нажмите кнопку **Повторное создание ключей**. Этот процесс может занять от 1 до 10 минут.

![вставить изображение для повторного создания ключа SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

При создании ключа развертывается новый сертификат, выданный центром сертификации.

## <a name="next-steps"></a>Дальнейшие действия

* [Добавление сети доставки содержимого](app-service-web-tutorial-content-delivery-network.md)
