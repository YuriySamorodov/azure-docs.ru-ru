---
title: "Создание среды Azure Time Series Insights | Документация Майкрософт"
description: "В этой статье описывается создание новой среды \"Аналитика временных рядов\" с помощью портала Azure."
services: time-series-insights
ms.service: time-series-insights
author: op-ravi
ms.author: omravi
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 6dba703851161a1eebce0101be8076682f09c76f
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Создание среды Time Series Insights на портале Azure
В этой статье описывается, как создать новую среду "Аналитика временных рядов" с помощью портала Azure.

Служба "Аналитика временных рядов" позволяет быстро начать работу с визуализацией и выполнением запросов к данным, поступающих в Центры Интернета вещей и концентраторы событий Azure. Благодаря этому вы можете опрашивать большие объемы данных временных рядов за секунды.  Эта технология разработана специально для Интернета вещей (IoT) и способна обрабатывать терабайты данных.

## <a name="steps-to-create-the-environment"></a>Процедура создания среды
Чтобы создать среду, сделайте следующее:

1.  Выполните вход на [портал Azure](https://portal.azure.com).

2.  Нажмите кнопку **+Создать**.

3.  Выберите категорию **Интернет вещей**, а затем — **Аналитика временных рядов**.

   ![Создание среды Time Series Insights](media/time-series-insights-get-started/1-new-tsi.png)

4.  На странице **Аналитика временных рядов** выберите **Создать**.

5. Заполните обязательные параметры. В следующей таблице объясняется каждый параметр:
   
   ![Создание группы ресурсов Time Series Insights](media/time-series-insights-get-started/2-create-tsi.png)
   
   Настройка|Рекомендуемое значение|Описание
   ---|---|---
   Имя среды | Уникальное имя | Это имя представляет среду в [обозревателе временных рядов](https://insights.timeseries.azure.com).
   Подписки | Ваша подписка | Если у вас несколько подписок, лучше выбрать ту, которая содержит источник событий. Time Series Insights может автоматически определить ресурсы Центра Интернета вещей и концентратора событий Azure в одной подписке.
   Группа ресурсов | Создайте новую или используйте существующую | Группы ресурсов — это набор совместно используемых ресурсов Azure. Можно выбрать существующую группу ресурсов, например ту, которая содержит концентратор событий или центр Интернета вещей. Или же можно создать новую, если этот ресурс не связан с другими ресурсами.
   Расположение | Ближайший источник событий | Предпочтительно выбирать то же расположение центра обработки данных, которое содержит данные об источниках событий. Это позволит избежать дополнительных затрат на обеспечение пропускной способности между регионами и между зонами, а также дополнительной задержки при перемещении данных за пределы региона.
   Ценовая категория  | S1 | Выберите необходимую пропускную способность. Чтобы обеспечить наименьшие затраты и определить начальную производительность, выберите S1.
   Capacity | 1 | Производительность — множитель, применяемый к скорости входящих данных, емкости хранилища и затратам, связанным с выбранным номером SKU.  Емкость среды можно изменить после ее создания. Чтобы обеспечить наименьшие затраты, выберите значение производительности 1. 
  
6. Установите флажок **Закрепить на панели мониторинга**, чтобы упростить доступ к среде "Аналитика временных рядов" в будущем.

   ![Закрепление Time Series Insights на панели мониторинга](media/time-series-insights-get-started/3-pin-create.png)

7. Выберите **Создать**, чтобы начать процесс подготовки. Это может занять несколько минут.

8. На панели инструментов щелкните символ **Уведомления** (значок колокольчика), чтобы отслеживать процесс развертывания.

   ![Просмотр уведомлений](media/time-series-insights-get-started/4-notifications.png)

После успешного развертывания можно выбрать функцию **Перейти к ресурсу**, чтобы настроить другие свойства, определить уровень защиты с помощью политики доступа к данным, добавить источники событий и выполнить другие действия.

## <a name="next-steps"></a>Дальнейшие действия
* [Определите политики доступа к данным](time-series-insights-data-access.md) для защиты среды.
* [Добавьте источник событий концентратора событий](time-series-insights-how-to-add-an-event-source-eventhub.md) в среду "Аналитика временных рядов Azure". 
* [Отправьте события](time-series-insights-send-events.md) в источник событий.
* Просмотрите среду в [обозревателе службы "Аналитика временных рядов"](https://insights.timeseries.azure.com).
