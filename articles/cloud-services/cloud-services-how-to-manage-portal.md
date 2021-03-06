---
title: "Общие задачи управления облачными службами | Документация Майкрософт"
description: "Узнайте, как управлять облачными службами с помощью портала Azure. В этих примерах используется портал Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 9af1fdeb5cfe69631cabe13bd341b43319175aae
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2017
---
# <a name="how-to-manage-cloud-services"></a>Управление облачными службами
В области **Облачные службы (классические)** портала Azure можно обновить роль службы или развертывание, перенести промежуточное развертывание в рабочую среду, привязать ресурсы к облачной службе для просмотра их зависимостей и масштабирования, а также удалить облачную службу или развертывание.

Дополнительные сведения о том, как масштабировать облачную службу, доступны [здесь](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Практическое руководство. Обновление роли облачной службы или развертывания
Если необходимо обновить код приложения для облачной службы, нажмите **Обновление** в колонке облачной службы. Одновременно можно обновить как одну, так и все роли. Для обновления можно передать новый файл пакета или конфигурации службы.

1. На [портале Azure][Azure portal] выберите облачную службу, которую требуется обновить. Откроется колонка экземпляра облачной службы.
2. В колонке нажмите кнопку **Обновить** .

    ![Кнопка «Обновить»](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Обновите развертывание с новым файлом пакета службы (.cspkg) и файлом конфигурации службы (.cscfg).

    ![Обновление развертывания](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **При необходимости** обновите метку развертывания и учетную запись хранения.
5. Если какая-либо роль имеет только один экземпляр роли, установите флажок **Развернуть, даже если одна роль или несколько содержат отдельный экземпляр** , чтобы обеспечить возможность обновления.

    Служба Azure гарантирует доступность облачной службы в течение 99,95 % времени, только если для каждой роли определены как минимум два экземпляра (виртуальные машины). Наличие двух экземпляров роли позволяет обрабатывать запросы клиентов на одной виртуальной машине во время обновления другой.

6. Установите флажок **Запуск развертывания** , чтобы применить обновление после завершения передачи пакета.
7. Нажмите **ОК** , чтобы начать обновление службы.

## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Практическое руководство. Переключение из промежуточного в рабочее развертывание
Если вы решили развернуть новый выпуск облачной службы, то вы можете протестировать его в промежуточном развертывании. Используйте команду **Переключить** для переключения URL-адресов двух развертываний и повышения нового выпуска до рабочего развертывания.

Соответствующая команда представлена на странице **Облачные службы** или на панели мониторинга.

1. На [портале Azure][Azure portal] выберите облачную службу, которую требуется обновить. Откроется колонка экземпляра облачной службы.
2. В колонке нажмите кнопку **Заменить** .

    ![Переключение облачных служб](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Откроется следующий запрос подтверждения.

    ![Переключение облачных служб](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Проверьте данные развертывания и нажмите кнопку **ОК** , чтобы заменить развертывание.

    Переключение осуществляется мгновенно, поскольку при этом изменяются только виртуальные IP-адреса развертываний.

    Для экономии вычислительных ресурсов вы можете удалить промежуточную среду после того, как убедитесь в соответствии рабочего развертывания заданным характеристикам.

### <a name="common-questions-about-swapping-deployments"></a>Часто задаваемые вопросы о переключении развертываний

**Каковы предварительные требования для переключения развертываний?**

Существует два ключевых требования для успешного переключения развертываний.

- Если вы хотите использовать статический IP-адрес для вашего рабочего слота, необходимо также зарезервировать такой адрес для промежуточного слота. В противном случае переключение завершится ошибкой.

- Все экземпляры роли должны быть запущены перед выполнением переключения. Состояние экземпляров можно проверить в колонке обзора на портале Azure. Также можно использовать команду [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) в Windows PowerShell.

Обратите внимание, что обновления гостевой ОС и операции восстановления службы также могут быть причиной сбоя при переключении развертывания. Дополнительные сведения см. в статье [Устранение неполадок, которые могут возникнуть при развертывании облачной службы](cloud-services-troubleshoot-deployment-problems.md).

**Увеличивают ли переключения время простоя приложения? Что делать в этом случае?**

Как описано в предыдущем разделе, переключение развертывания обычно происходит быстро, так как это просто изменение конфигурации в Azure Load Balancer. В некоторых случаях, однако, это может занять около десяти секунд и привести к временному сбою подключения. Чтобы ограничить воздействие на клиентов, рассмотрите возможность реализации [логики повтора для клиента](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Практическое руководство. Привязка ресурсов к облачной службе
Портал Azure не связывает ресурсы вместе в отличие от классического портала Azure. Вместо этого разверните дополнительные ресурсы в ту же группу ресурсов, которая используется облачной службой.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Практическое руководство. Удаление развертываний и облачной службы
До удаления облачной службы необходимо удалить все текущие развертывания.

Для экономии вычислительных ресурсов вы можете удалить промежуточную среду после того, как убедитесь в соответствии рабочего развертывания заданным характеристикам. Вам выставляются счета за вычислительные ресурсы развернутых экземпляров роли, которые остановлены.

Чтобы удалить развертывание или облачную службу, выполните следующие действия.

1. На [портале Azure][Azure portal] выберите облачную службу, которую требуется удалить. Откроется колонка экземпляра облачной службы.
2. В колонке нажмите кнопку **Удалить** .

    ![Переключение облачных служб](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Можно удалить всю облачную службу, выбрав **Облачная служба и ее развертывания**, или выбрать **Рабочее развертывание** или **Промежуточное развертывание**.

    ![Переключение облачных служб](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Нажмите кнопку **Удалить** внизу.
5. Чтобы удалить службу, щелкните **Удалить облачную службу**. Нажмите кнопку **Да**в запросе подтверждения.

> [!NOTE]
> Если был включен режим подробного мониторинга, то при удалении облачной службы необходимо вручную удалить соответствующие данные из учетной записи хранения. Дополнительные сведения о поиске таблиц метрик см. в [этой](cloud-services-how-to-monitor.md) статье.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Практическое руководство: поиск дополнительных сведений об ошибках развертывания
В верхней части колонки **Обзор** есть строка состояния. Если щелкнуть эту строку, откроется новая колонка с информацией об ошибках. Если развертывание не содержит ошибок, отобразится пустая колонка.

![Переключение облачных служб](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Дальнейшие действия
* [Общая настройка облачной службы](cloud-services-how-to-configure-portal.md).
* Узнайте, как [развернуть облачную службу](cloud-services-how-to-create-deploy-portal.md).
* Настройте [пользовательское доменное имя](cloud-services-custom-domain-name-portal.md).
* Настройка [SSL-сертификатов](cloud-services-configure-ssl-certificate-portal.md).
