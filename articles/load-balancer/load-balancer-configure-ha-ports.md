---
title: "Настройка высокодоступных портов для Azure Load Balancer | Документы Майкрософт"
description: "Сведения о том, как использовать высокодоступные порты для балансировки нагрузки внутреннего трафика на всех портах."
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2017
ms.author: kumud
ms.openlocfilehash: 4cd65c01d75af8539f5fa13dbbd2aaec548aea0b
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>Настройка высокодоступных портов для внутреннего балансировщика нагрузки

Эта статья содержит пример развертывания высокодоступных портов для внутреннего балансировщика нагрузки. Сведения о конкретных конфигурациях виртуальных сетевых устройств (NVA) см. на веб-сайтах соответствующих поставщиков.

>[!NOTE]
> Сейчас компонент "Порты с высоким уровнем доступности" находится на этапе предварительной версии. В период действия предварительной версии эта функция может не отличаться таким же уровнем доступности и надежности, как функции, предоставляемые в общедоступной версии. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

На рис. 1 приведена конфигурация для примера развертывания, описанного в этой статье:
- Виртуальные сетевые устройства развертываются в серверном пуле внутреннего балансировщика нагрузки с конфигурацией высокодоступных портов. 
- UDR, применяемый в подсети промежуточной сети, направляет весь трафик на виртуальные сетевые устройства, совершая следующий прыжок по виртуальному IP-адресу внутреннего балансировщика нагрузки. 
- Внутренний балансировщик нагрузки распределяет трафик на одно из активных виртуальных сетевых устройств согласно алгоритму балансировки нагрузки.
- Виртуальное сетевое устройство обрабатывает трафик и перенаправляет его исходное расположение в подсети серверной части.
- Обратный путь также может использовать тот же маршрут, если в подсети серверной части настроен соответствующий UDR. 

![Пример развертывания высокодоступных портов](./media/load-balancer-configure-ha-ports/haports.png)

Рис. 1. Виртуальные сетевые устройства, развернутые в зоне действия внутреннего балансировщика нагрузки с высокодоступными портами 

## <a name="preview-sign-up"></a>Регистрация в предварительной версии

Чтобы зарегистрироваться в предварительной версии компонента "Порты с высоким уровнем доступности" в Load Balancer уровня "Стандартный", зарегистрируйте свою подписку, чтобы получить доступ, используя Azure CLI 2.0 или PowerShell.  Зарегистрируйте подписку для:

1. [Предварительной версии Load Balancer уровня "Стандартный"](https://aka.ms/lbpreview#preview-sign-up). 
2. [Предварительной версии высокодоступных портов](https://aka.ms/haports#preview-sign-up).

>[!NOTE]
>Чтобы использовать эту функцию, помимо регистрации для использования компонента "Порты с высоким уровнем доступности" необходимо также зарегистрироваться для получения [предварительной версии](https://aka.ms/lbpreview#preview-sign-up) Load Balancer уровня "Стандартный". На регистрацию для получения предварительных версий компонента "Порты с высоким уровнем доступности" и Load Balancer уровня "Стандартный" может уйти до часа.

## <a name="configuring-ha-ports"></a>Настройка высокодоступных портов

Конфигурация высокодоступных портов включает в себя настройку внутреннего балансировщика нагрузки с виртуальными сетевыми устройствами в серверном пуле, конфигурацию зонда балансировщика нагрузки для определения работоспособности виртуальных сетевых устройств, а также правило балансировщика нагрузки с высокодоступными портами. Общая конфигурация для балансировщика нагрузки описана в статье [Начало работы](load-balancer-get-started-ilb-arm-portal.md). Эта статья посвящена конфигурации высокодоступных портов.

Конфигурация включает в себя установку значения **0** для интерфейсного порта и внутреннего порта, а также значения **Все** для протокола. Эта статья описывает, как настроить высокодоступные порты с помощью портала Azure, PowerShell и Azure CLI 2.0.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Настройка правила балансировщика нагрузки с высокодоступными портами с помощью портала Azure

Для этой конфигурации на портале Azure присутствует флажок **Порты высокой доступности**. При его установке соответствующая конфигурация портов и протокола заполняется автоматически. 

![Настройка высокодоступных портов через портал Azure](./media/load-balancer-configure-ha-ports/haports-portal.png)

Рис. 2. Настройка высокодоступных портов через портал

### <a name="configure-ha-ports-lb-rule-via-resource-manager-template"></a>Настройка правила балансировки нагрузки для высокодоступных портов с помощью шаблона Resource Manager

Вы можете настроить высокодоступные порты с помощью версии API 2017-08-01 для Microsoft.Network/loadBalancers в ресурсе Load Balancer. В следующем фрагменте кода JSON показаны изменения в конфигурации подсистемы балансировки нагрузки для высокодоступных портов через REST API.

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>Настройка правила балансировщика нагрузки с высокодоступными портами с помощью PowerShell

Воспользуйтесь следующей командой, чтобы создать правило балансировщика нагрузки с высокодоступными портами при создании внутреннего балансировщика нагрузки с помощью PowerShell:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Настройка правила балансировщика нагрузки с высокодоступными портами с помощью Azure CLI 2.0

На шаге 4 процедуры [создания набора внутреннего балансировщика нагрузки](load-balancer-get-started-ilb-arm-cli.md) используйте следующую команду, чтобы создать правило балансировщика нагрузки с высокодоступными портами.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [высокодоступных портах](load-balancer-ha-ports-overview.md)
