---
title: "Развертывание решения для удаленного мониторинга в Azure | Документация Майкрософт"
description: "В этом руководстве показано, как подготовить предварительно настроенное решение для удаленного мониторинга на сайте azureiotsuite.com."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 61d36113e60c6bb02ea053493ae461993b18c447
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Развертывание предварительно настроенного решения для удаленного мониторинга

В этом руководстве показано, как подготовить предварительно настроенное решение для удаленного мониторинга. Решение развертывается на сайте azureiotsuite.com. Его также можно развернуть с помощью интерфейса командной строки. Дополнительные сведения об этом варианте см. в разделе о [развертывании предварительно настроенного решения из командной строки](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line).

Из этого руководства вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Настройка предварительно настроенного решения.
> * Развертывание предварительно настроенного решения.
> * Вход в предварительно настроенное решение.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством требуется активная подписка Azure.

Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-preconfigured-solution"></a>Развертывание предварительно настроенного решения.

Прежде чем развернуть предварительно настроенное решение в подписке Azure, необходимо выбрать некоторые параметры конфигурации:

1. Войдите на сайт [azureiotsuite.com](https://www.azureiotsuite.com) с помощью учетных данных Azure и щелкните **+**, чтобы создать новое решение:

    ![Создание решения](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. На плитке **Remote monitoring preview** (Удаленный мониторинг (предварительная версия)) щелкните **Выбрать**.

    ![Выбор удаленного мониторинга](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. На странице **Create Remote Monitoring solution** (Создание решения для удаленного мониторинга) укажите **имя** предварительно настроенного решения для удаленного мониторинга.

1. Выберите **базовое** или **стандартное** развертывание. Если вы развертываете решение, чтобы понять, как оно работает, или для запуска демонстрации, выберите параметр **Базовый**, чтобы свести к минимуму затраты.

1. Выберите **Java** или **.NET** в качестве языка. Все микрослужбы доступны для реализаций .NET или Java.

1. Дополнительные сведения о вариантах конфигурации см. на панели **Solution details** (Сведения о решении).

1. Выберите **регион** и **подписку**, которые вы хотите использовать для подготовки решения.

1. Щелкните **Создать решение** , чтобы начать процесс подготовки. Этот процесс обычно занимает несколько минут:

    ![Сведения о решении для удаленного мониторинга](media/iot-suite-remote-monitoring-deploy/createform.png)

Сведения об устранении неполадок см. в разделе [What to do when a deployment fails](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) (Действия в случае сбоя развертывания) в репозитории GitHub.

## <a name="sign-in-to-the-preconfigured-solution"></a>Вход в предварительно настроенное решение

После подготовки вы можете войти в предварительно настроенное решение для удаленного мониторинга.

1. На странице **Подготовленные решения** выберите новое решение для удаленного мониторинга:

    ![Выбор нового решения](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. Вы можете просмотреть сведения о решении для удаленного мониторинга на отобразившейся панели. Чтобы подключиться к решению для удаленного мониторинга, выберите **Панель мониторинга решений**.

    > [!NOTE]
    > Решение для удаленного мониторинга можно удалить на этой панели после завершения работы с ним.

    ![Панель решения](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Панель решения для удаленного мониторинга отображается в браузере.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * Настройка предварительно настроенного решения.
> * Развертывание предварительно настроенного решения.
> * Вход в предварительно настроенное решение.

Теперь, когда вы развернули решение для удаленного мониторинга, [изучите возможности панели мониторинга решения](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->