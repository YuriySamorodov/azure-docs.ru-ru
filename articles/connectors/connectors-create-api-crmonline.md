---
title: "Подключение к Dynamics 365 (Интернет) из Azure Logic Apps | Документация Майкрософт"
description: "Создайте рабочие процессы приложения логики, управляющие сущностями Dynamics 365 (Интернет) через API, предоставляемые соединителем Dynamics 365"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a>Подключение к Dynamics 365 из рабочих процессов приложения логики

Благодаря приложениям логики можно подключаться к Dynamics 365 (Интернет) и создавать полезные бизнес-процессы для создания записей, обновления элементов или возвращения списка записей. С помощью соединителя Dynamics 365 можно делать следующее:

* формировать бизнес-процессы на основе данных, получаемых из Dynamics 365 (Интернет);
* использовать действия, которые получают ответ и делают выходные данные доступными для использования другими действиями. Например, при обновлении элемента в Dynamics 365 (Интернет) можно отправлять сообщение электронной почты с помощью Office 365.

В этой статье показано, как создать приложение логики, которое создаст задачу в Dynamics 365 после создания нового интереса (потенциального клиента).

## <a name="prerequisites"></a>Предварительные требования
* Учетная запись Azure.
* Учетная запись Dynamics 365 (Интернет).

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Создание задачи при создании интереса в Dynamics 365

