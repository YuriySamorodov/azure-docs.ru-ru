---
title: "Выполнение проверки доступа для участников группы или пользователей с доступом к приложению с помощью Azure AD | Документация Майкрософт"
description: "Узнайте, как завершить проверку доступа для участников группы или пользователей с доступом к приложению в Azure Active Directory."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b301ff06c01d51c02f7d7393801b35cd8965403c
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/02/2017
---
# <a name="complete-an-access-review-of-members-of-a-group-or-users-access-to-an-application-in-azure-ad"></a>Выполнение проверки доступа для участников группы или пользователей с доступом к приложению с помощью Azure AD

Администраторы могут использовать Azure Active Directory (Azure AD), чтобы [создать проверку доступа](active-directory-azure-ad-controls-create-access-review.md) для участников группы или пользователей, назначенных для приложения. Azure AD автоматически отправляет рецензентам сообщение, в котором им предлагается проверить доступ. Если пользователь не получил сообщение, ему можно отправить [инструкции по проверке доступа](active-directory-azure-ad-controls-perform-access-review.md). По завершении периода проверки доступа или после того, как администратор остановил проверку, следуйте инструкциям в этой статье, чтобы просмотреть и применить результаты.

## <a name="view-an-access-review-in-the-azure-portal"></a>Просмотр проверки доступа на портале Azure

1. Перейдите к [странице проверок доступа](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/), а затем на вкладку **Программы** и выберите программу, содержащую элемент управления проверки доступа.

2. Щелкните **Управление** и выберите элемент управления проверки доступа. Если в программе существует множество элементов управления, можно отфильтровать их по определенному типу и отсортировать по состоянию. Также можно найти имя элемента управления проверки доступа или отображаемое имя владельца, который его создал. 

## <a name="stop-a-review-that-hasnt-finished"></a>Остановка проверки, которая не завершена

Если еще не наступила запланированная дата окончания проверки, администратор может выбрать **Остановить**, чтобы завершить проверку раньше. После остановки проверки пользователи больше не смогут быть проверены. Невозможно перезапустить проверку после ее остановки.

## <a name="apply-the-changes"></a>Применение изменений 

После завершения проверки доступа из-за того, что была достигнута ее дата окончания или администратор остановил ее вручную, выберите **Применить**. Результат проверки применяется путем обновления группы или приложения. Если пользователю было отказано в доступе во время проверки, когда администратор нажмет эту кнопку, Azure AD удалит членство или назначение в приложении пользователя. 

Нажатие кнопки **Применить** не влияет на динамическую или на созданную в локальном каталоге группу. Если вы хотите изменить группу, которая создана в локальной среде, скачайте результаты и примените изменения к представлению группы в этом каталоге.

## <a name="download-the-results-of-the-review"></a>Скачивание результатов проверки

Чтобы получить результаты проверки, щелкните **Утверждения**, а затем выберите **Скачать**. Полученный CSV-файл можно просмотреть в Excel или в других программах, в которых открываются CSV-файлы.

## <a name="optional-delete-a-review"></a>Удаление проверки (необязательно)
Если проверка больше не нужна, ее можно удалить. Выберите **Удалить**, чтобы удалить проверку из Azure AD.

> [!IMPORTANT]
> При удалении проверки предупреждение не отображается. Поэтому убедитесь, что выбрана нужная проверка.
> 
> 

## <a name="next-steps"></a>Дальнейшие действия

- [Управление пользовательским доступом с помощью проверок доступа Azure AD](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
- [Управление гостевым доступом с помощью проверок доступа Azure AD](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
- [Управление программами и их элементами управления](active-directory-azure-ad-controls-manage-programs-controls.md)
- [Создание проверки доступа для членства в группе или работы с приложением](active-directory-azure-ad-controls-create-access-review.md)
- [Создание проверки доступа для пользователей в роли администратора Azure AD](active-directory-privileged-identity-management-how-to-start-security-review.md)
