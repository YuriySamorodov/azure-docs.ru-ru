---
title: "Управление рабочей областью в службе машинного обучения | Документация Майкрософт"
description: "Управление доступом к рабочим областям Машинного обучения Azure, а также развертывание веб-служб API ML и управление ими"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2f90234bdd5c917a502d24cd16256bc11c7fbed0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Управление рабочей областью машинного обучения Azure

> [!NOTE]
> Сведения об управлении веб-службами на портале веб-служб машинного обучения см. в разделе [Управление веб-службой с помощью портала веб-служб машинного обучения Azure](manage-new-webservice.md).
> 
> 

Рабочими областями машинного обучения можно управлять с помощью портала Azure или классического портала Azure.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Использование портала Azure

Вот как можно управлять рабочей областью на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com/), используя учетную запись администратора подписки Azure.
2. В поле поиска в верхней части страницы введите "machine learning workspaces" и выберите **Machine Learning Workspaces** (Рабочие области машинного обучения).
3. Щелкните рабочую область, которой нужно управлять.

Помимо стандартной информации об управлении ресурсами и параметров вам доступны следующие возможности:

- Страница **Свойства**. На ней отображаются сведения о рабочей области и ресурсах. Кроме того, на этой странице можно изменить подписку и группу ресурсов, с которым и связана эта рабочая область.
- **Повторная синхронизация ключей хранилища**. Рабочая область хранит ключи в учетной записи хранения. Если учетная запись хранения изменила ключи, то можно щелкнуть **Повторно синхронизировать ключи**, чтобы синхронизировать ключи с рабочей областью.

Для управления веб-службами, связанными с этой рабочей областью, используйте портал веб-службы машинного обучения. Полные сведения см. в статье [Управление веб-службой с помощью портала веб-служб машинного обучения Azure](manage-new-webservice.md).

