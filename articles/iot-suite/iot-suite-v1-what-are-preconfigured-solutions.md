---
title: "Предварительно настроенные решения IoT Azure | Документация Майкрософт"
description: "Описание предварительно настроенных решений IoT Azure и их архитектуры со ссылками на дополнительные ресурсы."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 76df013e8e5868fcc9f5d95aa523a6a56dea7163
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2017
---
# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Что такое предварительно настроенные решения Azure IoT Suite?

Предварительно настроенные решения Azure IoT Suite — это реализация стандартных шаблонов IoT, которые вы можете развернуть в Azure с помощью своей подписки. Предварительно настроенные решения можно использовать:

* в качестве отправной точки для собственных решений IoT;
* для ознакомления с распространенными шаблонами, которые используются при проектировании и разработке решений IoT.

Каждое предварительно настроенное решение — это полная комплексная реализация, в которой для создания данных телеметрии используются виртуальные устройства.

Вы можете скачать полный исходный код для настройки и расширения решения в соответствии со своими требованиями, связанными с Интернетом вещей.

> [!NOTE]
> Чтобы развернуть одно из предварительно настроенных решений, посетите страницу [Microsoft Azure IoT Suite][lnk-azureiotsuite]. В [руководстве по началу работы с предварительно настроенными решениями Интернета вещей][lnk-getstarted-preconfigured] подробно описывается, как развернуть и запустить одно из таких решений.

В следующей таблице показано соответствие этих решений конкретным функциям IoT.

| Решение | Прием данных | Удостоверение устройства | Управление устройствами | Команды и управление | Правила и действия | Прогнозная аналитика |
| --- | --- | --- | --- | --- | --- | --- |
| [Удаленный мониторинг][lnk-getstarted-preconfigured] |Да |Да |Да |Да |Да |- |
| [Диагностическое обслуживание][lnk-predictive-maintenance] |Да |Да |- |Да |Да |Да |
| [Подключенная фабрика][lnk-getstarted-factory] |Да |Да |Да |Да |Да |- |

* *Прием данных*: прием данных в облако в нужном масштабе.
* *Удостоверения устройств*: управление уникальными удостоверениями устройств и контроль доступа устройств к решению.
* *Управление устройствами*: управление метаданными устройств и выполнение операций, таких как перезагрузка устройства и обновление встроенного ПО.
* *Команды и управление*: отправка сообщений на устройство из облака для выполнения на устройстве определенных действий.
* *Правила и действия*: правила используются в серверной части решения для операций с определенными данными, отправляемыми с устройства в облако.
* *Прогнозная аналитика*: данные, отправляемые с устройства в облако, анализируются в серверной части решения, что позволяет предсказать время выполнения определенных действий. Пример — анализ данных телеметрии, поступающих от двигателя самолета, который позволяет определить время обслуживания двигателя.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Обзор предварительно настроенного решения для удаленного мониторинга

В этой статье мы решили обсудить предварительно настроенное решение для удаленного мониторинга, чтобы проиллюстрировать основные элементы проектирования, используемые другими решениями.

На следующей схеме представлены основные элементы решения для удаленного мониторинга. Дополнительные сведения об этих элементах приведены в следующих разделах.

![Архитектура предварительно настроенного решения удаленного мониторинга][img-remote-monitoring-arch]

## <a name="devices"></a>Устройства

Когда вы развертываете предварительно настроенные решения удаленного мониторинга, для такого решения заранее подготавливаются четыре виртуальных устройства, которые имитируют устройство охлаждения. Эти виртуальные устройства имеют встроенную модель, которая передает сведения о температуре и влажности в виде данных телеметрии. Эти имитации устройств включены для выполнения следующих задач:

- демонстрация сквозного потока данных через решение;
- предоставление удобного источника данных телеметрии;
- предоставление целевого объекта для методов или команд, если вы являетесь разработчиком серверной части и используете решение в качестве отправной точки при создании собственной реализации.

Имитации устройств в решении могут отвечать на следующие взаимодействия между облаком и устройствами:

- *Методы ([прямые методы][lnk-direct-methods])*: метод двунаправленного взаимодействия, при котором подключенное устройство должно отвечать немедленно.
- *Команды (сообщения, оправляемые из облака на устройство)*: метод односторонней связи, при котором устройство получает команду из долгосрочной очереди.

