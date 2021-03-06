---
title: "Производительность и масштабирование в устойчивых функциях — Azure"
description: "Общие сведения о расширении устойчивых функций для Функций Azure."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 10ce74097388a0283797e4692126c5039e8d4dd0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Производительность и масштабирование в устойчивых функциях (Функции Azure)

Чтобы оптимизировать производительность и масштабирование, важно понять уникальные характеристики масштабирования [устойчивых функций](durable-functions-overview.md).

Чтобы понять реакции на события масштабирования, необходимо изучить некоторые сведения о базовом поставщике службы хранилища Azure, используемом устойчивыми функциями.

## <a name="history-table"></a>Таблица журнала

Таблица журнала — это таблица службы хранилища Azure, содержащая события журнала для всех экземпляров оркестрации. При выполнении экземпляров в таблицу добавляются новые строки. Ключ раздела этой таблицы является производным от идентификатора экземпляра оркестрации. В большинстве случаев эти значения являются случайными, что обеспечивает оптимальное распределение внутренних разделов в службе хранилища Azure.

## <a name="internal-queue-triggers"></a>Триггеры внутренней очереди

Функции оркестратора и действия активируются внутренними очередями в учетной записи хранения по умолчанию приложения-функции. Существует два типа очередей в устойчивых функциях: очередь **управления** и **рабочих элементов**.

### <a name="the-work-item-queue"></a>Очередь рабочих элементов

В устойчивых функциях на каждый центр задач имеется одна очередь рабочих элементов. Это основная очередь, и ее поведение аналогично любым другим очередям `queueTrigger` в Функциях Azure. Эта очередь используется для активации *функций действий* без отслеживания состояния. Когда приложение устойчивых функций масштабируется до нескольких виртуальных машин, все они конкурируют за получение работы из очереди рабочих элементов.

### <a name="control-queues"></a>Очередь управления

*Очередь управления* является более сложной, чем очередь рабочих элементов. Она используется для активации функций оркестратора с отслеживанием состояния. Так как экземпляры функции оркестратора представляют собой единичные экземпляры с отслеживанием состояния, невозможно использовать конкурирующую потребительскую модель для распределения нагрузки между виртуальными машинами. Вместо этого сообщения оркестратора распределяются по нескольким очередям управления. Дополнительные сведения об этом приведены в последующих разделах.

Очереди управления содержат различные типы сообщений жизненного цикла оркестрации. Примеры включают [сообщения об управлении оркестрацией](durable-functions-instance-management.md), сообщения *ответа* функции действия и сообщения таймера.

## <a name="orchestrator-scale-out"></a>Развертывание оркестратора

Функции действий не отслеживают состояние и автоматически развертываются путем добавления виртуальных машин. Функции оркестратора, в свою очередь, *разделяются* между одной или несколькими очередями управления. Число очередей управления является фиксированным, и его нельзя изменить после начала создания нагрузки.

При развертывании нескольких экземпляров узлов функции (обычно на разных виртуальных машинах) каждый экземпляр блокируется на одной из очередей управления. Эта блокировка гарантирует, что экземпляр оркестрации одновременно выполняется только на одной виртуальной машине. Это означает, что если центр задач настроен с тремя очередями управления, экземпляры оркестрации можно распределять максимум между тремя виртуальными машинами. Для повышения емкости выполнения функции действия можно добавить дополнительные виртуальные машины.  Однако дополнительные ресурсы не будут использоваться для выполнения функций оркестратора.

На схеме ниже показано взаимодействие узла Функций Azure с сущностями хранилища в развернутой среде.

![Схема масштабирования](media/durable-functions-perf-and-scale/scale-diagram.png)

Как видно, все виртуальные машины могут конкурировать за сообщения в очереди рабочих элементов. Тем не менее получать сообщения из очередей управления могут только три виртуальные машины, и каждая из них блокирует одиночную очередь управления.

Экземпляры оркестрации распределяются между экземплярами очереди управления, запустив внутреннюю хэш-функцию для идентификатора экземпляра оркестрации. Идентификаторы экземпляров автоматически генерируются и являются случайными по умолчанию. Это гарантирует, что экземпляры распределяются между всеми доступными очередями управления. Текущее число по умолчанию поддерживаемых разделов очереди управления — **4**.

> [!NOTE]
> Сейчас невозможно настроить количество разделов в Функциях Azure. [Сейчас рассматривается возможность поддержки такого варианта настройки](https://github.com/Azure/azure-functions-durable-extension/issues/73).

По своей природе функции оркестратора упрощенные, поэтому они не нуждаются в большой вычислительной мощности. По этой причине не нужно создавать большое количество разделов очереди управления для получения большой пропускной способности. Вместо этого большая часть работы выполняется в функциях действий без отслеживания состояния, которые можно масштабировать бесконечно.

## <a name="auto-scale"></a>Автомасштабирование

Как и все Функции Azure, запущенные в плане потребления, устойчивые функции поддерживают автомасштабирование через [контроллер масштабирования Функций Azure](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). Контроллер масштабирования отслеживает длину очереди рабочих элементов и каждую из очередей управления, добавляя или удаляя ресурсы виртуальной машины соответственно. Если длина очереди управления со временем увеличивается, контроллер масштабирования продолжит добавлять экземпляры до достижения количества разделов очереди управления. Если длина очереди рабочих элементов со временем увеличивается, контроллер масштабирования продолжит добавлять ресурсы виртуальной машины до достижения количества разделов очереди управления.

## <a name="thread-usage"></a>Использование потока

Функции оркестратора выполняются в одном потоке. Это необходимо, чтобы гарантировать детерминированное выполнение функции оркестратора. В связи с этим необходимо помнить, что поток функций оркестратора должен выполнять только важные задачи, а не операции ввода-вывода (запрещенные по определенным причинам), блокировки или запуска дополнительных потоков. Все эти операции должны выполняться с помощью функций действий.

Функции действий имеют такие же реакции на события, как и регулярные активируемые очередью функции. Это означает, что они могут безопасно выполнять операции ввода-вывода, операции с интенсивным потреблением ЦП и использовать несколько потоков. Так как триггеры действия не отслеживают состояние, они могут свободно масштабироваться на неограниченное количество виртуальных машин.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Установка расширения устойчивых функций и примеров (Функции Azure)](durable-functions-install.md)
