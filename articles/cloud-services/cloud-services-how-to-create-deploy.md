---
title: "Создание и развертывание облачной службы | Документация Майкрософт"
description: "Узнайте, как создать и развернуть облачную службу с помощью функции \"Быстрое создание\" в Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 2a2172a78bfd3ac923edbc9de366b035629dd27b
ms.contentlocale: ru-ru
ms.lasthandoff: 07/19/2017

---
# <a name="how-to-create-and-deploy-a-cloud-service"></a>Создание и развертывание облачной службы
> [!div class="op_single_selector"]
> * [Портал Azure](cloud-services-how-to-create-deploy-portal.md)
> * [Классический портал Azure](cloud-services-how-to-create-deploy.md)
> 
> 

Классический портал Azure поддерживает два способа создания и развертывания облачной службы: **быстрое создание** и **настраиваемое создание**.

В этом разделе описывается быстрое создание облачной службы, а также последующая передача и развертывание соответствующего пакета в Azure с помощью функции **Отправить** . При выборе этого способа на классическом портале Azure отображаются все необходимые для работы ссылки. Чтобы одновременно выполнить развертывание создаваемой облачной службы, воспользуйтесь функцией **Настраиваемое создание**.

> [!NOTE]
> Если вы планируете опубликовать облачную службу из Visual Studio Team Services (VSTS), то воспользуйтесь функцией **Быстрое создание**, а затем настройте публикацию VSTS на странице **Быстрый запуск** или на панели мониторинга.
> 
> 

## <a name="concepts"></a>Основные понятия
Для развертывания приложения в качестве облачной службы в Azure необходимы три компонента:

* **Определение службы.**  
  Файл определения облачной службы с расширением CSDEF, в котором определяется модель службы, включая число ролей.
* **Конфигурация службы.**  
  Файл с расширением CSCFG, в котором задаются значения для любых параметров конфигурации облачной службы и отдельных ролей, включая число экземпляров ролей.
* **Пакет службы.**  
  Пакет службы (с расширением CSPKG) содержит код и конфигурации приложений, а также файл определения службы.

