---
title: "Использование репозитория Git в проекте Azure Machine Learning Workbench | Документация Майкрософт"
description: "В этой статье объясняется, как использовать репозиторий Git в сочетании с проектом Azure Machine Learning Workbench."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: hning86
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 59b07c9834904e01256b75344ba2e6892e56438c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="using-git-repository-with-an-azure-machine-learning-workbench-project"></a>Использование репозитория Git в проекте Azure Machine Learning Workbench
Этот документ содержит сведения о том, как Azure Machine Learning Workbench использует Git для обеспечения повторяемости эксперимента обработки и анализа данных. Также в ней есть инструкции, позволяющие связать проект с облачным репозиторием Git.

## <a name="introduction"></a>Введение
Azure Machine Learning Workbench с самого момента создания включала интеграцию с Git. При создании нового проекта папка проекта автоматически инициализируется для работы с локальным Git, и одновременно создается второй (скрытый) репозиторий Git с именем ветви _AzureMLHistory/<идентификатор_проекта>_, где сохраняются изменения в папке проекта при каждом выполнении. 

Если вы сопоставите проект Машинного обучения Azure с репозиторием Git, размещенном в проекте Visual Studio Team Service (VSTS), вы получите возможность автоматического управления версиями, как локально, так и удаленно. Такая связь позволяет любому пользователю с доступом к удаленному репозиторию скачать самый свежий исходный код на другой компьютер (роуминг).  

> [!NOTE]
> VSTS поддерживает собственный список управления доступом, который не зависит от службы "Экспериментирование в Машинном обучении Azure". Для разных репозиториев Git, рабочих областей или проектов Машинного обучения Azure может использоваться разный режим доступа пользователей, и может потребоваться управление эти доступом. Поэтому, чтобы другие участники команды могли использовать ваш проект Машинного обучения Azure (включая доступ на уровне кода), вы должны предоставить им не только общий доступ к рабочей области, но и явные права доступа к репозиторию VSTS Git. 

С помощью Git вы можете явно управлять системой управления версиями, используя ветвь _master_ или создавая новые ветви в репозитории. Вы можете ограничиться локальным репозиторием Git или передавать данные в удаленный репозиторий Git, если он предоставлен.

Следующая схема демонстрирует взаимоотношения между проектом Машинного обучения Azure и репозиторием VSTS Git.

![AML Git](media/using-git-ml-project/aml_git.png)

Чтобы начать работу с удаленным репозиторием Git, выполните следующие основные действия.

> [!NOTE]
> В настоящее время Машинное обучение Azure поддерживает репозитории Git только с учетными записями VSTS. В будущем планируется поддержка для общих репозиториев Git (таких как GitHub и др.).

## <a name="step-1-create-an-azure-ml-experimentation-account"></a>Шаг 1. Создание учетной записи службы "Экспериментирование в Машинном обучении Azure"
Создайте учетную запись службы "Экспериментирование в Машинном обучении Azure" и установите приложение Azure ML Workbench, если вы еще не сделали этого. Дополнительные сведения см. в [этом кратком руководстве](quickstart-installation.md).

