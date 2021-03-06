---
title: "Оптимизация производительности MySQL на ОС Linux | Документация Майкрософт"
description: "Узнайте, как оптимизировать MySQL в виртуальной машине Azure под управлением Linux."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.contentlocale: ru-ru
ms.lasthandoff: 06/03/2017


---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Оптимизация производительности MySQL на виртуальных машинах Linux в Azure
Существует множество факторов, влияющих на производительность MySQL в Azure, которые зависят и от выбора виртуального оборудования, и от конфигурации программного обеспечения. Эта статья посвящена оптимизации производительности с помощью конфигураций хранилища, системы и базы данных.

> [!IMPORTANT]
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель Azure Resource Manager](../../../resource-manager-deployment-model.md) и классическая модель. В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний Майкрософт рекомендует использовать модель диспетчера ресурсов. Сведения об оптимизации виртуальных машин Linux с помощью модели Resource Manager см. в статье [Оптимизация виртуальной машины Linux в Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Использование RAID на виртуальной машине Azure
Хранилище — ключевой фактор, влияющий на производительность базы данных в облачных средах. По сравнению с одним диском, RAID может обеспечить более быстрый доступ за счет параллелизма. Дополнительные сведения см. в описании [стандартных уровней RAID](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

С помощью RAID в Azure можно существенно увеличить пропускную способность и время ответа для операций ввода-вывода. Наши лабораторные тесты показали, что при удвоении количества дисков RAID (с двух до четырех, с четырех до восьми и т. д.) удваивается пропускная способность ввода-вывода дисков, а время ответа операций ввода-вывода уменьшается в среднем в два раза. Дополнительные сведения см. в [приложении А](#AppendixA).  

Помимо дисковых операций ввода-вывода производительность MySQL увеличивается при увеличении уровня RAID.  Дополнительные сведения см. в [приложении Б](#AppendixB).  

Кроме того, вас может заинтересовать размер блоков. Обычно чем больше размер блока, тем ниже нагрузка, особенно для объемных операций записи. Но если размер блока слишком большой, это может привести к дополнительной нагрузке, которая нивелирует преимущества RAID. Текущий размер блоков по умолчанию — 512 КБ. Он является оптимальным для большинства рабочих сред. Дополнительные сведения см. в [приложении В](#AppendixC).   

Для виртуальных машин разных типов существуют ограничения на количество дисков, которые можно добавить. Эти ограничения описаны в статье [Размеры для облачных служб](http://msdn.microsoft.com/library/azure/dn197896.aspx). Чтобы выполнить пример c RAID в этой статье, вам понадобится 4 подключенных диска данных, хотя вы можете настроить RAID и с меньшим количеством дисков.  

В этой статье предполагается, что вы уже создали виртуальную машину Linux, а также установили и настроили MySQL. Дополнительную информацию о начале работы см. в статье "Как установить MySQL в Azure".  

### <a name="set-up-raid-on-azure"></a>Настройка RAID в Azure
Ниже объясняется, как создать RAID в Azure с использованием портала Azure. RAID также можно настроить с помощью сценариев Windows PowerShell.
В этом примере мы настроим RAID 0 с 4 дисками.  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a>Добавление диска данных в виртуальную машину
На портале Azure перейдите к панели мониторинга и выберите виртуальную машину, к которой нужно добавить диск данных. В этом примере виртуальная машина — mysqlnode1.  

<!--![Virtual machines][1]-->

Щелкните **Диски** и выберите **Подключить новый**.

![Добавление дисков в виртуальную машину](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Создайте новый диск объемом 500 ГБ. Для параметра **Настройки кэша узла** необходимо задать значение **Нет**.  Закончив, нажмите кнопку **ОК**.

![Присоединить пустой диск](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Теперь вы добавили один пустой диск в виртуальную машину. Повторите этот шаг еще три раза, чтобы настроить четыре диска данных для RAID.  

Добавленные диски можно просмотреть в виртуальной машине, открыв журнал сообщений ядра. Например, чтобы просмотреть этот журнал в Ubuntu, используйте следующую команду:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a>Создание RAID с дополнительными дисками
Далее описано, как выполняется [Настройка программного RAID-массива в Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Если вы используете файловую систему XFS, после создания RAID выполните следующие шаги.
>
>

Чтобы установить файловую систему XFS в ОС Debian, Ubuntu или Linux Mint, используйте следующую команду:  

    apt-get -y install xfsprogs  

Чтобы установить файловую систему XFS для ОС Fedora, CentOS или RHEL, используйте следующую команду:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Настройка нового пути к хранилищу
Чтобы настроить новый путь к хранилищу, используйте следующую команду:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a>Копирование исходных данных в новый путь к хранилищу
Чтобы скопировать данные в новый путь к хранилищу, используйте следующую команду:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Изменение разрешений таким образом, чтобы MySQL могла получить доступ (на чтение и запись) к диску данных
Для изменения разрешений используйте следующую команду:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a>Настройка алгоритма планирования дисковых операций ввода-вывода
В Linux реализовано четыре типа алгоритмов планирования операций ввода-вывода:  

* алгоритм NOOP (без операции);
* алгоритм крайнего срока (крайний срок);
* алгоритм организации полностью равноправных очередей (CFQ);
* алгоритм бюджетного периода (прогнозирование).  

Вы можете выбрать различные планировщики операций ввода-вывода с различными сценариями для оптимизации производительности. В среде с полностью произвольным порядком доступа производительность не зависит от используемого алгоритма: алгоритма равноправных очередей или алгоритма крайнего срока. Для среды базы данных MySQL мы рекомендуем алгоритм крайнего срока, поскольку он повышает стабильность. Если выполняется много последовательных операций ввода-вывода, метод равноправных очередей может снизить производительность дисковых операций ввода-вывода.   

Для твердотельных накопителей и некоторых других видов оборудования алгоритм NOOP или алгоритм крайнего срока обеспечивают более высокую производительность, чем планировщик по умолчанию.   

В ядре версий до 2.5 по умолчанию для операций ввода-вывода использовался алгоритм крайнего срока. Начиная с ядра 2.6.18 для планирования операций ввода-вывода по умолчанию используется алгоритм равноправных очередей.  Этот параметр можно указать при загрузке ядра или динамически изменять во время работы системы.  

Следующий пример для семейства дистрибутивов Debian демонстрирует, как узнать, какой используется планировщик, и установить в качестве планировщика по умолчанию алгоритм NOOP.  

### <a name="view-the-current-io-scheduler"></a>Просмотр текущего планировщика операций ввода-вывода
Чтобы узнать используемый планировщик, выполните следующую команду:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Вы увидите следующий результат, который указывает тип текущего планировщика:  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a>Изменение текущего устройства (/dev/sda) для алгоритма планирования операций ввода-вывода
Выполните следующую команду, чтобы изменить текущее устройство:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Настраивать алгоритм только для устройства /dev/sda бесполезно. Его необходимо настроить для всех дисков данных, на которых расположена база данных.  
>
>

Вы увидите следующий результат, означающий, что файл grub.cfg успешно перестроен и в качестве планировщика по умолчанию теперь используется алгоритм NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Для семейства дистрибутивов Red Hat потребуется только одна команда:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Настройка параметров операций системного файла
Мы рекомендуем отключить функцию ведения журнала времени *atime* в файловой системе. Atime — время последнего доступа к файлу. При каждом обращении к файлу файловая система записывает метку времени в журнал. Однако эта информация используется редко. При необходимости эту функцию можно отключить, что позволит уменьшить общее время доступа к диску.  

Для этого следует изменить файл конфигурации файловой системы /etc/fstab и добавить параметр **noatime** .  

Например, добавьте в файл vim /etc/fstab параметр noatime, как показано в следующем примере:  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Затем подключите файловую систему заново с помощью следующей команды:  

    mount -o remount /RAID0

Проверьте измененный результат. Теперь при изменении файла время доступа не обновляется. В следующих примерах представлен код до и после изменения.

До:        

![Код до внесения изменений][5]

После:

![Код после внесения изменений][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Увеличение максимального количества системных дескрипторов для увеличения степени параллелизма
MySQL — база данных с высокой степенью параллелизма. Количество параллельных дескрипторов по умолчанию для Linux — 1024. Однако их не всегда достаточно. Выполните следующие шаги, чтобы увеличить максимальное количество параллельных дескрипторов системы для поддержки высокой степени параллелизма MySQL.

### <a name="modify-the-limitsconf-file"></a>Изменение файла limits.conf
Чтобы увеличить максимально допустимое количество параллельных дескрипторов, добавьте следующие четыре строки в файл /etc/security/limits.conf. Обратите внимание, что максимальное количество, поддерживаемое системой, — 65536.   

    * soft nofile 65536
    * hard nofile 65536
    * soft nproc 65536
    * hard nproc 65536

### <a name="update-the-system-for-the-new-limits"></a>Обновление операционной системы для новых ограничений
Для обновления системы выполните следующие команды:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a>Обеспечение обновления ограничений при загрузке
Вставьте следующие команды в файл /etc/rc.local, чтобы они выполнялись при каждой загрузке.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>Оптимизация базы данных MySQL
При настройке MySQL в Azure можно использовать те же методы повышения производительности, что и на локальной машине.  

Основные правила оптимизации операций ввода-вывода:   

* увеличьте размер кэша;
* уменьшите время ответа при выполнении операций ввода-вывода.  

Чтобы оптимизировать параметры сервера MySQL, можно обновить файл my.cnf, который является файлом конфигурации по умолчанию для серверных и клиентских компьютеров.  

Следующие элементы конфигурации являются основными факторами, влияющими на производительность MySQL.  

* **innodb_buffer_pool_size**. Буферный пул содержит буферизованные данные и индекс. Обычно для этого параметра задается значение, равное 70 % объема физической памяти.
* **innodb_log_file_size**. Это размер журнала повторяемых операций. Журналы повторяемых операций используются, чтобы обеспечить высокую скорость, надежность и возможность восстановления операций записи после сбоя. Для этого параметра задается значение 512 МБ, что обеспечит достаточно места для ведения журнала операций записи.
* **max_connections**. Иногда приложения не закрывают подключения должным образом. Если установить большее значение, у сервера будет больше времени для очистки неактивных подключений. Максимально возможное количество подключений — 10 000, но мы рекомендуем ограничить его до 5000.
* **Innodb_file_per_table**. Этот параметр позволяет включить или отключить возможность InnoDB сохранять таблицы в отдельных файлах. Если включить этот параметр, вы сможете эффективно применять некоторые расширенные функции администрирования. С точки зрения производительности это позволит ускорить передачу табличного пространства и оптимизировать управление мусором. Рекомендуемое значение для этого параметра — ON.</br></br>
Начиная с версии MySQL 5.6 по умолчанию устанавливается значение ON, поэтому дополнительных действий не требуется. В более ранних версиях этот параметр по умолчанию отключен. Его необходимо изменить прежде, чем загружать данные, поскольку его значение влияет только на новые таблицы.
* **innodb_flush_log_at_trx_commit**. По умолчанию установлено значение 1. Допустимый диапазон значений — от 0 до 2. Значение по умолчанию — наиболее подходящий вариант для автономной базы данных MySQL. При установке значения 2 достигается наибольший уровень целостности данных. Это значение подходит для главного узла в кластере MySQL. При установке значения 0 допускается потеря данных, которая может повлиять на надежность и в некоторых случаях увеличить производительность. Это значение подходит для подчиненного узла в кластере MySQL.
* **Innodb_log_buffer_size**. Буфер журнала позволяет выполнять транзакции без необходимости записи журнала на диск до их фиксации. Однако при работе с большим двоичным объектом или текстовым полем кэш будет быстро заполняться, что увеличит частоту дисковых операций ввода-вывода. Лучше увеличить размер буфера, если для переменной состояния Innodb_log_waits установлено значение, отличное от 0.
* **query_cache_size**. Лучше всего сразу отключить его. Установите для параметра query_cache_size значение 0 (начиная с MySQL 5.6 это значение используется по умолчанию) и используйте другие методы для ускорения запросов.  

В [приложении Г](#AppendixD) вы найдете сравнение производительности до и после оптимизации.

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Включение журнала медленных запросов MySQL для анализа узкого места производительности
Журнал медленных запросов MySQL может помочь вам определить медленные запросы в MySQL. После включения этого механизма вы можете использовать инструменты MySQL, такие как **mysqldumpslow** , для определения узкого места производительности.  

По умолчанию он отключен. Если включить ведение журнала медленных запросов, может немного увеличиться нагрузка на ЦП. Мы рекомендуем включать его ненадолго, только для устранения узких мест производительности. Включение журнала медленных запросов:

1. Измените файл my.cnf, добавив в его конец следующие строки:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Перезапустите сервер MySQL.

        service  mysql  restart

3. С помощью команды **show** проверьте, действуют ли изменения настроек.

![Включение Slow-query-log][7]   

![Результаты Slow-query-log][8]

В этом примере видно, что функция медленных запросов включена. Затем можно использовать инструмент **mysqldumpslow** , чтобы определить узкие места производительности и оптимизировать ее (например, путем добавления индексов).

## <a name="appendices"></a>Приложения
Здесь приводятся результаты теста производительности, выполненного в целевой лабораторной среде. Они предоставляют обобщенную информацию по эффективности разных стратегий повышения производительности. Результаты могут отличаться для разных сред и версий продукта.

### <a name="AppendixA"></a>Приложение А  
**Производительность диска (количество операций ввода-вывода в секунду) с разными уровнями RAID**

![Количество операций ввода-вывода в секунду с разными уровнями RAID][9]

**Команды тестирования**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> В рабочей нагрузке этого теста используется 64 потока в попытке достичь верхнее предельное значение RAID.
>
>

### <a name="AppendixB"></a>Приложение Б  
**Сравнение производительности (пропускной способности) MySQL с разными уровнями RAID**   
(Файловая система XFS)

![Сравнение производительности MySQL с разными уровнями RAID][10]  
![Сравнение производительности MySQL с разными уровнями RAID][11]

**Команды тестирования**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Сравнение производительности (OLTP) MySQL с разными уровнями RAID**  
![Сравнение производительности (OLTP) MySQL с разными уровнями RAID][12]

**Команды тестирования**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Приложение В   
**Сравнение производительности диска (количества операций ввода-вывода в секунду) для разного размера блоков**  
(Файловая система XFS)

![][13]

**Команды тестирования**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Для этого теста использовались файлы размером 30 ГБ и 1 ГБ и файловая система XFS с RAID 0 (4 диска).

### <a name="AppendixD"></a>Приложение Г  
**Сравнение производительности (пропускной способности) MySQL до и после оптимизации**  
(Файловая система XFS)

![Сравнение производительности (пропускной способности) MySQL до и после оптимизации][14]

**Команды тестирования**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Значения параметров конфигурации по умолчанию и для оптимизации выглядят следующим образом.**

| Параметры | значение по умолчанию | Оптимизация |
| --- | --- | --- |
| **innodb_buffer_pool_size** |None |7 ГБ |
| **innodb_log_file_size** |5 МБ |512 МБ |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 МБ |128 МБ |
| **query_cache_size** |16 МБ |0 |

Дополнительную информацию о [параметрах конфигурации для оптимизации](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html) см. в [официальной документации MySQL](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Тестовая среда**  

| Оборудование | Сведения |
| --- | --- |
| ЦП |AMD Opteron(tm), процессор 4171 HE, 4 ядра |
| Память |14 ГБ |
| Диск |10 ГБ на диск |
| ОС |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png


