---
title: "Проектирование первой базы данных SQL Azure на языке C# | Документация Майкрософт"
description: "Узнайте, как спроектировать свою первую базу данных SQL Azure и подключиться к ней с помощью программы на языке C#, используя ADO.NET."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases, mvc, devcenter
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 08/25/2017
ms.author: genemi
ms.openlocfilehash: 1e13a9a89873b189e1ea05c0208e7c8646881655
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a>Проектирование базы данных SQL Azure и подключение к ней с помощью C# и ADO.NET

База данных SQL Azure — это реляционная база данных как услуга (DBaaS) в Microsoft Cloud (Azure). Из этого руководства вы узнаете, как с помощью портала Azure и ADO.NET с Visual Studio выполнить следующие действия: 

> [!div class="checklist"]
> * создать базу данных на портале Azure;
> * настроить правило брандмауэра на уровне сервера на портале Azure;
> * подключиться к базе данных с помощью ADO.NET и Visual Studio;
> * создать таблицы с помощью ADO.NET;
> * вставить, обновить и удалить данные с помощью ADO.NET; 
> * выполнить запрос данных с помощью ADO.NET.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Убедитесь, что установлен [Visual Studio Community 2017, Visual Studio Professional 2017 или Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).

<!-- The following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- The following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали об основных задачах базы данных, таких как создание базы данных и таблиц, загрузка и запрос данных, а также восстановление базы данных до предшествующей точки во времени. Вы научились выполнять следующие задачи:
> [!div class="checklist"]
> * Создание базы данных
> * Настройка правила брандмауэра.
> * подключаться к базе данных с помощью [Visual Studio и C#](sql-database-connect-query-dotnet-visual-studio.md);
> * создание таблиц.
> * вставлять, обновлять и удалять данные;
> * Запрос данных

Перейдите к следующему руководству, чтобы узнать о переносе данных.

> [!div class="nextstepaction"]
>[Перенос базы данных SQL Server в базу данных SQL Azure](sql-database-migrate-your-sql-server-database.md)