## <a name="step-2-create-a-team-project-or-use-an-existing-team-project"></a>Шаг 2. Создание командного проекта или использование существующего командного проекта
На [портале Azure](https://portal.azure.com/) создайте новый **командный проект**.
1. Щелкните **+**
2. Выполните поиск по запросу **Командный проект**.
3. Введите необходимые сведения.
    - Имя: имя команды.
    - Управление версиями: **Git**.
    - Подписка — подписка, в которой есть учетная запись службы "Экспериментирование в Машинном обучении Azure".
    - Расположение — лучше всего использовать регион, который находится близко к ресурсам службы "Экспериментирование в Машинном обучении Azure".
4. Щелкните **Создать**. 

![Создание нового командного проекта на портале Azure](media/using-git-ml-project/create_vsts_team.png)


> [!TIP]
> Для входа в систему обязательно используйте учетную запись Azure Active Directory (AAD), с которой выполнялся вход в Azure Machine Learning Workbench. В противном случае новый командный проект может получить неправильный идентификатор клиента, и Машинное обучение Azure не сможет его найти. В таком случае придется использовать интерфейс командной строки, чтобы указать токен VSTS.

После создания командного проекта вы можете переходить к следующему шагу.

Чтобы перейти прямо к новому командному проекту, используйте URL-адрес `https://<team_project_name>.visualstudio.com`.

> [!NOTE]
> В настоящее время Машинное обучение Azure поддерживает только пустые репозитории Git без главной ветви. Чтобы удалить главную ветвь перед использованием репозитория, используйте параметр --force в интерфейсе командной строки. 

## <a name="step-3-create-a-new-azure-ml-project-with-a-remote-git-repo"></a>Шаг 3. Создание нового проекта Azure ML с удаленным репозиторием Git
Запустите Azure ML Workbench и создайте новый проект. В текстовое поле репозитория Git введите или вставьте URL-адрес репозитория VSTS Git, полученный на шаге 2. Обычно этот адрес выглядит так: http://<имя_учетной_записи_VSTS>.visualstudio.com/_git/<имя_проекта>

![Создание проекта Azure ML с репозиторием Git](media/using-git-ml-project/create_project_with_git_rep.png)

Теперь у вас есть новый проект Машинного обучения Azure с интегрированным удаленным репозиторием Git, и все готово к началу работы. Папка проекта всегда инициализируется для использования Git в локальном репозитории Git. А для удаленного репозитория Git устанавливается признак Git _remote_, так что фиксации можно передавать в удаленный репозиторий Git.

## <a name="step-3a-associate-an-existing-azure-ml-project-with-a-vsts-git-repo"></a>Шаг 3а. Сопоставление существующего проекта Машинного обучения Azure с репозиторием VSTS Git
Если потребуется, вы можете создать проект Машинного обучения Azure без репозитория VSTS Git, ограничившись локальным репозиторием Git для сохранения моментальных снимков журнала выполнения. Позднее вы всегда сможете связать репозиторий VSTS Git с этим проектом Машинного обучения Azure, используя следующую команду.

```azurecli
# make sure you are in the project path so CLI has context of your current project
az ml project update --repo http://<vsts_account_name>.visualstudio.com/_git/<project_name
```

## <a name="step-4-capture-project-snapshot-in-git-repo"></a>Шаг 4. Сохранение снимка проекта в репозитории Git
Теперь можно выполнить несколько запусков проекта и вносить изменения между запусками. Для этого можно использовать классическое приложение или команду `az ml experiment submit` в интерфейсе командной строки. Для получения дополнительных сведений выполните инструкции [руководства по классификации цветков ириса](tutorial-classifying-iris-part-1.md). После каждого запуска, если были внесены изменения в файлах в папке проекта, создается моментальный снимок всей папки проекта, который передается в удаленный репозиторий Git. Чтобы просмотреть все ветви и фиксации, перейдите по URL-адресу репозитория VSTS Git.

![Ветвь истории запусков](media/using-git-ml-project/run_history_branch.png)

## <a name="step-5-restore-a-previous-project-snapshot"></a>Шаг 5. Восстановление из предыдущего моментального снимка проекта 
В Azure Machine Learning Workbench вы можете восстановить целиком всю папку проекта до состояния, сохраненного в истории запуска в виде моментального снимка проекта.
1. Щелкните **Запуски** в панели действий (значок песочных часов).
2. В представлении **Список запусков** щелкните нужный запуск, состояние которого вы хотите восстановить.
3. В представлении **сведений о запуске** выберите действие **Восстановить**.

![Ветвь истории запусков](media/using-git-ml-project/restore_project.png)

Кроме того, вы можете выполнить следующую команду из окна интерфейса командной строки Azure ML Workbench.

```azurecli
# discover the run I want to restore snapshot from:
az ml history list -o table

# restore the snapshot from a particular run
az ml project restore --run-id <run_id>
```

Эта команда перезапишет целиком всю папку проекта данными из моментального снимка, который был создан в начале этого конкретного запуска. Вы **потеряете все изменения**, внесенные с тех пор в папке текущего проекта. Поэтому при выполнении этой команды следует соблюдать крайнюю осторожность.

## <a name="step-6-use-the-master-branch"></a>Шаг 6. Использование главной ветви
Чтобы избежать случайной потери состояния текущего проекта, можно сохранить проект в главную ветвь репозитория Git. Действия с главной ветвью Git можно выполнять из командной строки (или с помощью любого удобного клиентского средства Git). Например:

```
# make sure you are on the master branch
git checkout master

# stage all changes
git add -A

# commit all changes locally on the master branch
git commit -m 'this is my updates so far'

# push changes into the remote VSTS Git repo master branch.
git push origin master
```

Теперь вы можете безопасно восстановить созданный ранее моментальный снимок проекта, как описано в шаге 5. Все последние изменения в проекте вы сможете восстановить из фиксации, которую вы только что сохранили в главной ветви.

## <a name="authentication"></a>Аутентификация
Если вы используете функции журнала выполнения в Машинном обучении Azure только для создания и восстановления моментальных снимков проекта, можно не беспокоиться об аутентификации для репозитория Git. Эта задача решается на уровне службы "Экспериментирование".

Но если вы применяете собственные средства Git для управления версиями, важно правильно обрабатывать аутентификацию в удаленном репозитории Git в VSTS. Для этого вам нужно настроить на локальном компьютере аутентификацию в репозитории Git до того, как вы будете выполнять команды Git для этого удаленного репозитория Git. 

Проще всего создать пару ключей SSH и отправить открытый ключ из этой пары в репозиторий Git, используя страницу параметров безопасности.

### <a name="generate-ssh-key"></a>Создание ключа SSH 
Сначала создайте пару ключей SSH на локальном компьютере.

#### <a name="on-windows"></a>Действия для ОС Windows.
Запустите классическое приложение Git с графическим интерфейсом и в разделе меню _Справка_ щелкните элемент_Показать ключ SSH_.

![Ключ SSH](media/using-git-ml-project/git_gui.png)

Скопируйте ключ SSH в буфер обмена.

#### <a name="on-macos"></a>Действия для MacOS.
Быстрая процедура, выполняемая из оболочки командной строки:
```
# generate the SSH key
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# start the SSH agent in the background
$ eval "$(ssh-agent -s)"

# add newly generated SSH key to the SSH agent
$ ssh-add -K ~/.ssh/id_rsa

# display the public key so you can copy it.
$ more ~/.ssh/id_rsa.pub
```
Дополнительные сведения об этом действии можно найти [в этой статье на GitHub](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

### <a name="upload-public-key-to-git-repo"></a>Отправка открытого ключа в репозиторий Git
Перейдите на главную страницу учетной записи Visual Studio (https://<имя_учетной_записи_VSTS>.visualstudio.com) и выполните вход, а затем выберите пункт "Безопасность" под своим аватаром.

Выберите действие добавления открытого ключа SSH, вставьте сюда полученный на предыдущем этапе открытый ключ SSH и присвойте ему понятное имя. Вы можете добавить несколько ключей.

Теперь вы сможете локально выполнять команды Git для удаленного репозитория без дополнительных действий для аутентификации.

### <a name="read-more"></a>Дополнительные сведения
Внимательно изучите эти две статьи (можно использовать любой из описанных подходов), которые содержат сведения о включении локальной аутентификации для удаленного репозитория Git в VSTS.
- [Use SSH Key Authentication](https://www.visualstudio.com/en-us/docs/git/use-ssh-keys-to-authenticate) (Аутентификация с ключом SSH)
- [Use Git Credential Managers to Authenticate to Visual Studio VSTS](https://www.visualstudio.com/en-us/docs/git/set-up-credential-managers) (Использование диспетчера учетных данных Git для аутентификации в Visual Studio VSTS)


## <a name="next-steps"></a>Дальнейшие действия
Сведения об использовании командного процесса обработки и анализа данных для организации структуры проекта см. в статье [о применении TDSP для структурирования проекта](how-to-use-tdsp-in-azure-ml.md).
