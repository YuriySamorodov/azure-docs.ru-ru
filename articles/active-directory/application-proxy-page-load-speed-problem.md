---
title: "Загрузка приложения прокси приложения занимает слишком много времени | Документы Майкрософт"
description: "Устранение проблем с производительностью загрузки страницы в прокси приложения Azure AD."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Загрузка приложения прокси приложения занимает слишком много времени

Эта статья описывает, почему приложение прокси приложения Azure AD может загружаться слишком долго и как устранить эту проблему.

## <a name="overview"></a>Обзор
Если приложение работает, но наблюдается большая задержка, можно использовать некоторые аспекты топологии сети для повышения скорости. Оценку различных топологий см. в [документе с рекомендациями по сети](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Если эти рекомендации не помогают, других советов по настройке производительности у нас, к сожалению, пока нет. По мере того, как прокси-служба приложения охватывает все больше центров обработки данных, которые могут находиться ближе к вам, вы можете заметить сокращение задержки. Чтобы просмотреть полный список центров обработки данных Azure, см. [страницу проверки задержки](http://www.azurespeed.com/Azure/Latency). 

Центры обработки данных с прокси-службой приложения можно найти с помощью [средства проверки портов соединителя](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Отзывы о расположениях центров обработки данных для прокси приложения 
Могут существовать центры обработки данных Azure, которые пока не используют прокси приложения, но способны значительно сократить вашу задержку. Сообщения о расположении центров обработки данных направляйте на адрес <aadapfeedback@microsoft.com>, чтобы мы могли учитывать ваши отзывы при планировании расширения.

Мы работаем над некоторыми дополнительными возможностями, помогающими сократить задержку для клиентов, которые сталкиваются со слишком большими ее значениями, и обязательно поделимся с вами документацией, когда она станет доступна.

## <a name="next-steps"></a>Дальнейшие действия
[Работа с имеющимися локальными прокси-серверами](application-proxy-working-with-proxy-servers.md)
