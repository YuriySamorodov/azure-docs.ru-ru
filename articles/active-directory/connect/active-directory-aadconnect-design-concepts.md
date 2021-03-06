---
title: "Azure AD Connect: принципы проектирования | Документация Майкрософт"
description: "В этой статье описываются факторы, которые необходимо учитывать при проектировании реализации."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 53a0f766de9db7e6ee48b6659aad378620c0d727
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/14/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: принципы проектирования
В этой статье приведено описание факторов, которые должны учитываться при проектировании реализации Azure AD Connect. Здесь подробно рассмотрены некоторые вопросы, но часть из них вкратце рассмотрена в других статьях.

## <a name="sourceanchor"></a>sourceAnchor
Атрибут sourceAnchor определяется как *атрибут, который остается неизменным на протяжении всего времени существования объекта*. Он однозначно определяет объект независимо от того, является ли он локальным или находится в Azure AD. Этот атрибут также называется **immutableId**. Оба этих имени взаимозаменяемы.

Слово "неизменяемый" является в этой статье ключевым. Оно означает, что значение атрибута не может быть изменено. Так как значение этого атрибута, если оно уже задано, нельзя изменить, важно выбрать такой подход к проектированию, который подойдет для вашей ситуации.

Данный атрибут используется в следующих случаях.

* При создании нового сервера синхронизации или его пересоздании после аварийного восстановления: в таком случае этот атрибут связывает существующие объекты в Azure AD с локальными объектами.
* При смене модели, в которой используются только облачные удостоверения, на модель, в которой используются синхронизированные удостоверения: в таком случае этот атрибут позволяет жестко связывать существующие объекты в Azure AD с локальными объектами.
* При использовании федерации: в этом случае данный атрибут в сочетании с **userPrincipalName** используется в утверждении для уникальной идентификации пользователя.

В этой статье речь идет только об атрибуте sourceAnchor в контексте его значения для пользователей. Аналогичные правила действуют для всех типов объектов, но проблемы обычно возникают именно у пользователей.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Выбор значений атрибута sourceAnchor.
Значение атрибута должно соответствовать следующим правилам:

* Его длина должна быть меньше 60 символов.
  * Символы, отличные от a–z, A–Z или 0–9, кодируются, и каждый из них считается как три символа.
* Оно не должно содержать специальные символы: &#92; ! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Оно должно быть глобально уникальным.
* Значение должно относиться к строковому, целочисленному или двоичному типу.
* Значение не должно быть основано на имени пользователя.
* Значение не должно быть с учетом регистра и содержать значения, которые изменяются в зависимости от регистра.
* Значение должно присваиваться при создании объекта.

Если значение sourceAnchor не является строковым, служба Azure AD Connect шифрует его в формате Base64, чтобы исключить появление специальных символов. Если вы используете сервер федерации, отличный от ADFS, убедитесь, что сервер также может кодировать атрибут в формате Base64.

Атрибут sourceAnchor чувствителен к регистру. JohnDoe и johndoe — это разные значения. При этом не стоит создавать два объекта, имена которых отличаются только регистром.

При наличии одного локального леса следует использовать атрибут **objectGUID**. Этот атрибут используется, если вы применяете стандартные параметры Azure AD Connect. Он также используется в DirSync.

Если у вас есть несколько лесов и вы не перемещаете пользователей между ними или доменами, то и в этом случае мы рекомендуем использовать атрибут **objectGUID** .

Если же вы перемещаете пользователей между лесами или доменами, вам необходимо найти такой атрибут, который либо никогда не изменяется, либо может перемещаться вместе с пользователями. Рекомендуется использовать синтетический атрибут. Для этого подходит атрибут, который может содержать что-нибудь наподобие идентификатора GUID. При создании объекта создается новый идентификатор GUID, который присваивается пользователю. На сервере подсистемы синхронизации можно создать настраиваемое правило синхронизации, которое будет создавать это значение на основе идентификатора **objectGUID** и обновлять выбранный атрибут в службе ADDS. При перемещении объекта убедитесь, что это значение также копируется.

Другой вариант — выбрать существующий атрибут, который никогда не изменяется. Зачастую можно использовать атрибут **employeeID**. Если значение атрибута содержит буквы, исключите вероятность изменения регистра (на верхний или нижний) в этом значении. Не используйте атрибуты, содержащие имя пользователя. В случае женитьбы, замужества или развода имя может измениться, что недопустимо для этого атрибута. Это одна из причин того, почему мастер установки Azure AD Connect не позволяет выбирать такие атрибуты, как **userPrincipalName**, **mail** и **targetAddress**. Эти атрибуты содержат также знак "@", что недопустимо для sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Изменение атрибута sourceAnchor.
Значение атрибута sourceAnchor нельзя изменить после создания объекта в Azure AD и синхронизации удостоверения.

