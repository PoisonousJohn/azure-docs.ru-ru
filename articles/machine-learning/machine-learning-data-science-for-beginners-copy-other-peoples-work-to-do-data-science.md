---
title: "Копирование примеров обработки и анализа данных других пользователей в машинном обучении Azure | Документация Майкрософт"
description: "Секрет успешного процесса обработки и анализа данных: используйте работу других пользователей в своих целях. Воспользуйтесь примерами машинного обучения из коллекции Cortana Analytics."
keywords: "примеры обработки и анализа данных,пример машинного обучения,алгоритм кластеризации,пример алгоритма кластеризации"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: ec2be823-c325-4ad8-b8b2-3e664f1a44b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: 440a1ded1f1dc1b8fbe73e3bbcbbd68ec9422bf8
ms.contentlocale: ru-ru
ms.lasthandoff: 07/13/2017

---
# <a name="copy-other-peoples-work-to-do-data-science"></a>Копирование работы других пользователей для обработки и анализа данных
## <a name="video-5-data-science-for-beginners-series"></a>Видео 5. Обработка и анализ данных для начинающих
Одним из секретов успешного процесса обработки и анализа данных является умение использовать работу других пользователей в своих целях. Найдите в коллекции Cortana Analytics пример алгоритма кластеризации для выполнения собственного эксперимента машинного обучения.

Для получения оптимального результата просмотрите все видео. [Перейти к списку видео](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-copy-other-peoples-work-to-do-data-science/player]
>
>

## <a name="other-videos-in-this-series"></a>Другие видео из этого цикла
*Обработка и анализ данных для начинающих* — это пять коротких видеороликов с основными сведениями об обработке и анализе данных.

* Видео 1. [5 вопросов, на которые дают ответ обработка и анализ данных](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 мин 14 с)*
* Видео 2. [Готовы ли ваши данные к обработке и анализу?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 мин 56 с)*
* Видео 3. [Задайте вопрос, на который можно ответить с помощью данных](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 мин 17 с)*
* Видео 4. [Прогнозирование ответа с помощью простой модели](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 мин 42 с)*
* Видео 5. Копирование работы других пользователей для обработки и анализа данных.

## <a name="transcript-copy-other-peoples-work-to-do-data-science"></a>Расшифровка видео "Копирование работы других пользователей для обработки и анализа данных"
Добро пожаловать в пятое видео из цикла "Обработка и анализ данных для начинающих".

В этом видео вы узнаете, где можно взять примеры, служащие отправной точкой для начала работы. Чтобы извлечь максимальную пользу из этого видео, сначала необходимо просмотреть предыдущие видео этого цикла.

Одним из секретов успешного процесса обработки и анализа данных является умение использовать работу других пользователей в своих целях.

## <a name="find-examples-in-the-cortana-intelligence-gallery"></a>Поиск примеров в коллекции Cortana Intelligence
Корпорация Майкрософт предоставляет облачную службу [Машинное обучение Azure](https://azure.microsoft.com/services/machine-learning/), которую можно испытать бесплатно. Используя ее, вы получаете рабочую область, где можно экспериментировать с разными алгоритмами машинного обучения. Если вы разработаете готовое решение, его можно запустить как веб-службу.

В эту службу также входит **[коллекции Cortana Intelligence](http://aka.ms/CortanaIntelligenceGallery)**. Она содержит множество ресурсов, например коллекцию экспериментов Машинного обучения Azure, и модели, которые создали и предоставили другие пользователи. Эти эксперименты — отличный способ начать создание собственных решений, используя разработки других пользователей.

Эту коллекцию можно найти на сайте [aka.ms/CortanaIntelligenceGallery](http://aka.ms/CortanaIntelligenceGallery). Давайте внимательно его рассмотрим.

![коллекции Cortana Intelligence](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cortana-intelligence-gallery.png)

Если щелкнуть вверху вкладку **Experiments** (Эксперименты), то отобразятся недавно добавленные и самые популярные эксперименты в коллекции. Чтобы просмотреть другие эксперименты, щелкните вкладку **Browse All** (Просмотреть все) в верхней части экрана. Откроется страница, где можно ввести условия и выбрать фильтры для поиска.

## <a name="find-and-use-a-clustering-algorithm-example"></a>Поиск и использование примера с применением алгоритма кластеризации
Предположим, вы хотите увидеть, как работает кластеризация. В таком случае вам необходимо искать эксперименты, содержащие слова **clustering sweep**.

![Поиск экспериментов с применением кластеризации](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/search-for-clustering-experiments.png)

Вот интересный пример, который добавил один из пользователей в коллекцию.

![Эксперимент с применением кластеризации](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment.png)

Если щелкнуть этот эксперимент, откроется веб-страница с описанием работы, проделанной этим участником, и полученных результатов.

![Страница описания эксперимента с применением кластеризации](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment-description-page.png)

Обратите внимание на кнопку **Open in Studio**(Открыть в студии).

![Кнопка "Открыть в Studio"](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/open-in-studio.png)

Если нажать ее, откроется **Студия машинного обучения Azure**, где будет создана копия этого эксперимента. После этого она будет помещена в рабочую область пользователя. Эксперимент включает в себя набор данных, все выполненные операции обработки, использованные алгоритмы и сохраненные результаты.

![Эксперимент коллекции, открытый в студии машинного обучения — пример алгоритма кластеризации](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cluster-experiment-open-in-studio.png)

Теперь у нас есть отправная точка. Можно заменить имеющиеся данные на наши и настроить модель по своему усмотрению. Таким образом, у нас будет начальная база для создания решения, подготовленная опытными разработчиками.

## <a name="find-experiments-that-demonstrate-machine-learning-techniques"></a>Поиск экспериментов, демонстрирующих методы машинного обучения
В [коллекции Cortana Intelligence](http://aka.ms/CortanaIntelligenceGallery) есть эксперименты, предназначенные для предоставления практических примеров для новичков в области обработки и анализа данных. Например, здесь есть эксперимент, который показывает, как обрабатывать отсутствующие значения ([Methods for handling missing values](https://gallery.cortanaintelligence.com/Experiment/Methods-for-handling-missing-values-1)(Методы для обработки отсутствующих значений)). В нем содержится 15 разных методов подстановки пустых значений, а также описаны преимущества и особенности использования каждого из них.

![Эксперименты из коллекции, открытые в студии машинного обучения — методы для пропущенных значений](./media/machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science/experiment-methods-for-handling-missing-values.png)

[Коллекция Cortana Intelligence](http://aka.ms/CortanaIntelligenceGallery) — место для поиска рабочих экспериментов, которые можно использовать как отправную точку для разработки собственных решений.

Обязательно ознакомьтесь с другими видео из цикла "Обработка и анализ данных для начинающих" курса по Машинному обучению Microsoft Azure.

## <a name="next-steps"></a>Дальнейшие действия
* [Выполните свой первый эксперимент по обработке и анализу данных с использованием Машинного обучения Azure.](machine-learning-create-experiment.md)
* [Ознакомьтесь с введением в машинное обучение в Microsoft Azure.](machine-learning-what-is-machine-learning.md)

