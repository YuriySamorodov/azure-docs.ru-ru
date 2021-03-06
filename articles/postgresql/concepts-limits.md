---
title: "Ограничения в базе данных Azure для PostgreSQL | Документация Майкрософт"
description: "Описываются ограничения в базе данных Azure для PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: dbb88e033d5be73b7b069d69c095d8df2c1faf1b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Ограничения в базе данных Azure для PostgreSQL
Служба базы данных Azure для PostgreSQL работает в режиме общедоступной предварительной версии. В следующих разделах описываются действующие ограничения емкости и функциональных возможностей в службе базы данных.

## <a name="service-tier-maximums"></a>Максимумы уровней служб
База данных Azure для PostgreSQL имеет несколько уровней служб, которые можно выбрать при создании сервера. Дополнительные сведения см. в статье [Уровни служб в базе данных Azure для MySQL](concepts-service-tiers.md).  

На каждом уровне служб в предварительной версии службы существует максимальное число подключений, единиц вычислений и максимальный объем хранилища. Ниже перечислены эти ограничения. 

| | |
| :------------------------- | :---------------- |
| **Максимальное число подключений**        |                   |
| Базовый, 50 единиц вычислений     | 50 подключений    |
| Базовый, 100 единиц вычислений    | 100 подключений   |
| Стандартный, 100 единиц вычислений | 200 подключений   |
| Стандартный, 200 единиц вычислений | 300 подключений   |
| Стандартный, 400 единиц вычислений | 400 подключений   |
| Стандартный, 800 единиц вычислений | 500 подключений   |
| **Максимальное число единиц вычислений**      |                   |
| Уровень службы "Базовый"         | 100 единиц вычислений |
| Уровень службы "Стандартный"      | 800 единиц вычислений |
| **Максимальный объем хранилища**            |                   |
| Уровень службы "Базовый"         | 1 TБ              |
| Уровень службы "Стандартный"      | 1 TБ              |

Когда число подключений превышается, может появиться следующая ошибка:
> FATAL: sorry, too many clients already (Неустранимая ошибка: уже подключено слишком много клиентов)

## <a name="preview-functional-limitations"></a>Ограничения функциональных возможностей предварительной версии
### <a name="scale-operations"></a>Операции масштабирования
1.  В настоящее время динамическое масштабирование серверов между уровнями служб не поддерживается. Имеется в виду переключение между уровнями служб "Базовый" и "Стандартный".
2.  В настоящее время динамическое увеличение объема хранилища по запросу на предварительно созданном сервере не поддерживается.
3.  Уменьшение размера хранилища сервера не поддерживается.

### <a name="server-version-upgrades"></a>Обновления версии сервера
- В настоящее время автоматический переход между основными версиями ядра СУБД не поддерживается.

### <a name="subscription-management"></a>Управление подпиской
- В настоящее время динамическое перемещение предварительно созданных серверов между подпиской и группой ресурсов не поддерживается.

### <a name="point-in-time-restore"></a>Восстановление до точки во времени
1.  Восстановление в другой уровень служб и (или) до другого числа единиц вычислений и размера хранилища не допускается.
2.  Восстановление удаленного сервера не поддерживается.

## <a name="next-steps"></a>Дальнейшие действия
- Ознакомьтесь со статьей [Параметры и производительность базы данных Azure для MySQL: возможности разных уровней служб](concepts-service-tiers.md).
- Ознакомьтесь со статьей [Поддерживаемые версии в базе данных Azure для PostgreSQL](concepts-supported-versions.md).
- Просмотрите статью [How To Backup and Restore a server in Azure Database for PostgreSQL using the Azure portal](howto-restore-server-portal.md) (Архивация и восстановление сервера в базе данных Azure для PostgreSQL с помощью портала Azure).
