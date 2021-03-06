---
title: "Начало работы с условным доступом в Azure Active Directory | Документы Майкрософт"
description: "Тестирование условного доступа с помощью условия расположения."
services: active-directory
keywords: "условный доступ к приложениям, условный доступ посредством Azure Active Directory, безопасный доступ к ресурсам организации, политики условного доступа"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/21/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b9a95cea57f4d0a4a927679cd4802bb56420887a
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2017
---
# <a name="get-started-with-conditional-access-in-azure-active-directory"></a>Начало работы с условным доступом в Azure Active Directory

Условный доступ — функция Azure Active Directory, позволяющая вам определить условия, при которых авторизованным пользователям разрешен доступ к вашим приложениям. 

Эта статья содержит инструкции для тестирования условного доступа на основе условия расположения в среде.  


## <a name="scenario-description"></a>Описание сценария

Во многих организациях общим требованием является обязательная многофакторная идентификация для доступа к приложениям, который выполняется не из корпоративной интрасети. Вы легко можете реализовать это с помощью Azure Active Directory, настроив политику условного доступа на основе расположения. В этой статье вы найдете подробные инструкции по настройке соответствующей политики. С помощью [надежных IP-адресов](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips) политика отличает попытки доступа из корпоративной интрасети от попыток доступа из остальных расположений.


## <a name="prerequisites"></a>Предварительные требования

В сценарии из этой статьи предполагается, что вы знакомы с основными понятиями, описанными в статье об [условном доступе Azure Active Directory](active-directory-conditional-access-azure-portal.md).

Чтобы протестировать этот сценарий, вам потребуется выполнить следующие шаги.

- Создать тестового пользователя. 

- Назначить лицензию Azure AD Premium тестовому пользователю.

- Настроить управляемое приложение и назначить ему тестового пользователя.

- Настроить надежные IP-адреса.

Дополнительные сведения о надежных IP-адресах см. в разделе [Надежные IP-адреса](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).


## <a name="policy-configuration-steps"></a>Настройка политики

**Чтобы настроить политику условного доступа, выполните следующие действия.**

1. На портале Azure на панели навигации слева щелкните **Azure Active Directory**. 

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/01.png)

2. В колонке **Azure Active Directory** в разделе **Управление** щелкните **Условный доступ**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/02.png)
 
3. В колонке **Условный доступ** на панели инструментов сверху нажмите кнопку **Добавить**, чтобы открыть колонку **Создать**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/03.png)

4. В колонке **Создать** в текстовом поле **Имя** введите имя политики.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/04.png)

5. В разделе **Назначение** щелкните **Пользователи и группы**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/05.png)

6. В колонке **Пользователи и группы** выполните следующие действия.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/06.png)

    а. Щелкните **Выбрать пользователей и группы**.

    b. Нажмите кнопку **Выбрать**.

    c. В колонке **Выбрать** выберите тестового пользователя и нажмите кнопку **Выбрать**.

    d. В колонке **Пользователи и группы** нажмите кнопку **Готово**.

7. В колонке **Создать** в разделе **Назначения** щелкните **Облачные приложения**, чтобы открыть колонку **Облачные приложения**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. В колонке **Облачные приложения** выполните следующие действия.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/08.png)

    а. Щелкните **Выбрать приложения**.

    b. Нажмите кнопку **Выбрать**.

    c. В колонке **Выбрать** выберите облачное приложение и нажмите кнопку **Выбрать**.

    d. В колонке **Облачные приложения** нажмите кнопку **Готово**.

9. В колонке **Создать** в разделе **Назначения** щелкните **Условия**, чтобы открыть колонку **Условия**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/09.png)

10. В колонке **Условия** щелкните **Расположения**, чтобы открыть колонку **Расположения**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/10.png)

11. В колонке **Расположения** выполните следующие действия.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/11.png)

    а. Под параметром **Настройка** нажмите кнопку **Да**.

    b. На вкладке **Включить** выберите **Все расположения**.

    c. Откройте вкладку **Исключить** и выберите **All trusted IPs** (Все надежные IP-адреса).

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/12.png)

    d. Нажмите кнопку **Done**(Готово).

12. В колонке **Условия** нажмите кнопку **Готово**.

13. В колонке **Создать** в разделе **Элементы управления** щелкните **Предоставить**, чтобы открыть колонку **Предоставить**.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. В колонке **Предоставить** выполните следующие действия.

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    а. Выберите **Требовать многофакторную проверку подлинности**.

    b. Нажмите кнопку **Выбрать**.

15. В колонке **Создать** в разделе **Включить политику** нажмите кнопку **Вкл.**

    ![Условный доступ](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. В колонке**Создать** щелкните **Создать**.


## <a name="testing-the-policy"></a>Тестирование политики

Чтобы протестировать политику, обратитесь к приложению с устройств, к которым применимы следующие характеристики: 

1. IP-адрес устройства входит в диапазон настроенных надежных IP-адресов. 

1. IP-адрес устройства не входит в диапазон настроенных надежных IP-адресов.

Многофакторная идентификация должна требоваться только во время попытки подключения с устройства, IP-адрес которого не входит в диапазон настроенных надежных IP-адресов. 


## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в статье об [условном доступе в Azure Active Directory](active-directory-conditional-access-azure-portal.md).