Чтобы сравнить эти различные подходы, см. статью [Руководство по обмену данными между облаком и устройством][lnk-c2d-guidance].

Когда устройство впервые подключается к Центру Интернета вещей в рамках предварительно настроенного решения, оно отправляет в центр сообщение со сведениями об устройстве. В этом сообщении содержится список методов, на которые устройство может отвечать. В предварительно настроенном решении для удаленного мониторинга имитации устройств поддерживают следующие методы:

* *Initiate Firmware Update* (Инициировать обновление встроенного ПО): этот метод инициирует на устройстве асинхронную задачу, чтобы выполнить обновление встроенного ПО. Асинхронная задача использует сообщаемые свойства для доставки сведений об обновлении состояния на панель мониторинга решения.
* *Reboot* (Перезагрузка): этот метод приводит к перезагрузке имитации устройства.
* *FactoryReset* (Сброс до заводских настроек): этот метод вызывает сброс настроек имитации устройства к параметрам по умолчанию.

Когда устройство впервые подключается к Центру Интернета вещей в рамках предварительно настроенного решения, оно отправляет в центр сообщение со сведениями об устройстве. В этом сообщении содержится список команд, на которые устройство может отвечать. В предварительно настроенном решении для удаленного мониторинга имитации устройств поддерживают следующие команды:

* *Проверка связи с устройством*— устройство отвечает на эту команду подтверждением. С ее помощью вы можете убедиться, что устройство по-прежнему активно и находится в состоянии ожидания передачи данных.
* *Начать сбор данных телеметрии*— сообщает устройству о начале отправки данных телеметрии.
* *Завершить сбор данных телеметрии*— сообщает устройству о завершении отправки данных телеметрии.
* *Изменить заданную температуру*— контролирует имитированные данные телеметрии (значения температуры), отправляемые устройством. Эта команда используется при тестировании логики серверной части.
* *Диагностические данные телеметрии*— определяет, должно ли устройство отправить сведения о наружной температуре в качестве данных телеметрии.
* *Изменить состояние устройства* — задает свойство метаданных состояния устройства, о которых сообщает устройство. Эта команда используется при тестировании логики серверной части.

В решение можно добавить дополнительные имитации устройств, которые будут передавать аналогичные данные телеметрии и реагировать на одни и те же методы и команды.

Помимо реагирования на команды и методы решение также использует [двойники устройств][lnk-device-twin]. Устройства используют двойники устройств для передачи значений свойств в серверную часть решения. Панель мониторинга решения использует двойники устройств, чтобы задать на устройствах новые значения свойств. Например, в процессе обновления встроенного ПО имитация устройства сообщает состояние обновления с помощью сообщаемых свойств.

## <a name="iot-hub"></a>Центр Интернета вещей

В этом предварительно настроенном решении экземпляр Центра Интернета вещей соответствует *облачному шлюзу* в стандартной [архитектуре решения Интернета вещей][lnk-what-is-azure-iot].

Центр Интернета вещей получает данные телеметрии с устройства через одну конечную точку. Центр Интернета вещей также поддерживает отдельные конечные точки устройства, через которые каждое устройство может получать отправляемые ему команды.

Центр Интернета вещей предоставляет доступ к полученным данным телеметрии через конечную точку для чтения таких данных на стороне службы.

Функция управления устройствами в Центре Интернета вещей дает возможность управлять свойствами устройств на портале решения и планировать задания, которые выполняют следующие операции:

- перезагрузка устройств;
- изменение состояний устройств;
- обновление встроенного ПО.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

В предварительно настроенном решении используются три задания службы [Azure Stream Analytics][lnk-asa] (ASA), которые фильтруют поток поступающих с устройств телеметрических данных:

