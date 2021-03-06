---
title: "Сквозная проверка подлинности в Azure AD Connect. Обновление предварительной версии агентов проверки подлинности | Документация Майкрософт"
description: "В этой статье описывается процесс обновления сквозной аутентификации в Azure Active Directory (Azure AD)."
services: active-directory
keywords: "сквозная проверка подлинности azure ad connect, установка active directory, необходимые компоненты для azure ad, единый вход"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 0a7293f2b3a366b25e780ee75601dfbb2b35ddaa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-pass-through-authentication-upgrade-preview-authentication-agents"></a>Сквозная проверка подлинности в Azure Active Directory. Обновление предварительной версии агентов проверки подлинности

## <a name="overview"></a>Обзор

Эта статья предназначена для клиентов, которые используют предварительную версию сквозной аутентификации Azure AD. Недавно мы обновили и переименовали программное обеспечение агента проверки подлинности. Теперь вам нужно _вручную_ обновить предварительную версию агентов проверки подлинности, установленную на локальных серверах. Это обновление выполняется однократно. Все последующие обновления агентов проверки подлинности будут автоматическими. У вас есть несколько причин, чтобы выполнить это обновление.

- Для предварительных версий агентов проверки подлинности больше не предоставляются обновления безопасности или исправления ошибок.
-   Предварительные версии агентов проверки подлинности не удастся установить на дополнительные серверы, чтобы повысить уровень доступности.

## <a name="check-versions-of-your-authentication-agents"></a>Проверка версий для агентов проверки подлинности

### <a name="step-1-check-where-your-authentication-agents-are-installed"></a>Шаг 1. Узнайте, где установлены агенты проверки подлинности

Выполните следующие действия, чтобы выяснить, где установлены агенты проверки подлинности.

1. Войдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com), используя учетные данные глобального администратора для клиента.
2. В области навигации слева щелкните **Azure Active Directory**.
3. Выберите **Azure AD Connect**. 
4. Выберите **Сквозная проверка подлинности**. Откроется колонка со списком серверов, на которых установлены агенты проверки подлинности.

