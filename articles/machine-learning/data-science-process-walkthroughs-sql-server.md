---
title: "Пошаговые руководства по обработке и анализу данных в SQL Server с помощью T-SQL, R и Python | Документация Майкрософт"
description: "Примеры использования R, Python и T-SQL для выполнения прогнозной аналитики в SQL Server."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 333fd4ee6dcdcbfd347d6b5e8cb900474f6ec8cc
ms.contentlocale: ru-ru
ms.lasthandoff: 08/23/2017

---

# <a name="sql-server-data-science-walkthroughs-using-r-python-and-t-sql"></a>Пошаговые руководства по обработке и анализу данных в SQL Server с помощью T-SQL, R и Python

В этих пошаговых руководствах для выполнения прогнозной аналитики используются SQL Server, службы R для SQL Server и службы Python для SQL Server. Код R и Python развертывается в хранимых процедурах. Они предусматривают выполнение инструкций, описанных в процессе обработки и анализа данных группы. Процесс обработки и анализа данных группы представлен в статье [Жизненный цикл процесса обработки и анализа данных группы](data-science-process-overview.md). 

Дополнительные пошаговые инструкции по обработке и анализу данных группы объединены по используемой **платформе**: 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-python-and-sql-queries-with-sql-server"></a>Прогнозирование чаевых за поездку в такси с помощью запросов Python и SQL с SQL Server 

В пошаговом руководстве [Процесс обработки и анализа данных группы на практике: использование SQL Server](machine-learning-data-science-process-sql-walkthrough.md) показано создание и развертывание моделей машинного обучения для классификации и регрессии с использованием SQL Server для общедоступного набора данных о поездках и тарифах в такси Нью-Йорка.


## <a name="predict-taxi-tips-using-microsoft-r-with-sql-server"></a>Прогнозирование чаевых за поездку в такси с помощью Microsoft R с SQL Server 

Руководство [End-to-end data science walkthrough for R and SQL Server](https://msdn.microsoft.com/library/mt612857.aspx) (Комплексное пошаговое руководство по обработке и анализу данных с использованием R и SQL Server) позволяет специалистам по обработке и анализу данных использовать сочетание кода R, данных SQL Server и настраиваемых функций SQL для создания и развертывания модели R на сервере SQL Server. Пошаговое руководство предназначено для ознакомления разработчиков R со службами R Services (в базе данных).


## <a name="predict-taxi-tips-using-r-from-t-sql-or-stored-procedures-with-sql-server"></a>Прогнозирование чаевых за поездку в такси с помощью R из T-SQL или хранимых процедур с SQL Server

В пошаговом руководстве [End-to-end data science walkthrough for R and SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) (Полное пошаговое руководство по обработке и анализу данных для R и SQL Server), предназначенном для программистов SQL, описано создание решений расширенной аналитики с помощью Transact-SQL и служб R в SQL Server для ввода в эксплуатацию решения R. 


## <a name="predict-taxi-tips-using-python-in-sql-server-stored-procedures"></a>Прогнозирование чаевых за поездку в такси с помощью Python в хранимых процедурах SQL Server

В пошаговом руководстве [In-Database Python Analytics for SQL Developers](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) (Аналитика на Python в базе данных для разработчиков SQL), предназначенном для программистов SQL, описан практический опыт создания решения машинного обучения в SQL Server. Демонстрируется, как внедрить Python в приложение, добавив код Python в хранимые процедуры.


## <a name="next-steps"></a>Дальнейшие действия

Описание ключевых компонентов, составляющих процесс обработки и анализа данных группы, см. в статье [Жизненный цикл процесса обработки и анализа данных группы](data-science-process-overview.md).

Описание жизненного цикла процесса обработки и анализа данных группы, который можно использовать для структурирования проектов по обработке и анализу данных, см. в статье [Жизненный цикл процесса обработки и анализа данных группы](data-science-process-lifecycle.md). Этот жизненный цикл представляет стандартные этапы выполнения проектов, от самого начала и до завершения. 
