---
title: "Сведения о дисках и виртуальных жестких дисках для виртуальных машин Microsoft Azure под управлением Windows | Документация Майкрософт"
description: "Информация об основах использования дисков и виртуальных жестких дисков для виртуальных машин Windows в Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: b1beecf2e4268e358285c1101edcb13f6d592948
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a>Сведения о дисках и VHD для виртуальных машин Azure под управлением Windows
Как и любой другой компьютер, виртуальные машины в Azure используют диски как место хранения операционной системы, приложений и данных. Все виртуальные машины Azure имеют как минимум два диска — диск операционной системы Windows и временный диск. Диск операционной системы создается из образа, при этом и диск операционной системы, и образ являются виртуальными жесткими дисками (VHD), расположенными в учетной записи хранения Azure. Кроме того, виртуальные машины могут иметь один или несколько дисков данных, которые также хранятся на виртуальных жестких дисках. 

В этой статье рассматриваются различные варианты применения дисков и сведения о том, как создать и использовать различные типы дисков. Также доступна версия этой статьи для [виртуальных машин Linux](../linux/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Диски, используемые виртуальными машинами

Давайте рассмотрим, как виртуальные машины используют диски.

### <a name="operating-system-disk"></a>Диск операционной системы
У каждой виртуальной машины есть один подключенный диск операционной системы. Он зарегистрирован как диск SATA и обозначается буквой C: по умолчанию. Максимальная емкость этого диска составляет 2048 гигабайта (ГБ). 

### <a name="temporary-disk"></a>Временный диск
У каждой виртуальной машины есть временный диск. Он выступает в качестве временного хранилища для приложений и процессов и предназначен только для хранения данных, таких как страничные файлы или файлы подкачки. Данные на временном диске могут быть потеряны во время [обслуживания](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) или при [повторном развертывании виртуальной машины](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Во время обычной перезагрузки виртуальной машины данные на временном диске должны сохраниться.

Временный диск обозначается буквой D: по умолчанию и используется для хранения файла подкачки pagefile.sys. Сведения о переназначении буквы этого диска см. в статье [Изменение буквы диска для временного диска Windows](change-drive-letter.md). Размер временного диска зависит от размера виртуальной машины. Чтобы узнать больше, ознакомьтесь с [размерами виртуальных машин Windows](sizes.md).

Дополнительные сведения об использовании временного диска в Azure см. в статье [Общие сведения о временном диске на виртуальных машинах Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).


### <a name="data-disk"></a>Диск данных
Диск данных — виртуальный жесткий диск, подключенный к виртуальной машине для хранения данных приложений или других необходимых данных. Диски данных регистрируются как диски SCSI и обозначаются любой указанной буквой. Максимальная емкость каждого диска составляет 4095 ГБ. Размер виртуальной машины определяет, сколько дисков данных можно подключить и какой тип хранилища можно использовать для размещения дисков.

> [!NOTE]
> Дополнительные сведения о емкости виртуальных машин см. в статье [Размеры виртуальных машин Windows](sizes.md).
> 

Azure создает диск операционной системы при создании виртуальной машины из образа. Если используется образ, включающий диски данных, при создании виртуальной машины Azure также создает диски данных. В противном случае диски данных добавляются после создания виртуальной машины.

Диски данных можно добавить в виртуальную машину в любое время, **подключив** их к ней. Можно использовать виртуальный жесткий диск, отправленный или скопированный в учетную запись хранения или созданный Azure. Подключая диск данных, вы связываете VHD-файл с виртуальной машиной. VHD-файл "берется в аренду", поэтому его невозможно удалить из хранилища, пока он подключен.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Последняя рекомендация. Использование TRIM с неуправляемыми дисками уровня "Стандартный" 

Если вы используете неуправляемые диски уровня "Стандартный", то следует включить функцию TRIM. TRIM удаляет неиспользуемые блоки на диске, таким образом плата взимается только за ту часть хранилища, которая используется. Это позволяет сократить затраты, если вы создаете большие файлы, а затем удаляете их. 

Вы можете выполнить эту команду, чтобы проверить состояние функции TRIM. На виртуальной машине Windows откройте командную строку и введите:


```
fsutil behavior query DisableDeleteNotify
```

Если команда возвращает значение 0, то функция TRIM включена правильно. Если она возвращает значение 1, то для включения TRIM выполните следующую команду:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Примечание. Поддержка функции Trim начинается с Windows Server 2012 или Windows 8 и более поздних версий. Дополнительные сведения см. в статье [New API allows apps to send "TRIM and Unmap" hints to storage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints) (Новый API позволяет приложением отправлять подсказки (TRIM и Unmap) на носитель данных).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Дальнейшие действия
* [Подключите диск](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , чтобы увеличить емкость хранилища для виртуальной машины.
* [Измените букву временного диска Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , чтобы приложение могло использовать диск D для данных.

