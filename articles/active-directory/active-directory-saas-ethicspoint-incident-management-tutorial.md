---
title: "Руководство. Интеграция Azure Active Directory с решением EthicsPoint Incident Management (EPIM) | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и EthicsPoint Incident Management (EPIM)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: b5ac3afd973b5765ba151e766754934b49ac0e0c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Руководство. Интеграция Azure Active Directory с решением EthicsPoint Incident Management (EPIM)

В этом руководстве описано, как интегрировать решение EthicsPoint Incident Management (EPIM) с Azure Active Directory (Azure AD).

Интеграция EthicsPoint Incident Management (EPIM) с Azure AD обеспечивает следующие преимущества.

- С помощью Azure AD можно контролировать доступ к EthicsPoint Incident Management (EPIM).
- Вы можете включить автоматический вход пользователей в EthicsPoint Incident Management (EPIM) (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы настроить интеграцию Azure AD с EthicsPoint Incident Management (EPIM), вам потребуется следующее:

- подписка Azure AD;
- подписка EthicsPoint Incident Management (EPIM) с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление решения EthicsPoint Incident Management (EPIM) из коллекции.
2. Настройка и проверка единого входа в Azure AD

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>Добавление решения EthicsPoint Incident Management (EPIM) из коллекции
Чтобы настроить интеграцию EthicsPoint Incident Management (EPIM) с Azure AD, вам потребуется добавить EthicsPoint Incident Management (EPIM) из коллекции в список управляемых приложений SaaS.

**Чтобы добавить EthicsPoint Incident Management (EPIM) из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Приложения][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Приложения][3]

4. В поле поиска введите **EthicsPoint Incident Management (EPIM)**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. На панели результатов выберите **EthicsPoint Incident Management (EPIM)** и нажмите кнопку **Добавить**, чтобы добавить приложение.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD
В этом разделе описывается настройка и проверка единого входа Azure AD в EthicsPoint Incident Management (EPIM) с помощью тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо указать, какой пользователь в EthicsPoint Incident Management (EPIM) соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в EthicsPoint Incident Management (EPIM).

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в EthicsPoint Incident Management (EPIM).

Чтобы настроить и проверить единый вход Azure AD в EthicsPoint Incident Management (EPIM), вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа в Azure AD](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя EthicsPoint Incident Management (EPIM)](#creating-a-ethicspoint-incident-management-epim-test-user)** требуется для создания пользователя Britta Simon в EthicsPoint Incident Management (EPIM), связанного с одноименным пользователем в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

Цель этого раздела — включить единый вход Azure AD на портале Azure и настроить его в EthicsPoint Incident Management (EPIM).

**Чтобы настроить единый вход Azure AD в EthicsPoint Incident Management (EPIM), сделайте следующее:**

1. На портале Azure на странице интеграции с приложением **EthicsPoint Incident Management (EPIM)** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. В разделе **Домены и URL-адреса приложения EthicsPoint Incident Management (EPIM)** выполните указанные ниже действия.

    ![Настройка единого входа](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате:
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    b. В текстовом поле **Идентификатор** введите URL-адрес в следующем формате: `https://<companyname>.navexglobal.com/adfs/services/trust`

    c. В текстовом поле **URL-адрес ответа** введите URL-адрес в следующем формате: `https://<servername>.navexglobal.com/adfs/ls/`.

    > [!NOTE] 
    > Эти значения приведены в качестве примера. Укажите вместо них фактические значения URL-адреса ответа, идентификатора и URL-адреса входа. Чтобы получить эти значения, обратитесь в [службу поддержки клиентов EthicsPoint Incident Management (EPIM)](http://www.navexglobal.com/company/contact-us). 

4. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Настройка единого входа](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. Для настройки единого входа на стороне **EthicsPoint Incident Management (EPIM)** необходимо отправить скачанный **XML-файл метаданных** в [службу поддержки EthicsPoint Incident Management (EPIM)](http://www.navexglobal.com/company/contact-us).

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD
Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    а. В текстовом поле **Имя** введите **BrittaSimon**.

    b. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Щелкните **Создать**.
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a>Создание тестового пользователя EthicsPoint Incident Management (EPIM)

В этом разделе описано, как создать пользователя Britta Simon в EthicsPoint Incident Management (EPIM). Чтобы добавить пользователей на платформу EthicsPoint Incident Management (EPIM), обратитесь в [службу поддержки EthicsPoint Incident Management (EPIM)](http://www.navexglobal.com/company/contact-us).

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как разрешить пользователю Britta Simon использовать единый вход Azure, предоставив доступ к EthicsPoint Incident Management (EPIM).

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в EthicsPoint Incident Management (EPIM), сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **EthicsPoint Incident Management (EPIM)**.

    ![Настройка единого входа](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.
Щелкнув плитку EthicsPoint Incident Management (EPIM) на панели доступа, вы автоматически войдете в приложение EthicsPoint Incident Management (EPIM).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

