---
title: "Подключение управляемого диска данных к виртуальной машине Windows в Azure | Документация Майкрософт"
description: "Подключение нового управляемого диска данных к виртуальной машине Windows на портале Azure с помощью модели развертывания на основе диспетчера ресурсов."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Как подключить управляемый диск данных к виртуальной машине Windows на портале Azure

В этой статье демонстрируется подключение нового управляемого диска данных к виртуальной машине Windows на портале Azure. Перед этим ознакомьтесь со следующими советами:

* Размер виртуальной машины определяет, сколько дисков данных к ней можно подключить. Дополнительную информацию см. в статье [Размеры виртуальных машин](sizes.md).
* Если вы подключаете новый диск, его не надо предварительно создавать. Azure создаст его при подключении.

Вы также можете [присоединить диск данных с помощью Powershell](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Добавление диска данных
1. В меню слева щелкните **Виртуальные машины**.
2. Затем выберите виртуальную машину из списка.
3. В колонке виртуальной машины щелкните **Диски**.
   4. В колонке **Диски** щелкните **+ Add data disk** (+ Добавить диск данных).
5. В раскрывающемся списке для нового диска выберите **Create empty** (Создать пустой).
6. В колонке **Создание управляемого диска** введите имя диска и при необходимости настройте другие параметры. Закончив, щелкните **Создать**.
7. В колонке **Диски** щелкните "Сохранить", чтобы сохранить новую конфигурацию диска для виртуальной машины.
6. После того как Azure создаст диск и подключит его к виртуальной машине, он появится в параметрах дисков виртуальной машины в разделе **Диски данных**.


## <a name="initialize-a-new-data-disk"></a>Инициализация нового диска данных

1. Подключитесь к виртуальной машине.
1. Щелкните меню "Пуск" на виртуальной машине, введите **diskmgmt.msc** и нажмите клавишу **ВВОД**. Запустится оснастка "Управление дисками".
2. Оснастка "Управление дисками" определит новый неинициализированный диск, и откроется окно "Инициализация диска".
3. Убедитесь, что выбран новый диск, и нажмите кнопку **ОК**, чтобы его инициализировать.
4. Теперь новый диск отображается как **нераспределенный**. Щелкните правой кнопкой мыши в любой части диска и выберите **Создать простой том**. Будет запущен **мастер создания простых томов**.
5. Выполните инструкции мастера, оставляя все значения по умолчанию. По завершении щелкните **Готово**.
6. Закройте оснастку "Управление дисками".
7. Появится всплывающее окно, в котором необходимо форматировать диск перед его использованием. Щелкните **Форматировать диск**.
8. В диалоговом окне **Format new disk** (Форматирование нового диска) проверьте параметры и щелкните **Запустить**.
9. Вы получите предупреждение о том, что при форматировании дисков будут стерты все данные. Нажмите кнопку **ОК**.
10. После форматирования нажмите кнопку **ОК**.

## <a name="use-trim-with-standard-storage"></a>Использование функции TRIM в хранилище уровня "Стандартный"

Если вы используете хранилище уровня "Стандартный" (HDD), то следует включить функцию TRIM. TRIM удаляет неиспользуемые блоки на диске, таким образом плата взимается только за ту часть хранилища, которая используется. Это позволяет сократить затраты, если вы создаете большие файлы, а затем удаляете их. 

Вы можете выполнить эту команду, чтобы проверить состояние функции TRIM. На виртуальной машине Windows откройте командную строку и введите:

```
fsutil behavior query DisableDeleteNotify
```

Если команда возвращает значение 0, то функция TRIM включена правильно. Если она возвращает значение 1, то для включения TRIM выполните следующую команду:
```
fsutil behavior set DisableDeleteNotify 0
```

После удаления данных с диска можно обеспечить правильную очистку после операций TRIM, запустив дефрагментацию с помощью TRIM.

```
defrag.exe <volume:> -l
```

Полную очистку тома также можно произвести, отформатировав его.

## <a name="next-steps"></a>Дальнейшие действия
Если вашему приложению нужно использовать диск D для хранения данных, вы можете [изменить букву временного диска Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
