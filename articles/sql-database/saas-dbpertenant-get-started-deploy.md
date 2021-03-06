---
title: "Руководство по мультитенантному приложению SaaS, использующему базу данных SQL Azure | Документация Майкрософт"
description: "Разверните и изучите мультитенантное приложение SaaS Wingtip Tickets, которое демонстрирует создание отдельной базы данных для каждого клиента и другие схемы работы с приложениями SaaS и базами данных SQL Azure."
keywords: "руководство по базе данных sql"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: sstein
ms.openlocfilehash: 9b1ae219eb1278b818e3e1d4237d04fe54c980ec
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Развертывание и изучение мультитенантного приложения SaaS на основе базы данных SQL Azure, в котором используется отдельная база данных для каждого клиента

В этом руководстве вы развернете и изучите приложение SaaS Wingtip Tickets с отдельной базой для каждого клиента. В этом приложении SaaS сохраняются данные нескольких клиентов, и для каждого клиента создается отдельная база данных. Приложение разработано для демонстрации возможностей базы данных SQL Azure, которые упрощают работу со сценариями SaaS.

Если вы нажмете кнопку *Развертывания в Azure* ниже, то через пять минут вы получите облачное мультитенантное приложение SaaS, использующее базу данных SQL. Приложение развертывается с помощью трех примеров клиентов, каждый из которых содержит свою базу данных, развернутую в эластичный пул SQL. Приложение развертывается в подписку Azure, предоставляя полный доступ для изучения отдельных компонентов приложения и работы с ними. Исходный код приложения и скрипты управления доступны в репозитории GitHub WingtipTicketsSaaS-DbPerTenant.


Из этого руководства вы узнаете следующее:

> [!div class="checklist"]

> * как развернуть и изучить мультитенантное приложение SaaS Wingtip;
> * где можно получить исходный код приложения и скрипты управления;
> * О серверах, пулах и базах данных, формирующих приложение.
> * о сопоставлении клиентов с данными с помощью *каталога*;
> * как подготовить нового клиент;
> * как выполнить мониторинг активности клиента в приложении.

Чтобы изучить различные модели проектирования и управления SaaS, вы можете ознакомиться с доступной [серией связанных руководств](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials), которые являются продолжением этого руководства, необходимого для выполнения первоначального развертывания. Изучая руководства, постарайтесь разобраться с приведенными сценариями, а также понять способ реализации различных моделей SaaS. Знакомство со сценариями, представленными в каждом руководстве, необходимо, чтобы разобраться с реализацией множества возможностей базы данных SQL, упрощающих разработку приложений SaaS.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством выполните следующие предварительные требования:

