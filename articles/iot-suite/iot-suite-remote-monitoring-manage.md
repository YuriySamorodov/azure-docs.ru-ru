---
title: "Управление устройствами с помощью решения удаленного мониторинга в Azure | Документация Майкрософт"
description: "В этом руководстве объясняется, как управлять устройствами с помощью решения удаленного мониторинга."
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
ms.openlocfilehash: 84c2eaaab2dfc09c93fbfeac3fe2bfcc7066a411
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="manage-and-configure-your-devices"></a>Настройка устройств и управление ими

В этом руководстве описаны возможности управления устройством с помощью решения удаленного мониторинга. Для демонстрации этих возможностей в руководстве используется сценарий приложения IoT Contoso.

Компания Contoso заказала новые машины для расширения производства. Пока компания ждет доставки машин, нужно выполнить моделирование, чтобы проверить поведение решения. Как оператору, вам требуется настроить устройства в решении удаленного мониторинга и управлять ими.

Для расширенного управления и настройки устройств в решении удаленного мониторинга используются компоненты Центра Интернета вещей, такие как [задания](../iot-hub/iot-hub-devguide-jobs.md) и [прямые методы](../iot-hub/iot-hub-devguide-direct-methods.md). Чтобы узнать, как разработчик устройств реализует методы на физическом устройстве, см. статью о [настройке предварительно настроенного решения удаленного мониторинга](iot-suite-remote-monitoring-customize.md).

Из этого руководства вы узнаете, как выполнять такие задачи:

>[!div class="checklist"]
> * Подготовка имитированного устройства.
> * Тестирование имитированного устройства.
> * Вызов методов устройства из решения.
> * Перенастройка устройства.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством нужен развернутый экземпляр решения для удаленного мониторинга в подписке Azure.

Если решение удаленного мониторинга еще не развернуто, выполните инструкции из руководства [Deploy the remote monitoring preconfigured solution](iot-suite-remote-monitoring-deploy.md) (Развертывание предварительно настроенного решения удаленного мониторинга).

## <a name="provision-a-simulated-device"></a>Подготовка имитированного устройства

Откройте страницу **Устройства** в решении и выберите панель **Подготовка**. На панели **Подготовка** выберите тип устройства **Simulated** (Имитированное):

![Подготовка имитированного устройства](media/iot-suite-remote-monitoring-manage/devicesprovision.png)

Оставьте значение **1** для количества подготавливаемых устройств. Выберите значение **Модуль** для параметра **Модель устройства** и нажмите кнопку **Применить**, чтобы создать имитированное устройство:

![Подготовка имитированного устройства "Модуль"](media/iot-suite-remote-monitoring-manage/devicesprovisionengine.png)

Сведения о подготовке *физического* устройства см. в статье о [подключении устройства к предварительно настроенному решению для удаленного мониторинга](iot-suite-connecting-devices-node.md).

## <a name="test-the-simulated-device"></a>Тестирование имитированного устройства

Чтобы просмотреть сведения о новом имитированном устройстве, выберите его в списке устройств на странице **Устройства**. Сведения об устройстве отображаются на панели **Device detail** (Сведения об устройстве):

![Просмотр сведений об имитированном устройстве "Модуль"](media/iot-suite-remote-monitoring-manage/devicesviewnew.png)

На панели **Device detail** (Сведения об устройстве) проверьте, отправляет ли новое устройство данные телеметрии. Для просмотра различных потоков телеметрической информации с устройства выберите имя телеметрии, например **вибрация**:

![Выбор потока телеметрической информации для просмотра](media/iot-suite-remote-monitoring-manage/devicesvibration.png)

На панели **Device detail** (Сведения об устройстве) содержатся и другие сведения об устройстве, например значения тегов, методы, которые оно поддерживает, и свойства, о которых сообщает устройство.

Для просмотра данных диагностики прокрутите вниз до представления **Диагностика**.

## <a name="act-on-a-device"></a>Работа с устройством

Для работы с устройством выберите его в списке устройств и нажмите **Расписание**. Модель устройства **Модуль** указывает четыре метода, которые должно поддерживать устройство:

![Методы устройства "Модуль"](media/iot-suite-remote-monitoring-manage/devicesmethods.png)

Выберите **Перезапустить**, присвойте заданию имя **RestartEngine** и нажмите кнопку **Применить**:

![Расписание метода перезапуска](media/iot-suite-remote-monitoring-manage/devicesrestartengine.png)

Для отслеживания состояния задания на странице **Обслуживание** выберите раздел **Состояние системы**:

![Мониторинг запланированного задания](media/iot-suite-remote-monitoring-manage/maintenancerestart.png)

### <a name="methods-in-other-devices"></a>Методы для других устройств

Поддерживаемые методы зависят от типа устройства. При развертывании с физическими устройствами модель устройства указывает методы, которые должно поддерживать устройство. Как правило, разработчик устройств отвечает за разработку кода, благодаря которому устройство реагирует на вызов метода.

Чтобы запланировать выполнение метода на нескольких устройствах, можно выбрать несколько устройств в списке на странице **Устройства**. На панели **Расписание** отображаются типы метода, который является общим для всех выбранных устройств.

## <a name="reconfigure-a-device"></a>Перенастройка устройства

Чтобы изменить конфигурацию устройства, выберите его в списке на странице **Устройства** и нажмите **Перенастроить**. На панели перенастройки отображаются значения свойств выбранного устройства, которые можно изменить:

![Перенастройка устройства](media/iot-suite-remote-monitoring-manage/devicesreconfigure.png)

Чтобы внести изменения, добавьте имя задания, обновите значения свойств и нажмите кнопку **Применить**:

![Обновление значения для свойства устройства](media/iot-suite-remote-monitoring-manage/devicesreconfigurephysical.png)

Для отслеживания состояния задания на странице **Обслуживание** выберите раздел **Состояние системы**.

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять следующие задачи:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Подготовка имитированного устройства.
> * Тестирование имитированного устройства.
> * Вызов методов устройства из решения.
> * Перенастройка устройства.

Теперь, когда вы знаете, как управлять устройствами, предлагаем перейти к дальнейшим действиям:

* [Troubleshoot and remediate device issues](iot-suite-remote-monitoring-maintain.md) (Устранение неполадок на устройстве).
* [Test your solution with simulated devices](iot-suite-remote-monitoring-test.md) (Тестирование решения с помощью имитированных устройств).
* [Подключение устройства к предварительно настроенному решению для удаленного мониторинга](iot-suite-connecting-devices-node.md).

<!-- Next tutorials in the sequence -->