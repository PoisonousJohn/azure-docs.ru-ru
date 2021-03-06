---
title: "Azure Service Fabric: различия между версиями для Linux и Windows | Документация Майкрософт"
description: "Различия между предварительной версией Azure Service Fabric для Linux и Azure Service Fabric для Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 7b80bb7d4a4e6a1b4cf47ce87200f47339785c53
ms.contentlocale: ru-ru
ms.lasthandoff: 08/24/2017

---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Различия между Service Fabric для Linux (предварительная версия) и Windows (общедоступная версия)

Так как платформа Service Fabric для Linux доступна в предварительной версии, некоторые ее функции поддерживаются в Windows, но не в Linux. В конечном счете, когда появится общедоступная версия Service Fabric для Linux, наборы функций будут одинаковыми. В последующих выпусках это различие между наборами функций будет минимизировано. Между последними доступными версиями (то есть между версией 5.6 для Windows и версией 5.5 для Linux) существуют различия в следующих компонентах: 

* надежные коллекции (и надежные службы с отслеживанием состояния); 
* обратный прокси-сервер; 
* автономный установщик; 
* проверка схемы XML для файлов манифеста; 
* перенаправление консоли; 
* служба анализа сбоев (FAS);
* драйверы Docker Compose, а также драйверы томов и ведения журнала для контейнеров; 
* управление ресурсами для контейнеров и служб; 
* Служба DNS
* поддержка Azure Active Directory;
* команды интерфейса командной строки, эквивалентные некоторым командам PowerShell. 
* В кластере Linux можно выполнять только определенный набор команд PowerShell (подробнее об этом — в следующем разделе).

>[!NOTE]
>Перенаправление консоли не поддерживается в рабочих кластерах, даже в Windows.

Средства разработки в Windows и Linux также отличаются. В Windows используются Visual Studio, PowerShell, VSTS и трассировка событий Windows, а в Linux — Yeoman, Eclipse, Jenkins и LTTng.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Командлеты PowerShell, которые не работают в кластере Service Fabric для Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Дальнейшие действия
* [Подготовка среды разработки в Linux](service-fabric-get-started-linux.md)
* [Настройка среды разработки для Mac OS X](service-fabric-get-started-mac.md)
* [Создание первого приложения Azure Service Fabric](service-fabric-create-your-first-linux-application-with-java.md)
* [Getting started with Eclipse Plugin for Service Fabric Java application development](service-fabric-get-started-eclipse.md) (Начало работы с подключаемым модулем Eclipse для разработки приложения Service Fabric на Java)
* [Создание первого приложения Azure Service Fabric](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Manage an Azure Service Fabric application by using Azure Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) (Управление приложением Azure Service Fabric с помощью интерфейса командной строки Azure Service Fabric)

