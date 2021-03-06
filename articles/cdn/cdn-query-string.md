---
title: "Управление режимом кэширования Azure CDN с помощью строк запросов | Документация Майкрософт"
description: "Функция кэширования строк запросов в Azure CDN позволяет управлять кэшированием файлов, которые содержат строки запросов."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 17410e4f-130e-489c-834e-7ca6d6f9778d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: mazha
ms.openlocfilehash: 28e724f34c32edb0d5641b24f9ffedb7dc5f9680
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2017
---
# <a name="control-azure-content-delivery-network-caching-behavior-with-query-strings"></a>Управление режимом кэширования в сети доставки содержимого Azure с помощью строк запросов
> [!div class="op_single_selector"]
> * [Стандартный](cdn-query-string.md)
> * [Azure CDN уровня "Премиум" от Verizon](cdn-query-string-premium.md)
> 

## <a name="overview"></a>Обзор
В сети доставки содержимого (CDN) Azure можно управлять кэшированием файлов для веб-запросов, содержащих строку запроса. В веб-запросе такой строкой считается та часть запроса, которая следует после символа `?`. Строка запроса может содержать один или несколько параметров, которые разделяются символом `&`. Например, `http://www.domain.com/content.mov?data1=true&data2=false`. Если запрос содержит более одного параметра строки запроса, порядок параметров не имеет значения. 

> [!IMPORTANT]
> Продукты CDN уровня "Стандартный" и "Премиум" предоставляют одинаковые возможности кэширования строк запроса, но имеют разные пользовательские интерфейсы.  В этой статье описывается пользовательский интерфейс для **Azure CDN уровня "Стандартный" от Akamai** и **Azure CDN уровня "Стандартный" от Verizon**. Дополнительные сведения о кэшировании строк запроса с помощью **Azure CDN уровня "Премиум" от Verizon** см. в статье [Управление режимом кэширования Azure CDN с помощью строк запросов (ценовая категория "Премиум")](cdn-query-string-premium.md).

Доступны три режима кэширования строк запросов.

- **Пропуск строк запросов**. Режим по умолчанию. В этом режиме при первом запросе граничный узел CDN передает строки запросов от запрашивающей стороны в источник и кэширует ресурс. Все последующие запросы к этому ресурсу, которые обслуживаются из граничного узла, будут пропускать строки запроса до истечения срока действия кэшированного ресурса.
- **Обход кэширования для строк запросов**. В этом режиме запросы со строкой запроса не кэшируются на граничном узле CDN. Граничный узел получает ресурс непосредственно из источника и передает его запрашивающей стороне при каждом запросе.
- **Кэширование каждого уникального URL-адреса**. В этом режиме каждый запрос с уникальным URL-адресом, включая строку запроса, считается уникальным ресурсом с собственным кэшем. Например, ответ источника для запроса к `example.ashx?q=test1` сохраняется в кэше на граничном узле и возвращается для всех последующих кэшей с этой же строкой запроса. Запрос к `example.ashx?q=test2` будет сохранен в кэше как отдельный ресурс с собственным сроком действия.

## <a name="changing-query-string-caching-settings-for-standard-cdn-profiles"></a>Изменение параметров кэширования строки запроса для профилей CDN уровня "Стандартный"
1. Откройте профиль CDN, а затем выберите конечную точку CDN, которой вы хотите управлять.
   
   ![Конечные точки профиля CDN](./media/cdn-query-string/cdn-endpoints.png)
   
2. В разделе "Параметры" щелкните **Кэш**.
   
    ![Кнопка "Кэш" в профиле CDN](./media/cdn-query-string/cdn-cache-btn.png)
   
3. В списке **Режим кэширования строк запросов** выберите нужный режим и нажмите кнопку **Сохранить**.
   
  <!--- Replace screen shot after general caching goes live ![CDN query string caching options](./media/cdn-query-string/cdn-query-string.png) --->

> [!IMPORTANT]
> Так как распространение регистрационных сведений в сети CDN может занять некоторое время, изменения параметров кэширования строк вступают в силу не сразу. Для профилей **Azure CDN от Akamai** распространение обычно завершается в течение одной минуты. Для профилей **Azure CDN от Verizon** распространение обычно завершается в течение 90 минут, но в некоторых случаях может занимать больше времени.