Дополнительные сведения об этих компонентах и создании пакета см. [здесь](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Подготовка приложения
Перед развертыванием облачной службы необходимо создать пакет облачной службы (CSPKG-файл) на основе кода приложения и файл конфигурации облачной службы (CSCFG). Инструменты для подготовки необходимых файлов развертывания находятся в пакете SDK для Azure. Этот пакет можно установить со страницы [Загрузки Azure](https://azure.microsoft.com/downloads/) на языке, выбранном для разработки кода приложения.

Перед экспортом пакета службы необходимо отдельно настроить три компонента облачной службы:

* Если в развертываемой облачной службе будет применяться SSL-шифрование данных, [настройте приложение](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) для поддержки SSL.
* Если будут использоваться подключения к удаленному рабочему столу для экземпляров роли, [настройте роли](cloud-services-role-enable-remote-desktop.md) для удаленного рабочего стола.
* Чтобы включить подробный мониторинг для веб-службы, настройте для нее систему диагностики Azure. *Минимальный мониторинг* (по умолчанию) реализуется на основе счетчиков производительности основной операционной системы, собирающих данные для экземпляров ролей (виртуальные машины). В рамках подробного мониторинга* для более тщательного анализа проблем обработки приложений отслеживаются дополнительные метрики в экземплярах ролей. Дополнительные сведения о включении диагностики в Azure см. в статье [Включение системы диагностики Azure в облачных службах Azure](cloud-services-dotnet-diagnostics.md).

Чтобы создать облачную службу с развертыванием веб-ролей или рабочих ролей, необходимо [создать соответствующий пакет службы](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Перед началом работы
* Если пакет SDK для Azure не установлен, щелкните **Install Azure SDK** (Установить пакет Azure SDK). Откроется [страница загрузок Azure](https://azure.microsoft.com/downloads/), откуда можно скачать пакет SDK для языка, выбранного для разработки кода приложения. (Также это можно сделать позднее.)
* Для экземпляров роли с сертификатами создайте сертификаты. В облачных службах используется PFX-файл с закрытым ключом. [Сертификаты можно отправить в Azure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) как при создании, так и при развертывании облачной службы.
* Если планируется развертывание облачной службы в территориальной группе, создайте соответствующую группу. Территориальные группы предназначены для развертывания облачной и других служб Azure в одном расположении региона. Чтобы создать такую группу, откройте область **Сети** классического портала Azure и перейдите на страницу **Территориальные группы**.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Практическое руководство. Создание облачной службы с помощью функции "Быстрое создание"
1. Войдите на [классический портал Azure](http://manage.windowsazure.com/) и выберите **Создать**>**Вычисления**>**Облачная служба**>**Быстрое создание**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. В поле **URL-адрес**введите имя поддомена для общедоступного URL-адреса, который будет использоваться для доступа к облачной службе в рабочем развертывании. URL-адрес для развертываний в рабочей среде имеет следующий формат: http://*myURL*.cloudapp.net.
3. В поле **Регион или территориальная группа**выберите регион или группу, в которых требуется развернуть облачную службу. Чтобы развернуть облачную службу в том же расположении региона, что и другие службы Azure, укажите территориальную группу.
4. Выберите **Создать облачную службу**.
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Состояние процесса можно контролировать с помощью области сообщений в нижней части окна.
   
    Созданная служба откроется в области **Облачные службы** . Об успешном создании службы свидетельствует состояние "Создано".
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Практическое руководство. Отправка сертификата для облачной службы
1. На [классическом портале Azure](http://manage.windowsazure.com/) щелкните **Облачные службы**, выберите имя облачной службы, а затем — элемент **Сертификаты**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Щелкните **Отправить сертификат** или **Отправить**.
3. В поле**Файл** щелкните **Обзор** и выберите соответствующий сертификат (PFX-файл).
4. В поле **Пароль**введите закрытый ключ сертификата.
5. Нажмите кнопку **OK** (флажок).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Ход процесса отправки можно контролировать в показанной ниже области сообщений. По завершении этого процесса сертификат добавляется в таблицу. В области сообщений нажмите OK, чтобы закрыть окно сообщения.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Практическое руководство. Развертывание облачной службы
1. На [классическом портале Azure](http://manage.windowsazure.com/) щелкните **Облачные службы**, выберите имя облачной службы, а затем — элемент **Панель мониторинга**.
2. Щелкните **Загрузите новое рабочее развертывание** или **Отправить**.
3. В поле **Метка развернутого приложения** введите имя нового развертывания, например MyCloudServicev4.
4. В поле **Пакет** щелкните **Обзор** и выберите файл пакета службы (CSPKG).
5. В поле **Конфигурация** щелкните **Обзор** и выберите файл конфигурации службы (CSCFG).
6. Если в облачной службе будут содержаться роли с одним экземпляром, установите флажок **Развернуть, даже если одна или несколько ролей содержат отдельный экземпляр** , чтобы обеспечить развертывание.
   
    Azure гарантирует доступность облачной службы в течение 99,95 % времени в процессе обновления или обслуживания только в том случае, если для каждой роли определены как минимум два экземпляра. При необходимости после развертывания облачной службы можно добавить дополнительные экземпляры роли на странице **Масштабирование**. Дополнительные сведения см. в разделе [Соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/).
7. Нажмите кнопку **OK** (флажок), чтобы начать развертывание облачной службы.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Состояние развертывания можно контролировать с помощью области сообщений. Нажмите кнопку ОК, чтобы закрыть окно сообщения.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Проверка успешного завершения развертывания
1. Нажмите на кнопку **Панель мониторинга**.
   
    В строке состояния должна отображаться информация о том, что служба **Запущена**.
2. В разделе **Сводка**щелкните URL-адрес сайта, чтобы открыть облачную службу в веб-браузере.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Дальнейшие действия
* [Общая настройка облачной службы](cloud-services-how-to-configure.md).
* Настройка [пользовательского имени домена](cloud-services-custom-domain-name.md).
* [Управление облачной службой](cloud-services-how-to-manage.md).
* Настройка [SSL-сертификатов](cloud-services-configure-ssl-certificate.md).


