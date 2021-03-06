---
title: "Создание задания экспорта для импорта и экспорта Azure | Документация Майкрософт"
description: "Узнайте, как создать задание экспорта для службы импорта и экспорта Microsoft Azure."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 613d480b-a8ef-4b28-8f54-54174d59b3f4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: bdeac373aa8270bd9de8f135ec7166d744fd83ae
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="creating-an-export-job-for-the-azure-importexport-service"></a>Создание задания экспорта для службы импорта и экспорта Azure
Создание задания экспорта для службы импорта и экспорта Microsoft Azure с помощью интерфейса REST API включает следующие шаги:

-   выбор больших двоичных объектов для экспорта;

-   получение адреса для отправки;

-   создание задания экспорта;

-   отправка пустых дисков в Майкрософт через поддерживаемого перевозчика;

-   обновление задания экспорта с использованием сведений о посылке;

-   получение дисков от Майкрософт.

 Общие сведения о службе импорта и экспорта, а также руководство по использованию [портала Azure](https://portal.azure.com/) для создания заданий импорта и экспорта и управления ими см. в статье [Использование службы импорта и экспорта Azure для передачи данных в хранилище BLOB-объектов](storage-import-export-service.md).

## <a name="selecting-blobs-to-export"></a>Выбор больших двоичных объектов для экспорта
 Для создания задания экспорта следует указать список больших двоичных объектов, которые необходимо экспортировать из вашей учетной записи хранения. Выбрать большие двоичные объекты для экспорта можно несколькими способами:

-   использовать относительный путь к большому двоичному объекту, чтобы выбрать один BLOB-объект и все его моментальные снимки;

-   использовать относительный путь к большому двоичному объекту, чтобы выбрать один BLOB-объект без моментальных снимков;

-   использовать относительный путь к большому двоичному объекту и время создания моментального снимка, чтобы выбрать один моментальный снимок;

-   использовать префикс большого двоичного объекта, чтобы выбрать все BLOB-объекты и моментальные снимки с этим префиксом;

-   экспортировать все большие двоичные объекты и моментальные снимки в учетной записи хранения.

 Дополнительные сведения об указании больших двоичных объектов для экспорта см. в разделе об операции [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate).

## <a name="obtaining-your-shipping-location"></a>Получение расположения для отправки
Перед созданием задания экспорта необходимо получить название и адрес места для отправки, выполнив операцию [Get Location](https://portal.azure.com) или [List Locations](/rest/api/storageimportexport/listlocations). Операция `List Locations` вернет список расположений и почтовых адресов. В полученном списке выберите расположение и отправьте жесткие диски по соответствующему адресу. Также можно использовать операцию `Get Location`, чтобы сразу получить адрес доставки для определенного расположения.

Выполните следующие действия, чтобы получить адрес доставки.

-   Определите имя расположения своей учетной записи хранения. Оно находится в поле **Расположение** на **панели мониторинга** учетной записи хранения на классическом портале. Это значение также можно получить, отправив запрос с помощью операции API управления службами [Get Storage Account Properties](/rest/api/storagerp/storageaccounts#StorageAccounts_GetProperties) (Получить свойства учетной записи хранения).

-   Получите расположение, доступное для обработки этой учетной записи хранения, путем вызова операции `Get Location`.

-   Расположение можно использовать, если его свойство `AlternateLocations` указывает на это же расположение. В противном случае снова вызовите операцию `Get Location` для одного из альтернативных расположений. Исходное расположение может быть временно закрыто для обслуживания.

## <a name="creating-the-export-job"></a>Создание задания экспорта
 Чтобы создать задание экспорта, вызовите операцию [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate). Необходимо будет указать следующие сведения:

-   имя задания;

-   имя учетной записи хранения.

-   название расположения для отправки, полученное на предыдущем шаге;

-   тип задания (экспорт);

-   обратный адрес, по которому следует вернуть диски после завершения задания экспорта;

-   список больших двоичных объектов (или их префиксов) для экспорта.

## <a name="shipping-your-drives"></a>Отправка дисков
 Используйте инструмент импорта и экспорта Azure для определения количества дисков, которые необходимо отправить, в зависимости от выбранных для экспорта больших двоичных объектов и размера диска. Дополнительные сведения см. в статье [Использование средства импорта и экспорта Azure версии 1](storage-import-export-tool-how-to-v1.md).

 Упакуйте диски в одну посылку и отправьте их по адресу, полученному на предыдущем шаге. Запишите номер для отслеживания посылки для следующего шага.

> [!NOTE]
>  Диски необходимо отправить через поддерживаемого перевозчика, который предоставит номер для отслеживания посылки.

## <a name="updating-the-export-job-with-your-package-information"></a>Обновление задания экспорта с использованием сведений о посылке
 После получения номера для отслеживания выполните операцию [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) (Обновление свойств задания), чтобы обновить имя перевозчика и номер для отслеживания в задании. При необходимости можно также указать количество дисков, обратный адрес и дату отправки.

## <a name="receiving-the-package"></a>Получение посылки
 После обработки задания экспорта вы получите диски с зашифрованными данными. Чтобы получить ключ BitLocker для каждого диска выполните операцию [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get). Затем разблокируйте диск с помощью ключа. Файл манифеста диска на каждом диске содержит список файлов на диске, а также исходный адрес большого двоичного объекта для каждого файла.

## <a name="next-steps"></a>Дальнейшие действия

* [Использование REST API службы импорта и экспорта Azure](storage-import-export-using-the-rest-api.md)
