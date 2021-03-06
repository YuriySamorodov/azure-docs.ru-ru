---
title: "Приступая к работе с Azure AD версии 2 для iOS. Тестирование | Документация Майкрософт"
description: "В этой статье описано, как приложения iOS (Swift) могут вызывать API, которому необходимы маркеры доступа, с помощью конечной точки Azure Active Directory версии 2."
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: c002387c6d2c8ec83610398c337dc504e7253028
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Отправка тестового запроса к API Microsoft Graph из приложения iOS

Нажмите `Command` + `R` для запуска кода в симуляторе.

![Пример снимка экрана](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Когда вы будете готовы к проверке, нажмите кнопку *Вызвать API Microsoft Graph*, и вам будет предложено ввести имя пользователя и пароль.

### <a name="consent"></a>Согласие на предоставление разрешений
При первом входе в приложение появится экран согласия, как показано ниже, где необходимо явно дать согласие:

![Экран согласия](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Ожидаемые результаты
Сведения о профиле пользователя, полученные в результате вызова API Microsoft Graph, отображаются в разделе *Ведение журнала*.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Дополнительные сведения об областях и делегированных разрешениях

Для чтения профиля пользователя API Microsoft Graph требуется область `user.read`. По умолчанию эта область автоматически добавляется в каждое приложение, регистрируемое на портале регистрации. Для некоторых других API Microsoft Graph, а также пользовательских API для вашего внутреннего сервера могут потребоваться дополнительные области. Например, для Microsoft Graph требуется область `Calendars.Read`, чтобы отобразить список календарей пользователя. Чтобы получить доступ к календарю пользователя в контексте приложения, в сведения о регистрации приложения необходимо добавить делегированное разрешение `Calendars.Read`, а затем добавить область `Calendars.Read` в вызов `acquireTokenSilent`. При увеличении количества областей от пользователя может потребоваться дополнительное согласие.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]