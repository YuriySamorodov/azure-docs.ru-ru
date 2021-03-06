---
title: "Руководство по интеграции Azure Active Directory с BeeLine | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и BeeLine."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 93acbd90bbe5f0a40bf3f56edb766a0fdd30f68f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Руководство по интеграции Azure Active Directory с Beeline

В этом руководстве описано, как интегрировать BeeLine с Azure Active Directory (Azure AD).

Интеграция BeeLine с Azure AD обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к BeeLine.
- Вы можете включить автоматический вход пользователей в BeeLine (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с BeeLine, вам потребуется:

- подписка Azure AD;
- подписка BeeLine с активированной функцией единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление BeeLine из коллекции.
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-beeline-from-the-gallery"></a>Добавление BeeLine из коллекции
Чтобы настроить интеграцию приложения BeeLine с Azure AD, необходимо добавить это приложение из коллекции в список управляемых приложений SaaS.

**Чтобы добавить BeeLine из коллекции, выполните следующие действия.**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Приложения][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Приложения][3]

4. В поле поиска введите **BeeLine**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. На панели результатов выберите **BeeLine** и щелкните **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описана настройка и проверка единого входа Azure AD в BeeLine с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в BeeLine соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в BeeLine.

Чтобы установить эту связь, присвойте **имя пользователя** в Azure AD в качестве значения **Имя пользователя** в BeeLine.

Чтобы настроить и проверить единый вход Azure AD в BeeLine, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя BeeLine](#creating-a-beeline-test-user)** требуется для создания пользователя Britta Simon в BeeLine, связанного с соответствующим пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении BeeLine.

**Чтобы настроить единый вход Azure AD в BeeLine, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **BeeLine** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. В разделе **Домены и URL-адреса приложения BeeLine** выполните следующие действия:

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    а. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://projects.beeline.net/<instancename>`

    b. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате:
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Измените их на фактические значения идентификатора и URL-адреса ответа. Чтобы получить эти значения, обратитесь в [службу поддержки BeeLine](https://www.beeline.com/contact-us/).
 
4. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. Приложение Beeline ожидает утверждения SAML в определенном формате. Обратитесь в [службу поддержки BeeLine](https://www.beeline.com/contact-us/), чтобы определить правильный идентификатор пользователя, который будет сопоставляться в приложении. Необходимо выполнить рекомендации от [службы поддержки BeeLine](https://www.beeline.com/contact-us/), касающиеся атрибута, который она хочет использовать для данного сопоставления. Управлять значением этого атрибута можно на вкладке **Атрибуты пользователя** приложения. На следующем снимке экрана приведен пример. Утверждение **Идентификатор пользователя** было сопоставлено с атрибутом **userprincipalname**. Таким образом был получен уникальный идентификатор пользователя, который будет отправляться в приложение Beeline в каждом успешном ответе SAML.

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. В разделе **Настройка BeeLine** щелкните **Настроить BeeLine**, чтобы открыть окно **Настройка единого входа**. Скопируйте **URL-адрес выхода** и **идентификатор сущности SAM** из раздела **Краткий справочник**.

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. Для настройки единого входа на стороне **BeeLine** необходимо отправить скачанный **XML-файл метаданных** и **идентификатор сущности SAML**, **URL-адрес выхода** в [службу поддержки BeeLine](https://www.beeline.com/contact-us/).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    а. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Щелкните **Создать**.
 
### <a name="creating-a-beeline-test-user"></a>Создание тестового пользователя BeeLine

В этом разделе описано, как создать пользователя Britta Simon в приложении Beeline. Перед выполнением единого входа все пользователи должны быть подготовлены в приложении BeeLine. Для этого обратитесь в [службу поддержки BeeLine](https://www.beeline.com/contact-us/). 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к BeeLine, чтобы он мог использовать единый вход Azure.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в BeeLine, выполните следующие действия.**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **BeeLine**.

    ![Настройка единого входа](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа. Щелкнув элемент Beeline на панели доступа, вы автоматически войдете в приложение Beeline.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

