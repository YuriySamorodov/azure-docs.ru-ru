---
title: "Пример скрипта Azure CLI. Создание веб-приложения с непрерывным развертыванием из Visual Studio Team Services | Документация Майкрософт"
description: "Пример скрипта Azure CLI. Создание веб-приложения с непрерывным развертыванием из Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6c10b7654c02fae631e34572f12482928b39ceb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>Создание веб-приложения с непрерывным развертыванием из Visual Studio Team Services

Этот пример сценария создает веб-приложение в службе приложений со связанными ресурсами, а затем настраивает непрерывное развертывание из репозитория Visual Studio Team Services. Для этого примера потребуется:

* Репозиторий Visual Studio Team Services с кодом приложения, для которого у вас есть права администратора.
* [Личный маркер доступа](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) для учетной записи Visual Studio Team Services.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать интерфейс командной строки локально, для работы с этим руководством вам понадобится Azure CLI 2.0 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Пример скрипта

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from Visual Studio Team Services")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Описание скрипта

Этот скрипт использует следующие команды. Для каждой команды в таблице приведены ссылки на соответствующую документацию.

| Команда | Примечания |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Создает группу ресурсов, в которой хранятся все ресурсы. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Создает план службы приложений. |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Создает веб-приложение Azure. |
| [az webapp deployment source config](https://docs.microsoft.com/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) | Связывает веб-приложение Azure с репозиторием Git или Mercurial. |
| [az webapp browse](https://docs.microsoft.com/cli/azure/webapp#az_webapp_browse) | Открывает веб-приложение Azure в браузере. |

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об Azure CLI см. в [документации по Azure CLI](https://docs.microsoft.com/cli/azure/overview).

Дополнительные примеры скриптов Azure CLI для службы приложений см. в [документации по службе приложений Azure](../app-service-cli-samples.md).
