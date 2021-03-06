---
title: "Настройка Azure AD SSPR | Документация Майкрософт"
description: "Параметры настройки самостоятельного сброса пароля в Azure AD"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: f2b172208185e343c9c10d55036c20d60346778c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2017
---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Настройка функции самостоятельного сброса паролей в Azure AD

ИТ-специалисты, которые планируют развернуть функцию самостоятельного сброса пароля, могут настроить ее интерфейс в соответствии с потребностями пользователей.

## <a name="customize-the-contact-your-administrator-link"></a>Настройка ссылки "Обратитесь к администратору"

Даже если SSPR не включен, можно воспользоваться ссылкой "Обратитесь к администратору" на портале сброса паролей.  После выбора этой ссылки администраторам будет отправлено электронное сообщение с просьбой о помощи в изменении пароля пользователя либо пользователи будут направлены на указанный вами URL-адрес. Рекомендуется задать ссылку на адрес электронной почты или веб-сайт, который пользователи будут использовать для поддержки.

![Контакт][Contact]

Это сообщение электронной почты отправляется следующим получателям в следующем порядке:

1. Уведомление получают пользователи с ролью **Администратор паролей**, если она назначена.
2. Если администраторы паролей отсутствуют, уведомление получают пользователи с ролью **Администратор пользователей**.
3. Если ни одна из вышеупомянутых ролей не назначена, уведомление получат **глобальные администраторы**.

Во всех случаях уведомление будет отправлено не более чем 100 получателям.