1.  [Войдите в Azure](https://portal.azure.com).

2.  В поле поиска Azure введите `Logic apps` и нажмите клавишу ВВОД.

      ![Поиск Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  В разделе **Приложения логики** щелкните **Добавить**.

      ![Добавление приложения логики](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  Чтобы создать приложение логики, заполните поля **Имя**, **Подписка**, **Группа ресурсов** и **Расположение** и нажмите кнопку **Создать**.

5.  Выберите новое приложение логики. После получения уведомления **Развертывание прошло успешно** щелкните **Обновить**.

6.  В разделе **Средства разработки** щелкните **Конструктор приложений логики**. В списке шаблонов щелкните **Пустое приложение логики**.

7.  В поле поиска введите `Dynamics 365`. В списке триггеров Dynamics 365 выберите **365 Dynamics — при создании записи**.

8.  Если будет предложено войти в Dynamics 365, сделайте это.

9.  В разделе описания триггера введите следующие сведения.

  * **Название организации**. Выберите экземпляр Dynamics 365 для прослушивания приложением логики.

  * **Имя сущности**. Выберите сущность для ожидания передачи данных. Это событие действует как триггер для запуска приложения логики. 
  В этом пошаговом руководстве выберите **Интересы**.

  * **Как часто вам нужно проверять наличие элементов?** В этом разделе вы задаете, как часто приложение логики будет проверять на наличие обновлений, связанных с триггером. Значение по умолчанию — проверять на наличие обновлений каждые три минуты.

    * **Частота**. Выберите секунды, минуты, часы или дни.

    * **Интервал**. Введите значение секунд, минут, часов или дней, которые должны пройти до следующей проверки.

      ![Сведения о триггере приложения логики](./media/connectors-create-api-crmonline/trigger-details.png)

10. Выберите поле **+Новый шаг**, а затем щелкните **Добавить действие**.

11. В поле поиска введите `Dynamics 365`. В списке действий выберите **Dynamics 365 — создать новую запись**.

12. Введите следующие сведения:

    * **Название организации**. Выберите экземпляр Dynamics 365, в котором последовательность должна создать запись. 
    Обратите внимание, что это не должен быть экземпляр, который вызывает событие.

    * **Имя сущности**. Выберите сущность, которая будет создавать запись во время возникновения события. 
    В этом пошаговом руководстве выберите **Задачи**.

13. Щелкните в появившемся поле **Subject** (Субъект). В появившемся списке динамического содержимого можно выбрать одно из этих полей:

    * **Фамилия**. Выберите это поле, чтобы при создании записи задачи вставить фамилию потенциального клиента в поле Subject (Субъект) задачи.
    * **Тема**. Выберите это поле, чтобы при создании записи задачи вставить поле "Тема" для потенциального клиента в поле Subject (Субъект) задачи. 
    Щелкните поле **Тема**, чтобы добавить его в поле **Subject** (Субъект).

      ![Сведения о создании новой записи приложения логики](./media/connectors-create-api-crmonline/create-record-details.png)

14. Нажмите кнопку **Сохранить** на панели инструментов конструктора приложений логики.

    ![Кнопка "Сохранить" на панели инструментов конструктора приложений логики](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. Чтобы запустить приложение логики, щелкните **Запуск**.

    ![Кнопка "Сохранить" на панели инструментов конструктора приложений логики](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Теперь создайте запись интереса в Dynamics 365 for Sales, чтобы увидеть поток в действии.

## <a name="set-advanced-options-for-a-logic-app-step"></a>Настройка дополнительных параметров для шага приложения логики

Чтобы указать способ фильтрации данных на шаге приложения логики, щелкните **Показать расширенные параметры** на этом шаге, а затем добавьте фильтр или упорядочивание по запросу.

Например, можно использовать запрос фильтра, чтобы получить только активные учетные записи в поименном порядке. Чтобы выполнить эту задачу, введите запрос фильтра OData `statuscode eq 1` и в списке динамического содержимого выберите **Имя учетной записи**. См. дополнительные сведения о параметрах строки запроса [$filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) и [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2) на сайте MSDN.

![Расширенные параметры приложения логики](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Рекомендации по использованию расширенных параметров

При добавлении значения необходимо учитывать тип поля независимо от того, вводите ли вы значение или выбираете из списка динамического содержимого.

Тип поля  |Использование  |Место нахождения  |Имя  |Тип данных  
---------|---------|---------|---------|---------
Текстовые поля|В текстовые поля необходимо вводить одну строку текста или динамическое содержимое, представляющее собой поле текстового типа. Например, поля "Категория" и "Подкатегория".|Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |category |Одна строка текста        
Целочисленные поля | В некоторые поля необходимо ввести целое число или добавить динамическое содержимое, которое является полем целочисленного типа. Например, поля "Процент выполнения" и "Длительность". |Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |percentcomplete |Целое число         
Поля даты | В некоторые поля необходимо ввести дату в формате "мм/дд/гггг" или добавить динамическое содержимое, которое является полем даты. Например, такие поля, как "Дата создания", "Дата начала", "Фактическое начало", "Время последней приостановки", "Фактическое окончание" и "Дата выполнения". | Параметры > Настройки > Настроить систему > Сущности > Задачи > Поля |createdon |Дата и время
Поля, для которых требуется как идентификатор записи, так и тип поиска |Для некоторых полей, которые ссылаются на другую запись сущности, требуется как идентификатор записи, так и тип поиска. |Параметры > Настройки > Настроить систему > Сущности > Учетная запись > Поля  | accountid  | Первичный ключ

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Другие примеры полей, для которых требуется как идентификатор записи, так и тип поиска
В дополнение к предыдущей таблице ниже приведены примеры полей, которые не работают со значениями, выбранными в списке динамического содержимого. Вместо этого в PowerApps в эти поля необходимо ввести как идентификатор записи, так и тип поиска.  
* "Владелец" и "Тип владельца". Поле "Владелец" должно содержать допустимый идентификатор записи пользователя или рабочей группы. В поле "Тип владельца" должен быть задан тип **systemusers** (системные пользователи) или **рабочие группы**.
* "Клиент" и "Тип клиента". Поле "Клиент" должно содержать допустимый идентификатор учетной записи или записи контакта. В поле "Тип владельца" должен быть задан тип **учетные записи** или **контакты**.
* "В отношении" и поле типа "В отношении". Поле "В отношении" должно содержать допустимый идентификатор записи, например идентификатор учетной записи или записи контакта. В поле типа "В отношении" должен быть задан тип поиска записи, например **учетные записи** или **контакты**.

В следующем примере действия создания задачи в поле "В отношении" добавляется учетная запись, соответствующая идентификатору записи.

![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid-type-account.png)

В этом примере задача назначается для конкретного пользователя на основе идентификатора записи пользователя.

![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid-type-user.png)

Чтобы найти идентификатор записи, ознакомьтесь с разделом *Поиск идентификатора записи*.

## <a name="find-the-record-id"></a>Поиск идентификатора записи

1. Откройте запись, например для учетной записи.

2. На панели инструментов "Действия" щелкните **Всплывающее окно** ![запись всплывающего окна](./media/connectors-create-api-crmonline/popout-record.png).
Кроме того, на панели инструментов "Действия" щелкните **Отправить ссылку по почте**, чтобы скопировать полный URL-адрес в программу электронной почты по умолчанию.

   Идентификатор записи отображается между знаками кодирования URL-адреса %7b и %7d.

   ![Идентификатор записи и тип учетной записи](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Устранение неполадок
Чтобы устранить сбой шага в приложении логики, просмотрите сведения о состоянии события.

1. В разделе **Приложения логики** выберите свое приложение логики и щелкните **Обзор**. 

   Отобразится область "Сводка", где содержатся сведения о состоянии выполнения приложения логики. 

   ![Состояние выполнения приложения логики](./media/connectors-create-api-crmonline/tshoot1.png)

2. Чтобы получить дополнительные сведения о неудачных циклах выполнения, щелкните событие со сбоем. Чтобы развернуть шаг с ошибкой, щелкните его.

   ![Развертывание шага с ошибкой](./media/connectors-create-api-crmonline/tshoot2.png)

   Отобразятся сведения о шаге, которые могут помочь устранить причину сбоя.

   ![Сведения о шаге с ошибкой](./media/connectors-create-api-crmonline/tshoot3.png)

Дополнительные сведения об устранении неполадок в приложениях логики см. в статье [Диагностика сбоев приложений логики](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Сведения о соединителях

Информацию о существующих ограничениях, а также о триггерах и действиях, определенных в Swagger, см. в статье со [сведениями о соединителях](/connectors/crm/). 

## <a name="next-steps"></a>Дальнейшие действия
Чтобы узнать, какие еще соединители доступны в Logic Apps, просмотрите [список интерфейсов API](apis-list.md).
