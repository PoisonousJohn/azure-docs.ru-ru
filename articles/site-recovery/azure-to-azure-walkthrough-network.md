---
title: "Планирование сетей для репликации из VMware в Azure с помощью Site Recovery | Документация Майкрософт"
description: "В этой статье рассматривается планирование сетей, необходимое при репликации виртуальных машин Azure в Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: sujayt
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 7b37e853f6b97ba313111f9201f877846a28fae9
ms.contentlocale: ru-ru
ms.lasthandoff: 08/02/2017

---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Шаг 3. Планирование сетей для репликации виртуальной машины Azure

После прочтения [предварительных условий для развертывания](azure-to-azure-walkthrough-prerequisites.md) изучите информацию о рекомендациях по работе с сетями для репликации и восстановления виртуальных машин Azure из одного региона Azure в другой с помощью службы Azure Site Recovery. Именно об этом мы и поговорим в этой статье. 

- Изучив эту статью, вы получите четкое представление о том, как настроить исходящий доступ для виртуальных машин Azure, которые нужно реплицировать, и как подключиться из локального сайта к этим виртуальным машинам.
- Все комментарии можно добавить в конце этой статьи. Вопросы можно задать на [форуме по службам восстановления Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Сейчас репликация виртуальных машин Azure в Site Recovery находится на стадии предварительной версии.



## <a name="network-overview"></a>Обзор сети

Обычно виртуальные машины Azure расположены в виртуальной сети или подсети Azure, и подключение установлено из локального сайта к Azure с помощью Azure ExpressRoute или VPN-подключения.

Как правило, сети защищаются с помощью брандмауэров или групп безопасности сети (NSG). Для управления сетевым подключением брандмауэры могут использовать списки на основе IP- или URL-адресов. Группы безопасности сети используют правила на основе диапазона IP-адресов. 


Чтобы служба Site Recovery работала ожидаемым образом, необходимо внести некоторые изменения в исходящее сетевое подключение из виртуальных машин, которые требуется реплицировать. Site Recovery не поддерживает использование прокси-службы проверки подлинности для управления сетевым подключением. При использовании прокси-службы проверки подлинности репликацию невозможно включить. 


На следующей схеме показана типичная среда для приложения, работающего на виртуальных машинах Azure.

![customer-environment](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Среда виртуальных машин Azure**

Подключение также возможно установить из локального сайта к Azure с помощью Azure ExpressRoute или VPN-подключения. 

![customer-environment](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Локальное подключение к Azure**



## <a name="outbound-connectivity-for-urls"></a>Исходящие подключения для URL-адресов

При использовании прокси-сервера брандмауэра на основе URL-адресов для управления исходящими подключениями разрешите использование этих URL-адресов в Site Recovery.

**URL-адрес** | **Дополнительные сведения**  
--- | ---
*.blob.core.windows.net | Разрешает записывать данные из виртуальной машины в учетную запись хранения кэша в исходном регионе.
login.microsoftonline.com | Предоставляет авторизацию и проверку подлинности URL-адресов службы Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Обеспечивает связь со службой Site Recovery из виртуальной машины.
*.servicebus.windows.net | Необходимые для записи данные наблюдения и диагностики Site Recovery из виртуальной машины.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Исходящие подключения для диапазонов IP-адресов

- Вы можете автоматически создать соответствующие правила для группы безопасности сети, скачав и запустив [этот скрипт](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Мы советуем создать необходимые правила NSG в тестовой группе безопасности сети и проверить наличие проблем, прежде чем создавать правила для рабочей группы безопасности.
- Чтобы создать необходимое количество правил NSG, убедитесь, что ваша подписка есть в списке разрешений. Обратитесь в службу поддержки, чтобы увеличить ограничение правил NSG в вашей подписке.
При использовании любого прокси-сервера брандмауэра на основе IP-адресов или правил NSG для управления исходящими подключениями следующие диапазоны IP-адресов необходимо добавить в список разрешений в зависимости от расположения исходных и целевых виртуальных машин:

    - Все диапазоны IP-адресов в соответствии с исходным расположением. Скачайте [диапазоны](https://www.microsoft.com/download/confirmation.aspx?id=41653). Список разрешений необходим, чтобы данные можно было записать в учетную запись хранения кэша из виртуальной машины.
    - Все диапазоны IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365. Если новые IP-адреса добавляются в диапазон IP-адресов Office 365, необходимо создать новые правила NSG.
-     IP-адреса конечной точки службы Site Recovery ([доступны в XML-файле](https://aka.ms/site-recovery-public-ips)), которые зависят от вашего целевого расположения: 

   **Целевое расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
   --- | --- | ---
   Восточная Азия | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Юго-Восточная Азия | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Центральная Индия | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Южная Индия | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Северо-центральный регион США | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Северная Европа | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Западная Европа | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Восток США | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Запад США | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Южно-центральный регион США | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Центральный регион США | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Восток США 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Восточная часть Японии | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Западная часть Японии | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Южная часть Бразилии | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Восточная часть Австралии | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Юго-Восточная часть Австралии | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Центральная Канада | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Восточная Канада | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Западно-центральная часть США | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Западный регион США 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Западная часть Великобритании | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Южная часть Великобритании | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Конфигурация примера группы безопасности сети

В этом разделе показано, как настроить правила NSG для работы репликаций в виртуальной машине. При использовании правил NSG для управления исходящими подключениями используйте правила "Разрешить исходящие подключения по HTTPS" для всех необходимых диапазонов IP-адресов.

В этом примере исходное расположение виртуальной машины — "Восточная часть США", а целевое расположение репликации — "Центральная часть США".


### <a name="example"></a>Пример

#### <a name="east-us"></a>Восток США

1. Создайте правила, которые соответствуют [диапазонам IP-адресов восточной части США](https://www.microsoft.com/download/confirmation.aspx?id=41653). Список разрешений необходим, чтобы данные можно было записать в учетную запись хранения кэша из виртуальной машины.
2. Создайте правила для всех диапазонов IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365.
3. Создайте правила, которые соответствуют целевому расположению.

   **Расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
    --- | --- | ---
   Центральный регион США | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Центральный регион США

Эти правила необходимы, чтобы включить репликацию из целевого в исходный регион после отработки отказа.

1. Создайте правила, которые соответствуют [диапазонам IP-адресов центральной части США](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Создайте правила для всех диапазонов IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365.
3. Создайте правила, соответствующие исходному расположению.

   **Расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
    --- | --- | ---
   Восток США | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Имеющееся локальное подключение

При наличии канала ExpressRoute или VPN-подключения между локальным сайтом и исходным расположением в Azure придерживайтесь следующих рекомендаций.

**Конфигурация** | **Дополнительные сведения**
--- | ---
**Принудительное туннелирование** | Обычно маршрут по умолчанию (0.0.0.0/0) направляет исходящий интернет-трафик в локальное расположение, но это не рекомендуется. Трафик репликации и обмен данными со службой Site Recovery должны оставаться в пределах Azure.<br/><br/> В качестве решения добавьте определяемые пользователем маршруты (UDR) для [этих диапазонов IP-адресов](#outbound-connectivity-for-azure-site-recovery-ip-ranges), чтобы трафик не проходил в локальной среде.
**Соединение** | Если приложениям требуется подключаться к локальным компьютерам или локальные клиенты подключаются к приложению из локальной среды через VPN или ExpressRoute, убедитесь, что установлено по крайней мере [подключение типа "сеть — сеть"](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) между целевым регионом Azure и локальным сайтом.<br/><br/> Если между целевым регионом Azure и локальным сайтом большой объем трафика, создайте между ними еще одно [подключение ExpressRoute](../expressroute/expressroute-introduction.md).<br/><br/> Если вы хотите сохранить IP-адреса виртуальных машин после отработки отказа, оставьте подключение "сеть — сеть" и ExpressRoute целевого региона отключенным. Это гарантирует отсутствие конфликтов между диапазонами исходных и целевых IP-адресов.
**ExpressRoute** | Создайте канал ExpressRoute в исходном и целевом регионах.<br/><br/> Создайте подключение между исходной сетью и каналом ExpressRoute, а также между целевой сетью и каналом.<br/><br/> Мы рекомендуем использовать разные диапазоны IP-адресов в исходном и целевом регионе. Канал не сможет одновременно подключиться к нескольким сетям Azure с одинаковым диапазоном IP-адресов.<br/><br/> Вы можете создать виртуальные сети с одинаковым диапазоном IP-адресов в обоих регионах, а затем создать каналы ExpressRoute в обоих регионах. В случае отработки отказа отключите канал от исходной виртуальной сети и подключите к целевой виртуальной сети.<br/><br/> Если основной регион станет полностью недоступным, возможен сбой операции отключения. В этом случае подключение ExpressRoute к целевой виртуальной сети не будет установлено.
**ExpressRoute Premium** | Вы можете создать каналы в том же геополитическом регионе.<br/><br/> Для создания каналов в разных геополитических регионах требуется Azure ExpressRoute Premium.<br/><br/> При использовании версии Premium затраты увеличиваются. Если вы ее уже используете, дополнительные затраты не требуются.<br/><br/> Узнайте больше о [расположениях](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) и [ценах](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Дальнейшие действия

См. [раздел о создании хранилища (шаг 4)](azure-to-azure-walkthrough-vault.md).


