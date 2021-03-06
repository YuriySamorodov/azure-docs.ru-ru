---
title: "Выполнение конвейера и триггеры в фабрике данных Azure | Документация Майкрософт"
description: "В этой статье объясняется, как выполнить конвейер в фабрике данных Azure по запросу или путем создания триггера."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: 6f4c0b11039bbdaf29c90ec2358934dc1c24af90
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2017
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Выполнение конвейера и триггеры в фабрике данных Azure 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Версия 1 — общедоступная](v1/data-factory-scheduling-and-execution.md)
> * [Версия 2 — предварительный просмотр](concepts-pipeline-execution-triggers.md)

Термин **выполнение конвейера** во второй версии фабрики данных Azure обозначает отдельный экземпляр выполнения конвейера. Например, у вас есть конвейер, который выполняется в 8:00, 9:00 и 10:00. В этом случае выполняются три отдельных запуска конвейера. Каждый запуск конвейера имеет уникальный идентификатор запуска конвейера, однозначно его определяющий. Запуск конвейера обычно создается путем передачи аргументов в параметры, определенные в конвейерах. Конвейер можно выполнить двумя способами: **вручную** или с помощью **триггера**. Эта статья подробно описывает оба способа. 

> [!NOTE]
> Эта статья относится к версии 2 фабрики данных, которая в настоящее время доступна в предварительной версии. Если вы используете общедоступную версию 1 службы фабрики данных, см. статью [Планирование и исполнение с использованием фабрики данных](v1/data-factory-scheduling-and-execution.md).

## <a name="run-pipeline-on-demand"></a>Запуск конвейера по требованию
Используя этот метод, вы запускаете конвейер вручную. Он также называется выполнением конвейера "по требованию". 

Предположим, у вас есть конвейер с именем **copyPipeline**, который вы хотите выполнить. Это простой конвейер с одним действием, которое копирует данные из папки источника, расположенной в хранилище больших двоичных объектов Azure, в папку приемника в том же хранилище. Ниже приведен пример определения конвейера. 

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
Как мы видим в определении JSON, этот конвейер принимает два параметра: sourceBlobContainer и sinkBlobContainer. Значения этих параметров передаются во время выполнения. 

Чтобы запустить конвейер вручную, используйте одно из этих средств: .NET, REST, PowerShell и Python. 

### <a name="rest-api"></a>Интерфейс REST API
Ниже приведен пример команды REST.  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Полный пример вы найдете в кратком руководстве [Create an Azure data factory and pipeline by using the REST AP](quickstart-create-data-factory-rest-api.md) (Создание фабрики данных Azure и конвейера с помощью REST API).

### <a name="powershell"></a>PowerShell
Ниже приведен пример команды PowerShell. 

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

Параметры передаются в тексте полезных данных запроса. В .NET, Powershell и Python значения предоставляются в словаре, который передается в качестве аргумента при вызове.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

Полезные данные ответа содержат уникальный идентификатор запуска конвейера.

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Полный пример вы найдете в кратком руководстве [Create an Azure data factory and pipeline using PowerShell](quickstart-create-data-factory-powershell.md) (Создание фабрики данных Azure и конвейера с помощью PowerShell).

### <a name="net"></a>.NET 
Ниже приведен пример вызова .NET. 

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Полный пример вы найдете в кратком руководстве [Create a data factory and pipeline using .NET SDK](quickstart-create-data-factory-dot-net.md) (Создание фабрики данных Azure и конвейера с помощью пакета SDK для .NET).

> [!NOTE]
> API .NET можно использовать для вызова конвейеров фабрики данных из решения "Функции Azure", собственных веб-служб и т. д.

## <a name="triggers"></a>триггеры;
Второй способ запуска конвейера предоставляют триггеры. Триггеры обозначает единицу обработки, которая определяет время запуска для выполнения конвейера. В настоящее время фабрика данных поддерживает только триггер, вызывающий конвейер по часам. Он называется **триггер планировщика**. Фабрика данных пока не поддерживает триггеры на основе событий, например запуск конвейера после определенного события или получения файла.

Конвейеры и триггеры имеют связь "n-m". Один триггер может запускать несколько конвейеров, и несколько триггеров могут запускать один конвейер. Следующее определение JSON для триггера содержит свойство **pipelines**, которое ссылается на список конвейеров, инициируемых этим триггером, а также значения для параметров конвейеров.

