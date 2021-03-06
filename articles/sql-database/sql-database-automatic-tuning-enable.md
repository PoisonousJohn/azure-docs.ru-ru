---
title: "Включение автоматической настройки для базы данных SQL Azure | Документация Майкрософт"
description: "Вы можете легко включить автоматическую настройку для базы данных SQL Azure."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.translationtype: Human Translation
ms.sourcegitcommit: 5bbeb9d4516c2b1be4f5e076a7f63c35e4176b36
ms.openlocfilehash: b391b1f7aa37c5a06fc320ce892534187deb4959
ms.contentlocale: ru-ru
ms.lasthandoff: 06/13/2017


---
# <a name="enable-automatic-tuning"></a>Включение автоматической настройки

База данных SQL Azure — это автоматически управляемая служба данных, которая постоянно отслеживает запросы и определяет действия, которые вы можете выполнять для повышения производительности рабочей нагрузки. Вы можете просматривать рекомендации и вручную применять их, или позволить базе данных SQL Azure автоматически применять действия по исправлению, что известно как **режим автоматической настройки**. Автоматическую настройку можно включить на уровне сервера или базы данных.

## <a name="enable-automatic-tuning-on-server"></a>Включение автоматической настройки на сервере

Чтобы включить автоматическую настройку на сервере базы данных SQL Azure, перейдите на сервер на портале Azure, а затем выберите в меню **Автонастройка**. Выберите параметры автоматической настройки, которые необходимо включить, а затем выберите **Применить**:

![сервер;](./media/sql-database-automatic-tuning-enable/server.png)

Параметры автоматической настройки на сервере применяются ко всем базам данных на сервере. По умолчанию все базы данных наследуют конфигурацию из родительского сервера, но ее можно переопределить, а также указать отдельно для каждой базы данных.

## <a name="configure-automatic-tuning-on-database"></a>Настройка автоматических параметров для базы данных

Портал Azure позволяет отдельно указать конфигурацию автоматической настройки в каждой базе данных.

> [!NOTE]
> Мы рекомендуем управлять конфигурацией автоматической настройки на уровне сервера, чтобы те же параметры конфигурации можно было автоматически применить в каждой базе данных. Настройте автоматические параметры для отдельной базы данных, если она отличается от других на одном сервере.
>

Чтобы включить автоматическую настройку для отдельной базы данных, перейдите в базу данных на портале Azure, а затем выберите **Автонастройка**. Вы можете настроить отдельную базу данных, чтобы она наследовала параметры из базы данных, установив флажок, или вы можете отдельно указать конфигурацию для базы данных.

![База данных](./media/sql-database-automatic-tuning-enable/database.png)

После выбора соответствующей конфигурации щелкните **Применить**.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения об автоматической настройке и о том, как она повышает производительность, см. в [этой статье](sql-database-automatic-tuning.md).
* Общие сведения о рекомендациях по производительности базы данных SQL Azure см. в статье [Помощник по работе с базами данных SQL](sql-database-advisor.md).
* См. статью [Анализ производительности запросов базы данных Azure SQL](sql-database-query-performance.md), чтобы узнать о том, как просмотреть влияние на производительность основных запросов.

