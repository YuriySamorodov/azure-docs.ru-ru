---
title: "Пользовательский интерфейс диспетчера данных Microsoft Azure StorSimple | Документация Майкрософт"
description: "Описывает использование пользовательского интерфейса для службы диспетчера данных StorSimple (закрытая предварительная версия)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>Использование пользовательского интерфейса для службы диспетчера данных StorSimple (закрытая предварительная версия)

В этой статье объясняется, как использовать пользовательский интерфейс диспетчера данных StorSimple для преобразования данных, хранящихся на устройствах серии StorSimple 8000. Преобразованные данные можно затем использовать в других службах Azure, таких как службы мультимедиа Azure, Azure HDInsight, машинное обучение Azure и Поиск Azure. 


## <a name="use-storsimple-data-transformation"></a>Использование преобразования данных StorSimple

Диспетчер данных StorSimple — это ресурс, в котором можно создавать экземпляры преобразования данных. Служба преобразования данных позволяет перемещать данные из локального устройства StorSimple в большие двоичные объекты в хранилище Azure. В рабочем процессе необходимо указать сведения о вашем устройстве StorSimple и конкретных данных, которые нужно переместить в учетную запись хранения.

### <a name="create-a-storsimple-data-manager-service"></a>Создание новой службы диспетчера данных StorSimple

Для создания нового экземпляра службы диспетчера данных StorSimple выполните следующие действия.

1. Чтобы создать службу диспетчера данных StorSimple, перейдите на страницу [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager)

2. Щелкните значок **+** и выполните поиск по строке "StorSimple Data Manager" (Диспетчер данных StorSimple). Выберите нужную службу диспетчера данных StorSimple и нажмите кнопку **Создать**.

3. Если для вашей подписки разрешено создание такой службы, вы увидите колонку следующего вида.

    ![Создание ресурса диспетчера данных StorSimple](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Введите входные данные и нажмите кнопку **Создать**. Нужно выбрать расположение, в котором находятся учетные записи хранения и служба диспетчера StorSimple. В настоящее время поддерживаются только регионы "Западная часть США" и "Западная Европа". Поэтому именно в одном из этих регионов нужно разместить службу диспетчера StorSimple, службу диспетчера данных и соответствующую учетную запись хранения. Создание службы занимает около минуты.

### <a name="create-a-data-transformation-job-definition"></a>Создание определения задания для преобразования данных

В службе диспетчера данных StorSimple нужно создать определение задания для преобразования данных. Определение задания содержит информацию о том, какие данные вы хотите переместить в учетную запись хранения в собственном формате. 

Выполните следующие шаги, чтобы создать определение задания для преобразования данных.

1.  Перейдите к созданной службе. Щелкните **+ Определение задания**.

    ![Кнопка "+ Определение задания".](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Откроется новая колонка определения задания. Присвойте заданию имя и нажмите кнопку **Источник**. В колонке **Настройка источника данных** укажите сведения об устройстве StorSimple и нужных данных для преобразования.

    ![Создание определения задания](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Так как это новая служба диспетчера данных, репозитории данных еще не настроены. Чтобы добавить диспетчер StorSimple в качестве хранилища данных, щелкните **Добавить новый** в раскрывающемся списке репозиториев данных и нажмите кнопку **Добавить хранилище данных**.

4. Выберите **Диспетчер StorSimple серии 8000** в качестве типа репозитория и введите свойства **диспетчера StorSimple**. В поле **идентификатор ресурса** следует ввести номер, который указан до символа **:** в ключе регистрации для диспетчера StorSimple.

    ![Создание источника данных](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Когда закончите, нажмите кнопку **ОК**. Репозиторий данных будет сохранен, и позднее этот диспетчер StorSimple можно будет использовать в других определениях заданий, эти параметры не нужно будет вводить повторно. Диспетчер StorSimple появится в раскрывающемся списке через несколько секунд после нажатия кнопки **ОК**.

6.  В колонке **Настройка источника данных** введите имя устройства и имя тома, где хранятся нужные данные.

7.  В подразделе **Фильтр** введите корневой каталог, содержащий нужные данные (значение в этом поле должно начинаться с символа `\`). Здесь можно также добавить фильтры файлов.

8.  Служба преобразования данных работает с данными, которые передаются в Azure через моментальные снимки. Для этого задания вы можете указать, нужно ли создавать резервные копии при каждом выполнении (чтобы работать с новейшими данными) или использовать последнюю существующую в облаке резервную копию (если работа ведется с архивными данными).

    ![Сведения о новом источнике данных](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Далее нужно настроить параметры целевого расположения. Поддерживаются два типа целевых объектов —учетные записи хранения Azure и учетные записи служб мультимедиа Azure. Выберите учетную запись хранения, чтобы поместить файлы в большие двоичные объекты в этой учетной записи. Выберите учетную запись службы мультимедиа, чтобы поместить файлы в активы в этой учетной записи. И нужно еще раз добавить репозиторий. В раскрывающемся списке выберите **Добавить новый** и затем **Настройки**.

    ![Создание приемника данных](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Здесь можно выбрать тип репозитория, который вы хотите добавить, и другие параметры этого репозитория. В обоих случаях при запуске задания создается очередь хранилища. Эта очередь по мере готовности заполняется сообщениями о преобразованных больших двоичных объектах. Имя этой очереди совпадает с именем определения задания. Если вы выбрали в качестве типа репозитория **службы мультимедиа**, то можете дополнительно указать учетные данные хранилища, в котором создается очередь.

    ![Сведения о новом приемнике данных](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. После добавления репозитория данных (этот процесс займет несколько секунд) вы увидите новый репозиторий в раскрывающемся списке **Target account name** (Конечная учетная запись).  Выберите нужный целевой объект.

12. Щелкните **OK**, чтобы создать определение задания. Определение задания готово. Вы можете неоднократно использовать это определение задания в пользовательском интерфейсе.

    ![Добавление определения задания](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>Запуск определения задания

Определение задания нужно запускать каждый раз, когда потребуется переместить данные из StorSimple в учетную запись хранения, указанную в этом определении. При каждом запуске задания вы можете изменять некоторые его параметры. Для этого необходимо выполнить следующие шаги:

1. Выберите нужную службу диспетчера данных StorSimple и перейдите в раздел **Мониторинг**. Щелкните **Запустить сейчас**.

    ![Запуск определения задания](./media/storsimple-data-manager-ui/run-now.png)

2. Выберите определение задания, которое требуется запустить. Щелкните **Параметры запуска**, чтобы изменить параметры для конкретного запуска этого задания.

    ![Параметры выполнения задания](./media/storsimple-data-manager-ui/run-settings.png)

3. Щелкните **ОК** и нажмите кнопку **Запустить**, чтобы выполнить задание. На странице **Задания** в диспетчере данных StorSimple можно отслеживать выполнение задания.

    ![Список заданий и состояние](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. Помимо мониторинга заданий в колонке **Задания**, вы можете отслеживать сообщения в очереди хранилища, которые добавляются каждый раз при перемещении файла из StorSimple в учетную запись хранения.


## <a name="next-steps"></a>Дальнейшие действия

[Использование .NET SDK для запуска заданий диспетчера данных StorSimple](storsimple-data-manager-dotnet-jobs.md).