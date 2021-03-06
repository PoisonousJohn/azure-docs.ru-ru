---
title: "Проверка подключения с помощью службы \"Наблюдатель за сетями\" Azure (Azure CLI 2.0) | Документация Майкрософт"
description: "На этой странице объясняется, как использовать проверку подключения с помощью службы \"Наблюдатель за сетями\" в интерфейсе командной строки Azure CLI 2.0"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: c1deaa40bfda0bf3858ad56d3d6a90df34351278
ms.contentlocale: ru-ru
ms.lasthandoff: 08/17/2017

---

# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a>Проверка возможности подключения с помощью службы Azure "Наблюдатель за сетями" в Azure CLI 2.0

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-connectivity-powershell.md)
> - [CLI 2.0](network-watcher-connectivity-cli.md)
> - [Azure REST API](network-watcher-connectivity-rest.md)

Узнайте, как проверить возможность прямого подключения TCP между виртуальной машиной и определенной конечной точкой.

## <a name="before-you-begin"></a>Перед началом работы

В данной статье предполагается, что у вас есть следующие ресурсы:

* экземпляр службы "Наблюдатель за сетями" в регионе, в котором нужно проверить возможность подключения;

* виртуальные машины, возможность подключения к которым необходимо проверить.

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> Для проверки возможности подключения требуется расширение виртуальной машины `AzureNetworkWatcherExtension`. Информацию об установке расширения для виртуальной машины Windows см. в статье [Расширение виртуальной машины агента Наблюдателя за сетями для Windows](../virtual-machines/windows/extensions-nwa.md), а для виртуальной машины Linux — в статье [Расширение виртуальной машины агента Наблюдателя за сетями для Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="register-the-preview-capability"></a>Регистрация возможностей предварительной версии 

Проверка возможности подключения в настоящее время поддерживается в общедоступной предварительной версии. Для использования этой функции ее необходимо зарегистрировать. Для этого выполните следующий пример команды в интерфейсе командной строки.

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

Чтобы проверить, была ли регистрация успешно завершена, выполните приведенную ниже команду интерфейса командной строки.

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

Если компонент был правильно зарегистрирован, выходные данные должны выглядеть следующим образом. 

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowNetworkWatcherConnectivityCheck",
  "name": "Microsoft.Network/AllowNetworkWatcherConnectivityCheck",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
``` 

## <a name="check-connectivity-to-a-virtual-machine"></a>Проверка возможности подключения к виртуальной машине

В этом примере проверяется возможность подключения к целевой виртуальной машине через порт 80.

### <a name="example"></a>Пример

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a>Ответ

Следующий ответ взят из предыдущего примера.  В этом ответе параметр `ConnectionStatus` имеет значение **Unreachable** (Недоступно). Как видите, все отправленные пробы завершились неудачей. Попытка подключения завершилась сбоем в виртуальном модуле из-за пользовательского правила `NetworkSecurityRule` с именем **UserRule_Port80**, настроенного на блокировку входящего трафика на порту 80. Эти сведения можно использовать для анализа проблем с подключением.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a>Проверка проблем с маршрутизацией

В этом примере проверяется возможность подключения между виртуальной машиной и удаленной конечной точкой.

### <a name="example"></a>Пример

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a>Ответ

В следующем примере состояние `connectionStatus` отображается как **Unreachable** (Недоступно). В блоке `hops` в разделе `issues` видно, что трафик заблокирован из-за `UserDefinedRoute`.

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a>Проверка задержки веб-сайта

В следующем примере проверяется возможность подключения к веб-сайту.

### <a name="example"></a>Пример

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a>Ответ

В следующем ответе видно, что параметр `connectionStatus` отображается со значением **Reachable** (Достижимо). Когда подключение будет установлено, отобразятся значения задержки.

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-to-a-storage-endpoint"></a>Проверка возможности подключения к конечной точке хранилища

В следующем примере проверяется возможность подключения с виртуальной машины к учетной записи хранилища BLOB-объектов.

### <a name="example"></a>Пример

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a>Ответ

JSON-код ниже — это пример ответа на предыдущий командлет. Так как проверка выполнена успешно, свойство `connectionStatus` отображается со значением **Reachable** (Достижимо).  Также отображаются сведения о числе прыжков, необходимых для доступа к BLOB-объекту в хранилище, а также о задержке.

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об автоматизации записи пакетов с помощью оповещений на виртуальной машине см. в статье, посвященной [созданию записи пакетов, активируемой с использованием оповещений](network-watcher-alert-triggered-packet-capture.md).

Сведения о состоянии (разрешен или запрещен) входящего и исходящего трафика виртуальной машины см. в статье, посвященной [проверке потока IP-адресов](network-watcher-check-ip-flow-verify-portal.md).

