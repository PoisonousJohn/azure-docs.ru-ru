---
title: "Руководство по службе контейнеров Azure — подготовка ACR | Документация Майкрософт"
description: "Руководство по службе контейнеров Azure — подготовка ACR"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, контейнеры, микрослужбы, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 3e1f7617bf2fc52ee4c15598f51a46276f4dc57d
ms.contentlocale: ru-ru
ms.lasthandoff: 08/24/2017

---

# <a name="deploy-and-use-azure-container-registry"></a>Развертывание реестра контейнеров Azure и его использование

Реестр контейнеров Azure (ACR) является частным реестром на базе Azure для образов контейнеров Docker. В этом руководстве (здесь представлена вторая его часть из семи) рассматриваются основные шаги для развертывания экземпляра реестра контейнеров Azure и отправки в него образов контейнеров. В частности, рассматриваются такие шаги:

> [!div class="checklist"]
> * развертывание экземпляра реестра контейнеров Azure (ACR);
> * добавление тегов к образу контейнера для ACR;
> * отправка образа в ACR.

В последующих руководствах данный экземпляр ACR интегрируется с кластером Kubernetes службы контейнеров Azure для безопасного выполнения образов контейнеров. 

## <a name="before-you-begin"></a>Перед началом работы

В [предыдущей части руководства](./container-service-tutorial-kubernetes-prepare-app.md) мы создали образ контейнера для простого приложения Azure для голосования. Теперь мы поместим этот образ в реестр контейнеров Azure. Если вы еще не создали образ приложения Azure для голосования, выполните инструкции из статьи [Create container images to be used with Azure Container Service](./container-service-tutorial-kubernetes-prepare-app.md) (Создание образов контейнеров с помощью службы контейнеров Azure). Описанные здесь шаги подходят для любого образа контейнера.

Для этого руководства требуется Azure CLI версии 2.0.4 или более поздней. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Развертывание реестра контейнеров Azure

При развертывании реестра контейнеров Azure сначала необходимо создать группу ресурсов. Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими.

Создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#create). В этом примере создается группа ресурсов с именем *myResourceGroup* в регионе *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Создайте реестр контейнеров Azure с помощью команды[az acr create](/cli/azure/acr#create). Имя контейнера реестра **должно быть уникальным**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

В остальной части этого руководства acrname будет заменять в примерах имя контейнера реестра.

## <a name="container-registry-login"></a>Вход в реестр контейнеров

Войдите в свой экземпляр ACR, прежде чем отправлять в него образы. Используйте команду [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#login), чтобы выполнить операцию. Укажите уникальное имя реестра контейнеров, заданное для него при создании.

```azurecli
az acr login --name <acrName>
```

После выполнения эта команда возвращает сообщение Login Succeeded (Вход выполнен).

## <a name="tag-container-images"></a>Присвоение тегов образам контейнеров

Каждый образ контейнера должен иметь тег с именем сервера входа (loginServer), указанным для реестра. Данный тег используется для маршрутизации при отправке образов контейнеров в реестр образов.

Чтобы просмотреть список сохраненных образов, используйте команду [docker images](https://docs.docker.com/engine/reference/commandline/images/).

```bash
docker images
```

Выходные данные:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Чтобы получить имя loginServer, выполните следующую команду.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Теперь пометьте образ *azure-vote-front* с помощью тега loginServer реестра контейнеров. Кроме того, добавьте `:redis-v1` в конец имени образа. Этот тег обозначает номер версии образа.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Добавив все нужные теги, выполните команду [docker images] (https://docs.docker.com/engine/reference/commandline/images/), чтобы проверить правильность работы.

```bash
docker images
```

Выходные данные:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Отправка образов в реестр

Отправьте образ *azure-vote-front* в реестр. 

Используйте следующий пример, заменив в нем имя loginServer ACR именем loginServer своей среды.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Для завершения операции требуется несколько минут.

## <a name="list-images-in-registry"></a>Перечисление образов в реестре

Чтобы получить список образов, отправленных в реестр контейнеров Azure, выполните команду [az acr repository list](/cli/azure/acr/repository#list). Укажите в команде имя нужного экземпляра ACR.

```azurecli
az acr repository list --name <acrName> --output table
```

Выходные данные:

```azurecli
Result
----------------
azure-vote-front
```

Чтобы увидеть теги для конкретного образа, используйте команду [az acr repository show-tags](/cli/azure/acr/repository#show-tags).

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Выходные данные:

```azurecli
Result
--------
redis-v1
```

По завершении работы с этим руководством образ контейнера будет сохранен в частном экземпляре реестра контейнеров Azure. В следующих частях руководства мы развернем этот образ из ACR в кластер Kubernetes.

## <a name="next-steps"></a>Дальнейшие действия

В этом руководстве вы подготовили реестр контейнеров Azure для использования в кластере Kubernetes ACS. Были выполнены следующие действия:

> [!div class="checklist"]
> * развертывание экземпляра реестра контейнеров Azure;
> * добавление тегов к образу контейнера для ACR;
> * отправка образа в ACR.

Перейдите к следующему руководству, чтобы ознакомиться с развертыванием кластера Kubernetes в Azure.

> [!div class="nextstepaction"]
> [Развертывание кластера Kubernetes в службе контейнеров Azure](./container-service-tutorial-kubernetes-deploy-cluster.md)
