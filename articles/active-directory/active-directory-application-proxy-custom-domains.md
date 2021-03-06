---
title: "Личные домены в прокси приложения Azure AD | Документация Майкрософт"
description: "Вы можете управлять личными доменами в прокси приложения Azure AD, чтобы использовать один и тот же URL-адрес для приложения вне зависимости от того, откуда к нему обращаются пользователи."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: f254f4959c66aeb3eec522f31e0d8a6780358188
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Работа с пользовательскими доменами в прокси приложения Azure AD

При публикации приложения через прокси приложения Azure Active Directory можно создать внешний URL-адрес для перехода ваших пользователей при удаленной работе. Имя этого URL-адреса содержит домен по умолчанию *ваш_клиент-msappproxy.net*. Например, если вы опубликовали приложение Expenses и используете клиент Contoso, то внешним URL-адресом будет https://expenses-contoso.msappproxy.net. Если вы хотите использовать собственное доменное имя, настройте личный домен для своего приложения. 

Мы рекомендуем настраивать личные домены для ваших приложений, когда это возможно. Ниже перечислены некоторые преимущества личных доменов.

- Пользователи могут получить доступ к приложению, используя тот же URL-адрес, как при работе в сети, так и за ее пределами.
- Если все ваши приложения имеют одинаковые внутренние и внешние URL-адреса, то ссылки в одном приложении, которые указывают на другое приложение, продолжат работать даже за пределами корпоративной сети. 
- Вы можете управлять фирменной символикой и создавать необходимые URL-адреса. 


## <a name="configure-a-custom-domain"></a>Настройка личного домена

### <a name="prerequisites"></a>Предварительные требования

Прежде чем настроить личный домен, убедитесь, что у вас есть следующие необходимые компоненты: 
- [Проверенный домен, добавленный в Azure Active Directory](active-directory-domains-add-azure-portal.md).
- Пользовательский сертификат для домена в формате PFX-файла. 
- Локальное приложение, [опубликованное через прокси приложение](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Настройка личного домена

При наличии этих трех компонентов выполните указанные ниже действия для настройки личного домена.

1. Войдите на [портал Azure](https://portal.azure.com).
2. Перейдите в **Azure Active Directory** > **Корпоративные приложения** > **Все приложения** и выберите приложение, которым необходимо управлять.
3. Выберите **Прокси приложения**. 
4. Воспользуйтесь раскрывающимся списком в поле внешнего URL-адреса, чтобы выбрать личный домен. Если ваш домен не отображается в списке, значит он еще не проверен. 
5. Нажмите кнопку **Сохранить**.
5. Поле **Сертификат**, которое раньше было недоступным, станет доступным. Выберите это поле. 

   ![Щелкните, чтобы передать сертификат.](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Если вы уже передали сертификат для этого домена, то поле "Сертификат" отображает сведения об этом сертификате. 

6. Передайте сертификат PFX и введите пароль к нему. 
7. Выберите **Сохранить**, чтобы сохранить изменения. 
8. Добавьте [запись DNS](../dns/dns-operations-recordsets-portal.md), которая перенаправляет новый внешний URL-адрес к домену msappproxy.net. 

>[!TIP] 
>Для одного личного домена необходимо передать только один сертификат. Когда сертификат будет передан, вы сможете выбрать личный домен при публикации нового приложения. Вам не нужно выполнять дополнительную настройку, за исключением записи DNS. 

## <a name="manage-certificates"></a>Управление сертификатами

### <a name="certificate-format"></a>Формат сертификата
На методы подписки сертификатов нет никаких ограничений. Поддерживается шифрование на основе эллиптических кривых (ECC), альтернативное имя субъекта (SAN) и другие стандартные типы сертификатов. 

Можно использовать групповой сертификат, если он соответствует требуемому внешнему URL-адресу. 

### <a name="changing-the-domain"></a>Изменение домена
Все проверенные домены отображаются в раскрывающемся списке внешних URL-адресов для вашего приложения. Чтобы изменить домен, просто обновите поле приложения. Если в списке нет нужного домена, [добавьте его в качестве проверенного домена](active-directory-domains-add-azure-portal.md). Если вы выбрали домен без связанного сертификата, выполните шаги 5–7, чтобы добавить сертификат. Затем убедитесь, что обновили запись DNS для перенаправления из нового внешнего URL-адреса. 

### <a name="certificate-management"></a>Управление сертификатами
Вы можете использовать тот же сертификат для нескольких приложений, если они не находятся на одном и том же внешнем узле. 

По истечении срока действия сертификата вы получите предупреждение о том, что необходимо передать другой сертификат с помощью портала. Если сертификат отозван, пользователи могут увидеть предупреждение системы безопасности при получении доступа к приложению. Проверки отзыва сертификатов не выполняются.  Чтобы обновить сертификат для нужного приложения, перейдите к приложению, выполните шаги 5–7 для настройки личных доменов в опубликованных приложениях и загрузите новый сертификат. Если старый сертификат не используется другими приложениями, он будет автоматически удален. 

Сейчас все задачи управления сертификатами выполняются с помощью страниц отдельных приложений, поэтому необходимо управлять сертификатами в контексте соответствующих приложений. 

## <a name="next-steps"></a>Дальнейшие действия
* [Включите единый вход](active-directory-application-proxy-sso-using-kcd.md) в опубликованные приложения с помощью аутентификации Azure AD.
* [Включите условный доступ](application-proxy-enable-remote-access-sharepoint.md) к опубликованным приложениям.
* [Добавление имени личного домена в Azure Active Directory](active-directory-domains-add-azure-portal.md)


