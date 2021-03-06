---
title: "Масштабирование обработки мультимедиа с помощью портала Azure | Документация Майкрософт"
description: "В этом руководстве описывается, как масштабировать обработку данных мультимедиа с помощью портала Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: e6ef52f7aee8b2a0d0dd6ebc99ad7a8c5c11f525
ms.contentlocale: ru-ru
ms.lasthandoff: 07/06/2017

---
# <a name="change-the-reserved-unit-type"></a>Изменение типа зарезервированных единиц
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Портал](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Обзор

Учетная запись служб мультимедиа связана с типом зарезервированных единиц, который определяет скорость обработки задач обработки мультимедиа. Вы можете выбрать один из следующих типов зарезервированных единиц: **S1**, **S2** или **S3**. Например, если использовать тип зарезервированной единицы **S2**, задание по кодированию выполняется быстрее по сравнению заданием, для которого выбран тип **S1**.

Помимо указания типа зарезервированных единиц, можно также указать параметр, позволяющий использовать **зарезервированные единицы** при подготовке учетной записи. Количеством подготовленных зарезервированных единиц определяется количество задач мультимедиа, которые могут одновременно обрабатываться в данной учетной записи.

>[!NOTE]
>Зарезервированные единицы позволяют распараллелить всю обработку мультимедиа, включая задания индексирования с использованием индексатора мультимедийных данных Azure. Однако в отличие от кодировки, задания индексирования не будут обрабатываться быстрее при использовании более производительных зарезервированных единиц.

> [!IMPORTANT]
> Обязательно ознакомьтесь с этим [обзором](media-services-scale-media-processing-overview.md) , чтобы получить дополнительные сведения о масштабировании обработки мультимедиа.
> 
> 

## <a name="scale-media-processing"></a>Масштабирование обработки мультимедиа
Чтобы изменить тип зарезервированных единиц и число зарезервированных единиц, выполните следующие действия:

1. Выберите учетную запись служб мультимедиа Azure на [портале Azure](https://portal.azure.com/).
2. В окне **Параметры** выберите **Зарезервированные единицы мультимедиа**.
   
    Чтобы изменить число зарезервированных единиц для выбранного типа зарезервированных единиц, используйте ползунок **Media Served Units** (Зарезервированные единицы мультимедиа).
   
    Чтобы изменить **ТИП ЗАРЕЗЕРВИРОВАННЫХ ЕДИНИЦ**, выберите S1, S2 или S3.
   
    ![Страница "Процессоры"](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Нажмите кнопку СОХРАНИТЬ, чтобы сохранить изменения.
   
    Новые зарезервированные единицы выделяются сразу после нажатия кнопки "СОХРАНИТЬ".

## <a name="next-steps"></a>Дальнейшие действия
Просмотрите схемы обучения работе со службами мультимедиа.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


