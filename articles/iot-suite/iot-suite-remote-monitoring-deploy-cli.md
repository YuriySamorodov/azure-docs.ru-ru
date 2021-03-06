---
title: "Развертывание решения для удаленного мониторинга Java в Azure | Документация Майкрософт"
description: "В этом руководстве показано, как подготовить микрослужбы Java предварительно настроенного решения для удаленного мониторинга с помощью интерфейса командной строки."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: d0ed9a7fbb202b2008fee7f810ae0102b6f51c3d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>Развертывание предварительно настроенного решения для удаленного мониторинга с помощью интерфейса командной строки

В этом руководстве показано, как подготовить предварительно настроенное решение для удаленного мониторинга. Решение развертывается с помощью интерфейса командной строки. Его также можно развернуть с помощью пользовательского веб-интерфейса на сайте azureiotsuite.com. Дополнительные сведения об этом варианте см. в статье [Развертывание предварительно настроенного решения для удаленного мониторинга](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы развернуть предварительно настроенное решение для удаленного мониторинга, нужна активная подписка Azure.

Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](http://azure.microsoft.com/pricing/free-trial/).

Чтобы запустить интерфейс командной строки, на локальном компьютере необходимо установить [Node.js](https://nodejs.org/).

## <a name="install-the-cli"></a>Установка интерфейса командной строки

Чтобы установить интерфейс командной строки, в среде командной строки выполните следующую команду:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Вход в интерфейс командной строки

Прежде чем развернуть предварительно настроенное решение, необходимо войти в подписку Azure с помощью интерфейса командной строки:

```cmd/sh
pcs login
```

Для завершения процесса входа следуйте инструкциям на экране.

## <a name="deployment-options"></a>Варианты развертывания

При развертывании предварительно настроенного решения существует несколько параметров для настройки этого процесса:

| Параметр | Значения | Описание |
| ------ | ------ | ----------- |
| SKU    | `basic`, `standard` | _Базовое_ развертывание предназначено для тестирования и демонстрации. При этом все микрослужбы развертываются на одной виртуальной машине. _Стандартное_ развертывание предназначено для рабочей среды. При этом микрослужбы развертываются на нескольких виртуальных машинах. |
| Среда выполнения | `dotnet`, `java` | Позволяет выбирать языковую реализацию микрослужб. |

## <a name="deploy-the-preconfigured-solution"></a>Развертывание предварительно настроенного решения

### <a name="example-deploy-net-version"></a>Пример: развертывание версии .NET

Далее представлен пример базового развертывания предварительно настроенного решения версии .NET для удаленного мониторинга:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Пример: развертывание версии Java

Далее представлен пример стандартного развертывания предварительно настроенного решения версии Java для удаленного мониторинга:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>параметры команд предварительно настроенного решения

При выполнении команды `pcs` для развертывания решения запрашивается:

- Имя для решения. Это имя должно быть уникальным.
- Подписка Azure, которую нужно использовать.
- Расположение.
- Учетные данные для виртуальных машин, на которых размещены микрослужбы. Эти учетные данные можно использовать для доступа к виртуальным машинам, используемым для устранения неполадок.

После выполнения команды `pcs` отобразится URL-адрес нового развертывания предварительно настроенного решения. Кроме того, команда `pcs` создает файл `{deployment-name}-output.json` с дополнительной информацией, такой как имя подготовленного Центра Интернета вещей.

Чтобы получить дополнительные сведения о параметрах командной строки, выполните следующую команду:

```cmd/sh
pcs -h
```

Дополнительные сведения об интерфейсе командной строки см. в статье [Azure IoT PCS CLI Overview](https://github.com/Azure/pcs-cli/blob/master/README.md)(Обзор интерфейса командной строки для предварительно настроенного решения Центра Интернета вещей Azure).

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнять такие задачи:

> [!div class="checklist"]
> * Настройка предварительно настроенного решения.
> * Развертывание предварительно настроенного решения.
> * Вход в предварительно настроенное решение.

Теперь, когда вы развернули решение для удаленного мониторинга, [изучите возможности панели мониторинга решения](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->