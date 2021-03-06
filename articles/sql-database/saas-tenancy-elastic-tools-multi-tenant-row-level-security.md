---
title: "Мультитенантные приложения со средствами эластичных баз данных и безопасностью на уровне строк"
description: "Средства эластичных баз данных в сочетании с безопасностью на уровне строк позволяют создать приложение с высокой масштабируемостью уровня данных."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: b85673c7f0e8176a1700e55fe2ce33b7c191828a
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Мультитенантные приложения со средствами эластичных баз данных и безопасностью на уровне строк
[Средства эластичных баз данных](sql-database-elastic-scale-get-started.md) и [безопасность на уровне строк (RLS)](https://msdn.microsoft.com/library/dn765131) предоставляют мощный набор возможностей для гибкого и эффективного масштабирования уровня данных мультитенантного приложения с базой данных SQL Azure. Дополнительные сведения см. в статье [Шаблоны разработки для мультитенантных приложений SaaS и база данных SQL Azure](saas-tenancy-app-design-patterns.md). 

В этой статье показано, как использовать эти технологии совместно, чтобы создать приложение с высокомасштабируемым уровнем данных, который поддерживает мультиканальные сегменты, используя **ADO.NET SqlClient** и (или) **Entity Framework**.  

* **Средства эластичных баз данных** позволяют разработчикам развернуть уровень данных приложения, используя стандартные для отрасли методы сегментирования, набор библиотек .NET и шаблоны служб Azure. Управление сегментами с использованием клиентской библиотеки эластичных баз данных помогает автоматизировать и упростить многие инфраструктурные задачи, которые обычно связаны с сегментированием. 
* **Безопасность на уровне строк** позволяет разработчикам хранить данные для нескольких клиентов в одной базе данных, а также отфильтровывать строки, не принадлежащие клиенту, выполняющему запрос, с помощью политик безопасности. Централизующая логика доступа с RLS внутри базы данных, а не внутри приложения, упрощает обслуживание и снижает вероятность возникновения ошибок по мере роста базы кода приложения. 

При совместном использовании этих функций приложение может выиграть на снижении издержек и повышении эффективности за счет хранения данных для нескольких клиентов в одной базе данных сегментов. В то же время приложение по-прежнему обладает достаточной гибкостью, чтобы предоставлять изолированные, принадлежащие однотенантные сегменты для premium-клиентов, которым требуются более строгие гарантии производительности, поскольку мультитенантные сегменты не гарантируют равномерного распространения ресурсов среди клиентов.  

Другими словами, API-интерфейсы [маршрутизации на основе данных](sql-database-elastic-scale-data-dependent-routing.md) из библиотеки для клиентов эластичных баз данных автоматически подключают клиентов к нужной базе данных сегмента, которая содержит их ключ сегментирования (обычно TenantId). После подключения политика безопасности RLS внутри базы данных гарантирует, что клиентам доступны только те строки, которые содержат их TenantId. Предполагается, что все таблицы содержат столбец TenantId для обозначения того, какие строки принадлежат каждому из клиентов. 

![Архитектура приложений для ведения блогов][1]

## <a name="download-the-sample-project"></a>Загрузка примера проекта
### <a name="prerequisites"></a>Предварительные требования
* Используйте Visual Studio 2012 или более поздней версии. 
* Создайте три базы данных SQL Azure. 
* Скачайте пример проекта: [Elastic DB Tools for Azure SQL — Multi-Tenant Shards](http://go.microsoft.com/?linkid=9888163)
  * Заполните информацию по своим базам данных в начале файла **Program.cs** 

Этот проект расширяет проект, описанный в [Elastic DB Tools for Azure SQL — Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) , за счет добавления поддержки для баз данных с мультитенантными сегментами. Он создает простое консольное приложение для создания блогов и записей, рассчитанное на четырех клиентов и две базы данных с мультитенантными сегментами, как показано на схеме выше. 

Создайте и запустите приложение. В открывшемся диспетчере карт сегментов для средств эластичных баз данных выполните следующие проверки. 

1. С помощью Entity Framework и LINQ, создайте новый блог, а затем отобразите все блоги для каждого клиента.
2. С помощью ADO.NET SqlClient отобразите все блоги для клиента.
3. Попробуйте вставить блог неправильного клиента, чтобы проверить, возникнет ли ошибка.  

Обратите внимание, что поскольку RLS пока не включена в базах данных сегментов, каждая из этих проверок выдает ошибки: клиенты видят блоги, которые им не принадлежат, а приложение не запрещается вставить блог неправильному клиенту. В оставшейся части этой статьи описано, как решить эти проблемы, применив изоляцию клиентов с помощью RLS. Существует два шага: 

1. **Уровень приложений.** Измените код приложения, чтобы после открытия подключения в хранилище SESSION_CONTEXT всегда использовалось текущее значение TenantId. В примере проекта данный шаг уже выполнен. 
2. **Уровень данных.** Создайте в каждой базе данных сегментов политику безопасности RLS, чтобы фильтровать строки на основе значения TenantId, хранимого в SESSION_CONTEXT. Этот шаг необходимо выполнить для каждой базы данных сегмента, иначе строки в мультитенантных сегментах не будут фильтроваться. 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>Шаг 1. Уровень приложений — выбор значения TenantId в SESSION_CONTEXT
Когда приложение установит подключение к базе данных сегментов с помощью API-интерфейсов маршрутизации на основе данных из клиентской библиотеки эластичных баз данных, оно должно сообщить базе данных, для какого TenantId используется это подключение, чтобы политика безопасности RLS отфильтровала строки, принадлежащие другим клиентам. Чтобы передать эту информацию, рекомендуется сохранить текущее значение TenantId для этого подключения в [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Примечание. Также можно использовать [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), но SESSION_CONTEXT — лучший вариант, так как это хранилище проще в использовании. Кроме того, оно возвращает значение NULL по умолчанию и поддерживает пары "ключ — значение".)

### <a name="entity-framework"></a>Entity Framework
Для приложений, использующих Entity Framework, проще всего задать значение SESSION_CONTEXT в рамках переопределения ElasticScaleContext, как описано в разделе [Маршрутизация на основе данных с помощью EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Прежде чем возвращать подключение, созданное с использованием маршрутизации на основе данных, создайте и выполните команду SqlCommand, которая устанавливает в SESSION_CONTEXT для TenantId значение ключа сегментирования для этого подключения. Таким образом, чтобы задать нужное значение в SESSION_CONTEXT, необходимо написать код один раз. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data-dependent routing. This call opens a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call fails for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Теперь при каждом вызове ElasticScaleContext хранилище SESSION_CONTEXT будет автоматически использоваться с указанным значением TenantId: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
Для приложений, которые используют ADO.NET SqlClient, рекомендованный подход заключается в следующем. Вам нужно создать функцию-оболочку для ShardMap.OpenConnectionForKey(), которая перед возвратом подключения автоматически будет задавать в хранилище SESSION_CONTEXT нужное значение ключа TenantId. Чтобы в хранилище SESSION_CONTEXT всегда использовалось нужное значение, открывайте подключение только с помощью этой функции-оболочки.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs are listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Шаг 2. Уровень данных — создание политики безопасности на уровне строк
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Создание политики безопасности для фильтрации строк, доступных для всех клиентов
Теперь, когда перед выполнением запроса приложение задает в хранилище SESSION_CONTEXT нужное текущее значение TenantId, политика безопасности RLS может фильтровать запросы и исключать строки с разным значением TenantId.  

Политика RLS реализована в инструкциях T-SQL: определяемая пользователем функция определяет логику доступа, а политика безопасности связывает эту функцию с любым количеством таблиц. В нашем проекте эта функция проверяет, подключено ли к базе данных нужное приложение (а не другой пользователь SQL) и соответствует ли значение TenantId в SESSION_CONTEXT значению TenantId для определенной строки. Предикат фильтрации отберет для запросов SELECT, UPDATE и DELETE только строки, соответствующие этим условиям. А предикат блокировки предотвратит добавление или изменение строк, не соответствующих этим условиям, в запросах INSERT и UPDATE. Если значение SESSION_CONTEXT не задано, функция возвращает значение NULL и запрещает любые операции со строками, включая просмотр. 

Чтобы включить RLS, выполните следующий запрос T-SQL на всех сегментах, использующих Visual Studio (SSDT), среду SSMS или сценарий PowerShell, которые включены в проект (либо, если вы используете [службу заданий эластичной базы данных](sql-database-elastic-jobs-overview.md), вы можете использовать ее для автоматизации выполнения этого запроса T-SQL на всех сегментах): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> В более сложных проектах, требующих добавления предиката в сотни таблиц, можно использовать вспомогательную хранимую процедуру, которая автоматически создает политику безопасности, добавляя предикат во все таблицы схемы. Ознакомьтесь с записью блога о [применении безопасности на уровне строк ко всем таблицам с помощью вспомогательного скрипта](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Теперь если вы снова запустите пример приложения, клиенты увидят только те строки, которые им принадлежат. Кроме того, приложение не сможет ни вставлять строки, принадлежащие клиентам, которые в настоящее время не подключены к базе данных сегментов, ни изменять значение TenantId в видимых строках. Если приложение пытается выполнить любое из этих действий, создается исключение DbUpdateException.

Если позже вы добавите новую таблицу, следует изменить политику безопасности с помощью запроса ALTER и добавить в новую таблицу предикаты фильтрации и блокировки: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Добавление ограничений по умолчанию на автоматическое заполнение TenantId для команд INSERT
Для каждой таблицы при вставке строк можно установить стандартное ограничение на автоматическое заполнение ключа TenantId значением, в настоящее время хранимым в SESSION_CONTEXT. Например: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Теперь приложению не требуется указывать TenantId при вставке строк: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Если вы используете ограничения по умолчанию для проекта Entity Framework, рекомендуется НЕ включать столбец TenantId в свою модель данных EF. Дело в том, что запросы Entity Framework автоматически предоставляют значения по умолчанию, которые переопределят ограничения по умолчанию, созданные в инструкциях T-SQL с использованием SESSION_CONTEXT. Чтобы использовать ограничения по умолчанию, например в примере проекта, следует удалить TenantId из файла DataClasses.cs (и запустить Add-Migration в консоли диспетчера пакетов) и воспользоваться T-SQL, чтобы убедиться, что это поле существует только в таблицах базы данных. В этом случае EF не подставляет неправильные значения по умолчанию при вставке данных. 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(Необязательно) Включение суперпользователя для получения доступа ко всем строкам
Некоторым приложениям требуется суперпользователь, который имеет доступ ко всем строкам. Например, чтобы включить создание отчетов по всем клиентам всех сегментов или чтобы выполнять операции разделения и объединения сегментов, для которых потребуется перемещение строк между базами данных. Для этого потребуется создать нового пользователя SQL (в данном случае суперпользователя) в каждой базе данных сегмента. Затем измените политику безопасности, внеся в нее новую функцию предиката, которая разрешит пользователю доступ ко всем строкам:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Обслуживание, 
* **Добавление новых сегментов.** Выполните скрипт T-SQL, чтобы включить политику RLS для всех новых сегментов, иначе запросы в таких сегментах фильтроваться не будут.
* **Добавление новых таблиц.** Добавляйте предикаты фильтрации и блокировки в политику безопасности для всех сегментов каждый раз, когда создается новая таблица, иначе запросы в такой таблице фильтроваться не будут. Это действие можно автоматизировать с помощью триггера DDL, как это описано в записи блога [Apply Row-Level Security automatically to newly created tables](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx) (Автоматическое применение безопасности на уровне строк для вновь созданных таблиц).

## <a name="summary"></a>Сводка
Средства эластичных баз данных и безопасность на уровне строк могут использоваться совместно для масштабирования уровня данных приложения благодаря поддержке мультитенантных и однотенантных сегментов. Мультитенантные сегменты могут использоваться для более эффективного хранения данных (особенно в случаях, где большое количество клиентов имеет лишь несколько строк данных), в то время как однотенантные сегменты — для поддержки premium-клиентов с более строгими требованиями к производительности и изоляции.  Дополнительные сведения см. в [справочнике по безопасности на уровне строк](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Что такое пул эластичных БД Azure?](sql-database-elastic-pool.md)
* [Общие сведения о возможностях эластичных баз данных](sql-database-elastic-scale-introduction.md)
* [Шаблоны разработки для мультитенантных приложений SaaS с использованием базы данных Azure SQL](saas-tenancy-app-design-patterns.md)
* [Authentication in multitenant apps, using Azure AD and OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Сведения о приложении Tailspin Surveys](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Вопросы и запросы на функции
Все возникшие вопросы вы можете задать на [форуме по базам данных SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted), а запросы новых функций оставляйте на [форуме отзывов и предложений по базам данных SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


