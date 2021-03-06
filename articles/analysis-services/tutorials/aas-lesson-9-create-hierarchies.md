---
title: "Учебник по службам Azure Analysis Services: занятие 9 \"Создание иерархий\" | Документы Майкрософт"
description: 
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 28e4e24f5706e88ede25060d5459617befd4aea9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2017
---
# <a name="lesson-9-create-hierarchies"></a>Занятие 9. Создание иерархий

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

На этом занятии мы создадим иерархии. Иерархии — это группы столбцов, упорядоченные по уровням. Например, иерархия "География" может иметь подуровни для страны, области, района и города. Иерархии могут отображаться отдельно от других столбцов в списке полей клиентского приложения отчетов, что упрощает для пользователей клиента навигацию и включение в отчет. Дополнительные сведения см. в статье [Иерархии](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular).
  
Для создания иерархий используется конструктор моделей в *представлении схемы*. Создание иерархий и управление ими не поддерживается в представлении данных.  
  
Предполагаемое время выполнения этого занятия: **20 минут**  
  
## <a name="prerequisites"></a>Предварительные требования  
Этот раздел входит в учебник по табличному моделированию, который следует изучать в предложенном порядке. Прежде чем выполнять задачи в этом разделе, необходимо завершить предыдущее занятие: [Занятие 8. Создание перспектив](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Создание иерархий  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a>Создание иерархии категорий в таблице DimProduct  
  
1.  В конструкторе моделей (представление схемы) щелкните правой кнопкой мыши таблицу **DimProduct** и выберите пункт **Создать иерархию**. В нижней части окна таблицы появится новая иерархия. Назовите иерархию **Category**.  
  
2.  Щелкните и перетащите столбец **ProductCategoryName** в новую иерархию **Category**.  
  
3.  В иерархии **Category** щелкните правой кнопкой мыши столбец **ProductCategoryName** > **Переименовать**, а затем введите **Category**.  
  
    > [!NOTE]  
    > Переименование столбца в иерархии не приводит к переименованию этого столбца в таблице. Столбец в иерархии — это просто представление столбца в таблице.  
  
4.  Щелкните и перетащите столбец **ProductSubcategoryName** в иерархию **Category**. Назовите его **Subcategory**. 
  
5.  Щелкните правой кнопкой мыши столбец **ModelName**, выберите **Добавить иерархию**, а затем **Category**. Назовите его **Model**.

6.  Наконец, добавьте столбец **EnglishProductName** в иерархию категорий. Назовите его **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a>Создание иерархий в таблице DimDate  
  
1.  В таблице **DimDate** создайте иерархию с именем **Calendar**.  
  
3.  Добавьте следующие столбцы по порядку:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  В таблице **DimDate** создайте иерархию **Fiscal**. Включите следующие столбцы по порядку:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Наконец, в таблице **DimDate** создайте иерархию **ProductionCalendar**. Включите следующие столбцы по порядку:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Что дальше?
[Занятие 10. Создание разделов](../tutorials/aas-lesson-10-create-partitions.md). 
  
  
