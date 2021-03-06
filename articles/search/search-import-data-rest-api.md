---
title: "Отправка данных в службу поиска Azure c помощью REST API | Документация Майкрософт"
description: "Узнайте, как отправлять данные в индекс службы поиска Azure с помощью REST API."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: 8d0749fb-6e08-4a17-8cd3-1a215138abc6
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: f22a33ed86fbfc46dfa732239263a49f34c4afee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Отправка данных в службу поиска Azure с помощью REST API
> [!div class="op_single_selector"]
>
> * [Обзор](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
>
>

В этой статье объясняется, как использовать [REST API службы поиска Azure](https://docs.microsoft.com/rest/api/searchservice/) для импорта данных в индекс службы поиска Azure.

Прежде чем приступать к выполнению инструкций из этого руководства, вам нужно [создать индекс службы поиска Azure](search-what-is-an-index.md).

Для передачи документов в индекс с помощью REST API вам нужно отправить HTTP-запрос POST на URL-адрес конечной точки индекса. Текст HTTP-запроса является объектом JSON, который содержит документы для добавления, изменения или удаления.

## <a name="identify-your-azure-search-services-admin-api-key"></a>Определение ключа API администратора службы поиска Azure
Когда вы отправляете HTTP-запросы к службе с помощью REST API, *каждый* запрос API должен содержать ключ API, который был создан для подготовленной службы поиска. Если есть действительный ключ, для каждого запроса устанавливаются отношения доверия между приложением, которое отправляет запрос, и службой, которая его обрабатывает.

1. Чтобы найти ключи API своей службы, войдите на [портал Azure](https://portal.azure.com/).
2. Перейдите к колонке службы поиска Azure.
3. Щелкните значок "Ключи".

Ваша служба получит *ключи администратора* и *ключи запросов*.

* Первичные и вторичные *ключи администратора* предоставляют полный доступ ко всем операциям, включая возможность управлять службой, создавать и удалять индексы, индексаторы и источники данных. Ключей два, поэтому вы можете и дальше использовать вторичный ключ, если решите повторно создать первичный ключ, и наоборот.
* *Ключи запросов* предоставляют только разрешение на чтение индексов и документов; обычно они добавляются в клиентские приложения, которые создают запросы на поиск.

Для импорта данных в индекс можно использовать первичный или вторичный ключ администратора.

## <a name="decide-which-indexing-action-to-use"></a>Выбор операций индексирования
В интерфейсе REST API можно создавать HTTP-запросы POST с текстом запроса JSON, отправляемые в конечную точку вашего индекса службы поиска Azure по URL-адресу. Объект JSON в тексте HTTP-запроса содержит массив JSON с именем value. Этот массив включает объекты JSON — документы, которые вы хотите добавить в свой индекс, обновить или удалить.

Каждый объект JSON в массиве value представляет документ для индексирования, д.). В зависимости от выбранной операции для каждого документа будут включены только определенные поля.

| @search.action | Описание | Необходимые поля для каждого документа | Примечания |
| --- | --- | --- | --- |
| `upload` |Операция `upload` аналогична операции upsert, которая добавляет документ, если он новый, и обновляет либо заменяет его, если он уже существует. |Поле key, а также другие поля, которые вы хотите определить. |При обновлении или замене существующего документа все поля, не указанные в запросе, получат значение `null`. Это происходит, даже если для поля указано непустое значение. |
| `merge` |Обновляет существующий документ с использованием указанных полей. Если документ не существует в индексе, объединение завершится ошибкой. |Поле key, а также другие поля, которые вы хотите определить. |Поля, указанные в запросе на объединение, заменяют собой существующие поля документа. Это относится и к полям типа `Collection(Edm.String)`. Например, если документ содержит поле `tags` со значением `["budget"]` и вы выполняете операцию объединения со значением `["economy", "pool"]` для поля `tags`, в итоге поле `tags` примет значение `["economy", "pool"]`, а не `["budget", "economy", "pool"]`. |
| `mergeOrUpload` |Эта операция аналогична операции `merge`, если документ с указанным значением ключа уже существует в индексе. Если документа нет, эта операция выполняется так же, как и операция `upload` с новым документом. |Поле key, а также другие поля, которые вы хотите определить. |- |
| `delete` |Удаление указанного документа из индекса. |Только ключ |Все указанные поля, которые отличаются от ключевого поля, будут игнорироваться. Чтобы удалить из документа определенное поле, лучше воспользуйтесь операцией `merge`, явным образом задав для требуемого поля значение NULL. |

## <a name="construct-your-http-request-and-request-body"></a>Создание HTTP-запроса и текста запроса
Теперь, когда вы выбрали нужные значения полей для операций индекса, можно приступать к созданию HTTP-запроса и текста запроса JSON для импорта данных.

#### <a name="request-and-request-headers"></a>Запрос и заголовки запроса
В URL-адресе необходимо указать имя службы, имя индекса (в нашем случае это hotels), а также правильную версию API (текущая версия API на момент публикации этого документа — `2016-09-01` ). Необходимо также определить заголовки запросов `Content-Type` и `api-key`. Для заголовка последнего запроса используйте один из ключей администратора службы.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Текст запроса
```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

В этом примере мы используем операции `upload`, `mergeOrUpload` и `delete` в качестве операций поиска.

Предположим, что пример индекса с именем hotels уже заполнен несколькими документами. Обратите внимание: нам не понадобилось указывать все поля документа при использовании операции `mergeOrUpload`, а при использовании `delete` мы указали только ключ документа (`hotelId`).

Также помните о том, что в один запрос на индексирование можно включить максимум 1000 документов (или 16 МБ).

## <a name="understand-your-http-response-code"></a>Анализ кода HTTP-ответа
#### <a name="200"></a>200
После успешной отправки запроса на индексирование вы получите HTTP-ответ с кодом состояния `200 OK`. Текст JSON в HTTP-ответе будет выглядеть так:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Код состояния `207` возвращается, если хотя бы один элемент не удалось проиндексировать. Текст JSON HTTP-ответа будет содержать сведения о документах, которые не удалось проиндексировать.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [!NOTE]
> Часто это означает, что служба поиска настолько загружена, что запросы на индексирование начинают возвращать ответы `503`. В таком случае мы настоятельно рекомендуем задержать выполнение клиентского кода и повторить попытку позже. За это время система восстановится, что повысит вероятность успешной обработки будущих запросов. Непрерывные повторные попытки выполнить запрос только усугубят проблему.
>
>

#### <a name="429"></a>429
Код состояния `429` возвращается, если вы превысили квоту на количество документов, которые можно включить в индекс.

#### <a name="503"></a>503
Код состояния `503` возвращается, если ни один из элементов в запросе не был проиндексирован. Эта ошибка означает, что система перегружена и запрос не может быть обработан в данный момент.

> [!NOTE]
> В таком случае мы настоятельно рекомендуем задержать выполнение клиентского кода и повторить попытку позже. За это время система восстановится, что повысит вероятность успешной обработки будущих запросов. Непрерывные повторные попытки выполнить запрос только усугубят проблему.
>
>

Дополнительные сведения о действиях с документами и ответах об успешном выполнении и ошибках см. в статье, посвященной [добавлению, обновлению и удалению документов](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents). Дополнительные сведения о других кодах состояния HTTP, которые могут быть возвращены в случае сбоя, см. в статье [Коды состояния HTTP (поиск Azure)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

## <a name="next-steps"></a>Дальнейшие действия
Заполнив индекс службы поиска Azure, вы можете приступать к отправке запросов на поиск документов. Дополнительные сведения см. в статье [Запросы в службе поиска Azure](search-query-overview.md).
