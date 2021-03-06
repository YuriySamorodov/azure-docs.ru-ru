---
title: "Знакомство с API таблицы Azure Cosmos DB | Документация Майкрософт"
description: "Узнайте, как использовать Azure Cosmos DB для хранения больших объемов данных типа \"ключ — значение\" и выполнения запросов к ним с минимальной задержкой с помощью популярных API-интерфейсов MongoDB OSS."
services: cosmos-db
author: mimig
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: mimig1
ms.openlocfilehash: 9321cf648e149408e68456d30e331e05720b60ab
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Общие сведения об API таблиц Azure Cosmos DB

[Azure Cosmos DB](introduction.md) предоставляет API таблиц для приложений, созданных для хранилища таблиц Azure и требующих таких возможностей уровня "Премиум", как:

* [комплексные возможности глобального распределения](distribute-data-globally.md);
* [выделенная пропускная способность](partition-data.md) в любой точке мира;
* задержка меньше 10 миллисекунд на уровне 99-го процентиля;
* гарантированно высокий уровень доступности;
* [автоматическое вторичное индексирование](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

С помощью API таблицы вы можете перенести приложения, написанные для хранилища таблиц Azure, в Azure Cosmos DB, не изменяя код, и воспользоваться возможностями уровня "Премиум". API таблицы включает один клиентский пакет SDK для .NET.

Мы рекомендуем просмотреть следующий видеоролик, в котором Аравинд Рамачандран (Aravind Ramachandran) объясняет, как начать работу с API таблицы для Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="table-offerings"></a>Сравнение возможностей хранилища таблиц и API таблицы
Если вы сейчас используете хранилище таблиц Azure, при переходе на API таблицы Azure Cosmos DB вы получите следующие преимущества:

| | табличное хранилище Azure; | API таблиц Azure Cosmos DB |
| --- | --- | --- |
| Задержка | Низкая, без максимального ограничения по задержке. | Задержка операций чтения и записи (доступно при любых объемах по всему миру): менее 10 мс для операций чтения и менее 15 мс для операций записи на уровне 99-го процентиля. |
| Пропускная способность | Модель с переменной пропускной способностью. Ограничение масштабируемости таблиц — 20 000 операций в секунду. | Высокомасштабируемая с [выделенной зарезервированной пропускной способностью на каждую таблицу](request-units.md) в соответствии с соглашениями об уровне обслуживания. Учетные записи не имеют максимального ограничения пропускной способности и поддерживают выполнение более 10 млн операций в секунду на таблицу. |
| Глобальное распределение | Один регион с отдельным дополнительным регионом вторичных реплик для чтения для высокого уровня доступности. Вы не сможете инициировать отработку отказа. | [Комплексные возможности глобального распределения](distribute-data-globally.md) для 30 и более регионов. Поддержка [автоматической отработки отказа и отработки отказа вручную](regional-failover.md) в любое время повсеместно. |
| Индексация | Поддержка только первичного индекса для свойств PartitionKey и RowKey. Вторичные индексы не поддерживаются. | Поддержка автоматического и полного индексирования всех свойств без необходимости управления индексами. |
| Запрос | При выполнении запроса используется индекс для первичного ключа. В противном случае — сканирование. | Для ускорения выполнения запросов может использоваться автоматическая индексация свойств. |
| Целостность | Строгая согласованность в основном регионе. Итоговая согласованность в дополнительном регионе. | Поддержка [пяти точно определенных уровней согласованности](consistency-levels.md) с возможностью изменять показатели доступности, задержки, пропускной способности и согласованности в соответствии с потребностями приложений. |
| Цены | Оптимизированные для хранилища. | Оптимизированные для пропускной способности. |
| Соглашения об уровне обслуживания | Доступность на уровне 99,99 %. | Доступность на уровне 99,99 % в соответствии с соглашением об уровне обслуживания для всех учетных записей в пределах одного и нескольких регионов с нестрогой согласованностью и доступность для чтения на уровне 99,999 % для всех учетных записей базы данных в пределах нескольких регионов [Ведущие в отрасли универсальные соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/cosmos-db/) для обеспечения доступности. |

## <a name="get-started"></a>Начало работы

Создайте учетную запись Azure Cosmos DB на [портале Azure](https://portal.azure.com). Затем приступите к работе, ознакомившись со статьей [Azure Cosmos DB. Создание приложения .NET с помощью API таблицы](create-table-dotnet.md). 

> [!IMPORTANT]
> Если вы создали учетную запись API таблиц на этапе предварительной версии, создайте [новую учетную запись API таблиц](create-table-dotnet.md#create-a-database-account) для работы с общедоступными пакетами SDK для API таблиц.
>

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с приведенными ниже ресурсами:
* [Azure Cosmos DB. Создание приложения .NET с помощью API таблицы](create-table-dotnet.md)
* [Разработка с помощью API таблицы базы данных Azure Cosmos DB на языке .NET](tutorial-develop-table-dotnet.md)
* [Запрос табличных данных в базе данных Azure Cosmos DB с помощью API таблицы (предварительная версия)](tutorial-query-table.md)
* [Как настроить глобальное распределение Azure Cosmos DB с помощью API таблицы](tutorial-global-distribution-table.md)
* [API таблицы .NET для базы данных Azure Cosmos DB. Скачивание и заметки о выпуске](table-sdk-dotnet.md)
* [API таблиц Azure Cosmos DB (Java)](table-sdk-java.md)
* [API таблиц Azure Cosmos DB (Node.js)](table-sdk-nodejs.md)
* [Azure Cosmos DB Table API for Python: Release notes and resources](table-sdk-python.md) (API таблицы Python для базы данных Azure Cosmos DB. Заметки о выпуске и ресурсы)