По этой причине в Azure AD Connect действуют следующие ограничения.

* Атрибут sourceAnchor можно задать только при первоначальной установке. При повторном запуске мастера установки этот параметр доступен только для чтения. Для изменения этого параметра потребуется удалить и повторно установить программу.
* При установке другого сервера Azure AD Connect необходимо выбрать уже используемый атрибут sourceAnchor. Если вы ранее использовали DirSync и переходите на Azure AD Connect, вам потребуется использовать **objectGUID** , так как этот атрибут использовался в DirSync.
* Если после экспорта объекта в Azure AD значение sourceAnchor изменится, синхронизация Azure AD Connect выдаст ошибку и вы больше не сможете изменять этот объект, пока не устраните проблему, восстановив прежнюю версию sourceAnchor в исходном каталоге.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Использование msDS-ConsistencyGuid в качестве sourceAnchor
По умолчанию Azure AD Connect (версии 1.1.486.0 и более ранних версий) в качестве атрибута sourceAnchor использует идентификатор objectGUID. ObjectGUID создается системой. Его значение не указывается при создании локальных объектов AD. Как описано в разделе [sourceAnchor](#sourceanchor), в некоторых сценариях необходимо указать значение sourceAnchor. Если к вам относятся эти сценарии, то в качестве атрибута sourceAnchor необходимо использовать настраиваемый атрибут AD (например msDS-ConsistencyGuid).

Azure AD Connect (версии 1.1.524.0 и более поздних версий) упрощает использование msDS-ConsistencyGuid в качестве атрибута sourceAnchor. При использовании этой функции Azure AD Connect автоматически настраивает правила синхронизации для выполнения следующих действий:

1. Использование msDS-ConsistencyGuid в качестве атрибута sourceAnchor для объектов User. Для других типов объектов используется ObjectGUID.

2. Если для локального объекта User в AD не заполнен атрибут msDS-ConsistencyGuid, то Azure AD Connect записывает значение objectGUID обратно в атрибут msDS-ConsistencyGuid в локальном каталоге Active Directory. После заполнения атрибута msDS-ConsistencyGuid Azure AD Connect экспортирует объект в Azure AD.

>[!NOTE]
> После того, как локальный объект AD импортируется в Azure AD Connect (то есть импортируется в пространство соединителя AD и проецируется в метавселенную), его значение sourceAnchor больше невозможно изменить. Чтобы указать значение sourceAnchor для определенного локального объекта AD, настройте его атрибут msDS-ConsistencyGuid, прежде чем импортировать его в Azure AD Connect.

### <a name="permission-required"></a>Требования к разрешениям
Для работы этой функции учетной записи AD DS, используемой для синхронизации с локальным каталогом Active Directory, необходимо предоставить разрешение на запись атрибута msDS-ConsistencyGuid в локальном каталоге Active Directory.

### <a name="how-to-enable-the-consistencyguid-feature---new-installation"></a>Включение функции ConsistencyGuid — новая установка
Вы можете включить использование ConsistencyGuid в качестве sourceAnchor во время новой установки. В этом разделе подробно описывается экспресс- и пользовательская установка.

  >[!NOTE]
  > Только более поздние версии Azure AD Connect (1.1.524.0 и выше) поддерживают использование ConsistencyGuid в качестве sourceAnchor во время новой установки.

### <a name="how-to-enable-the-consistencyguid-feature"></a>Включение функции ConsistencyGuid
В настоящее время эту функцию можно включить только при новой установке Azure AD Connect.

#### <a name="express-installation"></a>Экспресс-установка
При установке Azure AD Connect в экспресс-режиме мастер Azure AD Connect автоматически определяет наиболее подходящий атрибут AD для использования в качестве атрибута sourceAnchor. При этом применяется следующая логика:

* Во-первых, мастер Azure AD Connect запрашивает у клиента Azure AD получение атрибута AD, который использовался в качестве атрибута sourceAnchor при предыдущей установке Azure AD Connect (если использовался). Если эти сведения доступны, то Azure AD Connect использует тот же атрибут AD.

  >[!NOTE]
  > Только более поздние версии Azure AD Connect (1.1.524.0 и выше) хранят в клиенте Azure AD сведения об используемом во время установки атрибуте sourceAnchor. Более ранние версии Azure AD Connect этого не делают.

* Если сведения об используемом атрибуте sourceAnchor недоступны, мастер проверяет состояние атрибута msDS-ConsistencyGuid в локальном каталоге Active Directory. Если для какого-то объекта в каталоге атрибут не настроен, то мастер использует msDS-ConsistencyGuid в качестве атрибута sourceAnchor. Если атрибут настроен в одном или нескольких объектах в каталоге, то мастер делает вывод, что он используется другими приложениями и не подходит в качестве атрибута sourceAnchor...

* В этом случае мастер снова начинает использовать objectGUID в качестве атрибута sourceAnchor.

* Когда атрибут sourceAnchor определен, мастер сохраняет сведения в клиенте Azure AD. Эти сведения будут использоваться при следующей установке Azure AD Connect.

По завершении экспресс-установки мастер сообщает, какой атрибут был выбран в качестве атрибута привязки к источнику.

![Мастер сообщает атрибут AD, выбранный для sourceAnchor](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Выборочная установка
При выборочной установке Azure AD Connect мастер Azure AD Connect предоставляет два варианта настройки атрибута sourceAnchor:

![Выборочная установка: настройка sourceAnchor](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Настройка | Описание |
| --- | --- |
| Параметр "Azure управляет привязкой к источнику" | Выберите этот параметр, если вы хотите, чтобы среда Azure AD выбирала этот атрибут автоматически. Если выбрать этот параметр, то мастер Azure AD Connect применит [логику выбора атрибута sourceAnchor, использованную при экспресс-установке](#express-installation). Как и при экспресс-установке, мастер сообщит, какой атрибут был выбран в качестве атрибута привязки к источнику после завершения выборочной установки. |
| Определенный атрибут | Выберите этот параметр, если вы хотите указать имеющийся атрибут AD в качестве атрибута sourceAnchor. |

### <a name="how-to-enable-the-consistencyguid-feature---existing-deployment"></a>Включение функции ConsistencyGuid — существующее развертывание
Если у вас есть развертывание Azure AD Connect, которое использует objectGUID как атрибут привязки к источнику, вы можете переключиться на использование ConsistencyGuid.

>[!NOTE]
> Только более поздние версии Azure AD Connect (1.1.552.0 и выше) поддерживают переключение с ObjectGuid на ConsistencyGuid в качестве атрибута привязки к источнику.

Переключение с objectGUID на ConsistencyGuid в качестве атрибута привязки к источнику:

1. Запустите мастер Azure AD Connect и щелкните **Настроить**, чтобы перейти на экран задач.

2. Выберите параметр задачи **Настроить привязку к источнику** и нажмите кнопку **Далее**.

   ![Включение ConsistencyGuid для существующего развертывания — шаг 2](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Введите учетные данные администратора Azure AD и нажмите кнопку **Далее**.

4. Мастер Azure AD Connect анализирует состояние атрибута msDS-ConsistencyGuid в локальном каталоге Active Directory. Если атрибут не настроен в каком-либо объекте в каталоге, Azure AD Connect считает, что никакие другие приложения сейчас его не используют и что его можно безопасно использовать как атрибут привязки к источнику. Нажмите кнопку **Next** (Далее), чтобы продолжить.

   ![Включение ConsistencyGuid для существующего развертывания — шаг 4](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. На экране **Готовность к настройке** щелкните **Настроить**, чтобы внести изменения в конфигурацию.

   ![Включение ConsistencyGuid для существующего развертывания — шаг 5](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. По завершении конфигурации мастер указывает, что msDS-ConsistencyGuid сейчас используется как атрибут привязки к источнику.

   ![Включение ConsistencyGuid для существующего развертывания — шаг 6](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Во время анализа (шаг 4), если атрибут настроен в одном объекте в каталоге или нескольких, то мастер делает вывод, что он используется другими приложениями и возвращает ошибку, как показано на снимке экрана ниже. Кроме того, эта ошибка может произойти, если вы ранее включили функцию ConsistencyGuid на сервере-источнике Azure AD Connect и пытаетесь сделать то же самое на промежуточном сервере.

![Включение ConsistencyGuid для существующего развертывания — ошибка](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

 Если вы уверены, что атрибут не используется другими приложениями, можно отменить вывод сообщения об ошибке. Для этого перезапустите мастер Azure AD Connect, указав **/SkipLdapSearchcontact**. Выполните следующую команду в командной строке:

```
"c:\Program Files\Microsoft Azure Active Directory Connect\AzureADConnect.exe" /SkipLdapSearch
```

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Влияние на конфигурацию AD FS или федерацию сторонних поставщиков
Если вы используете Azure AD Connect для управления локальным развертыванием AD FS, то Azure AD Connect автоматически обновляет правила утверждений для использования в качестве sourceAnchor того же атрибута AD. Это гарантирует, что утверждение ImmutableID, созданное ADFS, будет согласовано со значениями sourceAnchor, экспортированными в Azure AD.

Если вы управляете AD FS не из Azure AD Connect или используете для аутентификации сторонние серверы федерации, то необходимо вручную обновить правила утверждений, чтобы утверждение ImmutableID было согласовано со значениями, экспортированными в Azure AD, как описано в разделе [Изменение правил утверждений служб федерации Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). По завершении установки мастер возвращает следующее предупреждение:

![Настройка сторонней федерации](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-to-existing-deployment"></a>Добавление новых каталогов в существующее развертывание
Предположим, что вы развернули Azure AD Connect с включенной функцией ConsistencyGuid и теперь хотите добавить в развертывание другой каталог. При попытке добавить каталог мастер Azure AD Connect проверяет состояние атрибута mSDS-ConsistencyGuid в каталоге. Если атрибут настроен в одном или нескольких объектах в каталоге, то мастер делает вывод, что он используется другими приложениями и возвращает ошибку, как показано на снимке экрана ниже. Если вы уверены, что атрибут не используется существующими приложениями, то необходимо обратиться в службу поддержки для получения сведений об устранении этой ошибки.

![Добавление новых каталогов в существующее развертывание](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Вход в Azure AD
При интеграции локального каталога с Azure AD важно понять, как параметры синхронизации могут повлиять на способ проверки подлинности, используемый пользователем. Для аутентификации пользователя служба Azure AD использует имя участника-пользователя (userPrincipalName). Однако при синхронизации пользователей необходимо тщательно выбирать атрибут, который будет использоваться в качестве значения userPrincipalName.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Выбор атрибута для userPrincipalName
При выборе атрибута, который будет использоваться в Azure в качестве значения UPN (имени участника-пользователя), следует убедиться в следующем.

* Значения атрибута соответствуют синтаксису имени участника-пользователя (RFC 822), т. е. формат должен быть таким: username@domain.
* Суффикс в значениях соответствует одному из подтвержденных пользовательских доменов в Azure AD.

В стандартных параметрах предполагается, что для атрибута выбрано значение userPrincipalName. Если атрибут userPrincipalName не содержит значение, с помощью которого пользователи должны входить в Azure, выбирайте **выборочную установку**.

### <a name="custom-domain-state-and-upn"></a>Состояние и имя участника-пользователя пользовательского домена
Важно убедиться, что для суффикса имени участника-пользователя используется подтвержденный домен.

Джон (John) является пользователем в contoso.com. Вам нужно, чтобы после синхронизации пользователей с каталогом Azure AD azurecontoso.onmicrosoft.com Джон входил в Azure с помощью локального имени участника-пользователя john@contoso.com. Для этого вам нужно сначала добавить и подтвердить contoso.com как личный домен в службе Azure AD. Только после этого вы сможете синхронизировать пользователей. Если суффикс имени участника-пользователя Джона (contoso.com) не соответствует подтвержденному домену в службе Azure AD, то Azure AD заменяет такой суффикс на contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Не поддерживающие маршрутизацию локальные домены и имена участников-пользователей для Azure AD
В некоторых организациях используются домены, не поддерживающие маршрутизацию, например contoso.local, или простые одноуровневые домены наподобие contoso. В службе Azure AD нельзя подтвердить не поддерживающий маршрутизацию домен. Служба Azure AD Connect может синхронизироваться только с подтвержденным доменом в Azure AD. При создании каталога Azure AD служба создает маршрутизируемый домен (например, contoso.onmicrosoft.com), который становится для Azure AD доменом по умолчанию. Поэтому в данной ситуации возникает необходимость подтвердить любой другой маршрутизируемый домен, на случай если вы не хотите синхронизироваться с доменом onmicrosoft.com по умолчанию.

Дополнительные сведения о добавлении и подтверждении доменов см. в статье [Добавление имени личного домена в Azure Active Directory](../active-directory-domains-add-azure-portal.md).

Azure AD Connect определяет, когда используется не поддерживающая маршрутизацию доменная среда и соответствующим образом предупреждает, чтобы вы не продолжали, не изменив стандартные параметры. Если вы работаете в домене, который не поддерживает маршрутизацию, имена участников-пользователей, скорее всего, тоже имеют не поддерживающий маршрутизацию суффикс. Например, если вы работаете в домене contoso.local, служба Azure AD Connect предлагает вместо стандартных параметров использовать настраиваемые. С помощью настраиваемых параметров можно указать атрибут, который следует использовать в качестве имени участника-пользователя для входа в Azure после синхронизации пользователей с Azure AD.

## <a name="next-steps"></a>Дальнейшие действия
Узнайте больше об [интеграции локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).
