---
title: "Краткое руководство. Создание частного реестра Docker в Azure с помощью PowerShell"
description: "Быстрый способ изучить создание частного реестра контейнеров Docker с помощью Azure PowerShell."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.openlocfilehash: b58b10e644e934cc38a6e0512ba7642ab8bf27c4
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2017
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Создание реестра контейнеров Azure с помощью PowerShell

Реестр контейнеров Azure — это управляемая служба реестра контейнеров Docker, используемая для хранения частных образов контейнеров Docker. В этом руководстве рассматривается создание экземпляра реестра контейнеров Azure с помощью PowerShell.

Для работы с этим кратким руководством требуется модуль Azure PowerShell 3.6 или более поздней версии. Чтобы узнать версию, выполните команду `Get-Module -ListAvailable AzureRM`. Если вам необходимо выполнить установку или обновление, см. статью [об установке модуля Azure PowerShell](/powershell/azure/install-azurerm-ps).

Также необходим локально установленный модуль Docker. Docker содержит пакеты, которые позволяют быстро настроить Docker в любой системе [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) или [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="log-in-to-azure"></a>Вход в Azure

Войдите в подписку Azure с помощью команды `Login-AzureRmAccount` и следуйте инструкциям на экране.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Создать группу ресурсов

Создайте группу ресурсов Azure с помощью командлета [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Группа ресурсов — это логический контейнер, в котором происходит развертывание ресурсов Azure и управление ими.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Создание реестра контейнеров

Создайте экземпляр записи контроля доступа (ACR) с помощью команды [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry).

Имя реестра **должно быть уникальным**. В следующем примере используется имя *myContainerRegistry007*. Замените его уникальным значением.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Вход в ACR

Перед отправкой и извлечением образов контейнеров необходимо войти в экземпляр ACR. Сначала воспользуйтесь командой [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential), чтобы получить учетные данные администратора для экземпляра ACR.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Далее выполните команду [docker login](https://docs.docker.com/engine/reference/commandline/login/), чтобы войти в экземпляр ACR.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

После выполнения эта команда возвращает сообщение Login Succeeded (Вход выполнен).

## <a name="push-image-to-acr"></a>Отправка образа в ACR

Чтобы отправить образ в реестр контейнеров Azure, сначала нужно получить этот образ. При необходимости выполните следующую команду, чтобы извлечь предварительно созданный образ из Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Образ должен быть снабжен тегом имени сервера для входа в ACR. Выполните команду [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry), чтобы получить имя сервера входа для экземпляра ACR.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Присвойте образу тег с помощью команды [docker tag](https://docs.docker.com/engine/reference/commandline/tag/). Замените *acrLoginServer* именем сервера входа для вашего экземпляра ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Наконец, воспользуйтесь командой [docker push](https://docs.docker.com/engine/reference/commandline/push/) для отправки образов в экземпляр ACR. Замените *acrLoginServer* именем сервера входа для вашего экземпляра ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Ненужные группу ресурсов, экземпляр ACR и все образы контейнеров можно удалить с помощью команды [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Дальнейшие действия

В этом кратком руководстве вы создали реестр контейнеров Azure с помощью Azure CLI. Если вы хотите использовать реестр контейнеров Azure со службой "Экземпляры контейнеров Azure", перейдите к соответствующему руководству.

> [!div class="nextstepaction"]
> [Руководство по использованию службы "Экземпляры контейнеров Azure"](../container-instances/container-instances-tutorial-prepare-app.md)