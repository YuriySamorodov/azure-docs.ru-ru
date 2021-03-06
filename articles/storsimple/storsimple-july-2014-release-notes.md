---
title: "Заметки о выпуске версии StorSimple 8000 | Документация Майкрософт"
description: "Описание новых возможностей, открытых проблем и доступных решений для выпуска Microsoft Azure StorSimple от июля 2014 г."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: e6e087ad61ee54fef3552895c00a478fa51e2072
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Заметки о выпуске устройства версии StorSimple 8000. Июль 2014 г.

> [!NOTE]
> Классический портал StorSimple устарел. Диспетчеры устройств StorSimple автоматически перейдут на новый портал Azure в соответствии с графиком устаревания. Вы получите сообщение электронной почты и уведомление с портала, касающиеся этого перехода. Этот документ скоро также перестанет использоваться. Сведения, связанные с переходом, см. в [ответах на вопросы о перемещении на портал Azure](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Обзор

В следующих заметках о выпуске перечислены важные открытые вопросы, касающиеся устройства серии StorSimple 8000, связанные с общедоступным выпуском Microsoft Azure StorSimple за июль 2014 г. Этот выпуск соответствует версии программного обеспечения 6.3.9600.17215.

Если не указано обратное, эти заметки о выпуске действительны для всех моделей устройства StorSimple. Заметки о выпуске постоянно обновляются: обнаруживаются и добавляются критические проблемы, требующие временного решения. Перед развертыванием решения Microsoft Azure StorSimple необходимо учесть следующее.

## <a name="known-issues-in-this-release"></a>Известные проблемы в этом выпуске

В таблице ниже содержится сводка по известным проблемам в этом выпуске.  

| Нет. | Функция | Проблема | Комментарий или временное решение | Для физического устройства | Для виртуального устройства |
| --- | --- | --- | --- | --- | --- |
| 1 |Сброс к параметрам по умолчанию |В некоторых случаях при выполнении сброса к параметрам по умолчанию устройство StorSimple может зависнуть и отобразится следующее сообщение: **Выполняется сброс к параметрам по умолчанию (этап 8)**. Это происходит при нажатии клавиш CTRL+C во время выполнения командлета. |Не нажимайте клавиши CTRL+C после начала сброса. Если вы уже находитесь в процессе сброса, обратитесь в службу поддержки Майкрософт, чтобы уточнить дальнейшие действия. |Да |Нет |
| 2 |Дисковый кворум |В редких случаях, если большинство дисков в корпусе EBOD устройства 8600 отключены, что приводит к отсутствию дискового кворума, пул хранения переходит в автономный режим. Он будет оставаться отключенным даже если диски будут подключены повторно. |Устройство необходимо будет перезагрузить. Если проблема возникнет снова, обратитесь в службу поддержки Майкрософт. |Да |Нет |
| 3 |Сбои облачных моментальных снимков |В редких случаях создание облачного моментального снимка может завершиться с ошибкой: **Достигнуто максимально допустимое количество резервных копий**. Это происходит, когда на одном устройстве создается более 255 клонов одного и того же исходного тома, который был удален. | |Да |Да |
| 4 |Неправильный идентификатор контроллера |При замене контроллера, контроллер 0 может отображаться как контроллер 1. Во время замены контроллера, когда образ загружается с однорангового узла, идентификатор контроллера может изначально отображаться как идентификатор контроллера однорангового узла. Иногда такое поведение может также наблюдаться после перезагрузки системы. |Никаких действий от пользователя не требуется. Ситуация разрешится сама после завершения замены контроллера. |Да |Нет |
| 5 |Диаграммы мониторинга устройств |В службе StorSimple Manager диаграммы мониторинга устройства не работают, если обычная проверка подлинности или проверка подлинности NTLM включена в конфигурации прокси-сервера устройства. |Измените конфигурацию веб-прокси для устройства, зарегистрированного в службе StorSimple Manager, чтобы для проверки подлинности было установлено значение «НЕТ». Чтобы сделать это, запустите оболочку Windows PowerShell для командлета StorSimple Set-HcsWebProxy. |Да |Да |
| 6 |учетные записи хранения; |Использование службы хранилища для удаления учетной записи хранения не поддерживается. Это приведет к ситуации, в которой невозможно получить пользовательские данные. | |Да |Да |
| 7 |Восстановление размещения |Восстановление размещения в течение 24 часов с начала аварийного восстановления не поддерживается. | |Да |Нет |
| 8 |Отработка отказа устройства |Многократные отработки отказов контейнера томов из одного исходного устройства на разные целевые устройства не поддерживаются. Отработка отказа с одного неиспользуемого устройства на несколько устройств приведет к тому, что контейнеры томов на первом отказавшем устройстве потеряют право собственности на данные. После такой отработки отказа эти контейнеры томов при просмотре на классическом портале Azure отображаются или ведут себя по-другому. | |Да |Нет |
| 9 |Установка |Во время установки адаптера StorSimple для SharePoint для успешного завершения установки необходимо указать IP-адрес устройства. | |Да |Нет |
| 10 |Сетевые интерфейсы |Сетевые интерфейсы DATA 2 и DATA 3 переключены в программном обеспечении. |Если необходимо настроить эти интерфейсы, обратитесь в службу технической поддержки Майкрософт. |Да |Нет |

