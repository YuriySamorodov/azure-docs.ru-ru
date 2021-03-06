---
title: "Георепликация реестра контейнеров Azure"
description: "Приступите к созданию геореплицированных реестров контейнеров Azure и управлению ими."
services: container-registry
documentationcenter: 
author: SteveLas
manager: balans
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: 
ms.topic: overview-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: stevelas
ms.custom: 
ms.openlocfilehash: 7a05f12e7873b8280dd737b008e186626017125e
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2017
---
# <a name="geo-replication-in-azure-container-registry"></a>Георепликация в реестре контейнеров Azure

Компании, которым необходимо местное присутствие или выполнять оперативное резервное копирование, запускают службы из нескольких регионов Azure. Мы советуем разместить реестр контейнеров в каждом регионе, в котором запускаются образы, чтобы выполнять операции, аналогичные сетевым, для быстрой и надежной передачи слоев образа.

Георепликация позволяет реестру контейнеров Azure работать в качестве отдельного реестра и обслуживать несколько регионов с несколькими региональными реестрами.

> [!IMPORTANT]
> Функция георепликации реестра контейнеров Azure сейчас находится в **предварительной версии**. Предварительные версии предоставляются при условии, что вы соглашаетесь с [дополнительными условиями использования](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Некоторые аспекты этой функции могут быть изменены до выхода общедоступной версии.
>

Геореплицированный реестр предоставляет следующие преимущества:

* отдельные имена реестра, образа и тега можно использовать в нескольких регионах;
* доступ к реестру, который находится ближе все к сети, из региональных развертываний;
* нет дополнительных исходящих затрат, так как образы извлекаются из локального, реплицируемого реестра в регионе, в котором расположен узел контейнера;
* единое управление реестром в нескольких регионах.

## <a name="example-use-case"></a>Примеры использования
Contoso запускает общедоступный веб-сайт, расположенный в США, Канаде и Европе. Чтобы предоставить этим рынкам локальное содержимое, аналогичное сетевому, Contoso запускает кластеры Kubernetes [службы контейнеров Azure](/azure/container-service/kubernetes/) в западной части США, восточной части США, Центральной Канаде и Западной Европе. Веб-приложение, развернутое в качестве образа Docker, использует тот же код и образ во всех регионах. Содержимое, локальное для этого региона, извлекается из базы данных, которая уникально подготавливается в каждом регионе. Каждое региональное развертывание имеет свою уникальную конфигурацию для ресурсов, например, для локальной базы данных.

Команда разработчиков находится в Сиэтле, штат Вашингтон, и использует центр обработки данных в западной части США.

![Отправка в несколько реестров](media/container-registry-geo-replication/before-geo-replicate.png)<br />*Отправка в несколько реестров*

Перед использованием функций георепликации Contoso использовал в западной части США реестр, который размещается в США, а также дополнительный реестр в Западной Европе. Для обслуживания этих разных регионов команде разработчиков необходимо было отправлять образы в два разных реестра.

```bash
docker push contoso.azurecr.io/pubic/products/web:1.2
docker push contosowesteu.azurecr.io/pubic/products/web:1.2
```
![Извлечение из нескольких реестров](media/container-registry-geo-replication/before-geo-replicate-pull.png)<br />*Извлечение из нескольких реестров*

К типичным сложностям нескольких реестров относятся:

* Все кластеры восточной части США, западной части США и Центральной Канады извлекаются из реестра в западной части США. При этом взимается дополнительная исходящая плата, так как каждый из этих удаленных узлов контейнера извлекает образ из центров обработки данных в западной части США.
* Команда разработчиков должна отправлять образы в реестры в западной части США и Западной Европе.
* Команде разработчиков необходимо настроить и поддерживать каждое региональное развертывание с именами образов, ссылающихся на локальный реестр.
* Для каждого региона, необходимо настроить доступ к реестру.

## <a name="benefits-of-geo-replication"></a>Преимущества георепликации

![Извлечение из геореплицированного реестра](media/container-registry-geo-replication/after-geo-replicate-pull.png)

С помощью функции георепликации реестра контейнеров Azure можно реализовать следующие преимущества:

* Управление одним реестром во всех регионах: `contoso.azurecr.io`.
* Управление одной конфигурацией развертываний образов, так как все регионы использовали одинаковый URL-адрес образа: `contoso.azurecr.io/public/products/web:1.2`.
* Отправка в отдельный реестр, в то время как ACR управляет георепликацией, включая региональные веб-перехватчики для локальных уведомлений.

## <a name="configure-geo-replication"></a>Настройка георепликации
Настроить георепликацию так же просто, как и выбрать регионы на карте.

Георепликация — это функция [реестров только уровня "Премиум"](container-registry-skus.md). Вы можете изменить уровень "Базовый" или "Стандартный" на "Премиум" (если у вас его еще нет) на [портале Azure](https://portal.azure.com).

![Переключение SKU на портале Azure](media/container-registry-skus/update-registry-sku.png)

Чтобы настроить георепликацию для реестра уровня "Премиум", войдите на портал Azure по адресу http://portal.azure.com.

Перейдите к реестру контейнеров Azure и выберите **Replications** (Репликации).

![Репликации в реестре контейнера пользовательского интерфейса на портале Azure](media/container-registry-geo-replication/registry-services.png)

Отобразится карта со всеми текущими регионами Azure:

 ![Карта регионов на портале Azure](media/container-registry-geo-replication/registry-geo-map.png)

* синие шестиугольники отображают текущие реплики;
* зеленые шестиугольники отображают возможные регионы реплик;
* серые шестиугольники отображают регионы Azure, которые еще недоступны для репликации.

Чтобы настроить реплику, выберите зеленый шестиугольник, а затем нажмите кнопку **Создать**.

 ![Создание пользовательского интерфейса репликации на портале Azure](media/container-registry-geo-replication/create-replication.png)

Чтобы настроить дополнительные реплики, выберите зеленые шестиугольники для других регионов, а затем нажмите кнопку **Создать**.

ACR начинает синхронизацию образов между настроенными репликами. После завершения на портале отобразится состояние *Готово*. Состояние реплики на портале не обновляется автоматически. Нажмите кнопку "Обновить", чтобы просмотреть обновленное состояние.

## <a name="geo-replication-pricing"></a>Расходы, связанные с георепликацией

Георепликация — это функция [SKU уровня "Премиум"](container-registry-skus.md#premium) реестра контейнеров Azure. При репликации реестра в необходимые регионы с вас взимается плата за реестры уровня "Премиум" для каждого региона.

В предыдущем примере Contoso объединил два реестра в один, добавив реплики в восточную часть США, Центральную Канаду и Западную Европу. Contoso будет платить за уровень "Премиум" четыре раза в месяц. Для этого не нужна дополнительная настройка и управление. Теперь каждый регион извлекает свои образы локально, повышая производительность и надежность, без дополнительной исходящей платы за сеть начиная от западной части США и заканчивая Канадой и восточной частью США.

## <a name="summary"></a>Сводка

С помощью георепликации можно управлять региональными центрами обработки данных как одним глобальным облаком. Так как образы используются во многих службах Azure, вы можете воспользоваться преимуществами плоскости единого управления во время извлечения быстрых, надежных и локальных образов, которые аналогичны сетевым.

## <a name="next-steps"></a>Дальнейшие действия

Просмотрите руководство, состоящее из трех частей, [Подготовка геореплицированного реестра контейнеров Azure](container-registry-tutorial-prepare-registry.md). Рассмотрите создание геореплицированного реестра, создание контейнера, а затем его развертывание в несколько региональных экземпляров веб-приложений для контейнеров, выполнив единую команду `docker push`.

> [!div class="nextstepaction"]
> [Подготовка геореплицированного реестра контейнеров Azure](container-registry-tutorial-prepare-registry.md)