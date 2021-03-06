---
title: "Справочник по файлу host.json для Функций Azure"
description: "Справочная документация по файлу host.json для Функций Azure."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/12/2017
ms.author: tdykstra
ms.openlocfilehash: b3e5976a84e0ec91a41d683a426b58635fd5abd6
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="hostjson-reference-for-azure-functions"></a>Справочник по файлу host.json для Функций Azure

Файл метаданных *host.json* содержит параметры глобальной конфигурации, влияющие на все функции приложения-функции. В этой статье перечислены параметры, которые доступны. Схема JSON находится по адресу http://json.schemastore.org/host.

В [параметрах приложения](functions-app-settings.md) и в файле [local.settings.json](functions-run-local.md#local-settings-file) содержатся другие параметры глобальной конфигурации.

## <a name="sample-hostjson-file"></a>Пример файла host.json

В приведенном ниже примере файла *host.json* указаны все возможные параметры.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

В следующих разделах этой статьи объясняется каждое свойство верхнего уровня. Все они являются необязательными, если не указано иное.

## <a name="aggregator"></a>aggregator

Указывает, сколько вызовов функций обрабатывается при [расчете метрик для Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|batchSize|1000|Максимальное количество запросов, которое необходимо обработать.| 
|flushTimeout|00:00:30|Максимальный период времени, который необходимо обработать.| 

Вызовы функций обрабатываются, когда достигается одно из этих двух ограничений.

## <a name="applicationinsights"></a>applicationInsights

Управляет [функцией выборки в Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|isEnabled|нет|Включает или отключает выборку.| 
|maxTelemetryItemsPerSecond|5|Пороговое значение, при котором начинается выборка.| 

## <a name="eventhub"></a>eventHub

Параметр конфигурации для [триггеров и привязок концентратора событий](functions-bindings-event-hubs.md).

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|maxBatchSize|64|Максимальное число событий, получаемых в цикле получения.|
|prefetchCount|Недоступно|Значение PrefetchCount по умолчанию, которое будет использоваться базовым узлом EventProcessorHost.| 
|batchCheckpointFrequency|1|Количество пакетов событий, которые необходимо обработать перед созданием контрольной точки курсора EventHub.| 

## <a name="functions"></a>functions

Список функций, которые будет выполнять узел заданий.  Пустой массив означает выполнение всех функций.  Предназначен для использования только при [локальном выполнении](functions-run-local.md). В приложениях-функциях используйте свойство `disabled` в *function.json*, а не это свойство в *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Указывает время ожидания для всех функций. В планах потребления допускается диапазон от 1 секунды до 10 минут, а значение по умолчанию — 5 минут. В планах службы приложений время не ограничено, а значение по умолчанию — null, что означает отсутствие времени ожидания.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="http"></a>http

Параметры конфигурации для [триггеров и привязок http](functions-bindings-http-webhook.md).

```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|routePrefix|api|Префикс маршрута, который применяется ко всем маршрутам. Используйте пустую строку, чтобы удалить префикс по умолчанию. |
|maxOutstandingRequests|-1|Максимальное число невыполненных запросов, которое будет храниться в любое заданное время (-1 означает отсутствие ограничений). Это ограничение включает запросы, которые находятся в очереди, но еще не начали выполняться, а также все запросы в процессе выполнения. Все входящие запросы, превышающие это ограничение, отклоняются с ответом 429 "Too Busy" (Перегрузка). Вызывающая сторона может использовать этот ответ для применения стратегий повторов на основе времени. Данный параметр определяет только очереди, которые создаются по пути выполнения узла заданий. Он не затрагивает другие очереди, такие как очередь запросов ASP.NET. |
|maxConcurrentRequests|-1|Максимальное число HTTP-функций, которые будут выполняться параллельно (-1 означает отсутствие ограничений). Например, можно задать ограничение, если функции HTTP используют слишком много системных ресурсов при высокой степени параллелизма. Или, если ваши функции выполняют исходящие запросы к сторонней службе, то, возможно, эти вызовы следует ограничить по скорости.|
|dynamicThrottlesEnabled|нет|Заставляет конвейер обработки запросов периодически проверять счетчики производительности системы. Эти счетчики контролируют подключения, потоки, процессы, память и ЦП. Если по любому из счетчиков превышается встроенное пороговое значение (80 %), то запросы отклоняются с ответом 429 "Too Busy" (Перегрузка), пока показатели счетчиков не вернутся на нормальный уровень.|

## <a name="id"></a>id

Уникальный идентификатор для узла заданий. Это может быть глобальный уникальный идентификатор (GUID), вводимый в нижнем регистре с удаленными дефисами. Необходим при локальном выполнении. При запуске в Функциях Azure идентификатор создается автоматически, если параметр `id` не указан.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>logger

Управляет фильтрацией журналов, которые создаются [объектом ILogger](functions-monitoring.md#write-logs-in-c-functions) или [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|categoryFilter|Недоступно|Указывает параметры фильтрации по категории.| 
|defaultLevel|Информация|Для любых категорий, не указанных в массиве `categoryLevels`, отправляет журналы на этом уровне и выше в Application Insights.| 
|categoryLevels|Недоступно|Массив категорий, который определяет минимальный уровень ведения журнала для отправки в Application Insights для каждой категории. Указанная здесь категория управляет всеми категориями, которые начинаются с одного значения, при этом более длинные значения имеют приоритет. В предыдущем примере файла *host.json* все категории, которые начинаются с "Host.Aggregator", записываются в журнал на уровне `Information`. Все прочие категории, которые начинаются с "Host" (такие как "Host.Executor"), записываются в журнал на уровне `Error`.| 

## <a name="queues"></a>queues

Параметры конфигурации для [триггеров и привязок очереди хранилища](functions-bindings-storage-queue.md).

```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|maxPollingInterval|60 000|Максимальный интервал в миллисекундах между опросами очереди.| 
|visibilityTimeout|0|Интервал времени между повторными попытками, когда при обработке сообщения возникает сбой.| 
|batchSize|16|Число сообщений очереди, которые необходимо получить и обработать параллельно. Максимальное значение — 32.| 
|maxDequeueCount|5|Число повторных попыток обработки сообщения, прежде чем поместить его в очередь подозрительных сообщений.| 
|newBatchThreshold|batchSize/2|Пороговое значение, при котором загружается новый пакет сообщений.| 

## <a name="servicebus"></a>serviceBus

Параметр конфигурации для [триггеров и привязок служебной шины](functions-bindings-service-bus.md).

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|maxConcurrentCalls|16|Максимальное число параллельных вызовов к обратному вызову, которое должен инициировать процесс обработки сообщений. | 
|prefetchCount|Недоступно|Значение PrefetchCount по умолчанию, которое будет использоваться базовым компонентом MessageReceiver.| 
|autoRenewTimeout|00:05:00|Максимальный период времени, в течение которого блокировка сообщения будет продлеваться автоматически.| 

## <a name="singleton"></a>singleton

Параметры конфигурации для одноэлементной блокировки. Дополнительные сведения см. в [проблеме, посвященной одноэлементной блокировке, на портале GitHub](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|lockPeriod|00:00:15|Период времени, на который применяются блокировки уровня функции. Эти блокировки возобновляются автоматически.| 
|listenerLockPeriod|00:01:00|Период времени, на который применяются блокировки прослушивателя.| 
|listenerLockRecoveryPollingInterval|00:01:00|Интервал времени, используемый для восстановления блокировки прослушивателя, если блокировку прослушивателя не удалось получить при запуске.| 
|lockAcquisitionTimeout|00:01:00|Максимальный период времени, за который среда выполнения будет пытаться получить блокировку.| 
|lockAcquisitionPollingInterval|Недоступно|Интервал между попытками получения блокировки.| 

## <a name="tracing"></a>tracing

Параметры конфигурации для журналов, создаваемых с помощью объекта `TraceWriter`. Ознакомьтесь с разделами, посвященными ведению журналов, для [C#](functions-reference-csharp.md#logging) и [Node.js](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Свойство  |значение по умолчанию | Описание |
|---------|---------|---------| 
|consoleLevel|info|Уровень трассировки для ведения журнала консоли. Доступны следующие параметры: `off`, `error`, `warning`, `info` и `verbose`.|
|fileLoggingMode|debugOnly|Уровень трассировки для ведения журнала файлов. Доступны следующие параметры: `never`, `always` и `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Набор [каталогов общего кода](functions-reference-csharp.md#watched-directories), изменения которого должны отслеживаться.  Гарантирует, что если код в этих каталогах изменится, то эти изменения будут применены вашими функциями.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="durabletask"></a>durableTask

[Центр задач](durable-functions-task-hubs.md) для [устойчивых функций](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Имена центра задач должны начинаться с буквы и содержать только буквы и цифры. Если имя не указано, центру задач в приложении-функции назначается имя по умолчанию **DurableFunctionsHub**. Дополнительные сведения см. в статье о [центрах задач](durable-functions-task-hubs.md).


## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Узнайте, как обновить файл host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ознакомьтесь с глобальными параметрами в переменных среды](functions-app-settings.md)