![Центр администрирования Active Directory Azure — колонка "Сквозная проверка подлинности"](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

### <a name="step-2-check-the-versions-of-your-authentication-agents"></a>Шаг 2. Проверка версий для агентов проверки подлинности

Чтобы узнать используемые версии агентов проверки подлинности, выполните следующие действия на каждом сервера, выявленном на предыдущем шаге.

1. Выберите на локальном сервере пункты **Панель управления -> Программы -> Программы и компоненты**.
2. Если здесь вы найдете запись **Microsoft Azure AD Connect Authentication Agent** (Агент проверки подлинности Microsoft Azure AD Connect), никаких дополнительных действий на этом сервере выполнять не требуется.
3. Если здесь есть запись **Microsoft Azure AD Application Proxy Connector** (Соединитель прокси приложения Microsoft Azure AD) с номером версии 1.5.132.0 или еще меньшим, на этом сервере следует выполнить обновление вручную.

![Предварительная версия агента проверки подлинности](./media/active-directory-aadconnect-pass-through-authentication/pta6.png)

## <a name="best-practices-to-follow-before-starting-the-upgrade"></a>Рекомендации перед началом обновления

Перед обновлением подготовьте следующие важные ресурсы.

1. **Создайте учетную запись глобального администратора только для облака.** Не следует выполнять обновление без учетной запись глобального администратора только для облака. Она пригодится вам в экстренной ситуации, если агенты сквозной проверки подлинности будут работать неправильно. См. дополнительные сведения о [добавлении облачной учетной записи глобального администратора](../active-directory-users-create-azure-portal.md). Этот шаг очень важен для того, чтобы не потерять доступ к клиенту.
2.  **Обеспечьте высокий уровень доступности.** Если вы не делали этого раньше, [установите второй изолированный агент проверки подлинности](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability), чтобы обеспечить высокий уровень доступности для запросов на вход.

## <a name="upgrading-the-authentication-agent-on-your-azure-ad-connect-server"></a>Обновление агента проверки подлинности на сервере Azure AD Connect

Перед обновлением агента проверки подлинности следует обновить на этом сервере Azure AD Connect. Выполните следующие действия на основном и промежуточном серверах Azure AD Connect.

1. **Обновите Azure AD Connect.** Выполните действия из этой [статьи](./active-directory-aadconnect-upgrade-previous-version.md), чтобы обновить Azure AD Connect до последней версии.
2. **Удалите предварительную версию агента проверки подлинности.** Загрузите [этот скрипт PowerShell](https://aka.ms/rmpreviewagent) и запустите его на сервере с правами администратора.
3. **Скачайте последнюю версию агента аутентификации (версию 1.5.193.0 или более позднюю).** Войдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com) с учетными данными глобального администратора для клиента. Выберите элементы **Azure Active Directory -> Azure AD Connect -> Сквозная проверка подлинности -> Скачать агент**. Подтвердите согласие с [условиями обслуживания](https://aka.ms/authagenteula) и скачайте последнюю версию агента проверки подлинности. Можно также скачать агент аутентификации [отсюда](https://aka.ms/getauthagent).
4. **Установите последнюю версию агента проверки подлинности.** Запустите исполняемый файл, который вы скачали на шаге 3. По запросу введите учетные данные глобального администратора для арендатора.
5. **Проверьте установку новой версии.** Еще раз откройте элементы **Панель управления -> Программы -> Программы и компоненты** и убедитесь, что здесь есть запись **Microsoft Azure AD Connect Authentication Agent** (Агент проверки подлинности Microsoft Azure AD Connect).

>[!NOTE]
>Если просмотреть колонку "Сквозная проверка подлинности" в [центре администрирования Azure Active Directory](https://aad.portal.azure.com) после завершения предыдущих шагов, то вы увидите по две записи агентов аутентификации на каждом сервере. При этом один из агентов аутентификации **активен**, а другой **неактивен**. Это _ожидаемое поведение_. **Неактивная** запись автоматически удалится через несколько дней.

## <a name="upgrading-the-authentication-agent-on-other-servers"></a>Обновление агента проверки подлинности на других серверах

Выполните следующие действия, чтобы обновить агенты проверки подлинности на других серверах (на которых не установлен Azure AD Connect).

1. **Удалите предварительную версию агента проверки подлинности.** Загрузите [этот скрипт PowerShell](https://aka.ms/rmpreviewagent) и запустите его на сервере с правами администратора.
2. **Скачайте последнюю версию агента аутентификации (версию 1.5.193.0 или более позднюю).** Войдите в [центр администрирования Azure Active Directory](https://aad.portal.azure.com) с учетными данными глобального администратора для клиента. Выберите элементы **Azure Active Directory -> Azure AD Connect -> Сквозная проверка подлинности -> Скачать агент**. Подтвердите согласие с условиями обслуживания и скачайте последнюю версию агента.
3. **Установите последнюю версию агента проверки подлинности.** Запустите исполняемый файл, который вы скачали на шаге 2. По запросу введите учетные данные глобального администратора для арендатора.
4. **Проверьте установку новой версии.** Еще раз откройте элементы **Панель управления -> Программы -> Программы и компоненты** и убедитесь, что здесь есть запись с именем **Microsoft Azure AD Connect Authentication Agent** (Агент проверки подлинности Microsoft Azure AD Connect).

>[!NOTE]
>Если просмотреть колонку "Сквозная проверка подлинности" в [центре администрирования Azure Active Directory](https://aad.portal.azure.com) после завершения предыдущих шагов, то вы увидите по две записи агентов аутентификации на каждом сервере. При этом один из агентов аутентификации **активен**, а другой **неактивен**. Это _ожидаемое поведение_. **Неактивная** запись автоматически удалится через несколько дней.

## <a name="next-steps"></a>Дальнейшие действия
- [**Устранение неполадок**](active-directory-aadconnect-troubleshoot-pass-through-authentication.md). Узнайте, как устранить самые распространенные проблемы с этой функцией.
