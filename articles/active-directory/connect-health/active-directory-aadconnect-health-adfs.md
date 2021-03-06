---
title: "Использование Azure AD Connect Health с AD FS | Документация Майкрософт"
description: "В этой статье описывается, как Azure AD Connect Health отслеживает локальную инфраструктуру AD FS."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7946f11d209e6341caa3a11e946fb1596e758277
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Мониторинг AD FS с помощью Azure AD Connect Health
Приведенная ниже информация относится к мониторингу инфраструктуры AD FS с помощью Azure AD Connect Health. Сведения о мониторинге синхронизации Azure AD Connect с помощью Azure AD Connect Health см. в статье [Использование Azure AD Connect Health для синхронизации](active-directory-aadconnect-health-sync.md). Кроме того, сведения о мониторинге доменных служб Active Directory с помощью Azure AD Connect Health см. в статье [Использование Azure AD Connect Health с AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Оповещения AD FS
В разделе оповещений Azure AD Connect Health предоставляется список активных оповещений. Каждое оповещение содержит соответствующую информацию, действия по устранению и ссылки на связанную документацию.

Если дважды щелкнуть активное или разрешенное оповещение, появится новая колонка с дополнительной информацией, действиями, которые можно предпринять для устранения причин оповещения, и ссылками на соответствующую документацию. Можно также просмотреть данные журнала об оповещениях, которые были разрешены в прошлом.

![Azure AD Connect Health](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Аналитика использования для AD FS
Аналитика по использованию Azure AD Connect Health позволяет анализировать трафик аутентификации серверов федерации. Если дважды щелкнуть поле аналитики по использованию, откроется колонка аналитики по использованию, на которой показано несколько метрик и группирований.

> [!NOTE]
> Чтобы применять аналитику по использованию для AD FS, необходимо убедиться, что включен аудит AD FS. Дополнительные сведения см. в разделе [Включение аудита для AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Azure AD Connect Health](./media/active-directory-aadconnect-health/report1.png)

Чтобы выбрать дополнительные метрики, укажите диапазон времени или измените способ группировки, щелкнув правой кнопкой мыши диаграмму аналитики по использованию и выбрав "Изменить диаграмму". Затем можно будет указать диапазон времени, выбрать другую метрику и изменить группирование. Вы можете просмотреть распределение трафика аутентификации по различным метрикам и группировать каждую метрику с помощью соответствующего параметра "Группировать по", описанного в следующем разделе.

**Метрика "Всего запросов"**: общее число запросов, обработанных серверами AD FS.

|Сгруппировать по | Что означает группирование и почему это удобно? |
| --- | --- |
| Все | Показывает общее число запросов, обработанных всеми серверами AD FS.|
| Приложение | Группирует общее количество запросов по целевой проверяющей стороне. Такой способ группировки удобен, чтобы понять, сколько процентов от общего трафика получает каждое приложение. |
|  сервер; |Группирует общее количество запросов по серверу, обработавшему запрос. Такой способ группировки позволяет понять, как распределяется нагрузка всего трафика.
| Присоединение к рабочей области |Группирует общее количество запросов по следующему признаку: поступают ли запросы от устройств, присоединенных к рабочей области (т. е. известных устройств). Такой способ группировки позволяет определить, если доступ к вашим ресурсам осуществляется с помощью устройств, которые неизвестны в инфраструктуре удостоверений. |
|  Authentication Method | Группирует общее количество запросов по выбранному методу аутентификации. Такой способ группировки позволяет понять, какой метод аутентификации используется чаще всего. Ниже приведены возможные методы проверки подлинности:  <ol> <li>встроенная проверка подлинности Windows (Windows);</li> <li>аутентификация на основе форм (формы);</li> <li>единый вход (единый вход);</li> <li>проверка сертификата X509 (сертификат).</li> <br>Если серверы федерации получают запрос с файлом cookie для единого входа, запрос считается единым входом. В таких случаях, если файл cookie является допустимым, пользователю не предлагается ввести учетные данные, что обеспечивает прозрачный доступ к приложению. Обычно это используется при наличии нескольких проверяющих сторон, защищенных серверами федерации. |
| Сетевое расположение | Группирует общее количество запросов по сетевому расположению пользователя. Он может быть в интрасети или экстрасети. Такой способ группировки позволяет узнать, какой процент трафика поступает из экстрасети по сравнению с интрасетью. |


**Метрика "Общее число невыполненных запросов"**: общее количество невыполненных запросов, обработанных службой федерации. (Эта метрика доступна только в AD FS для Windows Server 2012 R2).

|Сгруппировать по | Что означает группирование и почему это удобно? |
| --- | --- |
| Тип ошибки | Отображает число ошибок для каждого стандартного типа. Этот способ группировки позволяет понять, ошибки каких типов наиболее распространены. <ul><li>"Неправильное имя или пароль" — ошибки из-за неправильного имени пользователя или пароля.</li> <li>"Блокировка экстрасети" — сбои из-за запросов, полученных от пользователя, для которого заблокирован доступ из экстрасети. </li><li> "Истек срок действия пароля" — сбои происходят из-за того, что пользователи входят в систему, используя пароль с истекшим сроком действия.</li><li>"Отключенная учетная запись" — сбои происходят из-за того, что пользователи входят в систему, используя отключенную учетную запись.</li><li>"Проверка подлинности устройства" — сбои происходят из-за того, что пользователям не удается выполнить проверку подлинности устройства.</li><li>"Проверка подлинности сертификата пользователя" — сбои происходят, так как пользователям не удается пройти проверку подлинности из-за недопустимого сертификата.</li><li>"Многофакторная идентификация" — сбои происходят из-за того, что пользователям не удается выполнить многофакторную идентификацию.</li><li>"Другие учетные данные", "Авторизация выдачи" — сбои из-за ошибок проверки подлинности.</li><li>"Делегирование выдачи" — сбои из-за ошибок делегирования выдачи.</li><li>"Принятие маркера" — сбои из-за отклонения службой федерации Active Directory маркера от стороннего поставщика удостоверений.</li><li>"Протокол" — сбой из-за ошибок протокола.</li><li>«Неизвестно» — все сбои. Другие типы ошибок, не относящиеся к определенным категориям.</li> |
| сервер; | Группирует ошибки на основании сервера. Это группирование позволяет понять, как ошибки распределяются между серверами. Неравномерное распределение может быть признаком того, что сервер находится в неисправном состоянии. |
| Сетевое расположение | Группирует ошибки по сетевому расположению запросов (интрасеть и экстрасеть). Это группирование позволяет понять, какого типа запросы завершаются неудачей. |
|  Приложение | Группирует сбои по целевому приложению (проверяющей стороне). Это группирование позволяет понять, какое целевое приложение вызывает большинство ошибок. |

**Метрика "Число пользователей"**: среднее число уникальных пользователей, активно проходящих аутентификацию с помощью AD FS.

|Сгруппировать по | Что означает группирование и почему это удобно? |
| --- | --- |
|Все |Эта метрика обеспечивает подсчет среднего числа пользователей, использующих службу федерации в рамках выбранного интервала времени. Пользователи не группируются. <br>Среднее значение зависит от выбранного интервала времени. |
| Приложение |Группирует среднее число пользователей по целевому приложению (проверяющей стороне). Это группирование позволяет понять, сколько пользователей использует каждое приложение. |

## <a name="performance-monitoring-for-ad-fs"></a>Мониторинг производительности AD FS
Мониторинг производительности Azure Active Directory Connect Health предоставляет данные мониторинга метрик. Если выбрать поле "Мониторинг", откроется новая колонка с подробной информацией о метриках.

![Azure AD Connect Health](./media/active-directory-aadconnect-health/perf1.png)

Если выбрать параметр «Фильтр» в верхней части колонки, можно отфильтровать информацию по серверу, чтобы просмотреть метрики отдельного сервера. Чтобы изменить метрику, щелкните правой кнопкой мыши диаграмму мониторинга под колонкой мониторинга и выберите "Изменить диаграмму" (или нажмите кнопку "Изменить диаграмму"). В новой открывшейся колонке можно будет из раскрывающегося списка выбрать дополнительные метрики и указать диапазон времени для просмотра данных производительности.

## <a name="reports-for-ad-fs"></a>Отчеты AD FS
Azure AD Connect Health предоставляет отчеты об активности и производительности AD FS. Эти отчеты помогают администраторам анализировать активность на серверах AD FS.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>50 пользователей, выполнившие наибольшее количество неудачных попыток входа с указанием неправильного имени пользователя или пароля
Одна из распространенных причин неудачных запросов на проверку подлинности на сервере AD FS заключается в использовании неправильных учетных данных (имени пользователя или пароля). Обычно это происходит, если пользователи используют сложный пароль, забыли свой пароль или ввели его с ошибками.

Есть и другие причины, которые могут привести к неожиданному числу запросов, обрабатываемых серверами AD FS. К ним относятся работа приложений, которые кэшируют учетные данные пользователя, в то время как срок их действия истек, или злонамеренные действия пользователя, который пытается войти в чужую учетную запись, используя набор хорошо известных паролей. Это важные причины, которые могут привести к всплеску запросов.

Azure AD Connect Health для AD FS предоставляет отчет о 50 пользователях, выполнивших наибольшее количество неудачных попыток входа с указанием неправильного имени пользователя или пароля. Этот отчет составляется на основе обработки событий аудита, генерируемых всеми серверами AD FS в фермах.

![Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report1a.png)

Такие отчеты предоставляют удобный доступ к следующим сведениям:

* Общее количество неудачных запросов с указанием неправильного имени пользователя или пароля за последние 30 дней.
* Среднее ежедневное количество пользователей, которым не удалось выполнить вход из-за ввода неправильного имени пользователя и пароля.

Щелкнув эту область, можно перейти к колонке основного отчета, содержащей дополнительные сведения. В ней представлен график с актуальной информацией, которая позволяет получить общее представление о запросах с указанием неправильного имени пользователя или пароля, а также список 50 пользователей, выполнивших наибольшее количество неудачных попыток входа.

На графике представлены указанные ниже данные.

* Общее ежедневное количество неудачных попыток входа с указанием неправильного имени пользователя и пароля.
* Общее ежедневное количество уникальных пользователей, выполнивших неудачные попытки входа.
* IP-адрес клиента для последнего запроса

![Azure AD Connect Health](./media/active-directory-aadconnect-health-adfs/report3a.png)

В отчете представлены следующие данные.

| Элемент отчета | Описание |
| --- | --- |
| Идентификатор пользователя. |Здесь отображается использовавшийся идентификатор пользователя. Это значение, введенное пользователем. В некоторых случаях отображаются сведения об использовании неправильного идентификатора пользователя. |
| Неудачные попытки |Здесь отображается общее количество неудачных попыток входа для определенного идентификатора пользователя. Данные таблицы отсортированы по количеству неудачных попыток в порядке убывания. |
| Последняя неудачная попытка |В этом поле содержится метка времени последней неудачной попытки. |
| IP-адрес последнего сбоя |Отображается IP-адрес клиента из последнего недопустимого запроса. |

> [!NOTE]
> Каждые два часа в этот отчет автоматически добавляются новые данные, собранные за этот период времени. Следовательно, сведения о входе в систему в рамках последнего двухчасового интервала могут отсутствовать в отчете.
>
>

## <a name="related-links"></a>Связанные ссылки
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Установка агента Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Операции Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Использование Azure AD Connect Health для синхронизации](active-directory-aadconnect-health-sync.md)
* [Using Azure AD Connect Health with AD DS (Использование Azure AD Connect Health с AD DS)](active-directory-aadconnect-health-adds.md)
* [Часто задаваемые вопросы об Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health: история версий](active-directory-aadconnect-health-version-history.md)