> [!NOTE]
> Для развертывания новых веб-служб или управления ими необходимо назначить роль участника или администратора той подписке, в которую развертывается веб-служба. Чтобы пригласить в рабочую область машинного обучения других пользователей, необходимо назначить им роли участников или администраторов подписки, чтобы они могли развертывать веб-службы или управлять ими. 
> 
>Дополнительные сведения о настройке прав доступа см. в статье [Просмотр назначенных прав доступа для пользователей и групп на портале Azure (общедоступная предварительная версия)](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-the-azure-classic-portal"></a>Использование классического портала Azure

С помощью классического портала Azure можно управлять рабочими областями машинного обучения для следующего:

* мониторинга использования рабочей области;
* настройки рабочей области для предоставления или запрета доступа к ней;
* управления веб-службами, созданными в рабочей области;
* удаления рабочей области.

Кроме того, вкладка панели мониторинга предоставляет общую информацию об использовании рабочей области и позволяет просмотреть сводку данных о ней.  

> [!TIP]
> В студии машинного обучения Azure на вкладке **Веб-службы** можно добавлять, обновлять или удалять веб-службы машинного обучения.
> 
> 

Вот как можно управлять рабочей областью на классическом портале Azure.

1. Войдите в учетную запись Microsoft Azure на [классическом порталe Azure](https://manage.windowsazure.com/) (используйте учетную запись, связанную с подпиской Azure).
2. На панели служб Microsoft Azure выберите **МАШИННОЕ ОБУЧЕНИЕ**.
3. Щелкните рабочую область, которой нужно управлять.

На странице рабочей области содержится три вкладки:

* **ПАНЕЛЬ МОНИТОРИНГА** — позволяет просматривать данные об использовании рабочей области и информацию о ней;
* **НАСТРОЙКА** — позволяет управлять доступом к рабочей области;
* **Веб-службы** — позволяет управлять веб-службами, опубликованными из этой рабочей области.

### <a name="to-monitor-how-the-workspace-is-being-used"></a>Мониторинг использования рабочей области
Щелкните вкладку **ПАНЕЛЬ МОНИТОРИНГА** .

На панели мониторинга можно просмотреть данные общего использования рабочей области и сводку информации о ней.

* На диаграмме **Вычисления** отображаются вычислительные ресурсы, которые использует рабочая область. Вы можете изменить представление для отображения относительных или абсолютных величин. Кроме того, можно изменить промежуток времени, отображаемый на диаграмме.
* **Обзор использования** отображает данные службы хранилища Azure, которое использует рабочая область.
* **Сводка** предоставляет сведенную информацию о рабочей области и полезные ссылки.

> [!NOTE]
> Ссылка **Войти в студию машинного обучения** позволяет открыть Студию машинного обучения Microsoft Azure, используя учетную запись Майкрософт, в которою вы вошли. Для учетной записи Майкрософт, использованной для входа на классический портал Azure для создания рабочей области, не предусмотрено автоматическое предоставление разрешений на открытие этой рабочей области. Чтобы открыть рабочую область, необходимо войти в учетную запись Майкрософт, определенную в качестве учетной записи владельца рабочей области, или получить приглашение присоединиться к рабочей области от владельца.
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a>Предоставление или приостановка доступа для пользователей
Выберите вкладку **НАСТРОЙКА** .

На вкладке конфигурации можно:

* Приостановить доступ к рабочей области машинного обучения, щелкнув «ОТКЛОНИТЬ». Пользователи больше не смогут открыть рабочую область в Студии машинного обучения. Чтобы восстановить доступ, щелкните «РАЗРЕШИТЬ».

Чтобы управлять дополнительными учетными записями с доступом к рабочей области в Студии машинного обучения, нажмите **Войти в Студию машинного обучения** на вкладке **Панель мониторинга** (см. приведенное выше примечание о ссылке **Войти в студию машинного обучения**). Рабочая область откроется в Студии машинного обучения. Здесь щелкните вкладку **Параметры**, а затем — **Пользователи**. Вы можете щелкнуть **Пригласить больше пользователей**, чтобы предоставить пользователям доступ к рабочей области, или выбрать пользователя и щелкнуть **Удалить**.

### <a name="to-manage-web-services-in-this-workspace"></a>Управление веб-службами в рабочей области
Щелкните вкладку **ВЕБ-СЛУЖБЫ** .

Отобразится список веб-служб, опубликованных из этой рабочей области.
Для управления веб-службами щелкните соответствующее имя в списке, чтобы открыть страницу веб-службы.

Для веб-службы может быть определена одна или несколько конечных точек.

* Кроме конечной точки «По умолчанию», вы можете определить дополнительные конечные точки. Чтобы добавить конечную точку, щелкните **Управление конечными точками** в нижней части панели мониторинга для открытия портала веб-служб машинного обучения Azure.
* Чтобы удалить конечную точку (конечную точку по умолчанию нельзя удалить), щелкните флажок в начале строки конечной точки, а затем нажмите **Удалить**. Это удалит конечную точку из веб-службы.
  
  > [!NOTE]
  > Если приложение использует конечную точку веб-службы, а конечная точка удалена, при следующей попытке получить доступ к службе в приложении отобразится сообщение об ошибке.
  > 
  > 

Чтобы открыть конечную точку веб-службы, щелкните ее имя. 

На панели мониторинга можно просмотреть общие сведения об использовании веб-службы за определенный период времени. Период для отображения можно выбрать с помощью раскрывающегося меню "Period" (Период) в правом верхнем углу диаграммы использования. На панели мониторинга отображаются следующие сведения.

* **Requests Over Time** (Запросы за период времени): отображает ступенчатую диаграмму с количеством запросов за выбранный период времени. Помогает выявить всплески в использовании веб-службы.
* **Request-Response Requests** (Вызовы "запрос-ответ"): отображает общее число вызовов "запрос-ответ", которые служба получила за выбранный период времени, а также сколько из них было неудавшихся.
* **Average Request-Response Compute Time** (Среднее время вычисления "запрос-ответ"): отображает среднее время, необходимое для обработки полученных запросов.
* **Batch Requests** (Пакетные запросы): отображает общее число пакетных запросов, которые служба получила за выбранный период времени, а также сколько из них было неудавшихся.
* **Average Job Latency** (Средняя задержка задания): отображает среднее время, необходимое для обработки полученных запросов.
* **Errors** (Ошибки): отображает общее число ошибок, произошедших при вызовах к веб-службе.
* **Services Costs** (Затраты на службу): отображает сведения о затратах согласно плану выставления счетов, связанному с данной службой.

На странице конфигурации можно обновить следующие свойства.

* **Description** (Описание): позволяет ввести описание веб-службы. Описание является обязательным полем.
* **Logging** (Ведение журнала): позволяет включить или отключить ведение журнала ошибок на конечной точке. Дополнительные сведения о ведении журнала см. в статье [Включение функции ведения журналов для веб-служб машинного обученияя](web-services-logging.md).
* **Enable Sample data** (Включить образцы данных): позволяет добавлять образцы данных, которые можно использовать для тестирования службы "запрос — ответ". Если вы создали веб-службу в Студии машинного обучения, то образец данных берется из данных, с помощью которых вы обучали модель. Если служба создана программным путем, то данные берутся из данных примеров, которые вы предоставили в составе пакета JSON.

