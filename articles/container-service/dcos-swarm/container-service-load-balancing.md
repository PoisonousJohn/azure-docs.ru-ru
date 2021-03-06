---
title: "Балансировка нагрузки контейнеров в кластере DC/OS Azure | Документация Майкрософт"
description: "Сведения о балансировке нагрузки нескольких контейнеров в кластере DC/OS в Службе контейнеров Azure."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Контейнеры, микрослужбы, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: b0ab5a47e335998a7f1135b5715e9c50b89b6a68
ms.contentlocale: ru-ru
ms.lasthandoff: 07/25/2017

---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Балансировка нагрузки контейнеров в кластере DC/OS в Службе контейнеров Azure
В этой статье рассматривается создание внутренней подсистемы балансировки нагрузки в управляемой службе контейнеров Azure DC/OS с помощью средства Marathon-LB. Это позволит масштабировать приложения по горизонтали. Вы также сможете воспользоваться преимуществами кластеров общедоступных и частных агентов, поместив подсистемы балансировки нагрузки в общедоступный кластер, а контейнеры приложений в частный кластер. Изучив это руководство, вы:

> [!div class="checklist"]
> * Настройка подсистемы балансировки нагрузки Marathon
> * Развертывание приложения с помощью подсистемы балансировки нагрузки
> * Настройка Azure Load Balancer

Для выполнения действий, описанных в этом руководстве, потребуется кластер ACS DC/OS. При необходимости его можно создать с помощью следующего [примера сценария](./../kubernetes/scripts/container-service-cli-deploy-dcos.md).

Для этого руководства требуется Azure CLI версии 2.0.4 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Общие сведения о балансировке нагрузки

В кластере DC/OS службы контейнеров Azure существует два уровня балансировки нагрузки: 

**Azure Load Balancer** предоставляет общедоступные точки входа (к ним будут обращаться пользователи). Azure Load Balancer предоставляется автоматически службой контейнеров Azure. По умолчанию для него настроены порты 80, 443 и 8080.

**Подсистема балансировки нагрузки Marathon (marathon-lb)** направляет входящие запросы экземплярам контейнера для обслуживания. Marathon-lb приспосабливается к изменениям по мере масштабирования контейнеров, предоставляющих веб-службу. Эта подсистема балансировки нагрузки не предоставляется по умолчанию в службе контейнеров, но ее легко установить.

## <a name="configure-marathon-load-balancer"></a>Настройка подсистемы балансировки нагрузки Marathon

Балансировщик нагрузки Marathon динамически перенастраивается на основе развернутых контейнеров. Она также устойчива к сбоям в работе контейнера или агента. В этой ситуации Apache Mesos просто перезапускает контейнер в другом расположении, и подсистема балансировки нагрузки marathon-lb перенастраивается.

Для установки подсистемы балансировки нагрузки в кластере открытого агента выполните следующую команду.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Развертывание приложения с балансировкой нагрузки

Установив пакет балансировщика нагрузки Marathon, можно развернуть контейнер приложения, который нужно сбалансировать. 

Получите полное доменное имя открытых агентов.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Создайте файл с именем *hello-web.json* и вставьте в этот файл следующее содержимое. Метку `HAPROXY_0_VHOST` следует заменить на полное доменное имя агентов DC/OS. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Для запуска приложения используйте интерфейс командной строки DC/OS. По умолчанию Marathon развертывает приложение в частный кластер. Это означает, что указанное выше развертывание доступно только для подсистемы балансировки нагрузки (обычно такая схема является предпочтительной).

```azurecli-interactive
dcos marathon app add hello-web.json
```

После развертывания приложения введите в адресную строку браузера полное доменное имя кластера агента, чтобы открыть приложение с балансировкой нагрузки.

![Изображение приложения с балансировкой нагрузки](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Настройка Azure Load Balancer

По умолчанию Azure Load Balancer использует порты 80, 8080 и 443. При использовании одного из этих трех портов (как это было сделано в примере выше) дополнительные действия не требуются. Будет использоваться полное доменное имя агента подсистемы балансировки нагрузки, а при каждом обновлении будет использоваться один из трех веб-серверов, выбранный с помощью циклического перебора. 

Если применяется другой порт, необходимо добавить правило циклического перебора и зонд для этого порта в подсистему балансировки нагрузки. Это можно сделать в [интерфейсе командной строки Azure](../../azure-resource-manager/xplat-cli-azure-resource-manager.md) с помощью команд `azure network lb rule create` и `azure network lb probe create`.

## <a name="next-steps"></a>Дальнейшие действия

Из этого учебника вы узнали о балансировке нагрузки в ACS с помощью подсистемы балансировки нагрузки Marathon и Azure Load Balancer и выполнили следующие действия:

> [!div class="checklist"]
> * Настройка подсистемы балансировки нагрузки Marathon
> * Развертывание приложения с помощью подсистемы балансировки нагрузки
> * Настройка Azure Load Balancer

Чтобы узнать об интеграции хранилища Azure с DC/OS в Azure, перейдите к следующему руководству.

> [!div class="nextstepaction"]
> [Подключение общей папки Azure в кластере DC/OS](container-service-dcos-fileshare.md)
