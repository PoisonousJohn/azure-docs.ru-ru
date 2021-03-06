---
title: "Управление службами Azure Analysis Services с помощью PowerShell | Документация Майкрософт"
description: "Описывается управление службами Azure Analysis Services с помощью PowerShell."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 5d50ee78da27149278edbd9521ddae8fcfe42c3b
ms.contentlocale: ru-ru
ms.lasthandoff: 07/20/2017

---

# <a name="manage-azure-analysis-services-with-powershell"></a>Управление службами Azure Analysis Services с помощью PowerShell

В этой статье описаны командлеты PowerShell, используемые для выполнения задач управления базами данных и сервером служб Azure Analysis Services. 

Для выполнения таких задач управления сервером, как создание или удаление сервера, приостановка или возобновление работы сервера или изменение уровня обслуживания (уровня служб), используются командлеты Azure Resource Manager (AzureRM). Для выполнения других задач управления базами данных, таких как добавление или удаление участников роли, обработка или секционирование, используются командлеты, включенные в том же модуле SqlServer, что и в службах SQL Server Analysis Services.

## <a name="permissions"></a>Разрешения
Для большинства задач PowerShell требуется, чтобы у пользователя были привилегии администратора на сервере служб Analysis Services, которым он управляет. Запланированные задачи PowerShell являются автоматическими операциями. У учетной записи, запускающей планировщик, должны быть привилегии администратора на сервере служб Analysis Services. 

Для выполнения операций с сервером с использованием командлетов AzureRm учетная запись или планировщик учетной записи должны также принадлежать к роли владельца для данного ресурса (указывается в настройках [управления доступом на основе ролей (RBAC) в Azure](../active-directory/role-based-access-control-what-is.md)). 

## <a name="server-operations"></a>Операции с сервером 
Командлеты служб Azure Analysis Services включены в модуль компонентов [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices). Для установки модулей командлетов AzureRM ознакомьтесь с [командлетами Azure Resource Manager](/powershell/azure/overview) в коллекции PowerShell.

|Командлет|Описание| 
|------------|-----------------| 
|[Export-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Экспорт журнала в файл.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Возвращает сведения об экземпляре сервера.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Создает экземпляр сервера.|
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Удаляет экземпляр сервера.|  
|[Suspend-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Приостанавливает работу экземпляра сервера.| 
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Возобновляет работу экземпляра сервера.|  
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Изменяет экземпляр сервера.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Проверяет существование экземпляра сервера.| 

## <a name="database-operations"></a>Операции с базой данных

Для операций с базами данных служб Azure Analysis Services используется тот же модуль [SqlServer](https://www.powershellgallery.com/packages/SqlServer), что и для служб SQL Server Analysis Services. Однако для служб Azure Analysis Services поддерживаются не все командлеты. 

Модуль SqlServer предоставляет командлеты для конкретных задач управления базой данных, а также командлет общего назначения Invoke-ASCmd, который принимает запрос TMSL или сценарий. Для служб Azure Analysis Services поддерживаются следующие командлеты из модуля SqlServer.

  
|Командлет|Описание|
|------------|-----------------| 
|[Add-RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Добавление участника в роль базы данных.| 
|[Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Архивация базы данных Analysis Services.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Удаление участника из роли базы данных.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Выполнение сценария TMSL.|
|[Invoke-ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Обработка базы данных.|  
|[Invoke-ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Обработка секции.| 
|[Invoke-ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Обработка таблицы.|  
|[Merge-Partition](https://msdn.microsoft.com/library/hh479576.aspx)|Объединение секции.|  
|[Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Восстановление базы данных Analysis Services.| 
  

## <a name="related-information"></a>Связанные сведения

* [Скачивание модуля PowerShell SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Скачивание SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [Модуль SqlServer в коллекции PowerShell](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabular Model Programming for Compatibility Level 1200 and higher](https://msdn.microsoft.com/library/mt712541.aspx) (Программирование табличной модели для уровня совместимости 1200 и более высокого)

