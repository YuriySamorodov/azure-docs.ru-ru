---
title: "Приступая к работе с Azure Automation DSC | Документация Майкрософт"
description: "Описание и примеры самых распространенных задач по настройке требуемого состояния службы автоматизации Azure"
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: a3816593-70a3-403b-9a43-d5555fd2cee2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/21/2016
ms.author: magoedte;eslesar
ms.openlocfilehash: 8a10d961ad7c107c68b57c64ee6c88544ff8832b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-azure-automation-dsc"></a>Приступая к работе с DSC службы автоматизации Azure
В этом разделе объясняется, как выполнять самые распространенные задачи с помощью Desired State Configuration (DSC) службы автоматизации Azure, например создание, импорт и компилирование конфигураций, подключение компьютеров для управления, а также просмотр отчетов. Общие сведения о DSC службы автоматизации Azure см. в [этой статье](automation-dsc-overview.md). Документацию по DSC см. в статье [Общие сведения о службе настройки требуемого состояния Windows PowerShell](https://msdn.microsoft.com/PowerShell/dsc/overview).

В этом разделе содержатся пошаговые инструкции по использованию DSC службы автоматизации Azure. Если необходим пример среды, настроенной без выполнения действий, описанных в этом разделе, можно использовать [следующий шаблон ARM](https://github.com/azureautomation/automation-packs/tree/master/102-sample-automation-setup). Этот шаблон позволяет настроить полнофункциональную среду DSC службы автоматизации Azure, в том числе виртуальную машину Azure, которой управляет DSC соответствующей службы.

## <a name="prerequisites"></a>Предварительные требования
Для выполнения примеров в этом разделе необходимо следующее.

* Учетная запись службы автоматизации Azure. Указания по созданию учетной записи запуска от имени пользователя для службы автоматизации Azure см. в статье [Проверка подлинности модулей Runbook в Azure с помощью учетной записи запуска от имени](automation-sec-configure-azure-runas-account.md).
* Виртуальная машина Azure Resource Manager (неклассическая) под управлением Windows Server 2008 R2 или более поздней версии. Инструкции по созданию виртуальной машины см. в статье [Создание первой виртуальной машины Windows на портале Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

## <a name="creating-a-dsc-configuration"></a>Создание конфигурации DSC
Мы создадим простую [конфигурацию DSC](https://msdn.microsoft.com/powershell/dsc/configurations) , которая обеспечивает наличие или отсутствие компонента Windows **веб-сервера** (IIS) в зависимости от назначения узлов.

1. Запустите интегрированную среду сценариев Windows PowerShell (или любой текстовый редактор).
2. Введите следующий текст:
   
    ```powershell
    configuration TestConfig
    {
        Node WebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Present'
                Name                 = 'Web-Server'
                IncludeAllSubFeature = $true
   
            }
        }
   
        Node NotWebServer
        {
            WindowsFeature IIS
            {
                Ensure               = 'Absent'
                Name                 = 'Web-Server'
   
            }
        }
        }
    ```
3. Сохраните файл как `TestConfig.ps1`.

При этой конфигурации в каждом блоке узла вызывается один ресурс, [ресурс WindowsFeature](https://msdn.microsoft.com/powershell/dsc/windowsfeatureresource), что обеспечивает наличие или отсутствие компонента **веб-сервера** .

## <a name="importing-a-configuration-into-azure-automation"></a>Импорт конфигурации в службу автоматизации Azure
Теперь мы импортируем конфигурацию в учетную запись службы автоматизации.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Конфигурации DSC**.
4. В колонке **Конфигурации DSC** щелкните **Добавить конфигурацию**.
5. В колонке **Импорт конфигурации** перейдите к файлу `TestConfig.ps1` на своем компьютере.
   
    ![Снимок экрана: колонка **Импорт конфигурации**](./media/automation-dsc-getting-started/AddConfig.png)
6. Нажмите кнопку **ОК**.

## <a name="viewing-a-configuration-in-azure-automation"></a>Просмотр конфигурации в службе автоматизации Azure
После импорта конфигурацию можно просмотреть на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Конфигурации DSC**.
4. В колонке **Конфигурации DSC** щелкните **TestConfig** (это имя конфигурации, импортированной при выполнении предыдущей процедуры).
5. В колонке **Настройка TestConfig** щелкните **Просмотреть источник конфигурации**.
   
    ![Снимок экрана: колонка "TestConfig configuration" (Конфигурация TestConfig)](./media/automation-dsc-getting-started/ViewConfigSource.png)
   
    Откроется колонка **Источник конфигурации TestConfig** , в которой будет отображаться код PowerShell для конфигурации.

## <a name="compiling-a-configuration-in-azure-automation"></a>Компилирование конфигурации в службе автоматизации Azure
Прежде чем применить требуемое состояние к узлу, конфигурацию DSC, определяющую это состояние, следует компилировать в одну или несколько конфигураций узла (MOF-документы) и разместить на опрашивающем сервере Automation DSC. Подробное описание компилирования конфигураций в Azure Automation DSC см. в [этой статье](automation-dsc-compile.md). Дополнительные сведения о компилировании конфигураций см. в статье [Конфигурации DSC](https://msdn.microsoft.com/PowerShell/DSC/configurations).

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Конфигурации DSC**.
4. В колонке **Конфигурации DSC** щелкните **TestConfig** (имя импортированной ранее конфигурации).
5. В колонке **Настройка TestConfig** щелкните **Скомпилировать**, а затем нажмите кнопку **Да**. Запустится задание компиляции.
   
    ![Снимок экрана: колонка "TestConfig configuration" (Конфигурация TestConfig) с выделенной кнопкой "Скомпилировать"](./media/automation-dsc-getting-started/CompileConfig.png)

> [!NOTE]
> Во время компилирования конфигурации в службе автоматизации Azure MOF-файлы любой созданной конфигурации узла автоматически развертываются на опрашивающем сервере.
> 
> 

## <a name="viewing-a-compilation-job"></a>Просмотр задания компиляции
После запуска компиляции задание можно просмотреть на плитке **Задания компиляции** в колонке **Конфигурация**. На плитке **Задания компиляции** отображаются выполняющиеся, выполненные и невыполненные задания. При открытии колонки задания компиляции отображаются сведения об этом задании, в том числе все обнаруженные ошибки или предупреждения, входные параметры, используемые в конфигурации, и журналы компиляции.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Конфигурации DSC**.
4. В колонке **Конфигурации DSC** щелкните **TestConfig** (имя импортированной ранее конфигурации).
5. На плитке **Задания компиляции** колонки **Настройка TestConfig** щелкните одно из перечисленных заданий. Откроется колонка **Задание компиляции** с датой начала задания компиляции.
   
    ![Снимок экрана: колонка "Задание компиляции"](./media/automation-dsc-getting-started/CompilationJob.png)
6. Щелкните любую плитку в колонке **Задание компиляции** , чтобы просмотреть дополнительные сведения о задании.

## <a name="viewing-node-configurations"></a>Просмотр конфигураций узлов
При успешном выполнении задания компиляции создается одна или несколько конфигураций узлов. Конфигурация узла — это MOF-документ, который развертывается на опрашивающем сервере и готов к извлечению и применению одним или несколькими узлами. Конфигурации узлов можно просмотреть в учетной записи службы автоматизации в колонке **Конфигурации узла DSC** . Имя конфигурации узла имеет следующий формат: *имя_конфигурации*.*имя_узла*.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Конфигурации узла DSC**.
   
    ![Снимок экрана: колонка "Конфигурации узла DSC"](./media/automation-dsc-getting-started/NodeConfigs.png)

## <a name="onboarding-an-azure-vm-for-management-with-azure-automation-dsc"></a>Подключение виртуальной машины Azure для управления с использованием DSC службы автоматизации Azure
DSC службы автоматизации Azure можно применять для управления виртуальными машинами Azure (классическими или Resource Manager), локальными виртуальными машинами, компьютерами Linux, виртуальными машинами AWS и локальными физическими компьютерами. В этом разделе рассматривается подключение только виртуальных машин Azure Resource Manager. Дополнительные сведения о подключении машин других типов см. в статье [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md).

### <a name="to-onboard-an-azure-resource-manager-vm-for-management-by-azure-automation-dsc"></a>Чтобы подключить виртуальную машину Azure Resource Manager для управления с помощью DSC службы автоматизации Azure, сделайте следующее.
1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Узлы DSC**.
4. В колонке **Узлы DSC** щелкните **Добавить виртуальную машину Azure**.
   
    ![Снимок экрана: колонка "Узлы DSC" с выделенной кнопкой "Добавить виртуальную машину Azure"](./media/automation-dsc-getting-started/OnboardVM.png)
5. В колонке**Добавить виртуальные машины Azure** щелкните **Выберите виртуальную машину для присоединения**.
6. В колонке **Выбрать виртуальные машины** выберите нужную виртуальную машину и нажмите кнопку **ОК**.
   
   > [!IMPORTANT]
   > Это должна быть виртуальная машина Azure Resource Manager под управлением Windows Server 2008 R2 или более поздней версии.
   > 
   > 
7. В колонке **Добавить виртуальные машины Azure** щелкните **Настроить данные регистрации**.
8. В колонке **Регистрация** в поле **Имя конфигурации узла** введите имя конфигурации узла, которую необходимо применить к виртуальной машине. Это имя должно совпадать с именем конфигурации узла в учетной записи службы автоматизации. На этом этапе указывать имя необязательно. После подключения узла назначенную конфигурацию узла можно изменить.
   Установите флажок **Перезагрузить узел при необходимости** и нажмите кнопку **ОК**.
   
    ![Снимок экрана: колонка "Регистрация"](./media/automation-dsc-getting-started/RegisterVM.png)
   
    Указанная конфигурация узла будет применяться к виртуальной машине с периодичностью, указанной в поле **Частота обновления режима настройки**. Виртуальная машина будет проверять наличие обновлений для конфигурации узла с периодичностью, указанной в поле **Частота обновления**. Дополнительные сведения об использовании этих значений см. в статье [Настройка локального диспетчера конфигураций](https://msdn.microsoft.com/PowerShell/DSC/metaConfig).
9. В колонке **Добавить виртуальные машины Azure** нажмите кнопку **Создать**.

Azure запустит процесс подключения виртуальной машины. По завершении процесса виртуальная машина будет отображаться в учетной записи службы автоматизации в колонке **Узлы DSC** .

## <a name="viewing-the-list-of-dsc-nodes"></a>Просмотр списка узлов DSC
Список всех виртуальных машин, подключенных для управления, можно просмотреть в учетной записи службы автоматизации в колонке **Узлы DSC** .

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Узлы DSC**.

## <a name="viewing-reports-for-dsc-nodes"></a>Просмотр отчетов для узлов DSC
Каждый раз, когда DSC службы автоматизации Azure проверяет согласованность на управляемом узле, узел отправляет отчет о состоянии на опрашивающий сервер. Вы можете просмотреть отчеты в колонке для соответствующего узла.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Узлы DSC**.
4. На плитке **Отчеты** щелкните один из отчетов в списке.
   
    ![Снимок экрана: колонка отчетов](./media/automation-dsc-getting-started/NodeReport.png)

В колонке для отдельного отчета можно увидеть следующие сведения о состоянии для соответствующей проверки согласованности.

* Состояние отчета. Находится ли узел в состоянии "Совместимый" или "Несовместимый", а конфигурация — в состоянии "Сбой" (когда для узла установлен режим **applyandmonitor**, а виртуальная машина находится не в требуемом состоянии).
* Время начала проверки согласованности.
* Общее время выполнения проверки согласованности.
* Тип проверки согласованности.
* Все ошибки, в том числе код и сообщение ошибки. 
* Все ресурсы DSC, используемые в конфигурации, и состояние каждого ресурса (находится ли узел в требуемом состоянии для этого ресурса). Щелкните каждый ресурс, чтобы получить дополнительные сведения о нем.
* Имя, IP-адрес и режим конфигурации узла.

Кроме того, можно щелкнуть **Просмотреть необработанный отчет**, чтобы просмотреть фактические данные, которые узел отправляет на сервер. Дополнительные сведения об использовании этих данных см. в статье [Использование сервера отчетов DSC](https://msdn.microsoft.com/powershell/dsc/reportserver).

После подключения узла первый отчет станет доступным через некоторое время. Может пройти около 30 минут, прежде чем первый отчет станет доступным после подключения узла.

## <a name="reassigning-a-node-to-a-different-node-configuration"></a>Переназначение узла другой конфигурации
Узел можно назначить для использования конфигурации, которая отличается от назначенной изначально.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Узлы DSC**.
4. В колонке **Узлы DSC** щелкните имя узла, который необходимо переназначить.
5. В колонке этого узла нажмите кнопку **Назначить узел**.
   
    ![Снимок экрана: колонка "Узел" с выделенной кнопкой "Assign Node" (Назначить узел)](./media/automation-dsc-getting-started/AssignNode.png)
6. В колонке **Назначить конфигурацию узла** выберите конфигурацию, которой необходимо назначить узел, и нажмите кнопку **ОК**.
   
    ![Снимок экрана: колонка "Назначить конфигурацию узла"](./media/automation-dsc-getting-started/AssignNodeConfig.png)

## <a name="unregistering-a-node"></a>Отмена регистрации узла
Если больше не требуется управлять узлом с использованием DSC службы автоматизации Azure, можно отменить регистрацию.

1. Войдите на [портал Azure](https://portal.azure.com).
2. В меню концентратора щелкните **Все ресурсы** , а затем — имя учетной записи службы автоматизации.
3. В колонке **Учетная запись службы автоматизации** щелкните **Узлы DSC**.
4. В колонке **Узлы DSC** щелкните имя узла, регистрацию которого нужно отменить.
5. В колонке для этого узла нажмите кнопку **Отменить регистрацию**.
   
    ![Снимок экрана: колонка "Узел" с выделенной кнопкой "Отменить регистрацию"](./media/automation-dsc-getting-started/UnregisterNode.png)

## <a name="related-articles"></a>Связанные статьи
* [Обзор Azure Automation DSC](automation-dsc-overview.md)
* [Подключение компьютеров для управления с помощью Azure Automation DSC](automation-dsc-onboarding.md)
* [Общие сведения о платформе Desired State Configuration Windows PowerShell](https://msdn.microsoft.com/powershell/dsc/overview)
* [Командлеты Automation DSC Azure](/powershell/module/azurerm.automation/#automation)
* [Цены на Automation DSC Azure](https://azure.microsoft.com/pricing/details/automation/)

