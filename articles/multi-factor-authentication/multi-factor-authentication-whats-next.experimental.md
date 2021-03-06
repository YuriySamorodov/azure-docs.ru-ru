---
title: "Настройка Многофакторной идентификации Azure | Документация Майкрософт"
description: "Это страница Многофакторной идентификации Azure с описанием дальнейших действий в отношении MFA.  Сюда относятся отчеты, предупреждения о мошенничестве, разовый обход проверки, пользовательские голосовые сообщения, кэширование, надежные IP-адреса и пароли приложений."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: richagi
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: joflore
ms.openlocfilehash: 31b32079de19c6c9822c388f60269b07a8c70198
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Настройка параметров Многофакторной идентификации Azure
Из этой статьи вы узнаете, как управлять настроенной и запущенной службой Многофакторной идентификации Azure (MFA).  В статье рассматриваются способы наиболее эффективного использования Многофакторной идентификации Azure.  Описываемые возможности представлены не в каждой версии службы Многофакторной идентификации Azure.

| Функция | Описание | 
|:--- |:--- |
| [Предупреждение о мошенничестве](#fraud-alert) |Предупреждение о мошенничестве можно настроить так, чтобы пользователи могли сообщать о несанкционированных попытках доступа к своим ресурсам. |
| [Разовый обход](#one-time-bypass) |Разовый обход позволяет пользователю выполнить проверку подлинности единожды посредством "обхода" многофакторной идентификации. |
| [Пользовательские голосовые сообщения](#custom-voice-messages) |Пользовательские голосовые сообщения позволяют использовать собственные записи или приветствия через многофакторную идентификацию. |
| [Кэширование](#caching-in-azure-multi-factor-authentication) |Кэширование позволяет задать определенный период времени таким образом, чтобы последующие попытки проверки подлинности проводились автоматически. |
| [Надежные IP-адреса](#trusted-ips) |Администраторы управляемого или федеративного клиента имеют возможность использовать надежные IP-адреса, чтобы обойти двухфакторную проверку подлинности для тех пользователей, которые выполняют вход из локальной интрасети компании. |
| [Пароли приложений](#app-passwords) |Пароли приложений позволяют приложениям, которые не поддерживают MFA, обойти эту проверку. |
| [Сохранение данных Многофакторной идентификации Azure для добавленных в память устройств и браузеров](#remember-multi-factor-authentication-for-devices-that-users-trust) |Позволяет запомнить устройства на заданное количество дней после того, как пользователь успешно выполнит вход с помощью MFA. |
| [Выбор методов проверки](#selectable-verification-methods) |Позволяет выбирать доступные пользователям методы проверки подлинности. |

## <a name="access-the-azure-mfa-management-portal"></a>Получение доступа к порталу управления Azure MFA

Настройки некоторых функций, описанных в этой статье, можно найти на портале управления Многофакторной идентификации Azure. Вы можете получить доступ к этому порталу через классический портал Azure двумя способами. Первый способ — это управление поставщиком многофакторной идентификации. Второй — через параметры службы MFA. 

### <a name="use-an-authentication-provider"></a>Использование поставщика проверки подлинности

Если вы используете поставщик многофакторной идентификации на основе фактического потребления, используйте этот метод для получения доступа к порталу управления.

Для доступа к порталу управления MFA через поставщика Многофакторной идентификации Azure войдите на классический портал Azure с правами администратора и выберите параметр Active Directory. Щелкните вкладку **Поставщики многофакторной проверки подлинности**, а затем выберите каталог и нажмите кнопку **Управление** внизу.

### <a name="use-the-mfa-service-settings-page"></a>Использование страницы параметров службы MFA 

Использовать страницу параметров службы "Многофакторная идентификация" можно при наличии одной из следующих лицензий:
- Многофакторная идентификация Azure
- Azure AD Premium 
- Enterprise Mobility + Security.

Для доступа к порталу управления MFA через страницу "Параметры службы" войдите на классический портал Azure с правами администратора и выберите параметр Active Directory. Выберите свой каталог и перейдите на вкладку **Настройка** . В разделе многофакторной идентификации щелкните **Управление параметрами службы**. В нижней части страницы "Параметры службы MFA" щелкните **Перейти на портал** .


## <a name="fraud-alert"></a>Предупреждение о мошенничестве
Предупреждение о мошенничестве можно настроить так, чтобы пользователи могли сообщать о несанкционированных попытках доступа к своим ресурсам.  Пользователи могут сообщать о мошенничестве с помощью мобильного приложения или через свой телефон.

### <a name="set-up-fraud-alert"></a>Настройка предупреждения о мошенничестве
1. Перейдите на портал управления MFA, следуя инструкциям в верхней части этой страницы.
2. На портале управления Многофакторной идентификации Azure щелкните **Параметры** в разделе "Настройка".
3. В разделе "Предупреждение о мошенничестве" страницы "Параметры" установите флажок **Разрешить пользователям отправлять предупреждения о мошенничестве**.
4. Щелкните **Сохранить**, чтобы применить изменения. 

### <a name="configuration-options"></a>Варианты настройки

- **Блокировать пользователя при получении сообщения о мошенничестве.** Если пользователь сообщает о мошенничестве, учетная запись соответствующего пользователя блокируется.
- **Код для сообщения о мошенничестве во время начального приветствия.** Чтобы подтвердить двухфакторную проверку подлинности, пользователи обычно нажимают кнопку #. Если они хотят сообщить о мошенничестве, сперва нужно ввести код, а затем нажать кнопку #. По умолчанию используется код **0**, но вы можете его изменить.

> [!NOTE]
> Голосовые приветствия корпорации Майкрософт по умолчанию просят пользователей нажать 0# для отправки предупреждения о мошенничестве. Если используется любой другой код, кроме 0, необходимо записать и отправить собственное пользовательское голосовое приветствие с соответствующими инструкциями.

![Снимок экрана, где показаны параметры предупреждения о мошенничестве](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Как пользователи сообщают о мошенничестве 
Сообщить о поступлении предупреждения о мошенничестве можно двумя способами:  через мобильное приложение или по телефону.  

#### <a name="report-fraud-with-the-mobile-app"></a>Сообщение о мошенничестве с помощью мобильного приложения
1. Когда вы получите на телефон подтверждение, щелкните его, чтобы открыть приложение Microsoft Authenticator.
2. В уведомлении щелкните **Запретить**. 
3. Выберите **Сообщить о мошенничестве**.
4. Закройте приложение.

#### <a name="report-fraud-with-a-phone"></a>Сообщение о мошенничестве с помощью телефона
1. Когда вы получите на телефон проверочный вызов, ответьте на него.  
2. Чтобы сообщить о мошенничестве, введите код мошенничества (по умолчанию — 0), а затем знак #. Вы будете оповещены об отправке предупреждения о мошенничестве.
3. Завершите вызов.

### <a name="view-fraud-reports"></a>Просмотр отчетов о мошенничестве
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Выберите слева элемент Active Directory.
3. Вверху выберите **Поставщики многофакторной проверки подлинности**, чтобы показать список таких поставщиков.
4. Выберите поставщика многофакторной аутентификации и в нижней части страницы щелкните **Управление**. Откроется портал управления Многофакторной идентификации Azure.
5. На портале управления Многофакторной идентификации Azure в разделе "Просмотреть отчет" щелкните **Предупреждение о мошенничестве**.
6. Укажите диапазон дат, который требуется просмотреть в сообщении. Также можно указать имена и состояние пользователей, а также их номера телефонов.
7. Щелкните **Выполнить**, чтобы отобразить отчет с предупреждениями о мошенничестве. Щелкните **Экспорт в CSV**, если необходимо экспортировать отчет.

## <a name="one-time-bypass"></a>Разовый обход
Разовый обход позволяет пользователю выполнить проверку подлинности единожды, обходя двухфакторную проверку подлинности. Обход проверки является временным процессом и истекает через заданное количество секунд. Поэтому в ситуациях, когда мобильное приложение или телефон не получает уведомлений или вызовов, можно включить функцию разового обхода проверки, чтобы пользователь мог иметь доступ к нужному ресурсу.

### <a name="create-a-one-time-bypass"></a>Создание разового обхода проверки
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на портал управления MFA, следуя инструкциям в верхней части этой страницы.
3. Если в левой части вы видите имя поставщика Azure MFA, возле которого стоит значок **+**, щелкните **+**. Отображаются разные группы репликации сервера MFA и группа Azure по умолчанию. Выберите необходимую группу.
4. В разделе "Администрирование пользователей" щелкните **Одноразовый обход проверки**.
5. На странице "Одноразовый обход проверки" щелкните **Новый одноразовый обход проверки**.

  ![Облако](./media/multi-factor-authentication-whats-next/create1.png)

6. Введите имя пользователя, длительность и причину обхода. Щелкните **Обход проверки**.
7. Отчет времени начинается немедленно, и пользователь должен войти до истечения срока действия разового обхода проверки. 

### <a name="view-the-one-time-bypass-report"></a>Просмотр отчета о разовом обходе проверки
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Выберите слева элемент Active Directory.
3. Вверху выберите **Поставщики многофакторной проверки подлинности**, чтобы показать список таких поставщиков.
4. Выберите поставщика многофакторной аутентификации и в нижней части страницы щелкните **Управление**. Откроется портал управления Многофакторной идентификации Azure.
5. На портале управления Многофакторной идентификации Azure в левой части щелкните **Разовый обход** в разделе "Просмотреть отчет".
6. Укажите диапазон дат, который требуется просмотреть в сообщении. Также можно указать имена и состояние пользователей, а также их номера телефонов.
7. Щелкните **Выполнить**, чтобы отобразился отчет об обходах. Щелкните **Экспорт в CSV**, если необходимо экспортировать отчет.

## <a name="custom-voice-messages"></a>Пользовательские голосовые сообщения
Пользовательские голосовые сообщения позволяют использовать собственные записи или приветствия для двухфакторной проверки подлинности. Пользовательские голосовые сообщения можно использовать вместе с записями Майкрософт или вместо них.

Прежде чем начать, ознакомьтесь со следующей информацией:

* Поддерживаются только форматы файлов WAV и MP3.
* Максимальный размер файла составляет 5 МБ.
* Сообщения о проверке подлинности должны длиться меньше, чем 20 секунд. Сообщения, длительностью более 20 секунд, не дают пользователям достаточно времени для ответа до истечения времени проверки.

### <a name="set-up-a-custom-message"></a>Настройка пользовательского сообщения

Создание пользовательского сообщения состоит из двух этапов. Сперва нужно загрузить сообщение, а затем включить его для пользователей.

Чтобы загрузить пользовательское сообщение, сделайте следующее:

1. Создайте пользовательское голосовое сообщение с помощью одного из поддерживаемых форматов файлов.
2. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
3. Перейдите на портал управления MFA, следуя инструкциям в верхней части этой страницы.
4. На портале управления Многофакторной идентификации Azure в разделе "Настройка" щелкните **Голосовые сообщения**.
5. На странице настройки голосовых сообщений щелкните **Новое голосовое сообщение**.
   ![Облако](./media/multi-factor-authentication-whats-next/custom1.png)
6. На странице настройки новых голосовых сообщений щелкните **Управление звуковыми файлами**.
   ![Облако](./media/multi-factor-authentication-whats-next/custom2.png)
7. На странице настройки звуковых файлов щелкните **Отправить звуковой файл**.
   ![Облако](./media/multi-factor-authentication-whats-next/custom3.png)
8. На странице настройки отправки звуковых файлов щелкните **Обзор**, перейдите к своему голосовому сообщению и щелкните **Открыть**.
9. Добавьте описание и щелкните **Отправить**.
10. После отправки голосового сообщения появится сообщение с подтверждением об успешной отправке файла.

Чтобы включить сообщение для пользователей, сделайте следующее:

1. В левой части щелкните **Голосовые сообщения**.
2. В разделе "Голосовые сообщения" щелкните **Новое голосовое сообщение**.
3. Из раскрывающегося списка "Язык" выберите язык.
4. Если сообщение предназначено для конкретного приложения, укажите его в поле "Приложение".
5. В раскрывающемся списке "Тип сообщения" выберите тип сообщения, который будет переопределен нашим новым пользовательским сообщением.
6. В раскрывающемся списке "Звуковой файл" выберите звуковой файл, загруженный в первой части.
7. Щелкните **Создать**. Появится сообщение о том, что голосовое сообщение успешно создано.
    ![Облако](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Кэширование в службе Многофакторной идентификации Azure
Кэширование позволяет задать определенный период времени таким образом, чтобы последующие попытки проверки подлинности в рамках этого периода проводились автоматически. Кэширование используется, когда в локальных системах, таких как VPN, отправляется множество запросов проверки в то время, как первый запрос еще обрабатывается. Это позволяет последующим запросам автоматически завершиться успешно после того, как пользователь успешно пройдет первую проверку. 

Для входа в Azure AD не может использоваться кэширование.

### <a name="set-up-caching"></a>Настройка кэширования 
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на портал управления MFA, следуя инструкциям в верхней части этой страницы.
3. На портале управления Многофакторной идентификации Azure в разделе "Настройка" щелкните **Кэширование**.
4. На странице настройки кэширования щелкните **Новый кэш**.
5. Выберите "Тип кэша" и продолжительность кэширования в секундах. Щелкните **Создать**.

<center>![Облако](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Надежные IP-адреса
Доверенные IP-адреса — это функция службы "Многофакторная идентификация Azure", которую администраторы управляемого или федеративного клиента используют для обхода двухфакторной проверки подлинности. Пользователи проходят ее при входе из локальной частной сети компании. Эта функция доступна в полной версии Многофакторной идентификации Azure и недоступна в бесплатной версии для администраторов. Дополнительные сведения о том, как получить полную версию службы Многофакторной идентификации Azure, см. в [этой статье](multi-factor-authentication.md).

| Тип клиента Azure AD | Доступные варианты Надежных IP |
|:--- |:--- |
| Управляемые |<li>Определенные диапазоны IP-адресов — администраторы могут задать диапазон IP-адресов, которые могут обходить двухфакторную проверку подлинности применительно к пользователям, которые выполняют вход из интрасети компании.</li> |
| Федеративные |<li>Все федеративные пользователи — все федеративные пользователи, которые выполняют вход из сети организации, будут обходить двухфакторную проверку подлинности, предоставляя выданное AD FS утверждение.</li><br><li>Определенные диапазоны IP-адресов — администраторы могут задать диапазон IP-адресов, которые могут обходить двухфакторную проверку подлинности применительно к пользователям, которые выполняют вход из интрасети компании. |

Подобный обход работает только в пределах интрасети компании. 

**Условия для пользователей в пределах корпоративной сети**

Если функция "Надежные IP-адреса" отключена, для потоков браузера требуется проходить двухфакторную проверку подлинности, а для более старых расширенных клиентских приложений необходимо указывать пароли приложений. 

При включении доверенных IP-адресов двухфакторная проверка подлинности *не* требуется для потоков браузера. Пароли приложений *не* требуются для более старых полнофункциональных клиентских приложений при условии, что пользователь еще не создал пароль приложения. Если пароль приложения уже создан, он остается обязательным. 

**Условия для пользователей вне корпоративной сети**

Независимо от того, включена ли функция "Надежные IP-адреса", для потоков браузера требуется выполнять двухфакторную проверку подлинности, а для более старых расширенных клиентских приложений необходимо указать пароли приложений. 

### <a name="to-enable-trusted-ips"></a>Для включения функции "Надежные IP-адреса"
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на страницу параметров службы MFA, следуя инструкциям в начале этой статьи.
3. На странице "Параметры службы" в разделе "Надежные IP-адреса" есть два параметра:
   
   * **Для запросов от федеративных пользователей, исходящих из моей интрасети.** Установите этот флажок. Все федеративные пользователи, которые выполняют вход из корпоративной сети, обойдут двухфакторную проверку подлинности, предоставляя выданное AD FS утверждение.
   * **Для запросов от определенного диапазона общедоступных IP-адресов.** Введите IP-адреса в соответствующем текстовом поле в нотации CIDR. Например: xxx.xxx.xxx.0/24 для IP-адресов в диапазоне xxx.xxx.xxx.1 — xxx.xxx.xxx.254 или xxx.xxx.xxx.xxx/32 для одного IP-адреса. Можно ввести до 50 диапазонов IP-адресов. Пользователи, выполняющие вход с этих IP-адресов, обходят двухфакторную проверку подлинности.
4. Щелкните **Сохранить**.
5. После применения обновлений щелкните **Закрыть**.

![Надежные IP-адреса](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Пароли приложений
Некоторые приложения, например Office 2010 или более старой версии и Apple Mail, не поддерживают двухфакторную проверку подлинности. Они не настроены для использования второй проверки подлинности. Для пользования этими приложениями вместо традиционного пароля необходимо использовать "пароли приложений". Пароль приложения позволяет приложению обойти двухфакторную проверку подлинности и продолжить работу.

> [!NOTE]
> Современная проверка подлинности для клиентов Office 2013
> 
> Все клиенты Office 2013 (включая Outlook) и более поздних версий поддерживают новые протоколы проверки подлинности и могут использовать двухфакторную проверку подлинности. После включения для этих клиентов не нужно вводить пароли приложений.  Дополнительные сведения см. в записи блога [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/) (Анонсирована общедоступная предварительная версия современной службы многофакторной идентификации для Office 2013).

### <a name="important-things-to-know-about-app-passwords"></a>Важная информация о паролях приложений
Ниже приведен список важных моментов, которые следует знать о паролях приложений.

* Пароли приложений необходимо вводить в поле ввода только раз. Пользователям не нужно хранить их и вводить каждый раз.
* В действительности пароль создается автоматически, а не задается пользователем. Автоматически созданный пароль сложнее подобрать, и он более безопасен.
* Максимальное количество паролей на пользователя — 40. 
* В приложениях, которые кэшируют пароли для использования в локальных сценариях, могут возникать ошибки, так как пароль приложения используется только ресурсами, которые также используют идентификатор организации. Например, сообщения электронной почты Exchange, которые хранятся локально, но архивируются в облаке. Один пароль работать не будет.
* При запуске многофакторной проверки подлинности можно использовать свой пароль с некоторыми небраузерными клиентами. Пароли приложений нельзя использовать для задач администрирования. Обязательно создайте учетную запись службы с надежным паролем для запуска сценариев PowerShell и не включайте для этой учетной записи двухфакторную проверку подлинности.

> [!WARNING]
> Пароли приложений не работают в гибридных средах, где клиенты взаимодействуют как с локальными, так и с облачными конечными точками автообнаружения. Аутентификация для паролей домена выполняется в локальной среде, а для паролей приложений — в облачной.

### <a name="naming-guidance-for-app-passwords"></a>Рекомендации по присвоению имен для паролей приложений
Имена паролей приложений должны указывать название устройства, на котором они будут использоваться. Например, если у вас есть ноутбук с небраузерными приложениями, создайте один пароль для приложения с именем "Ноутбук" и используйте его. Затем на настольном компьютере создайте для этого приложения новый пароль приложения с именем "Настольный компьютер". 

Корпорация Майкрософт рекомендует создавать один пароль приложения для каждого устройства, а не для каждого приложения.

### <a name="federated-sso-app-passwords"></a>Федеративные (SSO) пароли приложений
Azure AD поддерживает федерацию (единый вход) с локальными службами домена Active Directory в Windows Server (AD DS). Если в организации настроена федерация с Azure AD и необходимо использовать Многофакторную идентификацию Azure, тогда информация о паролях приложения является важной. Этот раздел относится только к федеративным клиентам.

* Пароль приложения проверяется Azure AD и поэтому обходит федерацию. Федерация активно используется только при задании пароля приложения.
* В случае с федеративными пользователями мы не будем переходить на поставщика удостоверений в отличие от пассивного потока. Пароли хранятся в идентификаторе организации. Если пользователь уходит из компании, данная информация должна попадать в идентификатор организации с помощью DirSync в режиме фактического времени. Отключение или удаление учетной записи может занять до трех часов с учетом синхронизации, что задерживает процесс отключения или удаления пароля приложения в Azure AD.
* Параметры контроля доступа локальных клиентов не учитываются при использовании пароля приложения.
* Пароль приложения не предусматривает возможность локального протоколирования или контроля проверки подлинности.
* Некоторые современные архитектуры при использовании двухфакторной проверки подлинности могут запрашивать как учетные данные, так и пароли приложений. Хотя это зависит от того, где выполняется аутентификация. Что касается клиентов, выполняющих проверку подлинности в локальной инфраструктуре, можно использовать имя пользователя и пароль организации. Что касается клиентов, выполняющих проверку подлинности в Azure AD, можно использовать пароль приложения.

  Предположим, например, что имеется архитектура, которая состоит из таких экземпляров:

  * Вы задаете федеративную связь своего локального экземпляра Active Directory с Azure AD
  * Вы используете Exchange online
  * Вы используете Lync локально
  * Вы используете службу Многофакторной идентификации Azure

  ![Подтверждение](./media/multi-factor-authentication-whats-next/federated.png)

  В таких случаях необходимо сделать следующее:

  * При выполнении входа в Lync используйте имя пользователя и пароль вашей организации.
  * При попытке доступа к адресной книге через клиент Outlook, который подключается к Exchange online, используйте пароль приложения.

### <a name="allow-app-password-creation"></a>Разрешение на создание пароля приложения
По умолчанию пользователи не могут создавать пароли приложений, но эту функцию можно включить самостоятельно. Чтобы разрешить пользователям создавать пароли приложений, сделайте следующее:

1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на страницу параметров службы MFA, следуя инструкциям в начале этой статьи.
3. Установите переключатель **Allow users to create app passwords to sign into non-browser apps** (Разрешить пользователям создавать пароли приложений для входа во внебраузерные приложения).

![Создание паролей приложений](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Создание паролей приложений
Пользователи могут создавать пароли приложений во время первоначальной регистрации. В конце процесса регистрации им предлагается такой вариант, позволяющий это делать.

Пользователи также могут создавать пароли приложений после регистрации, изменяя параметры на портале Azure или Office 365. Дополнительные сведения и подробные инструкции для пользователей см. в статье [Что такое пароли приложений в службе Многофакторной идентификации Azure](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Запоминание данных многофакторной проверки подлинности для устройств, которым доверяет пользователь
Запоминание данных многофакторной проверки подлинности для устройств и браузеров, которым доверяют пользователи, — это бесплатная функция, доступная всем пользователям MFA. Служба "Многофакторная идентификация" позволяет пользователям обходить MFA в течение заданного количества дней после входа. Этот компонент повышает степень удобства, уменьшая количество выполнений двухфакторной проверки подлинности на одном устройстве.

Однако если учетная запись или устройство скомпрометированы, запоминание данных MFA для доверенных устройств может повлиять на безопасность. В случае потери доверенного устройства или компрометации учетной записи организации нужно [восстановить Многофакторную идентификацию на всех устройствах](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Это действие отменяет статус всех доверенных устройств, и пользователю необходимо будет снова выполнить двухфакторную проверку подлинности. Вы также можете сообщить пользователям о необходимости восстановления MFA на их устройствах, используя указания в разделе [Повторное выполнение двухфакторной проверки подлинности устройства, помеченного как доверенное](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted).

### <a name="how-it-works"></a>Принцип работы

Запоминание данных проверки подлинности происходит при добавлении в браузер постоянного файла cookie, когда пользователь во время входа устанавливает флажок "Больше не спрашивать **X** дн.". До истечения срока действия файла cookie запрос на выполнение MFA отображаться не будет. Если пользователь на том же устройстве откроет другой браузер или очистит файлы cookie, запрос на проверку отобразится снова. 

Флажок "Больше не спрашивать **X** дн." не отображается в приложениях, работающих вне браузера, независимо от того, поддерживают они современные методы проверки подлинности или нет. Эти приложения используют токены обновления, которые предоставляют новые маркеры доступа каждый час. Во время проверки маркера обновления Azure AD проверяет, выполнялась ли последняя двухфакторная проверка подлинности в течение настроенного количества дней. 

Напоследок следует заметить, что при запоминании MFA на доверенных устройствах сокращается количество аутентификаций в веб-приложениях (которые обычно запрашиваются каждый раз), но также увеличивается число аутентификаций в современных клиентах аутентификации (которые обычно запрашиваются каждые 90 дней).

> [!NOTE]
>Эта функция несовместима с функцией "Оставаться в системе" в AD FS (пользователи выполняют двухфакторную проверку подлинности для AD FS через сервер Azure MFA или стороннее решение MFA). Если пользователи используют функцию "Оставаться в системе" в AD FS, а затем помечают устройство как доверенное для MFA, они не смогут выполнять проверку по истечении срока действия функции запоминания данных. Azure AD отправляет запрос на выполнение новой двухфакторной проверки подлинности, но AD FS возвращает токен с исходным утверждением MFA и исходной датой, а не выполняет запрашиваемый процесс. Этот процесс отключает цикл выполнения проверки между Azure AD и AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Включение запоминания данных Многофакторной идентификации
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на страницу параметров службы MFA, следуя инструкциям в начале этой статьи.
3. На странице "Параметры службы" в разделе управления параметрами пользовательского устройства установите флажок **Разрешить пользователям сохранять данные многофакторной проверки подлинности на доверенных устройствах**.
   ![Запоминание устройств](./media/multi-factor-authentication-whats-next/remember.png)
4. Задайте количество дней, в течение которых доверенным устройствам будет разрешено обходить двухфакторную проверку подлинности. По умолчанию — 14 дней.
5. Щелкните **Сохранить**.
6. Нажмите кнопку **Закрыть**

### <a name="mark-a-device-as-trusted"></a>Пометка устройства как доверенного

После включения этой функции пользователи могут помечать устройства как доверенные, установив флажок **Don't ask again** (Больше не спрашивать) при входе в систему.

![Снимок экрана, на котором показан флажок Don't ask again (Больше не спрашивать)](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Выбор методов проверки
Вы можете указать, какие методы проверки будут доступны пользователям. В таблице ниже приведен краткий обзор каждого метода.

Когда пользователям регистрируют свои учетные записи для MFA, они могут выбрать удобный метод проверки на основе включенных вами параметров. Рекомендации по настройке регистрации см. в статье [Настройка учетной записи для двухфакторной проверки подлинности](multi-factor-authentication-end-user-first-time.md).

| Метод | Описание |
|:--- |:--- |
| Звонок на телефон |На телефон осуществляется автоматический голосовой вызов. Для проверки подлинности пользователь отвечает на вызов и нажимает кнопку # на клавиатуре телефона. Этот номер телефона не синхронизируется в локальной службе Active Directory. |
| SMS на телефон |На телефон приходит текстовое сообщение с кодом проверки. Пользователю предлагается ответить на текстовое сообщение с кодом проверки или ввести код проверки в интерфейсе входа. |
| Уведомление в мобильном приложении |Отправляется push-уведомление на телефон или зарегистрированное устройство. Пользователь просматривает уведомление и выбирает **Проверить**, чтобы выполнить проверку. <br>Приложение Microsoft Authenticator доступно для [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) и [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Код проверки от мобильного приложения |Приложение Microsoft Authenticator создает код проверки OATH каждые 30 секунд. Пользователь вводит этот код проверки в интерфейсе входа.<br>Приложение Microsoft Authenticator доступно для [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) и [iOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-to-enabledisable-authentication-methods"></a>Включение и отключение методов проверки подлинности
1. Перейдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите на страницу параметров службы MFA, следуя инструкциям в начале этой статьи.
3. На странице "Параметры службы" в разделе параметров проверки установите или снимите флажки рядом с нужными методами.
   ![Варианты проверки](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Щелкните **Сохранить**.
5. Нажмите кнопку **Закрыть**
