---
title: "Устранение неполадок службы синхронизации файлов Azure (предварительная версия) | Документация Майкрософт"
description: "Устранение распространенных неполадок службы синхронизации файлов Azure."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 265c5f660c4bee53a2faf4a073384587eb3f65fc
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Устранение неполадок службы синхронизации файлов Azure (предварительная версия)
С помощью службы синхронизации файлов Azure (предварительная версия) вы можете централизованно хранить файловые ресурсы организации в службе файлов Azure, обеспечивая гибкость, производительность и совместимость локального файлового сервера. Это достигается путем преобразования Windows Server в быстрый кэш общего файлового ресурса Azure. Для локального доступа к данным вы можете использовать любой протокол, доступный в Windows Server, в том числе SMB, NFS и FTPS. Кроме того, вы можете создать любое количество кэшей в любом регионе.

Эта статья поможет диагностировать и устранить проблемы, возникающие при развертывании службы синхронизации файлов Azure. Кроме того, здесь описано, как собирать важные журналы из системы, если требуется более глубокое исследование проблемы. Если вы не нашли ответ на свой вопрос, свяжитесь с нами, используя следующие каналы (в порядке эскалации):

1. Раздел комментариев этой статьи.
2. [Форум службы хранилища Azure](https://social.msdn.microsoft.com/Forums/home?forum=windowsazuredata).
3. [UserVoice службы файлов Azure](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Служба поддержки Майкрософт. Чтобы создать запрос на поддержку на портале Azure, на вкладке **Справка** нажмите кнопку **Справка и поддержка**, а затем выберите **Новый запрос на поддержку**.

## <a name="agent-installation-and-server-registration"></a>Установка агента и регистрация сервера
<a id="agent-installation-failures"></a>**Устранение сбоев при установке агента**  
Если при установке агента службы синхронизации файлов Azure происходит сбой, то в командной строке с повышенными привилегиями выполните следующую команду, чтобы включить ведение журнала во время установки агента:

```
StorageSyncAgent.msi /l*v Installer.log
```

Просмотрите файл installer.log, чтобы определить причину сбоя установки. 

> [!Note]  
> Установка агента завершится ошибкой, если компьютер настроен на использование центра обновления Майкрософт, но при этом служба "Центр обновления Windows" не запущена.

<a id="server-registration-missing"></a>**Сервер не указан в списке зарегистрированных серверов на портале Azure**  
Если сервер не указан в списке **зарегистрированных серверов** для службы синхронизации хранилища:
1. Войдите на сервер, который необходимо зарегистрировать.
2. Откройте проводник и перейдите в каталог установки агента синхронизации хранилища (расположение по умолчанию — C:\Program Files\Azure\StorageSyncAgent). 
3. Запустите ServerRegistration.exe и завершите работу мастера, чтобы зарегистрировать сервер в службе синхронизации хранилища.

<a id="server-already-registered"></a>**При регистрации сервера во время установки агента службы синхронизации файлов отображается следующее сообщение: This server is already registered (Сервер уже зарегистрирован)** 

![Снимок экрана диалогового окна регистрации сервера с сообщением об ошибке Server is already registered (Сервер уже зарегистрирован)](media/storage-sync-files-troubleshoot/server-registration-1.png)

Это сообщение появляется, если сервер был зарегистрирован в службе синхронизации хранилища ранее. Для отмены регистрации сервера в текущей службе синхронизации хранилища и регистрации в новой службе синхронизации хранилища выполните действия, описанные в разделе [Отмена регистрации сервера в службе синхронизации хранилища](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Если сервер не указан в списке **Зарегистрированные серверы** в службе синхронизации хранилища, выполните следующие команды PowerShell на сервере, регистрацию которого вы хотите отменить:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Чтобы также удалить регистрацию кластера, если сервер является частью кластера, можно использовать необязательный параметр *Reset-StorageSyncServer-CleanClusterRegistration*.

<a id="web-site-not-trusted"></a>**Во время регистрации сервера приходят многочисленные ответы, указывающее, что веб-сайт является ненадежным. Почему?**  
Эта ошибка возникает, если во время регистрации сервера включена политика **усиленной безопасности Internet Explorer**. Дополнительные сведения о том, как правильно отключить политику **усиленной безопасности Internet Explorer**, см. в разделе [Подготовка серверов Windows к работе со службой синхронизации файлов Azure](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) в статье [Как развернуть службу синхронизации файлов Azure (предварительная версия)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Управление группами синхронизации
<a id="cloud-endpoint-using-share"></a>**Создание облачной конечной точки завершается сбоем с такой ошибкой: The specified Azure FileShare is already in use by a different CloudEndpoint (Указанный общий файловый ресурс Azure уже используется другой облачной конечной точкой)**  
Эта ошибка возникает, если общий файловый ресурс Azure уже используется другой облачной конечной точкой. 

Если вы видите это сообщение и общий файловый ресурс Azure в настоящее время не используется облачной конечной точкой, выполните приведенные ниже действия, чтобы очистить метаданные службы синхронизации файлов Azure в общем файловом ресурсе Azure.

> [!Warning]  
> Удаление метаданных в общих файловых ресурсах Azure, которые в настоящее время используются облачной конечной точкой, приведет к сбою операций синхронизации службы синхронизации файлов Azure. 

1. На портале Azure перейдите к общему файловому ресурсу Azure.  
2. Щелкните общий файловый ресурс Azure правой кнопкой мыши и выберите **Изменить метаданные**.
3. Щелкните **SyncService** правой кнопкой мыши и выберите **Удалить**.

<a id="cloud-endpoint-authfailed"></a>**Создание облачной конечной точки завершается сбоем с такой ошибкой: AuthorizationFailed**  
Эта проблема возникает, если учетная запись пользователя не имеет соответствующих прав на создание конечной точки облака. 

Чтобы создать конечную точку облака, ваша учетная запись пользователя должна иметь следующие разрешения авторизации Майкрософт:  
* Чтение. Получение определения роли.
* Запись. Создание или изменение определения пользовательской роли.
* Чтение. Получение назначения роли.
* Запись. Создание назначения роли.

Следующие встроенные роли имеют необходимые разрешения авторизации Майкрософт:  
* Владелец
* Администратор доступа пользователей

Чтобы определить, имеет ли ваша роль пользователя учетной записи необходимые разрешения:  
1. На портале Azure выберите **Группы ресурсов**.
2. Выберите группу ресурсов, в которой расположена учетная запись хранения, а затем выберите **Управление доступом (IAM)**.
3. Выберите **роли** (например, владелец или участник) для учетной записи пользователя.
4. В списке **Поставщик ресурсов** выберите **Авторизация Майкрософт**. 
    * **Назначение ролей** должно иметь разрешения на **чтение** и **запись**.
    * **Определение роли** должно иметь разрешения на **чтение** и **запись**.

<a id="cloud-endpoint-deleteinternalerror"></a>**Удаление облачной конечной точки завершается сбоем с такой ошибкой: MgmtInternalError**  
Эта ошибка может возникать, если общий файловый ресурс Azure или учетная запись хранения были удалены перед удалением облачной конечной точки. Эта проблема будет исправлена в будущем обновлении. Сейчас облачную конечную точку можно удалить только после удаления общего файлового ресурса Azure или учетной записи хранения.

Между тем, чтобы предотвратить возникновение этой проблемы, удалите облачную конечную точку перед удалением общего файлового ресурса Azure или учетной записи хранения.

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Если файл был создан непосредственно в файловом ресурсе Azure через SMB или портал, сколько времени требуется для синхронизации файла с серверами в группе синхронизации?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Сбой синхронизации на сервере**  
Если произошел сбой синхронизации на сервере:
1. Убедитесь, что конечная точка сервера существует на портале Azure для каталога, с которым вы хотите синхронизировать общий файловый ресурс Azure:
    
    ![Снимок экрана группы синхронизации с облачной конечной точкой и сервера на портале Azure](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

2. В средстве просмотра событий просмотрите журналы событий диагностики и операций, расположенные в папке Applications and Services\Microsoft\FileSync\Agent.
    1. Убедитесь, что сервер имеет подключение к Интернету.
    2. Убедитесь, что служба синхронизации файлов Azure выполняется на сервере. Для этого откройте оснастку MMC "Службы" и убедитесь, что служба агента синхронизации хранилища (FileSyncSvc) выполняется.

<a id="replica-not-ready"></a>**Сбой синхронизации с такой ошибкой: 0x80c8300f - The replica is not ready to perform the required operation (0x80c8300f. Реплика не готова для выполнения требуемой операции)**  
Эта ошибка возникает, если вы создали облачную конечную точку и используете общий файловый ресурс Azure, содержащий данные. Когда задание по обнаружению изменений на общем файловом ресурсе Azure завершится (это может занять до 24 часов), то синхронизация должна начать работать правильно.

<a id="broken-sync-files"></a>**Устранение неполадок отдельных файлов, которые не синхронизируются**  
Если отдельные файлы не синхронизируются:
1. В средстве просмотра событий просмотрите журналы событий диагностики и операций, расположенные в папке Applications and Services\Microsoft\FileSync\Agent.
2. Убедитесь, что в файле нет открытых дескрипторов.

    > [!NOTE]
    > Служба синхронизации файлов Azure периодически принимает моментальные снимки VSS для синхронизации файлов с открытыми дескрипторами.

## <a name="cloud-tiering"></a>Распределение по уровням облака 
<a id="files-fail-tiering"></a>**Устранение неполадок при распределении файлов по уровням**  
Если файлы не удается распределить по уровням в службе файлов Azure, сделайте следующее:

1. Убедитесь, что файлы существуют в общем файловом ресурсе Azure.

    > [!NOTE]
    > Файлы должны быть синхронизированы с общими файловыми ресурсами Azure, прежде чем их можно будет распределить по уровням.
2. В средстве просмотра событий просмотрите журналы событий диагностики и операций, расположенные в папке Applications and Services\Microsoft\FileSync\Agent.
    1. Убедитесь, что сервер имеет подключение к Интернету. 
    2. Убедитесь, что выполняются драйверы фильтра службы синхронизации файлов Azure (StorageSync.sys и StorageSyncGuard.sys).
        - В командной строке с повышенными привилегиями запустите `fltmc`. Убедитесь, что указаны драйверы StorageSync.sys и StorageSyncGuard.sys фильтра файловой системы.

<a id="files-fail-recall"></a>**Устранение неполадок в файлах, которые не удалось отозвать**  
Если файлы не удалось отозвать:
1. В средстве просмотра событий просмотрите журналы событий диагностики и операций, расположенные в папке Applications and Services\Microsoft\FileSync\Agent.
    1. Убедитесь, что файлы существуют в общем файловом ресурсе Azure.
    2. Убедитесь, что сервер имеет подключение к Интернету. 
    3. Убедитесь, что выполняются драйверы фильтра службы синхронизации файлов Azure (StorageSync.sys и StorageSyncGuard.sys).
        - В командной строке с повышенными привилегиями запустите `fltmc`. Убедитесь, что указаны драйверы StorageSync.sys и StorageSyncGuard.sys фильтра файловой системы.

<a id="files-unexpectedly-recalled"></a>**Устранение неполадок с файлами, неожиданно вызванными на сервере**  
Антивирусные программы, приложения архивации, а также другие приложения, которые считывают большое количество файлов, вызывают непреднамеренные отзывы, если они не учитывают автономный атрибут пропуска и пропускают чтение содержимого этих файлов. Пропуск автономных файлов для продуктов, поддерживающих эту возможность, позволяет избежать непреднамеренных отзывов во время операций, таких как антивирусные проверки или задания резервного копирования.

Обратитесь к поставщику программного обеспечения за информацией о том, как настроить решение, чтобы пропустить чтение автономных файлов.

Непреднамеренные отзывы также могут возникать в других сценариях, например при просмотре файлов в проводнике. Открытие папки с облачными многоуровневыми файлами в проводнике может привести к непреднамеренным отзывам на сервере. Это еще более вероятно, если на сервере включено антивирусное решение.

## <a name="general-troubleshooting"></a>Общие действия по устранению неполадок
При возникновении проблем на сервере со службой синхронизации файлов Azure для начала сделайте следующее:
1. В средстве просмотра событий просмотрите журналы событий диагностики и операций.
    - Проблемы синхронизации, многоуровневых файлов и отзывов записываются в журналы событий диагностики и операций, которые расположены в папке Applications and Services\Microsoft\FileSync\Agent.
    - Проблемы, связанные с управлением сервером (например, параметры конфигурации), записываются в журналы событий диагностики и операций, которые расположены в папке Applications and Services\Microsoft\FileSync\Management.
2. Убедитесь, что служба синхронизации файлов Azure выполняется на сервере.
    - Откройте оснастку MMC "Службы" и убедитесь, что выполняется служба агента синхронизации хранилища (FileSyncSvc).
3. Убедитесь, что выполняются драйверы фильтра службы синхронизации файлов Azure (StorageSync.sys и StorageSyncGuard.sys).
    - В командной строке с повышенными привилегиями запустите `fltmc`. Убедитесь, что указаны драйверы StorageSync.sys и StorageSyncGuard.sys фильтра файловой системы.

Если проблема не устранена, запустите средство AFSDiag.
1. Создайте каталог (например, C:\Output), в котором будут сохранены выходные данные средства AFSDiag.
2. Откройте окно PowerShell, а затем выполните следующие команды (нажимайте клавишу ВВОД после каждой команды).

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Для службы синхронизации файлов Azure в режиме ядра на уровне трассировки введите **1** (если не указано создание дополнительных подробных трассировок) и нажмите клавишу ВВОД.
4. Для службы синхронизации файлов Azure в пользовательском режиме на уровне трассировки введите **1** (если не указано создание дополнительных подробных трассировок) и нажмите клавишу ВВОД.
5. Воспроизведите проблему. После завершения введите **D**.
6. ZIP-файл, содержащий журналы и файлы трассировки, сохраняется в указанном вами каталоге выходных данных.

## <a name="see-also"></a>См. также
- [Часто задаваемые вопросы о службе "Файлы Azure"](storage-files-faq.md)
- [Устранение неполадок службы файлов Azure в Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Устранение неполадок службы файлов Azure в Linux](storage-troubleshoot-linux-file-connection-problems.md)
