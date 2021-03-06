---
title: "Руководство по организации сети Azure Site Recovery для репликации виртуальных машин из Azure в Azure | Документация Майкрософт"
description: "Руководство по организации сети для репликации виртуальных машин Azure"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: a2ea66f2ed3815d0375001571e64dad338f16e3e
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/30/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Руководство по организации сети для репликации виртуальных машин Azure

>[!NOTE]
> Репликация Azure Site Recovery для виртуальных машин Azure сейчас доступна в предварительной версии.

Эта статья содержит указания по организации сети для Azure Site Recovery при репликации и восстановлении виртуальных машин Azure из одного региона в другой. Дополнительные сведения о необходимых компонентах Azure Site Recovery см. [здесь](site-recovery-support-matrix-azure-to-azure.md).

## <a name="site-recovery-architecture"></a>Архитектура Site Recovery

Site Recovery предоставляет простой способ репликации приложений, работающих на виртуальных машинах Azure, в другой регион Azure, чтобы их можно было восстановить в случае сбоя в основном регионе. Дополнительные сведения об архитектуре этого сценария и Site Recovery см. [здесь](concepts-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Сетевая инфраструктура

На следующей диаграмме показана типичная среда Azure для приложения, работающего на виртуальных машинах Azure:

![customer-environment](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Если вы используете Azure ExpressRoute или VPN-подключение из локальной сети в Azure, среда выглядит следующим образом:

![customer-environment](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Как правило, клиенты защищают свои сети с помощью брандмауэров и (или) групп безопасности сети (NSG). Брандмауэры могут использовать утвержденный список на основе URL или IP-адресов для контроля подключения к сети. Группы безопасности сети разрешают правила для использования диапазонов IP-адресов и управления сетевым подключением.

>[!IMPORTANT]
> Если для управления сетевым подключением используется прокси-сервер, прошедший проверку подлинности, эта функция не поддерживается и Site Recovery невозможно включить.

В следующих разделах рассматриваются изменения в исходящих сетевых подключениях виртуальных машин Azure, необходимые для работы Site Recovery.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Исходящие подключения для URL-адресов Azure Site Recovery

При использовании любого прокси-сервера брандмауэра на основе URL-адреса для управления исходящими подключениями, внесите в список этих разрешений URL-адреса службы Azure Site Recovery:


**URL-адрес** | **Назначение**  
--- | ---
*.blob.core.windows.net | Это необходимо, чтобы данные можно было записать в учетную запись хранения кэша в исходном регионе из виртуальной машины.
login.microsoftonline.com | Требуется для авторизации и проверки подлинности URL-адресов службы Site Recovery.
*.hypervrecoverymanager.windowsazure.com | Требуется для обмена данными между службой Site Recovery и виртуальной машиной.
*.servicebus.windows.net | Необходимые для записи данные наблюдения и диагностики Site Recovery из виртуальной машины.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Исходящие подключения для диапазона IP-адресов Azure Site Recovery

>[!NOTE]
> Чтобы автоматически создать необходимые правила NSG в группе безопасности сети, [скачайте и используйте этот скрипт](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Рекомендуется создать необходимые правила NSG в тестовой группе безопасности сети и проверить наличие проблем, прежде чем создавать правила для рабочей группы безопасности.
> * Чтобы создать необходимое количество правил NSG, убедитесь, что ваша подписка есть в списке разрешений. Обратитесь в службу поддержки, чтобы увеличить ограничение правил NSG в вашей подписке.

При использовании любого прокси-сервера брандмауэра на основе IP-адресов или правил NSG для управления исходящими подключениями следующие диапазоны IP-адресов необходимо добавить в список разрешений в зависимости от расположения исходных и целевых виртуальных машин:

- Все диапазоны IP-адресов в соответствии с исходным расположением. (Вы можете скачать [диапазоны IP-адресов](https://www.microsoft.com/download/confirmation.aspx?id=41653)). Список разрешений необходим, чтобы данные можно было записать в учетную запись хранения кэша из виртуальной машины.

- Все диапазоны IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365.

    >[!NOTE]
    > Если новые IP-адреса добавляются в диапазон IP-адресов Office 365 в будущем, необходимо создать новые правила NSG.

- IP-адреса конечной точки службы Site Recovery ([ доступны в XML-файле](https://aka.ms/site-recovery-public-ips)), которые зависят от вашего целевого расположения:

   **Целевое расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
   --- | --- | ---
   Восточная Азия | 52.175.17.132 | 13.94.47.61
   Юго-Восточная Азия | 52.187.58.193 | 13.76.179.223
   Центральная Индия | 52.172.187.37 | 104.211.98.185
   Южная Индия | 52.172.46.220 | 104.211.224.190
   Северо-центральный регион США | 23.96.195.247 | 168.62.249.226
   Северная Европа | 40.69.212.238 | 52.169.18.8
   Западная Европа | 52.166.13.64 | 40.68.93.145
   Восток США | 13.82.88.226 | 104.45.147.24
   Запад США | 40.83.179.48 | 104.40.26.199
   Южно-центральный регион США | 13.84.148.14 | 104.210.146.250
   Центральный регион США | 40.69.144.231 | 52.165.34.144
   Восток США 2 | 52.184.158.163 | 40.79.44.59
   Восточная часть Японии | 52.185.150.140 | 138.91.1.105
   Западная часть Японии | 52.175.146.69 | 138.91.17.38
   Южная часть Бразилии | 191.234.185.172 | 23.97.97.36
   Восточная часть Австралии | 104.210.113.114 | 191.239.64.144
   Юго-Восточная часть Австралии | 13.70.159.158 | 191.239.160.45
   Центральная Канада | 52.228.36.192 | 40.85.226.62
   Восточная Канада | 52.229.125.98 | 40.86.225.142
   Западно-центральная часть США | 52.161.20.168 | 13.78.149.209
   Западный регион США 2 | 52.183.45.166 | 13.66.228.204
   Западная часть Великобритании | 51.141.3.203 | 51.141.14.113
   Южная часть Великобритании | 51.140.43.158 | 51.140.189.52
   Юг Соединенного Королевства 2 | 13.87.37.4| 13.87.34.139
   Север Соединенного Королевства | 51.142.209.167 | 13.87.102.68
   Центральная Корея | 52.231.28.253 | 52.231.32.85
   Южная Корея | 52.231.298.185 | 52.231.200.144

## <a name="sample-nsg-configuration"></a>Конфигурация примера группы безопасности сети
В этом разделе описывается настройка правил NSG для выполнения репликации Site Recovery на виртуальной машине. При использовании правил NSG для управления исходящими подключениями используйте правила "Разрешить исходящие подключения по HTTPS" для всех необходимых диапазонов IP-адресов.

>[!Note]
> Чтобы автоматически создать необходимые правила NSG в группе безопасности сети, [скачайте и используйте этот скрипт](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Например, если исходное расположение виртуальной машины — "Восточная часть США", а целевое расположении репликации — "Центральная часть США", выполните действия, описанные в следующих двух разделах.

>[!IMPORTANT]
> * Рекомендуется создать необходимые правила NSG в тестовой группе безопасности сети и проверить наличие проблем, прежде чем создавать правила для рабочей группы безопасности.
> * Чтобы создать необходимое количество правил NSG, убедитесь, что ваша подписка есть в списке разрешений. Обратитесь в службу поддержки, чтобы увеличить ограничение правил NSG в вашей подписке.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>Правила NSG в группе безопасности сети восточной части США

* Создайте правила, которые соответствуют [диапазонам IP-адресов восточной части США](https://www.microsoft.com/download/confirmation.aspx?id=41653). Список разрешений необходим, чтобы данные можно было записать в учетную запись хранения кэша из виртуальной машины.

* Создайте правила для всех диапазонов IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP V4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365.

* Создайте правила, которые соответствуют целевому расположению.

   **Расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
    --- | --- | ---
   Центральный регион США | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>Правила NSG в группе безопасности сети центральной части США

Эти правила необходимы, чтобы включить репликацию из целевого в исходный регион после отработки отказа:

* Создайте правила, которые соответствуют [диапазонам IP-адресов центральной части США](https://www.microsoft.com/download/confirmation.aspx?id=41653). Список разрешений необходим, чтобы данные можно было записать в учетную запись хранения кэша из виртуальной машины.

* Правила для всех диапазонов IP-адресов, которые соответствуют [конечным точкам проверки подлинности и удостоверениям IP v4](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) Office 365.

* Правила, соответствующие исходному расположению.

   **Расположение** | **IP-адреса службы Site Recovery** |  **IP-адрес мониторинга Site Recovery**
    --- | --- | ---
   Восток США | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Рекомендации для существующей конфигурации ExpressRoute или VPN из Azure в локальную среду

При наличии канала ExpressRoute или VPN-подключения между локальным и исходным расположением в Azure следуйте указаниям, изложенным в этом разделе.

### <a name="forced-tunneling-configuration"></a>Конфигурация с принудительным туннелированием

В типовой клиентской конфигурации определяется собственный основной маршрут (0.0.0.0/0), который направляет исходящий интернет-трафик в локальное расположение. Мы не рекомендуем этот вариант. Трафик репликации и взаимодействие служб Site Recovery должны оставаться в рамках Azure. Решение предназначено для добавления определяемых пользователем маршрутов (UDRs) для [этих диапазонов IP-](#outbound-connectivity-for-azure-site-recovery-ip-ranges) , чтобы трафик репликации не приходил в локальной среде.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Взаимодействие между целевым и локальным расположением

Руководствуйтесь следующими рекомендациями для подключений между целевым расположением и локальным расположением.
- Если приложению требуется подключаться к локальным компьютерам или если есть клиенты, подключающиеся к приложению из локальной среды через VPN или ExpressRoute, убедитесь, что установлено по крайней мере [подключение типа "сайт — сайт"](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) между целевым регионом Azure и локальным центром обработки данных.

- Если предполагается большой объем трафика между целевым регионом Azure и локальным центром обработки данных, между ними необходимо создать другое [подключение ExpressRoute](../../expressroute/expressroute-introduction.md).

- Если вы хотите сохранить IP-адреса виртуальных машин после отработки отказа, оставьте подключение "сайт — сайт" и ExpressRoute целевого региона отключенным. Это необходимо для предупреждения конфликта между диапазонами IP-адресов исходного и целевого региона.

### <a name="best-practices-for-expressroute-configuration"></a>Советы и рекомендации по конфигурации ExpressRoute
Следуйте следующим рекомендациям по конфигурации ExpressRoute.

- Необходимо создать канал ExpressRoute в исходном и целевом регионах. Затем необходимо создать подключение между:
  - Исходной виртуальной сетью и каналом ExpressRoute.
  - Целевой виртуальной сетью и каналом ExpressRoute.

- В качестве части стандартного ExpressRoute можно создать каналы в том же геополитическом регионе. Для создания каналов ExpressRoute в разных геополитических регионах требуется Azure ExpressRoute уровня Premium, который предусматривает дополнительные издержки. (Если вы уже используете ExpressRoute уровня "Премиум", дополнительная плата не взимается). Дополнительные сведения см. в разделе [Регионы Azure с расположениями ExpressRoute в пределах геополитических регионов](../../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) и на [странице цен на ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

- Мы рекомендуем использовать разные диапазоны IP-адресов в исходном и целевом регионе. Канал ExpressRoute не сможет одновременно подключиться к с двум виртуальными сетями Azure с одинаковым диапазоном IP-адресов.

- Вы можете создать виртуальные сети с одинаковым диапазоном IP-адресов в обоих регионах, а затем создать каналы ExpressRoute в обоих регионах. В случае отработки отказа отключите канал от исходной виртуальной сети и подключите к целевой виртуальной сети.

 >[!IMPORTANT]
 > Если основной регион станет полностью недоступным, возможен сбой операции отключения. Это сделает невозможным взаимодействие целевой виртуальной сети с ExpressRoute.

## <a name="next-steps"></a>Дальнейшие действия
Включите защиту рабочих нагрузок, [выполнив репликацию виртуальных машин Azure](azure-to-azure-quickstart.md).
