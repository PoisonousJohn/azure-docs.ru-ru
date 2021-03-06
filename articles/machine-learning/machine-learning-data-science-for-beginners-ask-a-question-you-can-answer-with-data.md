---
title: "Задайте вопрос, на который можно ответить с помощью данных. Задачи обработки и анализа данных в Azure. Машинное обучение Azure | Документация Майкрософт"
description: "Узнайте, как формулировать точные вопросы обработки и анализа данных. В этом вам поможет видео 3 из цикла \"Обработка и анализ данных для начинающих\". Включает сравнение вопросов классификации и регрессии."
keywords: "задачи обработки и анализа данных,вопросы обработки и анализа данных,формулирование вопроса,вопросы регрессии,вопросы классификации,точный вопрос"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: 5b9501e3-9964-417a-8ffc-8913103da77b
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: 403c496d4f032d1f373dacc16868abb40f968b6f
ms.contentlocale: ru-ru
ms.lasthandoff: 07/13/2017

---
# <a name="ask-a-question-you-can-answer-with-data"></a>Задайте вопрос, на который можно ответить с помощью данных
## <a name="video-3-data-science-for-beginners-series"></a>Видео 3. Обработка и анализ данных для начинающих
Узнайте, как формулировать задачи обработки и анализа данных в форме вопроса. В этом вам поможет видео 3 из цикла "Обработка и анализ данных для начинающих". Это видео содержит сравнение вопросов для алгоритмов классификации и регрессии.

Для получения оптимального результата просмотрите все видео. [Перейти к списку видео](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Другие видео из этого цикла
*Обработка и анализ данных для начинающих* — это пять коротких видеороликов с основными сведениями об обработке и анализе данных.

* Видео 1. [5 вопросов, на которые дают ответ обработка и анализ данных](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 мин 14 с)*
* Видео 2. [Готовы ли ваши данные к обработке и анализу?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 мин 56 с)*
* Видео 3. Задайте вопрос, на который можно ответить с помощью данных.
* Видео 4. [Прогнозирование ответа с помощью простой модели](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 мин 42 с)*
* Видео 5. [Копирование работы других пользователей для обработки и анализа данных](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 мин 18 с)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Расшифровка видео "Задайте вопрос, на который можно ответить с помощью данных"
Вы смотрите третье видео из цикла "Обработка и анализ данных для начинающих".  

В нем вы получите некоторые советы о том, как сформулировать вопрос, на который можно ответить с помощью данных.

Это видео может быть более полезным, если вы сначала просмотрите два предыдущих видео из этого цикла: "5 вопросов, на которые дают ответ обработка и анализ данных" и "Готовы ли ваши данные к обработке и анализу?"

## <a name="ask-a-sharp-question"></a>Постановка точного вопроса
Мы говорили о том, что обработка и анализ данных — это процесс использования имен (также называемых категориями или метками) и чисел для прогнозирования ответа на вопрос. Но это должен быть не просто любой вопрос, а *точный вопрос*

На общий вопрос не обязательно отвечать именем или числом. А на точный вопрос — обязательно.

Представьте себе, что вы нашли волшебную лампу с джином, который честно ответит на любой ваш вопрос. Но это хитрый джин, и он постарается дать максимально неопределенный и противоречивый ответ. Необходимо задать ему такой конкретный вопрос, чтобы у него не было другого выхода, кроме как сообщить вам то, что вы хотите знать.

Если задать ему общий вопрос, такой как "Что произойдет с моими акциями?", то джин может ответить: "Цена изменится". И это будет правдивый ответ, но не очень полезный.

Но если задать точный вопрос, такой как "Какой будет цена продажи моих акций на следующей неделе?", джин будет вынужден дать конкретный ответ и предсказать цену продажи.

## <a name="examples-of-your-answer-target-data"></a>Примеры ответа: целевые данные
После того, как вопрос сформулирован, проверьте, содержат ли ваши данные примеры ответов.

Если наш вопрос "Сколько будут стоить мои акции на следующей неделе?", необходимо убедиться, что наши данные включают журнал котировки акций.

Если наш вопрос "Какой автомобиль в моем парке сломается первым?", необходимо убедиться, что наши данные включают информацию о предыдущих поломках.

![Целевые данные — примеры ответа. Сформулируйте вопрос обработки и анализа данных.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Эти примеры ответов называются целевыми данными. Целевые данные — это то, что мы пытаемся спрогнозировать о будущих точках данных, будь то категория или число.

Если у вас нет целевых данных, необходимо их добавить. Без этого вы не сможете получить ответ на вопрос.

## <a name="reformulate-your-question"></a>Изменение формулировки вопроса
Иногда следует изменить текст вопроса, чтобы получить более практичный ответ.

Вопрос "Указывают ли эти данные на A или B?" прогнозирует категорию (или имя или метку) данных. Чтобы ответить на него, используется *алгоритм классификации*.

Вопрос "Сколько?" или "Как много?" прогнозирует сумму. Чтобы ответить на него, используется *алгоритм регрессии*.

Чтобы продемонстрировать, как это можно изменить, давайте возьмем для примера вопрос "Какой новостной репортаж представляет наибольший интерес для читателя?" Здесь запрашивается прогноз одного варианта из множества вариантов. Другими словами: "Это A, B, C или D?" В этом случае используется алгоритм классификации.

Но ответить на этот вопрос будет проще, если изменить его формулировку на следующую: "Насколько интересна этому читателю каждая новость из этого списка?" Теперь каждой статье можно присвоить числовую оценку, и тогда будет легко определить статью с наивысшей оценкой. Это пример перефразирования вопроса классификации в вопрос регрессии или вопрос "Сколько?"

![Измените формулировку вопроса. Сравнение вопроса классификации и вопроса регрессии.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

То, как задается вопрос, является ключом к выбору алгоритма ответа.

Можно заметить, что некоторые группы алгоритмов (такие как в нашем примере с новостным репортажем) тесно связаны между собой. Формулировку вопроса можно изменить таким образом, чтобы использовался алгоритм, который дает наиболее практичный ответ.

Но самое главное — задайте точный вопрос, то есть вопрос, на который можно ответить данными. И убедитесь, что у вас есть необходимые данные для ответа на него.

Мы обсудили некоторые основные принципы формирования вопроса, на который можно ответить с помощью данных.

Обязательно ознакомьтесь с другими видео из цикла "Обработка и анализ данных для начинающих" курса по Машинному обучению Microsoft Azure.

## <a name="next-steps"></a>Дальнейшие действия
* [Выполните первый эксперимент по обработке и анализу данных с использованием Студии машинного обучения Microsoft Azure.](machine-learning-create-experiment.md)
* [Ознакомьтесь с введением в машинное обучение в Microsoft Azure.](machine-learning-what-is-machine-learning.md)

