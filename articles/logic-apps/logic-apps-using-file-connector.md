---
title: "Подключение к локальной файловой системе из Azure Logic Apps | Документация Майкрософт"
description: "Подключение к локальным файловым системам из рабочих процессов приложения логики через локальный шлюз данных и соединитель файловой системы"
keywords: "файловые системы, локальные"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: LADocs; deli
ms.openlocfilehash: 7738b3346af49cb8aa811eb17003d1b72b1bbe46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Подключение к локальным файловым системам из приложений логики с помощью соединителя файловой системы

Для управления данными и безопасного доступа к локальным ресурсам приложения логики могут использовать локальный шлюз данных. В этой статье на примере базового сценария показано, как подключиться к локальной файловой системе: копирование файла, переданного в Dropbox, в файловый ресурс, а затем отправка электронного сообщения.

## <a name="prerequisites"></a>Предварительные требования

* Скачайте последнюю версию [локального шлюза данных](https://www.microsoft.com/download/details.aspx?id=53127).

* Установите и настройте последнюю версию локального шлюза данных (не ниже версии 1.15.6150.1). Пошаговые инструкции см. в статье [Доступ к локальным источникам данных из приложений логики с помощью локального шлюза данных](http://aka.ms/logicapps-gateway). Шлюз необходимо установить на локальном компьютере, прежде чем выполнять дальнейшие действия.

* Базовые знания [создания приложений логики](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Добавление триггера и действий для подключения к файловой системе

1. Создайте пустое приложение логики. В качестве первого действия добавьте триггер **Dropbox – When a file is created** (Dropbox — при создании файла). 

2. Для триггера выберите **Следующий шаг** > **Добавить действие**. 

3. В поле поиска введите "файловая система" в качестве фильтра. Когда отобразятся все действия для соединителя файловой системы, выберите действие **File System - Create file** (Файловая система — создать файл). 

   ![Поиск соединителя File](media/logic-apps-using-file-connector/search-file-connector.png)

4. Если у вас еще нет подключения к файловой системе, то вам будет предложено создать такое подключение. 

5. Установите флажок **Connect via on-premises data gateway** (Подключение через локальный шлюз данных). Когда отобразятся свойства подключения, настройке подключение, как указано в таблице.

   ![Настройка подключения](media/logic-apps-using-file-connector/create-file.png)

   | Настройка | Описание |
   | ------- | ----------- |
   | **Корневая папка** | Укажите корневую папку для файловой системы. Это может быть локальная папка на компьютере, где установлен локальный шлюз данных, или сетевая папка, к которой этот компьютер имеет доступ. <p>**Подсказка.** Корневая папка является основной родительской папкой, которая используется для относительных путей всех действий с файлами. | 
   | **Тип проверки подлинности** | Тип аутентификации, который используется файловой системой. | 
   | **Имя пользователя** | Укажите имя пользователя {*домен*\\*имя_пользователя*} для ранее установленного шлюза. | 
   | **Пароль** | Укажите пароль для ранее установленного шлюза. | 
   | **Шлюз** | Выберите ранее установленный шлюз. | 
   ||| 

6. После предоставления всех сведений о подключении нажмите кнопку **Создать**. 

   Logic Apps настраивает и проверяет подключение, гарантируя, что оно работает правильно. 
   Если подключение настроено правильно, то отобразятся параметры для действия, выбранного ранее. 
   Теперь соединитель файловой системы готов к использованию.

7. Настройте действие **Create file** (Создать файл) для копирования файлов из Dropbox в корневую папку локального файлового ресурса.

   ![Действие для создания файла](media/logic-apps-using-file-connector/create-file-filled.png)

8. После этого действия для копирования файла добавьте действие Outlook, которое будет отправлять сообщение электронной почты, чтобы соответствующие пользователи знали о новом файле. Укажите адреса получателей, тему и текст сообщения. 

   В списке **Динамическое содержимое** можно выбрать выходные данные из соединителя файлов, чтобы добавить дополнительные сведения в сообщение электронной почты.

   ![Действие для отправки электронного сообщения](media/logic-apps-using-file-connector/send-email.png)

9. Сохраните приложение логики. Тестирование приложения путем загрузки файла в Dropbox. Файл будет скопирован в локальную общую папку, а вы получите сообщение электронной почты об этой операции.

Поздравляем, вы только что получили работающее приложение логики, которое может подключаться к локальной файловой системе. 

Попробуйте изучить другие функции, которые предлагает соединитель, например:

- Создание файла
- List files in folder (Вывод списка файлов в папке)
- Добавление файла
- Удаление файла
- Получение содержимого файла
- Получение содержимого файла с помощью пути
- Получение метаданных файла
- Получение метаданных файла с помощью пути
- List files in root folder (Вывод списка файлов в корневой папке)
- Обновление файла

## <a name="view-the-swagger"></a>Просмотр Swagger

Ознакомьтесь с [дополнительными сведениями о Swagger](/connectors/fileconnector/). 

## <a name="get-support"></a>Получение поддержки

* Если у вас возникли вопросы, то посетите [форум Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Чтобы улучшить Azure Logic Apps и соединители, голосуйте за идеи или предлагайте собственные на [сайте пользовательских мнений Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Дальнейшие действия

* [Подключение к локальным данным](../logic-apps/logic-apps-gateway-connection.md) 
* [См. статью Мониторинг приложений логики.](../logic-apps/logic-apps-monitor-your-logic-apps.md)
* [Сценарии B2B с пакетом интеграции Enterprise](../logic-apps/logic-apps-enterprise-integration-overview.md)