Дополнительные сведения о различных ролях администраторов и их назначении см. в документе [Назначение ролей администратора в Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

### <a name="disable-contact-your-administrator-emails"></a>Отключение отправки электронных сообщений при выборе ссылки "Обратитесь к администратору"

Если организации не требуется уведомлять администраторов о запросах на сброс паролей, можно включить описанную ниже конфигурацию.

* Включение самостоятельного сброса пароля для всех пользователей Этот параметр находится в разделе **Сброс пароля > Свойства**.
    * Если вы не хотите, чтобы пользователи могли сбрасывать пароли, можно ограничить доступ пустой группой **не рекомендуемый параметр**.
* Настройте ссылку на службу технической поддержки для предоставления URL-адреса или адреса mailto, который пользователи могут использовать для получения помощи. Последовательно выберите **Сброс пароля > Настройка > Адрес электронной почты или URL-адрес нестандартной службы технической поддержки**.

## <a name="customize-adfs-sign-in-page-for-sspr"></a>Настройка страницы входа служб федерации Active Directory для SSPR

Администраторы служб федерации Active Directory могут добавить ссылку на страницу входа согласно инструкциям в статье [Добавьте описание sign\ в страницы](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description).

Используя приведенную ниже команду на сервере служб федерации Active Directory, можно добавить ссылку на страницу входа служб федерации Active Directory, которая позволяет пользователям напрямую приступить к процессу самостоятельного сброса пароля.

``` Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href=’https://passwordreset.microsoftonline.com’>Can’t access your account?</A></p>" ```

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Настройка выполнения входа и оформления панели доступа

Вы можете настроить логотип, который отображается вместе с изображением страницы входа, в соответствии с фирменной символикой организации.

Эти изображения отображаются в следующих случаях:

* После того, как пользователь вводит свое имя пользователя.
* Когда пользователь обращается к настраиваемому URL-адресу.
    * При добавлении параметра whr к адресу страницы сброса паролей, например https://login.microsoftonline.com/?whr=contoso.com.
    * При добавлении параметра username к адресу страницы сброса паролей, например https://login.microsoftonline.com/?username=admin@contoso.com.

### <a name="graphics-details"></a>Сведения о графических изображениях

Следующие параметры позволяют изменять визуальные характеристики страницы входа. Их можно найти в разделе **Azure Active Directory**, **Корпоративная фирменная символика**, **Изменение корпоративной фирменной символики**.

* Изображение страницы входа должно быть PNG или JPG-файлом размера 1420 x 1200 пикселей не более 500 КБ. Для достижения наилучших результатов мы рекомендуем использовать изображение около 200 КБ.
* Цвет фона страницы входа используется при подключениях с высокой задержкой. Он должен быть задан в шестнадцатеричном формате RGB.
* Изображение баннера должно быть PNG- или JPG-файлом 60x280 пикселей размером не более 10 КБ.
* Квадратный логотип (обычная и темная тема) должен быть PNG- или JPG-файлом 240x240 пикселей (изменяемое значение) размером не более 10 КБ.

### <a name="sign-in-text-options"></a>Параметры текста входа

Следующие параметры позволяют добавить на страницу входа текст, относящийся к вашей организации. Эти параметры можно найти в разделе **Azure Active Directory**, **Корпоративная фирменная символика**, **Изменение корпоративной фирменной символики**.

* **Подсказка по имени пользователя** заменяет текст примера someone@example.com чем-то более подходящим для пользователей. Мы рекомендуем оставить параметр по умолчанию при поддержке внутренних и внешних пользователей.
* **Текст страницы входа** должен быть не более 256 символов в длину. Он будет отображаться, когда пользователь будет выполнять вход в сеть и подключаться к Azure AD в Windows 10. Используйте этот текст для условий использования, инструкций и советов для пользователей. **Любой пользователь может видеть страницу входа в систему. Поэтому не размещайте на ней никаких конфиденциальных сведений.**

### <a name="keep-me-signed-in-disabled"></a>Возможность оставаться в системе отключена

Параметр "Возможность оставаться в системе отключена" позволяет пользователю продолжать работу после того, как он закрыл и снова открыл окно браузера. Он не влияет на время действия сеанса. Этот параметр можно найти в разделе **Azure Active Directory > Корпоративная фирменная символика > Изменение корпоративной фирменной символики**.

Некоторые функции SharePoint Online и Office 2010 зависят от возможности пользователей установить этот флажок. Если скрыть этот параметр, пользователи могут получать непредвиденные дополнительные запросы на выполнение входа.

### <a name="directory-name"></a>Имя каталога

Атрибут имени можно изменить в разделе **Azure Active Directory > Свойства** для отображения понятного названия организации на портале и при автоматическом взаимодействии. Этот параметр лучше заметен в виде автоматических сообщений электронной почты в следующих формах:

* Понятное имя в сообщении электронной почты "Майкрософт от демонстрационной учетной записи CONTOSO".
* Тема в сообщении электронной почты "Электронное письмо с кодом проверки демонстрационной учетной записи CONTOSO".

## <a name="next-steps"></a>Дальнейшие действия

* [Как развернуть самостоятельный сброс пароля](active-directory-passwords-best-practices.md)
* [Сброс или изменение пароля](active-directory-passwords-update-your-own-password.md)
* [Регистрация для самостоятельного сброса пароля](active-directory-passwords-reset-register.md)
* [Вопросы по лицензированию](active-directory-passwords-licensing.md)
* [Какие данные используются для SSPR и какие сведения нужно указывать для пользователей](active-directory-passwords-data.md)
* [Доступные пользователям методы проверки подлинности](active-directory-passwords-how-it-works.md#authentication-methods)
* [Параметры политики для SSPR](active-directory-passwords-policy.md)
* [Что такое обратная запись паролей и каково ее назначение](active-directory-passwords-writeback.md)
* [Как сообщать о действиях в SSPR](active-directory-passwords-reporting.md)
* [Обзор всех параметров SSPR и их значение](active-directory-passwords-how-it-works.md)
* [Как устранить неполадки самостоятельного сброса пароля](active-directory-passwords-troubleshoot.md)
* [Вопросы, не вошедшие в другие статьи](active-directory-passwords-faq.md)

[Contact]: ./media/active-directory-passwords-customize/sspr-contact-admin.png "Пример обращения по электронной почте к администратору для получения помощи при сбросе пароля"