В Cloud Shell создайте [веб-приложение](../articles/app-service/app-service-web-overview.md) в рамках плана `myAppServicePlan` службы приложений с помощью команды [az webapp create](/cli/azure/webapp#create). 

Веб-приложение предусматривает пространство для размещения кода, а также предоставляет URL-адрес для просмотра развернутого приложения.

В следующей команде замените *\<имя_приложения>* уникальным именем (допустимые символы: `a-z`, `0-9` и `-`). Если `<app_name>` не является уникальным, отобразится следующее сообщение об ошибке: "Веб-сайт с указанным именем <имя_приложения> уже существует". URL-адрес приложения по умолчанию: `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

После создания веб-приложения в Azure CLI отображаются следующие сведения:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Вы создали пустое веб-приложение с включенным развертыванием Git.

> [!NOTE]
> URL-адрес удаленного репозитория Git отображается в свойстве `deploymentLocalGitUrl` в формате `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Сохраните этот URL-адрес для дальнейшего использования.
>

Перейдите на сайт, чтобы просмотреть только что созданное веб-приложение.

```bash
http://<app_name>.azurewebsites.net
```
