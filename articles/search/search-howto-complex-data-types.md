---
title: "Моделирование сложных типов данных в службе поиска Azure | Документация Майкрософт"
description: "Вложенные или иерархические структуры данных можно моделировать в индексе Поиска Azure с помощью плоских наборов строк и типа данных \"Коллекции\"."
services: search
documentationcenter: 
author: LiamCa
manager: pablocas
editor: 
tags: complex data types; compound data types; aggregate data types
ms.assetid: e4bf86b4-497a-4179-b09f-c1b56c3c0bb2
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: liamca
ms.openlocfilehash: d576fd7bb267ae7a100589413185b595e3b2be42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-model-complex-data-types-in-azure-search"></a>Моделирование сложных типов данных в службе поиска Azure
Внешние наборы данных, используемые для заполнения индекса Поиска Azure, иногда включают иерархические или вложенные подструктуры, которые невозможно аккуратно разделить на табличные наборы строк. К примерам таких структур можно отнести несколько расположений и номеров телефонов для одного клиента, несколько цветов и размеров для одного SKU, несколько авторов на одну книгу и т. д. В моделировании эти структуры называются *сложные типы данных*, *составные типы данных*, *композитные типы данных*, *агрегатные типы данных* и т. д.

В службе поиска Azure не предусмотрена встроенная поддержка сложных типов данных. Однако в такой ситуации есть проверенное решение — процесс, состоящий из двух этапов: преобразование в плоскую структуру и воссоздание внутренней структуры с помощью типа данных **Коллекция**. Метод, описанный в этой статье, можно использовать для поиска содержимого, поиска содержимого с использованием фасетов, а также фильтрации и сортировки содержимого.

## <a name="example-of-a-complex-data-structure"></a>Пример сложной структуры данных
Как правило, соответствующие данные представляют собой набор документов в формате JSON или XML или элементы в хранилище NoSQL, например Azure Cosmos DB. Структурно проблема возникает из-за наличия нескольких дочерних элементов, по которым нужно выполнить поиск и фильтрацию.  Для демонстрации решения используйте в качестве примера следующий документ JSON, в котором указан набор контактов:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Поля id, name и company можно легко сопоставить друг с другом как поля в индексе Поиска Azure. Однако поле locations содержит массив расположений с набором идентификаторов и описаниями расположений. С учетом того, что в Поиске Azure нет типа данных, поддерживающего такое поле, нужно подобрать другой способ моделирования в Поиске Azure. 

> [!NOTE]
> Этот прием также описан в записи блога Кирка Эванса (Kirk Evans) [Indexing DocumentDB with Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/) (Индексирование DocumentDB с помощью службы поиска Azure). В ней показан метод преобразования структуры данных в плоскую форму с получением полей `locationsID` и `locationsDescription`, которые являются [коллекциями](https://msdn.microsoft.com/library/azure/dn798938.aspx) (или массивом строк).   
> 
> 

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Часть 1. Преобразование массива в отдельные поля
Чтобы создать индекс службы поиска Azure, вмещающий этот набор данных, создайте отдельные поля для вложенной подструктуры: `locationsID` и `locationsDescription` с типом данных [Коллекция](https://msdn.microsoft.com/library/azure/dn798938.aspx) (или массив строк). В этих полях будут проиндексированы значения "1" и "2" в поле `locationsID` для Артема Кузнецова и "3" и "4" в поле `locationsID` для Алины Ковалевой.  

Данные в Поиске Azure будут выглядеть следующим образом: 

![пример данных, 2 строки](./media/search-howto-complex-data-types/sample-data.png)

## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Часть 2. Добавление поля коллекции в определении индекса
В схеме индекса определения полей могут выглядеть примерно как в приведенном ниже примере.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Проверка поведения при поиске и расширение индекса при необходимости
Если индекс создан, а данные загружены, можно проверить выполнение запроса на поиск к набору данных в решении. Каждое поле типа **Коллекция** должно поддерживать **поиск**, **фильтрацию** и **поиск с использованием фасетов**. Вы сможете выполнять такие запросы:

* поиск всех людей, работающих в центральном офисе AdventureWorks (значение Adventureworks Headquarters);
* получение числа людей, работающих в главном офисе (значение Home Office);  
* определение отделов, в которых трудятся люди, работающие в главном офисе (значение Home Office), а также определение количества людей в каждом из офисов.  

Этот прием не подходит в ситуациях, когда нужно выполнить поиск с использованием идентификатора и описания расположения. Например:

* Найти всех людей, работающих в главном офисе (значение Home Office) с идентификатором расположения 4.  

Если помните, исходное содержимое выглядело так:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Однако теперь, когда данные разделены на отдельные поля, мы не знаем, какой из идентификаторов относится к главному офису, в котором работает Алина Ковалева (`locationsID 3` или `locationsID 4`).  

В этом случае определите в индексе другое поле, объединяющее все данные в одну коллекцию.  В нашем примере это поле будет называться `locationsCombined`. Содержимое мы разделим с помощью значка `||`, хотя можно выбрать любой разделитель, который, по вашему мнению, будет уникальным набором символов для содержимого. Например: 

![пример данных, 2 строки с разделителем](./media/search-howto-complex-data-types/sample-data-2.png)

С помощью поля `locationsCombined` теперь можно разместить еще больше запросов, например:

* отображение числа людей, работающих в главном офисе (значение Home Office) с идентификатором расположения "4";  
* поиск людей, работающих в главном офисе (значение Home Office) с идентификатором расположения "4". 

## <a name="limitations"></a>Ограничения
Этот метод полезен в ряде сценариев, но не подходит в каждом случае.  Например:

1. Если в сложном типе данных нет статического набора полей и было невозможно сопоставить все возможные типы с одним полем. 
2. Обновление вложенных объектов требует дополнительных усилий, так как необходимо определить, что именно нужно обновить в индексе Поиска Azure.

## <a name="sample-code"></a>Пример кода
Пример индексирования сложного набора данных JSON в Поиск Azure и выполнения запросов к этому набору данных см. в этом [репозитории GitHub](https://github.com/liamca/AzureSearchComplexTypes).

## <a name="next-step"></a>Дальнейшие действия
[Голосуйте за встроенную поддержку сложных типов данных](https://feedback.azure.com/forums/263029-azure-search) на странице UserVoice Поиска Azure и предоставьте дополнительные сведения о реализации функции на наше рассмотрение. Для связи со мной вы можете написать в Twitter через канал @liamca.