* Установите Azure PowerShell. Дополнительные сведения см. в статье [Начало работы с Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Развертывание приложения SaaS Wingtip Tickets

Разверните приложения, выполнив следующие действия.

1. Нажмите кнопку **Развертывание в Azure**, чтобы открыть на портале Azure шаблон развертывания приложения SaaS Wingtip Tickets с отдельной базой данных для каждого клиента. Для шаблона нужно указать значения двух параметров: имя новой группы ресурсов и имя пользователя, которые будут отличать это развертывание от других развертываний приложения SaaS Wingtip Tickets с отдельной базой данных для каждого клиента. На следующем шаге содержатся сведения о настройке этих значений.

   Запишите точные значения, которые вы используете, так как их нужно будет ввести в файле конфигурации позже.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Введите необходимые значения параметров для развертывания.

    > [!IMPORTANT]
    > Некоторые проверки подлинности и брандмауэры сервера не защищены специально, так как предназначены для демонстрации. **Создайте новую группу ресурсов**. Не используйте существующие группы ресурсов, серверы или пулы. Не используйте в рабочей среде это приложение, его скрипты или созданные им ресурсы. Поработав с приложением, удалите эту группу ресурсов, чтобы завершить связанное выставление счетов.

    * **Группа ресурсов** — щелкните **Создать** и укажите **имя** новой группы ресурсов. 
    * **Расположение** — выберите **Расположение** из раскрывающегося списка.
    * **Пользователь** — для некоторых ресурсов требуются глобально уникальные имена. Чтобы гарантировать уникальность имени, при каждом развертывании приложения указывайте значение, по которому созданные ресурсы будут отличаться от ресурсов, созданных при других развертываниях приложения Wingtip. Мы рекомендуем создать короткое имя **пользователя**, например объединив инициалы и произвольное число (*af1*), а затем добавить это значение к имени группы ресурсов (например, *wingtip-af1*). Параметр **Пользователь** может содержать только буквы, цифры и дефисы (без пробелов). В качестве первого и последнего символа используйте букву или цифру (желательно символы нижнего регистра).


1. **Разверните приложение.**

    * Установите флажок, чтобы принять условия.
    * Щелкните **Приобрести**.

1. Отслеживайте состояние развертывания. Для этого щелкните **Уведомления** (значок колокольчика справа от поля поиска). Развертывание приложения SaaS Wingtip Tickets занимает примерно пять минут.

   ![Развертывание выполнено](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Загрузка и разблокировка скриптов управления для приложения Wingtip Tickets

Пока выполняется развертывание приложения, загрузите исходный код и скрипты управления.

> [!IMPORTANT]
> Исполняемое содержимое (скрипты, библиотеки DLL) может быть заблокировано Windows, в то время как ZIP-файлы можно загрузить с внешнего источника, а затем извлечь их содержимое. При извлечении скриптов из ZIP-файла выполните указанные ниже действия, чтобы разблокировать ZIP-файл перед извлечением. После этого скрипты можно выполнять.

1. Перейдите к [репозиторию GitHub WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
1. Выберите **Clone or download** (Клонировать или скачать).
1. Выберите **Download ZIP** (Загрузить ZIP) и сохраните файл.
1. Щелкните правой кнопкой мыши файл **WingtipTicketsSaaS-DbPerTenant-master.zip** и выберите **Свойства**.
1. На вкладке **Общие** выберите **Разблокировать** и **Применить**.
1. Нажмите кнопку **ОК**.
1. Извлеките файлы.

Скрипты находятся в папке *..\\WingtipTicketsSaaS-DbPerTenant-master\\Learning Modules*.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Обновление файла конфигурации пользователя для развертывания

Прежде чем запускать любые скрипты, настройте значения *группы ресурсов* и *пользователя* в файле **UserConfig.psm1**. Задайте для этих переменных значения, указанные во время развертывания.

1. В *интегрированной среде сценариев PowerShell* откройте файл ...\\Learning Modules\\*UserConfig.psm1*. 
1. В полях *ResourceGroupName* и *Name* введите специфические для развертывания значения (только в строках 10 и 11).
1. Сохраните изменения.

Эти значения используются почти во всех скриптах.

## <a name="run-the-application"></a>Выполнение приложения

Это приложение предназначено для объектов, где проводятся культурные мероприятия, таких как концертные залы, джазовые и спортивные клубы. Объекты регистрируются в Wingtip Tickets как клиенты, что дает им удобную возможность анонсировать события и продавать билеты. Каждому объекту присваивается персонализированный веб-сайт для управления анонсами событий и продажи билетов. Каждый клиент действует самостоятельно и изолированно от других клиентов. На самом деле каждый клиент получает базу данных SQL, развернутую в эластичный пул SQL.

Центральная страница **Концентратор событий** содержит список клиентов в вашем развертывании со ссылками.

1. Откройте _концентратор событий_ в веб-браузере по адресу: http://events.wingtip-dpt.&lt;ПОЛЬЗОВАТЕЛЬ&gt;.trafficmanager.net. Строку &lt;Пользователь&gt; замените именем пользователя для конкретного развертывания.

    ![Концентратор событий](media/saas-dbpertenant-get-started-deploy/events-hub.png)

1. Щелкните **Fabrikam Jazz Club** в *концентраторе событий*.

   ![События](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)


Чтобы управлять распределением входящего трафика, приложение использует [*диспетчер трафика Azure*](../traffic-manager/traffic-manager-overview.md). Для страниц событий для определенного клиента необходимо, чтобы имена клиентов были включены в URL-адреса. Все URL-адреса клиентов содержат одинаковое значение *Пользователь* и имеют следующий формат: http://events.wingtipp-dpt.&lt;ПОЛЬЗОВАТЕЛЬ&gt;.trafficmanager.net/*fabrikamjazzclub*. Приложение событий анализирует имя клиента из URL-адреса и создает на его основе ключ для доступа к каталогу, в котором используется [*управления размещением сегментов*](sql-database-elastic-scale-shard-map-management.md). Каталог сопоставляет ключ с расположением базы данных клиента. **Концентратор событий** использует расширенные метаданные в каталоге, чтобы получить имя клиента, связанное с каждой базой данных, чтобы предоставить список URL-адресов.

В рабочей среде обычно создают запись CNAME DNS, чтобы [*указать интернет-домен компании*](../traffic-manager/traffic-manager-point-internet-domain.md) для профиля диспетчера трафика.

## <a name="start-generating-load-on-the-tenant-databases"></a>Запуск создания нагрузки в базах данных клиента

Теперь после развертывания приложение готово к работе. Скрипт *Demo-LoadGenerator* PowerShell запускает рабочую нагрузку, выполняющуюся во всех клиентских базах данных. Реальная нагрузка в приложении SaaS обычно случайна и непредсказуема. Для имитации этого типа нагрузки генератор создает нагрузку, распределенную по всем клиентам, со случайными пиковыми нагрузками в каждом клиенте, происходящими со случайными интервалами. По этой причине шаблон нагрузки появляется в течение нескольких минут, поэтому перед мониторингом нагрузки генератор должен работать по крайней мере три или четыре минуты.

1. В *интегрированной среде сценариев PowerShell* откройте скрипт ...\\Learning Modules\\Utilities\\*Demo-LoadGenerator.ps1*.
1. Нажмите клавишу **F5**, чтобы запустить скрипт и генератор нагрузки (оставьте значения параметра по умолчанию).

> [!IMPORTANT]
> Для этого откройте новое окно интегрированной среды сценариев PowerShell. Генератор нагрузки выполняется как ряд заданий в локальном сеансе PowerShell. Скрипт *Demo-LoadGenerator.ps1* запускает скрипт генератора фактической нагрузки, выполняющийся как задача переднего плана, а также несколько заданий создания фоновой нагрузки. Задание генератора нагрузки вызывается для каждой базы данных, зарегистрированной в каталоге. Задания выполняются в локальном сеансе PowerShell, поэтому при его закрытии все задания останавливаются. Если приостановить работу компьютера, создание нагрузки будет приостановлено и будет возобновлено после включения компьютера.

После того, как генератор нагрузки вызывает задания создания нагрузки для каждого клиента, задача переднего плана остается в состоянии вызова заданий, в котором она запускает дополнительные фоновые задания для любых новых клиентов, которые затем подготавливаются. Вы можете нажать клавиши *CTRL+C* или кнопку *Остановить*, чтобы остановить задачу переднего плана, но имеющиеся фоновые задания будут продолжать создавать нагрузку на каждую базу данных. Если необходимо отслеживать и контролировать фоновые задания, используйте *Get-Job*, *Receive-Job* и *Stop-Job*. Во время выполнения задачи переднего плана нельзя использовать сеанс PowerShell для выполнения других скриптов. Для этого откройте новое окно интегрированной среды сценариев PowerShell.

Если необходимо перезапустить сеанс генератора нагрузки, например, с разными параметрами, можно остановить задачу переднего плана, а затем повторно запустить скрипт *Demo-LoadGenerator.ps1*. При повторном выполнении скрипта *Demo-LoadGenerator.ps1* сначала останавливаются все выполняемые задания, а затем перезапускается генератор, в результате чего запускается новый набор заданий с использованием текущих параметров.

Пока оставьте генератор нагрузки в состоянии вызова задания.


## <a name="provision-a-new-tenant"></a>Подготовка нового клиента

При первоначальном развертывании создаются три примера клиента, но давайте создадим другого клиента, чтобы увидеть, как это влияет на развернутое приложение. Рабочий процесс подготовки клиентов для приложения SaaS Wingtip Tickets подробно описан в [руководстве по подготовке и каталогам](saas-dbpertenant-provision-and-catalog.md). На этом шаге вы сможете быстро создать клиент.

1. В *интегрированной среде сценариев PowerShell* откройте файл ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1*.
1. Нажмите клавишу **F5** для запуска скрипта (оставьте значения по умолчанию).

   > [!NOTE]
   > Во многих скриптах SaaS Wingtip Tickets переменная *$PSScriptRoot* используется для перехода по папкам и вызова функций из других скриптов. Эта переменная вычисляется только при выполнении полного скрипта путем нажатия клавиши **F5**.  Если выделить несколько скриптов и запустить их (**F8**), может произойти ошибка, поэтому нажмите клавишу **F5** при выполнении скриптов.

Новая база данных клиента создается в эластичном пуле SQL, инициализируется и регистрируется в каталоге. Когда подготовка успешно завершится, в браузере откроется созданный сайт *События* для нового клиента.

![Новый клиент](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Обновите *концентратор событий*, чтобы увидеть в списке новый клиент.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Изучение серверов, пулов и клиентских баз данных

Теперь, когда вы запустили выполнение нагрузки в коллекции клиентов, давайте рассмотрим некоторые из развернутых ресурсов.

1. На [портале Azure](http://portal.azure.com) перейдите к списку серверов SQL Server и откройте сервер **catalog-dpt-&lt;пользователь&gt;**. Этот сервер каталога содержит две базы данных: **tenantcatalog** и **basetenantdb**. Вторая из них является шаблоном базы данных, который копируется для создания новых клиентов.

   ![databases](./media/saas-dbpertenant-get-started-deploy/databases.png)

1. Вернитесь к списку серверов SQL Server и откройте сервер **tenants1-dpt-&lt;пользователь&gt;**, который содержит клиентские базы данных. Каждая клиентская база данных является эластичной базой данных _уровня "Стандартный"_ в пуле уровня "Стандартный" на 50 eDTU. Кроме того, обратите внимание, что существует база данных _Red Maple Racing_ (подготовленная ранее клиентская база данных).

   ![server](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Отслеживание пула

Если генератор нагрузки выполнялся несколько минут, тогда доступен достаточный объем данных, чтобы рассмотреть некоторые функции мониторинга, встроенные в пулы и базы данных.

1. Перейдите к серверу **tenants1-dpt-&lt;пользователь&gt;**и щелкните **Pool1**, чтобы просмотреть сведения об использовании ресурсов пула (графики ниже созданы по результатам работы генератора нагрузки в течение часа).

   ![Отслеживание пула](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

Верхняя диаграмма демонстрирует использование eDTU для пула в целом, а на нижней представлено использование eDTU для 5 наиболее загруженных баз данных в пуле.  На этих диаграммах хорошо видно, насколько хорошо подходят эластичные пулы и база данных SQL для рабочих нагрузок приложения SaaS. Четыре базы данных, каждая из которых передает 40 eDTU, легко поддерживаются пулом, рассчитанным на 50 eDTU. Если они были подготовлены как автономные базы данных, каждая из них должна быть уровня S2 (50 DTU), чтобы поддерживать пиковые нагрузки. Стоимость 4 автономных баз данных уровня S2 в 3 раза превышала бы стоимость пула, а для пула по-прежнему предусмотрен большой резерв, так что он может вместить гораздо большее число баз данных. На самом деле клиенты базы данных SQL используют до 500 баз данных в пулах на 200 eDTU. Дополнительные сведения см. в статье об [отслеживании производительности примера приложения SaaS для WTP](saas-dbpertenant-performance-monitoring.md).


## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали следующее:

> [!div class="checklist"]

> * Развертывание приложения SaaS Wingtip Tickets
> * О серверах, пулах и базах данных, формирующих приложение.
> * О клиентах, сопоставленных с данными с помощью *каталога*.
> * Как подготовить новые клиенты.
> * Как просмотреть историю использования пула для отслеживания работы клиента.
> * Как удалить примеры ресурсов, чтобы прекратить связанное выставление счетов.

А теперь рекомендуем ознакомиться со статьей [Provision new tenants and register them in the catalog](saas-dbpertenant-provision-and-catalog.md) (Подготовка новых клиентов и их регистрация в каталоге).



## <a name="additional-resources"></a>Дополнительные ресурсы

* Дополнительные [руководства на основе приложения SaaS Wingtip Tickets с отдельной базой данных для каждого клиента](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
* Дополнительные сведения об эластичных пулах см. в статье [*Эластичные пулы помогают управлять несколькими базами данных SQL и масштабировать их*](sql-database-elastic-pool.md).
* Дополнительные сведения об эластичных заданиях см. в статье [*Управление масштабируемыми облачными базами данных*](sql-database-elastic-jobs-overview.md).
* Дополнительные сведения о шаблонах разработки для мультитенантных приложений SaaS и базы данных SQL Azure см. в [*этой статье*](saas-tenancy-app-design-patterns.md).
