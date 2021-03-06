---
title: "Запуск отработки аварийного восстановления для виртуальных машин Azure в дополнительный регион Azure с помощью службы Azure Site Recovery (предварительная версия)"
description: "Сведения о запуске отработки аварийного восстановления для виртуальных машин Azure в дополнительный регион Azure с помощью службы Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 5bcd3d64714951508d984c17326e845ae4842670
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2017
---
# <a name="run-a-disaster-recovery-drill-for-azure-vms-to-a-secondary-azure-region-preview"></a>Запуск отработки аварийного восстановления для виртуальных машин Azure в дополнительный регион Azure (предварительная версия)

Служба [Azure Site Recovery](site-recovery-overview.md) помогает реализовать стратегию непрерывности бизнес-процессов и аварийного восстановления (BCDR), обеспечивая работоспособность и доступность бизнес-приложений во время запланированных и незапланированных простоев. Site Recovery управляет аварийным восстановлением локальных компьютеров и виртуальных машин Azure, включая операции репликации, отработки отказа и восстановления.

Это руководство описывает, как выполнить отработку аварийного восстановления для виртуальной машины Azure из одного региона Azure в другой с тестовой отработкой отказа. Проверка отработки позволяет проверить стратегию репликации без потери данных и простоя и не влияет на рабочую среду. Из этого руководства вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Проверка соблюдения предварительных требований
> * Запуск тестовой отработки отказа для одной виртуальной машины

## <a name="prerequisites"></a>Предварительные требования

- Перед запуском тестовой отработки отказа рекомендуется проверить свойства виртуальной машины и подтвердить правильность указанных значений.  Свойства виртуальной машины доступны в разделе **Реплицированные элементы**. В колонке **Основные компоненты** отображается информация о параметрах и состоянии компьютеров.
- Мы рекомендуем использовать для тестовой отработки отказа отдельную сеть виртуальных машин Azure, а не сеть по умолчанию, настроенную при включении репликации.


## <a name="run-a-test-failover"></a>Запуск тестовой отработки отказа

1. В разделе **Параметры** > **Реплицированные элементы** щелкните значок **+Тестовая отработка отказа** для нужной виртуальной машины.

2. На странице **Тестовая отработка отказа** выберите точку восстановления:

   - **Последняя обработанная**. Отработка отказа выполняется до последней точки восстановления виртуальной машины, которая была обработана службой Site Recovery. Для нее отображается метка времени. Этот вариант не требует времени на обработку данных, обеспечивая низкий показатель целевого времени восстановления.
   - **Последняя точка во времени согласованности приложений**. Отработка отказа всех виртуальных машин выполняется до последней точки восстановления с согласованным состоянием приложений. Для нее отображается метка времени.
   - **Пользовательская**. Вы можете выбрать любую точку восстановления.

3. Теперь выберите целевую виртуальную сеть из дополнительного региона Azure, к которой будут подключаться виртуальные машины Azure после отработки отказа.

4. Нажмите кнопку **ОК**, чтобы начать отработку отказа. За ходом выполнения можно следить в окне свойств, которое можно открыть, щелкнув виртуальную машину. Также можно щелкнуть задание **Тестовая отработка отказа** в разделе "Имя хранилища" > **Параметры** > **Задания** > **Задания Site Recovery**.
5. Когда отработка отказа завершится, на вкладке **Виртуальные машины** портала Azure появится реплика виртуальной машины. Убедитесь, что виртуальная машина работает, имеет соответствующий размер и подключена к соответствующей сети.
6. Чтобы удалить виртуальные машины, созданные во время тестовой отработки отказа, щелкните **Cleanup test failover** (Очистить тестовою отработку) в реплицируемом элементе или в плане восстановления. В разделе **Примечания** можно записать и сохранить любые замечания, связанные с тестовой отработкой отказа.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Запуск отработки отказа в рабочей среде](azure-to-azure-tutorial-failover-failback.md)
