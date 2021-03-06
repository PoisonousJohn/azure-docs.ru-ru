---
title: "Использование мастера публикации приложений Azure в Visual Studio | Документация Майкрософт"
description: "Узнайте, как настроить различные параметры с помощью мастера публикации приложений Azure в Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 4d9e1564c3fcbdfd59edb0e24158df9954c26026
ms.contentlocale: ru-ru
ms.lasthandoff: 03/22/2017

---
# <a name="using-the-visual-studio-publish-azure-application-wizard"></a>Использование мастера публикации приложений Azure в Visual Studio
Разработанное в Visual Studio веб-приложение можно опубликовать в облачной службе Azure, используя мастер **публикации приложений Azure**. 

> [!NOTE]
> Эта статья посвящена развертыванию в облачных службах, а не на веб-сайтах. Сведения о развертывании на веб-сайтах см. в разделе [Развертывание веб-сайта Azure](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-the-publish-azure-application-wizard"></a>Доступ к мастеру публикации приложений Azure

Есть два способа открыть мастер публикации приложений Azure в зависимости от типа проекта Visual Studio.

**Если у вас проект облачной службы Azure:**

1. Создайте или откройте проект облачной службы Azure в Visual Studio.

1. В **обозревателе решений** щелкните проект правой кнопкой мыши и в контекстном меню выберите **Опубликовать**.

**Если у вас проект веб-приложения, который не является проектом Azure:**

1. Создайте или откройте проект облачной службы Azure в Visual Studio.

1. В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите в контекстном меню **Преобразовать** > **Преобразовать в проект облачной службы Microsoft Azure**. 

1. В **обозревателе решений** щелкните созданный проект Azure правой кнопкой мыши и в контекстном меню выберите **Опубликовать**.

## <a name="sign-in-page"></a>страница входа

![страница входа](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Учетная запись**: в раскрывающемся списке учетных записей выберите учетную запись или пункт **Добавить учетную запись**.

**Выбор подписки**: выберите подписку, которую следует использовать для развертывания.
   
## <a name="settings-page---common-settings-tab"></a>Страница "Параметры": вкладка "Общие параметры"   

![Общие параметры](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Облачная служба**: в раскрывающемся списке выберите существующую облачную службу или пункт **&lt;Создать>** и создайте облачную службу. Рядом с каждой облачной службой в скобках указывается центр обработки данных. Рекомендуется, чтобы расположение центра обработки данных для облачной службы и расположение центра обработки данных для учетной записи хранения совпадали (Дополнительные параметры).  

**Среда**: выберите **Рабочая среда** или **Промежуточная среда**. Выберите промежуточную среду, если хотите развернуть приложение в тестовой среде. 

**Конфигурация сборки**: выберите **Отладка** или **Выпуск**.

**Конфигурация службы**: выберите **Облако** или **Локально**.
   
**Включить удаленный рабочий стол для всех ролей**: установите этот флажок, если хотите удаленно подключаться к службе. Этот параметр в основном используется для устранения неполадок. Если этот флажок установлен, откроется диалоговое окно **Конфигурация удаленного рабочего стола** . Чтобы изменить конфигурацию, нажмите ссылку **Параметры**.
   
**Разрешить веб-развертывание для всех веб-ролей**: установите этот флажок, чтобы включить веб-развертывание для службы. Для использования этой функции должен быть установлен флажок **Включить удаленный рабочий стол для всех ролей**. Дополнительные сведения см. в статье [[Публикация облачной службы с помощью инструментов Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Страница "Параметры": вкладка "Дополнительные параметры"

![Дополнительные параметры](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Метка развернутого приложения**: оставьте имя по умолчанию или введите имя на свой выбор. Чтобы добавить дату к метке развертывания, оставьте флажок установленным. 
   
**Учетная запись хранения**: выберите учетную запись хранения для развертывания или выберите пункт **&lt;Создать> и создайте учетную запись хранения. Рядом с каждой учетной записью хранения в скобках указывается центр обработки данных. Рекомендуется, чтобы расположение центра обработки данных для учетной записи хранения и расположение центра обработки данных для облачной службы совпадали (Общие параметры).  
   
В учетной записи хранения Azure содержится пакет для развертывания приложения. После развертывания приложения пакет удаляется из учетной записи хранения.

**Удалить развертывание при сбое**: установите этот флажок, чтобы удалить развертывание в случае возникновения ошибок при публикации. Этот флажок должен быть снят, если вы хотите сохранить постоянный виртуальный IP-адрес облачной службы.

**Обновление развертывания**: установите этот флажок, если требуется развернуть только обновленные компоненты. Этот тип развертывания выполняется быстрее, чем полное развертывание. Этот флажок должен быть установлен, если вы хотите сохранить постоянный виртуальный IP-адрес облачной службы. 

**Обновление развертывания — Параметры**: используйте это диалоговое окно, чтобы указать другие параметры обновления ролей. Если выбрать **Добавочное обновление**, то все экземпляры приложения будут обновляться по очереди, обеспечивая доступность приложения в любое время. Если выбрать **Одновременное обновление**, то все экземпляры приложения будут обновляться одновременно. Одновременное обновление выполняется быстрее, но служба может быть недоступна во время обновления. 

![Параметры развертывания](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Включить IntelliTrace**: установите этот флажок, чтобы включить IntelliTrace. С помощью IntelliTrace можно записывать в журнал расширенные отладочные сведения для экземпляра роли при его запуске в Azure. Если вам необходимо найти причину проблемы, можете использовать журналы IntelliTrace для пошагового выполнения кода из Visual Studio, как если бы он запускался в Azure. Дополнительные сведения об использовании IntelliTrace см. в статье [Отладка опубликованной облачной службы Azure с помощью Visual Studio и IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Включить профилирование**: установите этот флажок, чтобы включить профилирование. Профилировщик Visual Studio позволяет выполнить глубокий анализ вычислительных аспектов работы облачной службы. Дополнительные сведения об использовании профилировщика Visual Studio см. в статье [Тестирование производительности облачной службы](./vs-azure-tools-performance-profiling-cloud-services.md).

**Enable Remote Debugger for all roles** (Включить удаленный отладчик для всех ролей): установите этот флажок, если требуется выполнять удаленную отладку. Дополнительные сведения об отладке облачных служб с помощью Visual Studio см. в статье [Отладка облачной службы или виртуальной машины Azure в Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Страница "Параметры диагностики"

![Параметры диагностики](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Система диагностики позволяет устранять неполадки в облачной службе Azure (или на виртуальной машине Azure). Дополнительные сведения о системе диагностики см. в статье [Настройка системы диагностики для облачных служб и виртуальных машин Azure](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Дополнительные сведения о службе Application Insights см. в статье [Что такое Azure Application Insights?](./application-insights/app-insights-overview.md)

## <a name="summary-page"></a>Страница "Сводка"

![Сводка](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Целевой профиль**: здесь вы можете создать профиль публикации на основе ранее выбранных параметров. Например, можно создать один профиль для тестовой среды, а другой — для рабочей. Чтобы сохранить профиль, щелкните значок **Сохранить** . Мастер создаст профиль и сохранит его в проекте Visual Studio. Чтобы изменить имя профиля, откройте список **Целевой профиль**, а затем выберите **<Управление…>**.
   
   > [!NOTE]
   > Профиль публикации отображается в обозревателе решений в Visual Studio, а параметры профиля записываются в файл с расширением AZUREPUBXML. Параметры сохраняются как атрибуты XML-тегов.
   > 
   > 

## <a name="publishing-your-application"></a>Публикация приложения

Когда все параметры развертывания проекта настроены, нажмите кнопку **Опубликовать** в нижней части диалогового окна. Состояние процесса можно отслеживать в окне **Вывод** в Visual Studio.

## <a name="next-steps"></a>Дальнейшие действия
- [Инструкции. Миграция и публикация веб-приложения в облачную службу Azure из среды Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Публикация облачной службы с помощью инструментов Azure](./vs-azure-tools-publishing-a-cloud-service.md)
- [Отладка опубликованной облачной службы Azure с помощью Visual Studio и IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [Тестирование производительности облачной службы](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Настройка системы диагностики для облачных служб и виртуальных машин Azure](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) 
- [Что такое Azure Application Insights?](./application-insights/app-insights-overview.md)
