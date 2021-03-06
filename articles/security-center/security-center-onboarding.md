---
title: "Подключение к центру безопасности Azure уровня \"Стандартный\"для повышения уровня безопасности | Документация Майкрософт"
description: " Узнайте, как выполнить подключение к центру безопасности Azure уровня \"Стандартный\" для повышения уровня безопасности. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: a6394b1b02ebfe518dc2f2b7f65eb677bb0ba5f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Подключение к центру безопасности Azure уровня "Стандартный" для повышения уровня безопасности
Обновите центр безопасности до уровня "Стандартный", чтобы воспользоваться преимуществами управления повышенной безопасностью и защиты от угроз для гибридных облачных рабочих нагрузок.  Пробную версию уровня "Стандартный" можно использовать бесплатно в течение 60 дней. Дополнительные сведения см. на странице [цен на центр безопасности](https://azure.microsoft.com/pricing/details/security-center/).

Центр безопасности уровня "Стандартный" включает в себя следующее:

- **Гибридная защита.** Получите комплексный обзор безопасности всех локальных и облачных рабочих нагрузок. Применяйте политики безопасности и непрерывно проверяйте уровень безопасности гибридных облачных рабочих нагрузок, чтобы обеспечить соответствие стандартам безопасности. Выполняйте сбор, поиск и анализ данных о безопасности из разных источников, включая брандмауэры и другие партнерские решения.
- **Расширенное обнаружение угроз.** Предотвращайте прогрессивные кибератаки, используя возможности расширенной аналитики и Microsoft Intelligent Security Graph.  Определяйте атаки и уязвимости нулевого дня с помощью встроенного анализа поведения и машинного обучения. Обнаруживайте входящие атаки и отслеживайте дальнейшую работу систем за счет мониторинга сетей, компьютеров и облачных служб. Ускорьте анализ с помощью интерактивных инструментов и контекстной аналитики угроз.
- **Элементы управления доступом и приложениями.** Блокируйте вредоносные и другие нежелательные программы, используя рекомендации списка разрешений с поддержкой машинного обучения, адаптированных к определенным рабочим нагрузкам. Уменьшите количество сетевых атак с помощью управляемого JIT-доступа к портам управления на виртуальных машинах Azure, чтобы значительно сократить вероятность атак методом подбора и других сетевых атак.

## <a name="detecting-unprotected-resources"></a>Обнаружение незащищенных ресурсов     
Центр безопасности автоматически обнаруживает подписки и рабочие области Azure, не включенные в центр безопасности уровня "Стандартный". Сюда входят подписки Azure, использующие версию центра безопасности уровня "Бесплатный" и рабочие нагрузки без включенного решения безопасности.

Вы можете полностью обновить подписку Azure до уровня "Стандартный", который наследуется всеми ресурсами в подписке, или определить уникальную политику, чтобы обновить только определенную группу ресурсов. Если параметры политики группы ресурсов уникальны, центр безопасности не будет переопределять политики цен при обновлении подписки до уровня "Стандартный". Уровень "Стандартный" применяется только к виртуальным машинам в подписках, которые отправляют отчеты рабочим областям, созданных центром безопасности. Если применить уровень "Стандартный" к рабочей области, он также будет применяться ко всем ресурсам, отправляющим отчеты рабочим областям.

> [!NOTE]
> Вам может потребоваться выполнить управление затратами и ограничить объем данных, собираемых для решения, ограничив его конкретным набором агентов. [Нацеливание решений](../operations-management-suite/operations-management-suite-solution-targeting.md) позволяет применять область к решениям и выполнять нацеливание подмножества компьютеров в рабочей области.  Если используется нацеливание решений, центр безопасности отображает рабочую область как область без решения.
>
>

## <a name="upgrade-an-azure-subscription"></a>Обновление подписки Azure
Чтобы обновить все подписки до уровня "Стандартный", сделайте следующее:
1. В главном меню центра безопасности выберите **Подключение**.
2. В разделе **Переход на улучшенную безопасность** в центре безопасности появится список подписок, доступных для подключения. Вы можете обновить все перечисленные подписки, выбрав **Применить план "Стандартный"**.

  ![Обновление всех подписок][1]

Чтобы обновить отдельную подписку до уровня "Стандартный", выберите раздел **Подключение**, щелкнув **Применить категорию "Стандартный"**. Чтобы обновить группу ресурсов в подписке уровня "Стандартный", выберите подписку:
1. Выберите подписку.  **Политика безопасности** предоставляет сведения о группе ресурсов, содержащейся в подписке.
2. Выберите подписку или группу ресурсов.

  ![Обновление всех подписок][2]

3. Выберите **Стандартный**, чтобы выполнить обновление с уровня "Бесплатный" до уровня "Стандартный".
4. Щелкните **Сохранить**.

> [!NOTE]
> При обновлении подписки до уровня "Стандартный" будет включена [автоматическая подготовка](security-center-enable-data-collection.md) (если она была отключена ранее). Мы рекомендуем выполнить автоматическую подготовку агентов мониторинга.
>
>

## <a name="upgrade-a-workspace"></a>Обновление рабочей области
Если применить уровень "Стандартный" к рабочей области, он также будет применяться ко всем ресурсам, отправляющим отчеты рабочим областям.

1. Вернитесь к колонке **Подключение**.
2. Выберите рабочую область.

  ![Обновление рабочей области][8]

3. Выберите **Стандартный** для обновления.  
4. Щелкните **Сохранить**.

   > [!NOTE]
   > Возможна ситуация, когда к рабочей области не будет применяться уровень "Бесплатный" или "Стандартный". Если вы выбрали уровень "Бесплатный", возможности центра безопасности уровня "Бесплатный" будут применяться только к виртуальным машинам Azure. Возможности уровня "Бесплатный" не применяются к компьютерам, не относящимся к Azure. Если вы выбрали уровень "Стандартный", возможности этого уровня будут применяться ко всем виртуальным машинам Azure и компьютерам, не относящимся к Azure, которые отправляют отчеты рабочим областям. Мы рекомендуем применять уровень "Стандартный", чтобы обеспечить расширенную безопасность для ресурсов Azure и сторонних ресурсов.
   >
   >

## <a name="onboard-non-azure-computers"></a>Подключение компьютеров, которые не относятся к Azure
Центр безопасности может отслеживать состояние безопасности компьютеров, не относящихся к Azure. Однако сначала необходимо подключить эти ресурсы. Компьютеры, не относящиеся к Azure, можно добавить из колонки **Подключение** или **Вычисления**. Мы рассмотрим оба метода.

### <a name="add-new-non-azure-computers-from-onboarding"></a>Добавление новых компьютеров, не относящихся к Azure, из колонки подключения

1. Вернитесь к колонке **Подключение**.   
2. Выберите **Do you want to add new non-Azure computers** (Добавить новые компьютеры, не относящиеся к Azure?).

  ![Добавление компьютера, который не относится к Azure][3]

Имеющиеся рабочие области отображаются в разделе **Add new Non-Azure computers** (Добавление новых компьютеров, не относящихся к Azure). Вы можете создать рабочую область или добавить компьютеры в имеющуюся. Чтобы создать рабочую область, щелкните ссылку для **добавления новой рабочей области**.

Мы рассмотрим оба метода:

- Создание рабочей области и добавление компьютера.
- Выбор имеющейся рабочей области и добавление компьютера.

**Создание рабочей области и добавление компьютера**

1. В разделе **Add new non-Azure computers** (Добавление новых компьютеров, не относящихся к Azure) щелкните ссылку для **добавления новой рабочей области**.

   ![Добавление новой рабочей области][4]

2. В разделе **Безопасность и аудит** выберите **Рабочая область OMS** для создания рабочей области.
3. В разделе **Рабочая область OMS** введите сведения для рабочей области.
4. В разделе **Рабочая область OMS** нажмите кнопку **ОК**.  После этого вы получите ссылку для загрузки агента и ключей Windows или Linux для идентификатора рабочей области, используемого при настройке агента.
5. В разделе **Безопасность и аудит** нажмите кнопку **ОК**.

**Выбор имеющейся рабочей области и добавление компьютера**

Вы можете добавить компьютер, выполнив рабочий процесс из колонки **подключений**, как показано выше, или из колонки **вычислений**. В этом примере мы используем колонку **вычислений**.

1. Вернитесь в главное меню центра безопасности и к **обзорной** панели мониторинга.

   ![Обзор][5]

2. Выберите плитку **Вычисления**.
3. В разделе **Вычисления** выберите **Добавить компьютеры**.

   ![Колонка вычислений][6]

4. В разделе **Add new non-Azure computers** (Добавление новых компьютеров, не относящихся к Azure) выберите рабочую область, к которой необходимо подключить компьютер, и нажмите кнопку **Добавить компьютеры**.

   ![Добавление компьютеров][7]

 Колонка **Прямой агент** содержит ссылку для загрузки агента и ключей Windows или Linux для идентификатора рабочей области, используемого при настройке агента.   

## <a name="next-steps"></a>Дальнейшие действия
Из этой статьи вы узнали, как подключить ресурсы Azure и сторонние ресурсы, чтобы получить преимущества расширенной безопасности центра безопасности.  Чтобы расширить возможности работы с подключаемыми ресурсами, ознакомьтесь со следующими статьями:

- [Включение сбора данных](security-center-enable-data-collection.md)
- [Отчет об исследовании угроз](security-center-threat-report.md)
- [Управление доступом к виртуальным машинам с помощью JIT](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/add-non-azure-resource.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-non-azure-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png
