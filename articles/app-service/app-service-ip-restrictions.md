---
title: "Ограничения IP-адресов в службе приложений Azure | Документация Майкрософт"
description: "Как использовать ограничения IP-адресов при помощи службы приложений Azure"
author: btardif
manager: stefsch
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/23/2017
ms.author: byvinyal
ms.openlocfilehash: 5fbd308e9f037038ad867f3d242da6573bc67081
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2017
---
# <a name="azure-app-service-static-ip-restrictions"></a>Ограничения статических IP-адресов в службе приложений Azure #

Ограничения IP-адресов позволяют определить список IP-адресов, с которых разрешен доступ к вашему приложению. Список разрешений может содержать отдельные IP-адреса или их диапазон, который определяется маской подсети.

Когда клиент отправляет запрос к приложению, его IP-адрес сравнивается с адресами из списка разрешений. Если IP-адрес отсутствует в списке, приложение возвращает код состояния [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

Ограничения IP-адресов определяются в файле web.config, который приложение использует в среде выполнения. При определенных обстоятельствах некоторые модули в конвейере HTTP могут выполняться до логики ограничений IP. В этом случае происходит сбой запроса с другим кодом ошибки HTTP.

Ограничения IP-адресов определяются для экземпляров плана службы приложений, присвоенных вашему приложению.

Чтобы добавить правило ограничения IP-адреса приложения, в меню откройте **Сеть**>**Ограничения IP-адресов** и выберите команду **Настроить ограничения IP-адресов**

![Ограничения IP-адресов] (media/app-service-ip-restrictions/ip-restrictions.png)

Здесь можно просмотреть список правил для ограничения IP-адресов, которые определены для приложения.

![список ограничений IP-адресов](media/app-service-ip-restrictions/browse-ip-restrictions.png)

Можно щелкнуть **[+] Добавить**, чтобы добавить новое правило ограничения IP-адресов.

![добавление ограничений IP-адресов](media/app-service-ip-restrictions/add-ip-restrictions.png)
