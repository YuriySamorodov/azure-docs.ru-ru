---
title: "Создание ресурса Azure Application Insights | Документация Майкрософт"
description: "Вручную настройте мониторинг Application Insights для нового работающего приложения."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: mbullwin
ms.openlocfilehash: 9023f3d9ae3ddd4d75b5853a08177cba7718cec1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2017
---
# <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
В Azure Application Insights данные о приложении отображаются в *ресурсе* Microsoft Azure. Таким образом, создание ресурса является частью [настройки Application Insights для мониторинга нового приложения][start]. Во многих случаях создать ресурс можно автоматически с помощью IDE. Однако в некоторых случаях создавать ресурс необходимо вручную. Например, чтобы иметь отдельные ресурсы для сборок разработки и производственных сборок приложения.

После создания ресурса можно получить его ключ инструментирования и использовать этот ключ для настройки пакета SDK в приложении. Ключ ресурса позволяет связать данные телеметрии с ресурсом.

## <a name="sign-up-to-microsoft-azure"></a>Регистрация в Microsoft Azure
Если у вас еще нет [учетной записи Майкрософт, получите ее сейчас](http://live.com). (Если вы используете такие службы, как Outlook.com, OneDrive, Windows Phone или XBox Live, значит, у вас уже есть учетная запись Майкрософт.)

Вам также потребуется подписка [Microsoft Azure](http://azure.com). Если у вашей группы или организации есть подписка Azure, владелец может добавить вас в нее с помощью вашей учетной записи Windows Live ID. Плата взимается только за используемый объем, а базовый план по умолчанию предусматривает бесплатное использование определенного объема в экспериментальных целях.

Если у вас есть доступ к подписке, войдите в Application Insights по адресу [http://portal.azure.com](https://portal.azure.com)и используйте свой Live ID для входа.

## <a name="create-an-application-insights-resource"></a>Создание ресурса Application Insights
Перейдите по адресу [portal.azure.com](https://portal.azure.com)и добавьте новый ресурс Application Insights.

![Нажмите "Создать" и "Application Insights"](./media/app-insights-create-new-resource/01-new.png)

* **Тип приложения** определяет содержимое колонки "Обзор" и свойства, доступные в [обозревателе метрик][metrics]. Если тип приложения не отображается, выберите пункт "Общие".
* **Подписка** представляет собой учетную запись для оплаты в Azure.
* **Группа ресурсов** – удобный способ для управления свойствами наподобие контроля доступа. Если вы уже создали другие ресурсы Azure, можно поместить новый ресурс в ту же группу.
* **Расположение** – это место, в котором мы храним ваши данные.
* Установив флажок **Закрепить на панели мониторинга**, вы сможете поместить плитку для быстрого доступа к ресурсу на главную страницу Azure. (рекомендуется).

После создания приложения откроется новая колонка. В этой колонке будут представлены данные о производительности и использовании приложения. 

Чтобы перейти сюда после следующего входа в Azure,используйте плитку быстрого доступа на начальной доске (на начальном экране). Ее также можно открыть, щелкнув «Обзор».

## <a name="copy-the-instrumentation-key"></a>Копирование ключа инструментирования
Ключ инструментирования идентифицирует созданный вами ресурс. Он должен указываться в SDK.

![Щелкните Essentials, выделите ключ инструментирования и нажмите клавиши CTRL + C.](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-the-sdk-in-your-app"></a>Установка пакета SDK в приложении
Установите пакет SDK Application Insights в приложении. Выполнение этого шага зависит от типа приложения. 

Используйте ключ инструментирования для настройки [пакета SDK, который можно установить в приложении][start].

Пакет SDK включает стандартные модули, которые отправляют данные телеметрии, не требуя написания кода. Для более подробного отслеживания действий пользователей или диагностики неполадок отправляйте собственные данные телеметрии, [используя API][api].

## <a name="monitor"></a>Просмотр данных телеметрии
Закройте колонку быстрого доступа, чтобы вернуться к колонке приложения на портале Azure.

Щелкните элемент "Поиск", чтобы открыть [Diagnostic Search][diagnostic] (Поиск по журналу диагностики), где отображаются первые события. 

Нажмите кнопку **Обновить** через несколько секунд, если ожидаете дополнительные данные.

## <a name="creating-a-resource-automatically"></a>Автоматическое создание ресурса
Вы можете написать [Сценарий PowerShell](app-insights-powershell.md) для автоматического создания ресурса.

## <a name="next-steps"></a>Дальнейшие действия
* [Создание панели мониторинга](app-insights-dashboards.md)
* [Поиск по журналу диагностики](app-insights-diagnostic-search.md)
* [Изучение метрик](app-insights-metrics-explorer.md)
* [Написание запросов аналитики](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

