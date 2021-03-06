---
title: "Выбор между облаком и сервером Azure MFA | Документация Майкрософт"
description: "Выберите подходящее решение многофакторной проверки подлинности, выяснив, что является объектом защиты и где находятся пользователи.  После этого выберите облако, сервер службы MFA или службы AD FS."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/02/2017
ms.author: joflore
ms.reviewer: richagi
ms.openlocfilehash: 0b05cc76f8d8b2d14ac87fa3c55479bf0cf2377b
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Выберите для себя решение "Многофакторная идентификация Microsoft Azure".
Так как существует несколько вариантов использования службы Многофакторной идентификации Azure (MFA), для определения наиболее подходящей версии следует ответить на некоторые вопросы.  Эти вопросы приведены ниже.

* [Что я пытаюсь защитить?](#what-am-i-trying-to-secure)
* [Где находятся пользователи?](#where-are-the-users-located)
* [Какие функции мне нужны?](#what-features-do-i-need)

В разделах этой статьи представлены рекомендации, которые помогут найти ответы на эти вопросы.

## <a name="what-am-i-trying-to-secure"></a>Что я пытаюсь защитить
Чтобы выяснить, какой из вариантов двухфакторной проверки подлинности вам нужен, сначала определите, что вы пытаетесь защитить с помощью второго метода проверки подлинности.  Это приложение в службе Azure?  Или в системе удаленного доступа?  Определив объект, который необходимо защитить, мы будем знать, где именно нужно активировать многофакторную проверку подлинности.  

| Что вы пытаетесь защитить | MFA в облаке | Сервер MFA |
| --- |:---:|:---:|
| Приложения Майкрософт, получающие данные напрямую из источника |● |● |
| Приложения SaaS в коллекции приложений |● |  |
| Веб-приложения, опубликованные через прокси приложения Azure AD |● |  |
| Приложения IIS, опубликованные не через прокси приложения Azure AD | |● |
| Удаленный доступ, например VPN или RDG | ● | ● |

## <a name="where-are-the-users-located"></a>Где находятся пользователи?
В зависимости от местонахождения пользователей можно определить, какое решение нам нужно для использования сервера MFA — облачное или локальное.

| Местонахождение пользователей | MFA в облаке | Сервер MFA |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD и локальная служба AD с использованием федерации в AD FS |● |● |
| Azure AD и локальная служба AD с использованием DirSync, Azure AD Sync, Azure AD Connect без синхронизации паролей |● |● |
| Azure AD и локальная служба AD с использованием DirSync, Azure AD Sync, Azure AD Connect с синхронизацией паролей |● | |
| Локальная служба Active Directory | |● |

## <a name="what-features-do-i-need"></a>Какие функции мне нужны?
В приведенной ниже таблице сравниваются возможности облачной службы Многофакторной идентификации Azure и сервера Многофакторной идентификации.

| Функция | MFA в облаке | Сервер MFA |
| --- |:---:|:---:|
| Уведомление от мобильного приложения в качестве второго фактора | ● | ● |
| Код подтверждения мобильного приложения в качестве второго фактора | ● | ● |
| Телефонный вызов в качестве второго фактора | ● | ● |
| Одностороннее SMS в качестве второго фактора | ● | ● |
| Двустороннее SMS в качестве второго фактора | | ●  (Не рекомендуется)| 
| Маркеры оборудования в качестве второго фактора | | ● |
| Пароли приложений для клиентов Office 365, которые не поддерживают MFA | ● | |
| Администраторский контроль над методами проверки подлинности | ● | ● |
| Режим ПИН-кода | | ● |
| Предупреждение о мошенничестве |● | ● |
| Отчеты службы "Многофакторная идентификация Microsoft Azure" |● | ● |
| Разовый обход | | ● |
| Настраиваемые приветствия для телефонных вызовов | ● | ● |
| Настройка идентификатора вызывающей стороны для телефонных звонков | ● | ● |
| Надежные IP-адреса | ● | ● |
| Запоминание данных MFA для доверенных устройств | ● | |
| Условный доступ | ● | ● |
| Кэш |  | ● |

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы имеете представление о различиях в возможностях службы "Многофакторная идентификация Azure" в облаке и на локальном сервере, можно приступать к настройке и использованию службы. **Выберите значок, соответствующий вашему сценарию.**

<center>

[![MFA в облаке](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Сервер MFA](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
