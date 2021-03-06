---
title: "Как выполнить резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL | Документация Майкрософт"
description: "Узнайте, как выполнить резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL с помощью Azure CLI."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 4cd77c4ae4d9487aad11ea790c5d88a4eaff6077
ms.contentlocale: ru-ru
ms.lasthandoff: 07/08/2017

---

# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Как выполнить резервное копирование и восстановление сервера в базе данных Azure для PostgreSQL с помощью Azure CLI

Используйте базу данных Azure для PostgreSQL, чтобы восстановить базу данных сервера с более ранней точки во времени за период от 7 до 35 дней.

## <a name="prerequisites"></a>Предварительные требования
Вот что вам нужно, чтобы выполнить инструкции, приведенные в этом руководстве:
- [сервер и база данных Azure для PostgreSQL](quickstart-create-server-database-azure-cli.md);

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Если вы решили установить и использовать Azure CLI локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы проверить версию, в командной строке Azure CLI введите `az --version`. Чтобы выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="back-up-happens-automatically"></a>Резервное копирование выполняется автоматически
При использовании базы данных Azure для PostgreSQL служба базы данных автоматически создает резервную копию службы каждые 5 минут. 

Для уровня "Базовый" доступны резервные копии за 7 дней. Для уровня "Стандартный" — за 35 дней. Дополнительные сведения см. в статье [Параметры и производительность базы данных Azure для PostgreSQL: возможности, доступные в каждой ценовой категории](concepts-service-tiers.md).

С помощью этой функции автоматического резервного копирования можно восстановить сервер и его базы данных до более ранней даты или точки во времени.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Восстановление базы данных до предшествующей точки во времени с помощью Azure CLI
База данных Azure для PostgreSQL позволяет восстановить сервер до предшествующей точки во времени. Восстановленные данные копируются на новый сервер, а имеющийся сервер остается неизменным. Например, если таблица была случайно удалена в полдень, ее можно восстановить до момента сразу перед полуднем. Затем вы можете извлечь отсутствующие таблицы и данные из восстановленной копии сервера. 

Чтобы выполнить восстановление сервера, используйте команду Azure CLI [az postgres server restore](/cli/azure/postgres/server#restore).

### <a name="run-the-restore-command"></a>Выполнение команды restore

Для восстановления сервера в командной строке Azure CLI введите следующую команду:

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Для команды `az postgres server restore` обязательны указанные ниже параметры.
| Настройка | Рекомендуемое значение | Описание  |
| --- | --- | --- |
| resource-group |  myResourceGroup |  Группа ресурсов, в которой находится исходный сервер.  |
| name | mypgserver-restored | Имя нового сервера, созданного командой restore. |
| restore-point-in-time | 2017-04-13T13:59:00Z | Выберите точку во времени, до которой необходимо выполнить восстановление. Значения даты и времени должны находиться в пределах срока хранения резервной копии исходного сервера. Используйте формат даты и времени ISO8601. Можно использовать местный часовой пояс, например `2017-04-13T05:59:00-08:00`. Также можно использовать формат UTC Zulu, например `2017-04-13T13:59:00Z`. |
| source-server | mypgserver-20170401 | Имя или идентификатор исходного сервера, с которого необходимо выполнить восстановление. |

При восстановлении сервера до более ранней точки во времени будет создан новый сервер. Исходный сервер и его базы данных, начиная с указанного момента во времени, копируются на новый сервер.

Значения расположения и ценовой категории для восстановленного сервера совпадают со значениями исходного сервера. 

Команда `az postgres server restore` выполняется в синхронном режиме. После завершения восстановления сервера его можно снова использовать, чтобы повторить процесс восстановления до другого момента во времени. 

После завершения восстановления найдите новый сервер, чтобы убедиться, что данные были восстановлены, как и ожидалось.

## <a name="next-steps"></a>Дальнейшие действия
[Библиотеки подключений для базы данных Azure для PostgreSQL](concepts-connection-libraries.md)