### <a name="basic-trigger-definition"></a>Базовое определение триггера. 
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
        },
        "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                          "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
            }
        ]
    }
```

## <a name="scheduler-trigger"></a>Триггер планировщика
Триггер планировщика запускает конвейер в определенное время по расписанию. Этот триггер поддерживает периодичность и сложные календарные режимы (каждую неделю, в 17: 00 по понедельникам и в 21:00 по четвергам). Гибкость триггера обеспечивает его независимость от шаблона набора данных, то есть он не различает данные с временными рядами и без них.

### <a name="scheduler-trigger-json-definition"></a>Определение JSON для триггера планировщика
При создании триггера планировщика вы можете указать расписание и режим повторений в формате JSON. Пример такого определения представлен в этом разделе. 

Чтобы триггер планировщика выполнял конвейер, включите в определении триггера ссылку на нужный конвейер. Конвейеры и триггеры имеют связь "n-m". Несколько триггеров могут запускать один конвейер. Один триггер может запускать несколько конвейеров.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ] 
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  Свойство **Параметры** является обязательным для **конвейеров**. Даже если ваш конвейер не принимает никаких параметров, включите для них пустой файл JSON, так как это свойство должно присутствовать.


### <a name="overview-scheduler-trigger-schema"></a>Обзор: схема триггера планировщика
Таблица ниже содержит обзор основных элементов, связанных с периодичностью выполнения и расписанием триггера.

Свойство JSON |     Описание
------------- | -------------
startTime | startTime имеет формат "дата и время". В простых расписаниях startTime указывает время первого запуска. В сложных расписаниях триггер не запускается раньше, чем startTime.
endTime | Указывает дату и время окончания для триггера. Триггер не будет выполняться позднее этого времени. Значение элемента endTime не может относиться к прошлому.
timeZone | В настоящее время поддерживается только формат UTC. 
recurrence | Объект recurrence указывает правила повторения для триггера. Объект recurrence поддерживает следующие элементы: frequency, interval, endTime, count и schedule. Если задан объект recurrence, нужно обязательно указать элемент frequency. Остальные элементы объекта recurrence не являются обязательными.
frequency | Представляет единицу частоты, с которой выполняется триггер. Поддерживаются такие значения: `minute`, `hour`, `day`, `week` и `month`.
interval | interval содержит положительное целое число. Это число обозначает количество единиц частоты, через которые выполняется триггер. Например, если interval имеет значение 3, а для элемента frequency выбран вариант week (неделя), триггер выполняется один раз каждые 3 недели.
schedule | Триггер с указанной частотой выполняется по расписанию. Значение schedule содержит изменения с учетом минут, часов, дней недели, чисел месяца и количества недель.


### <a name="schedule-trigger-example"></a>Пример триггера расписания

```json
{
    "properties": {
        "name": "MyTrigger",
        "type": "ScheduleTrigger",
        "typeProperties": {
            "recurrence": {
                "frequency": "Hour",
                "interval": 1,
                "startTime": "2017-11-01T09:00:00-08:00",
                "endTime": "2017-11-02T22:00:00-08:00"
            }
        },
        "pipelines": [{
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToBlobPipeline"
                },
                "parameters": {}
            },
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "SQLServerToAzureSQLPipeline"
                },
                "parameters": {}
            }
        ]
    }
}
```

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Общие сведения: параметры по умолчанию, ограничения и примеры для схемы триггера планировщика

Имя JSON | Тип значения | Обязательный? | Значение по умолчанию | Допустимые значения | Пример
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Строка | Да | None | Дата и время по спецификации ISO-8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Объект | Да | None | Объект recurrence | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Number | Нет | 1 | От 1 до 1000 | ```"interval":10```
endTime | Строка | Да | None | Значение даты-времени, представляющее время в будущем | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Объект | Нет | None | Объект schedule | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Подробно: startTime
В приведенной ниже таблице показано, как параметр startTime определяет время запуска триггера.

Значение startTime | Повторение без расписания | Повторение с расписанием
--------------- | --------------------------- | ------------------------
Время начала в прошлом | Вычисляется время первого выполнения, относящееся к будущему времени, после указанного времени начала.<p>Последующие выполнения вычисляются с учетом времени предыдущего выполнения.</p><p>Пример представлен после этой таблицы.</p> | Триггер выполняется _не раньше_ указанного времени начала. Первое выполнение производится по расписанию, которое отсчитывается от времени начала. <p>Последующие выполнения производятся по расписанию повторов.</p>
Время начала в будущем или в настоящем | Выполняется первый раз в указанное время начала. <p>Последующие выполнения производятся с учетом времени предыдущего выполнения.</p> | Триггер выполняется _не раньше_ указанного времени начала. Первое выполнение производится по расписанию, которое отсчитывается от времени начала.<p>Последующие выполнения производятся по расписанию повторов.</p>

Рассмотрим, как работает триггер, для которого время startTime установлено в прошлом, указан параметр recurrence и отсутствует параметр schedule. Допустим, текущее время `2017-04-08 13:00`, startTime имеет значение `2017-04-07 14:00`, а recurrence определяет повторы через каждые два дня (то есть frequency имеет значение day, а interval равен 2). Обратите внимание, что время startTime находится в прошлом, то есть наступает раньше текущего времени.

В этих условиях первое выполнение происходит `2017-04-09 at 14:00`. От времени начала ядро планировщика отсчитывает время повторных выполнений. Выполнения, которые приходятся на прошлое, игнорируются. Ядро берет очередной случай выполнения, который приходится на будущее. В данном случае startTime имеет значение `2017-04-07 at 2:00pm`, поэтому следующее выполнение состоится через 2 дня от этого времени начала, то есть `2017-04-09 at 2:00pm`.

Время первого выполнения останется тем же даже при значениях startTime `2017-04-05 14:00` или `2017-04-01 14:00`. Все последующие выполнения после первого вычисляются по расписанию schedule. В этом примере они состоятся `2017-04-11 at 2:00pm`, затем `2017-04-13 at 2:00pm`, `2017-04-15 at 2:00pm` и т. д.

И, наконец, если у триггера есть расписание schedule, в котором не указаны часы и (или) минуты, используются значения часов и (или) минут первого выполнения.

### <a name="deep-dive-schedule"></a>Подробный обзор: schedule
С одной стороны, параметр schedule может ограничивать число выполнений триггера. Например, если триггеру назначена ежемесячная частота и параметр schedule, который запускает триггер только в 31-й день месяца, этот триггер будет выполняться только в те месяцы, в которых есть 31 день.

Обратите внимание, что параметр schedule может увеличивать число выполнений триггера. Например, если триггеру назначена ежемесячная частота и параметр schedule, который запускает триггер в 1-й и 2-й день месяца, этот триггер будет выполняться 1-го и 2-го числа каждого месяца, а не один раз в месяц.

Если в параметре schedule задано несколько элементов, они применяются в порядке от большего к меньшему: номер недели, число месяца, день недели, час и минута.

В следующей таблице элементы параметра schedule описаны подробно.


Имя JSON | Описание | Допустимые значения
--------- | ----------- | ------------
minutes | Минуты часа, в которые будет выполняться триггер. | <ul><li>Целое число </li><li>массив целых чисел</li></ul>
hours | Часы дня, в которые будет выполняться триггер. | <ul><li>Целое число </li><li>массив целых чисел</li></ul>
weekDays | Дни недели, в которые будет выполняться триггер. Указываются только при выборе еженедельной частоты. | <ul><li>Понедельник, вторник, среда, четверг, пятница, суббота, воскресенье</li><li>Массив любого значения (максимальное количество элементов в массиве — 7)</li></p>Без учета регистра</p>
monthlyOccurrences | Определяет, в какие числа месяца будет выполняться триггер. Указываются только при выборе ежемесячной частоты. | Массив объектов monthlyOccurence: `{ "day": day,  "occurrence": occurence }`. <p> Элемент day определяет день недели, в который будет выполняться триггер, например `{Sunday}` означает каждое воскресенье месяца. Обязательный элемент.<p>Элемент occurence определяет конкретное повторение дня в течение месяца, например `{Sunday, -1}` означает последнее воскресенье месяца. необязательный параметр.
monthDays | День месяца, в который будет выполняться триггер. Указываются только при выборе ежемесячной частоты. | <ul><li>Любое значение <= -1 и >= -31</li><li>Любое значение >= 1 и <= 31</li><li>Массив значений</li>


## <a name="examples-recurrence-schedules"></a>Примеры: расписания повторений
Этот раздел содержит примеры расписания повторений с применением объекта schedule и его элементов.

Во всех представленных примерах для параметра interval предполагается значение 1. Кроме того, в каждом случае следует предполагать частоту, соответствующую параметру schedule. Например, нельзя использовать ежедневную частоту и параметр monthDays в расписании. Эти ограничения указаны в таблице в предыдущем разделе. 

Пример | Описание
------- | -----------
`{"hours":[5]}` | Запускается каждый день в 05:00.
`{"minutes":[15], "hours":[5]}` | Запускается каждый день в 05:15.
`{"minutes":[15], "hours":[5,17]}` | Запускается каждый день в 05:15 и 17:15.
`{"minutes":[15,45], "hours":[5,17]}` | Запускается каждый день в 05:15, 05:45, 17:15 и 17:45.
`{"minutes":[0,15,30,45]}` | Запускается каждые 15 минут.
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Запускается каждый час. Этот триггер запускается каждый час. Минута выполнения определяется параметром startTime, если он указан, или временем создания. Например, если триггер запущен или создан (в зависимости от ситуации) в 12:25, он будет запускаться в 00:25, 01:25, 02:25, ..., 23:25. Это расписание работает так же, как триггер с частотой hour (час), значением 1 для параметра interval и без параметра schedule. Отличие в том, что такое расписание позволяет создать другие триггеры с другими значениями частоты и интервала. Например, если бы параметр frequency имел значение month, расписание выполнялось бы раз в месяц, а не каждый день, как в данном случае, когда параметр frequency имеет значение day.
`{"minutes":[0]}` | Выполняется с наступлением каждого часа. Этот триггер также выполняется каждый час, но ровно в момент его наступления (например, в 00:00, 01:00, 02:00 и т. д.). Такая настройка эквивалентна триггеру, в котором параметр frequency имеет значение hour, параметр startTime указан с нулевыми минутами, а расписание не задано (параметр frequency имеет значение day); если же параметр frequency имеет значение week или month, расписание будет выполняться только один раз в неделю или в месяц соответственно.
`{"minutes":[15]}` | Выполняется через 15 минут после наступления каждого часа. Задание выполняется каждый час, начиная с 00:15, 01:15, 02:15 и т. д. и заканчивая 22:15 и 23:15.
`{"hours":[17], "weekDays":["saturday"]}` | Выполняется в 17:00 каждую субботу.
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Выполняется в 17:00 каждый понедельник, среду и пятницу.
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Выполняется в 17:15 и 17:45 каждый понедельник, среду и пятницу.
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Задание выполняется каждые 15 минут в каждый рабочий день.
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Выполняется каждые 15 минут в период с 09:00 до 16:45 каждый рабочий день.
`{"weekDays":["tuesday", "thursday"]}` | Выполняется по вторникам и четвергам в указанное время начала.
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Выполняется в 06:00 28-го числа каждого месяца (при ежемесячной частоте).
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Выполняется в 06:00 в последний день каждого месяца. Если вы хотите, чтобы триггер запускался в последний день месяца, используйте -1 вместо значения 28, 29, 30 или 31.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Выполняется в 06:00 в первый и последний день каждого месяца.
`{monthDays":[1,14]}` | Выполняется в первый и 14-й день каждого месяца в указанное время начала.
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Выполняется в первую пятницу каждого месяца в 05:00.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Выполняется в первую пятницу каждого месяца в указанное время начала.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Выполняется в третью пятницу с конца месяца, каждый месяц в указанное время начала.
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Выполняется в первую и последнюю пятницу каждого месяца в 05:15.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Выполняется в первую и последнюю пятницу каждого месяца в указанное время начала.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Выполняется в пятую пятницу каждого месяца в указанное время начала. Если пятой пятницы в месяце нет, конвейер не выполняется, поскольку по расписанию оно должно выполняться только по пятым пятницам.  Если вы хотите, чтобы триггер выполнялся в последнюю пятницу месяца, используйте вместо значения 5 значение -1.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Выполняется каждые 15 минут в последнюю пятницу месяца.
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Выполняется в 05:15, 05:45, 17:15 и 17:45 третьей среды каждого месяца.




## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующими руководствами: 

- [Создание фабрики данных и конвейера с помощью пакета SDK для .NET](quickstart-create-data-factory-dot-net.md)