* *Задание отправки сведений об устройстве*. Выводит данные в концентратор событий, который перенаправляет сообщения, связанные с регистрацией устройства, в реестр устройств решения — базу данных Azure Cosmos DB. Это сообщение отправляется при первом подключении устройства или в ответ на команду **Изменить состояние устройства**.
* *Задание отправки данных телеметрии.* Отправляет все необработанные данные телеметрии в хранилище BLOB-объектов Azure, где они автономно хранятся в неструктурированном виде, а также вычисляет значения сводных данных телеметрии, отображаемых на панели мониторинга решения.
* *Задание выполнения правил.* Отфильтровывает поток данных телеметрии, отсеивая значения, превышающие пороговые значения настроенных правил, а также выводит данные в концентратор событий. Когда правило выполняется, в представлении панели мониторинга на портале решения это событие отображается в виде новой строки в таблице журнала оповещений. Эти правила могут также запускать действие на основе параметров, определенных в представлениях **Правила** и **Действия** на портале решения.

В этом предварительно настроенном решении задания ASA представляют собой часть **серверной части решения Интернета вещей** в стандартной [архитектуре решения Интернета вещей][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Обработчик событий

В этом предварительно настроенном решении обработчик событий представляет собой часть **серверной части решения Интернета вещей** в стандартной [архитектуре решения Интернета вещей][lnk-what-is-azure-iot].

**Задание отправки сведений об устройстве** и **задание выполнения правил** отправляют свои выходные данные в концентраторы событий, чтобы эти данные перенаправлялись в другие службы серверной части. Для чтения сообщений из этих концентраторов событий в решении используется экземпляр [EventProcessorHost][lnk-event-processor], который выполняется в рамках [веб-задания][lnk-web-job]. **EventProcessorHost** использует:
- данные **задания отправки сведений об устройстве**, чтобы обновить данные устройства в базе данных Cosmos DB;
- данные **задания выполнения правил**, чтобы вызвать приложение логики и обновить предупреждения, отображаемые на портале решения.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Реестр удостоверений устройств, двойник устройства и Cosmos DB

Каждый Центр Интернета вещей включает [реестр удостоверений устройств][lnk-identity-registry], в котором хранятся ключи устройств. Центр Интернета вещей использует эти сведения для проверки подлинности устройств: для подключения к концентратору устройство должно иметь действительный ключ и оно должно быть зарегистрированным.

[Двойник устройства][lnk-device-twin] — это документ JSON, управляемый Центром Интернета вещей. Двойник устройства для устройства содержит описанные ниже компоненты.

- Сообщаемые свойства, отправляемые устройством в центр. Эти свойства можно просмотреть на портале решения.
- Требуемые свойства, которые необходимо отправить на устройство. Значения этих свойств можно задать на портале решения.
- Теги, которые существуют только на двойнике устройства, а не на устройстве. Эти теги можно использовать для фильтрации списков устройств на портале решения.

Это решение использует двойники устройств для управления метаданными устройств. Решение также использует базу данных Cosmos DB для хранения собственных дополнительных данных об устройствах, например команд, поддерживаемых каждым из устройств, и журнала команд.

Оно также должно обеспечивать синхронизацию данных в реестре удостоверений устройств с содержимым базы данных Cosmos DB. Для управления синхронизацией экземпляр **EventProcessorHost** использует данные **задания отправки сведений об устройстве** Stream Analytics.

## <a name="solution-portal"></a>Портал решения

![портал решения][img-dashboard]

Портал решения представляет собой веб-интерфейс, разворачиваемый в облаке в составе предварительно настроенного решения. Он позволяет:

* просматривать данные телеметрии и историю оповещений на панели мониторинга;
* подготавливать новые устройства;
* осуществлять управление и мониторинг устройств;
* отправлять команды на определенные устройства;
* вызывать методы на конкретных устройствах;
* управлять правилами и действиями.
* планировать выполнение заданий на одном или нескольких устройствах.

В этом предварительно настроенном решении портал представляет собой часть **серверной части решения Интернета вещей**, а также часть системы, отвечающую за **обработку данных и подключение к устройствам организации**, в стандартной [архитектуре решения Интернета вещей][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об архитектурах решений Интернета вещей см. в документе [Эталонная архитектура служб IoT Microsoft Azure][lnk-refarch].

Теперь вы знаете, что такое предварительно настроенное решение, и можете приступить к развертыванию предварительно настроенного решения для *удаленного мониторинга*. Ознакомьтесь со статьей [Начало работы с предварительно настроенными решениями][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-v1-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-v1-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md
