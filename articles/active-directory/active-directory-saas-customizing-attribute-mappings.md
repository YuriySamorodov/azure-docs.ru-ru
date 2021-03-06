---
title: "Настройка сопоставлений атрибутов в Azure AD | Документация Майкрософт"
description: "Узнайте, что такое сопоставление атрибутов для приложений SaaS в Azure Active Directory и как настроить его в соответствии с потребностями вашего бизнеса."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6921ca86efeea9d1255bb2d1773f55daa48b9b4a
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Настройка сопоставлений атрибутов для подготовки пользователей для приложений SaaS в Azure Active Directory
Microsoft Azure AD обеспечивает поддержку для подготовки пользователей для сторонних приложений SaaS, например Salesforce, Google Apps и др. При наличии подготовки пользователей для сторонних приложений SaaS портал управления Azure определяет значения их атрибутов в форме настройки "сопоставление атрибутов".

Существует набор предварительно настроенных сопоставлений атрибутов между объектами пользователей Azure AD и каждого приложения SaaS. Некоторые приложения управляют другими типами объектов, такими как группы или контакты. <br> 
 Сопоставления атрибута по умолчанию можно настроить в соответствии с потребностями вашего бизнеса. Это означает, что можно изменить или удалить существующие сопоставления атрибутов или создать новые.

Чтобы открыть эту функцию на портале Azure AD, щелкните конфигурацию **Сопоставления** в разделе **Подготовка**, который находится в подразделе **Управление** раздела **Корпоративное приложение**.


![Salesforce][5] 

При щелчке по конфигурации **Сопоставления** откроется соответствующая колонка **Сопоставление атрибута**.  
Существуют сопоставления атрибутов, которые необходимы для правильной работы приложения SaaS. Для требуемых атрибутов функция **Удалить** недоступна.


![Salesforce][6]  

В приведенном выше примере вы видите, что атрибут **Username** управляемого объекта в Salesforce заполняется значением **userPrincipalName** связанного объекта Azure Active Directory.

Щелкнув сопоставление, вы можете настроить **Сопоставления атрибутов**. При этом откроется колонка **Изменение атрибута**.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Основные сведения о типах сопоставления атрибутов
С помощью сопоставлений атрибутов можно управлять заполнением атрибутов в сторонних приложениях SaaS. Поддерживаются четыре типа сопоставлений.

* **Прямое** — целевой атрибут заполняется значением атрибута связанного объекта в Azure AD.
* **Постоянное** — целевой атрибут заполняется определенной строкой, указанной вами.
* **Выражение** — целевой атрибут заполняется на основе результата выражения, похожего на сценарий. 
  Дополнительные сведения см. в статье [Запись выражений для сопоставления атрибутов в Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Нет** — целевой атрибут остается без изменений. Тем не менее, если целевой атрибут пуст, он заполняется указанным вами значением по умолчанию.

Кроме этих четырех основных типов сопоставлений атрибутов, в сопоставлениях настраиваемых атрибутов также поддерживается назначение значений **по умолчанию**. Назначение значений по умолчанию гарантирует, что целевой атрибут заполняется значением, если нет значения ни в Azure AD, ни в целевом объекте. В наиболее распространенной конфигурации это поле остается пустым.


## <a name="understanding-attribute-mapping-properties"></a>Основные сведения о свойствах сопоставления атрибутов

В предыдущем разделе мы уже рассказали о свойстве типа для сопоставления атрибута.
Наряду с этим свойством сопоставления атрибутов также поддерживают следующие атрибуты:

- **Исходный атрибут** — атрибут пользователя из исходной системы (например, Azure Active Directory).
- **Целевой атрибут** — атрибут пользователя в целевой системе (например, ServiceNow).
- **Сопоставление объектов с помощью этого атрибута** — должно ли это сопоставление использоваться для уникальной идентификации пользователей между исходной и целевой системами. Этот параметр обычно задается в атрибуте userPrincipalName или mail в Azure AD, который обычно сопоставляется с полем имени пользователя в целевом приложении.
- **Приоритет сопоставления** — возможность указания нескольких атрибутов сопоставления. При указании нескольких атрибутов сопоставления они вычисляются в порядке, заданном в этом поле. Как только соответствие найдено, дальнейшие атрибуты сопоставления не вычисляются.
- **Применить это сопоставление**
    - **Всегда** — применить это сопоставление как при создании, так и при обновлении пользователей
    - **Только при создании** — применить это сопоставление только при создании пользователей


## <a name="what-you-should-know"></a>Необходимая информация

Процесс синхронизации в Microsoft Azure AD реализован очень эффективно. Во время цикла синхронизации в инициализированной среде обрабатываются только объекты, требующие обновления. Обновление сопоставлений атрибутов влияет на производительность цикла синхронизации. Обновление настройки сопоставления атрибута требует повторной оценки всех управляемых объектов. Рекомендуется сократить количество последовательных изменений сопоставлений атрибутов.

## <a name="next-steps"></a>Дальнейшие действия

* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
* [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS](active-directory-saas-app-provisioning.md)
* [Запись выражений для сопоставления атрибутов](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Фильтры области для подготовки пользователей](active-directory-saas-scoping-filters.md)
* [Автоматическая подготовка пользователей и групп из Azure Active Directory в приложениях с использованием SCIM](active-directory-scim-provisioning.md)
* [Уведомления о подготовке учетных записей](active-directory-saas-account-provisioning-notifications.md)
* [Список учебников по интеграции приложений SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

