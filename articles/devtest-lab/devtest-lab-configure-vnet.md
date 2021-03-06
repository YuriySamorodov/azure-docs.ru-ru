---
title: "Настройка виртуальной сети в Azure DevTest Labs | Документация Майкрософт"
description: "Узнайте, как настроить существующую виртуальную сеть и подсеть и использовать их на виртуальной машине в Azure DevTest Labs."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: tarcher
ms.openlocfilehash: 4b4c91805a7d5cbf37c8ba3fa3248e7cb0eb02b0
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Настройка виртуальной сети в Azure DevTest Labs
Как описано в статье [Добавление виртуальной машины с артефактами в лабораторию](devtest-lab-add-vm-with-artifacts.md), при создании виртуальной машины в лаборатории можно указать настроенную виртуальную сеть. Например, может потребоваться доступ к ресурсам корпоративной сети с виртуальных машин через виртуальную сеть, которая была настроена с помощью ExpressRoute или через VPN-подключение типа "сеть — сеть".

В этой статье описывается, как добавить существующую виртуальную сеть в настройки виртуальной сети лаборатории, чтобы она предлагалась для выбора при создании виртуальных машин.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Настройка виртуальной сети для лаборатории с помощью портала Azure
Ниже пошагово описывается процедура добавления существующей виртуальной сети (и подсети) в лабораторию, чтобы ее можно было использовать при создании виртуальной машины в этой лаборатории. 

1. Войдите на [портал Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Щелкните **Больше служб**, а затем выберите в списке **DevTest Labs**.
1. Из списка лабораторий выберите нужную лабораторию. 
1. В главной области лаборатории выберите **Конфигурация и политики**.

    ![Открытие области "Конфигурация и политики" лаборатории](./media/devtest-lab-configure-vnet/policies-menu.png)
1. В разделе **Внешние ресурсы** выберите **Виртуальные сети**. Отобразится список виртуальных сетей, настроенных для текущей лаборатории, а также виртуальная сеть по умолчанию, созданная для этой лаборатории. 
1. Щелкните **+ Добавить**.
   
    ![Добавление существующей виртуальной сети в лабораторию](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. В области **Виртуальная сеть** выберите **[Выбрать виртуальную сеть]**.
   
    ![Выбор существующей виртуальной сети](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. В области **Выбор виртуальной сети** выберите нужную виртуальную сеть. Отобразится список всех виртуальных сетей, которые находятся в том же регионе подписки, что и лаборатория.
1. После выбора виртуальной сети вы вернетесь в область **Виртуальная сеть**. Выберите подсеть из списка внизу.

    ![Список подсетей](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Отобразится область "Подсеть лаборатории".

    ![Область "Подсеть лаборатории"](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Укажите **Lab subnet name** (Имя подсети лаборатории).
   - Чтобы подсеть можно было использовать при создании виртуальной машины лаборатории, выберите **Use in virtual machine creation** (Использовать при создании виртуальной машины).
   - Чтобы включить [совместное использование общедоступного IP-адреса](devtest-lab-shared-ip.md), выберите **Enable shared public IP** (Включить совместное использование общедоступного IP-адреса).
   - Чтобы разрешить использование общедоступных IP-адресов в подсети, выберите **Allow public IP creation** (Разрешить создание общедоступных IP-адресов).
   - В поле **Maximum virtual machines per user** (Максимальное число виртуальных машин на пользователя) укажите максимальное число виртуальных машин на пользователя для каждой подсети. Чтобы разрешить использовать неограниченное число виртуальных машин, оставьте это поле пустым.
1. Нажмите кнопку **ОК**, чтобы закрыть колонку "Подсеть лаборатории".
1. Нажмите кнопку **Сохранить**, чтобы закрыть область "Виртуальная сеть".

После настройки виртуальной сети ее можно выбирать при создании виртуальной машины. Сведения о создании виртуальной машины и о выборе виртуальной сети см. в статье [Добавление виртуальной машины с артефактами в лабораторию в Azure DevTest Labs](devtest-lab-add-vm-with-artifacts.md). 

[Документация по виртуальной сети](https://docs.microsoft.com/azure/virtual-network) Azure содержит дополнительные сведения об использовании виртуальных сетей, включая инструкции по настройке виртуальной сети, управлению ею и ее подключению к вашей локальной сети.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Дальнейшие действия
После добавления в лабораторию нужной виртуальной сети следует [добавить виртуальную машину в лабораторию](devtest-lab-add-vm-with-artifacts.md).

