# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Поддерживаемый платформой перенос ресурсов IaaS из классической модели в модель Azure Resource Manager
В этой статье описывается, как обеспечить перенос ресурсов IaaS из классической модели в модель развертывания с помощью Resource Manager. Вы можете узнать больше о [возможностях и преимуществах Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md). Мы подробно рассматриваем, как связать ресурсы из двух моделей развертывания и обеспечить их сосуществование в подписке с помощью шлюзов с подключением типа "сеть — сеть" в виртуальной сети.

## <a name="goal-for-migration"></a>Цель миграции
Resource Manager позволяет развертывать сложные приложения с помощью шаблонов, настраивать виртуальные машины с помощью расширений виртуальных машин и внедрять управление доступом и добавление тегов. Azure Resource Manager позволяет выполнить масштабируемое параллельное развертывание виртуальных машин в группах доступности. Новая модель предоставляет также возможности независимого управления жизненным циклом вычислительных и сетевых ресурсов, а также ресурсов хранилища. И наконец, благодаря принудительному использованию виртуальных машин в виртуальной сети появилась возможность включать защиту по умолчанию.

Почти все функции для выполнения вычислений, работы с сетью и хранилищем из классической модели развертывания поддерживаются в модели Azure Resource Manager. Чтобы использовать преимущества новых возможностей Azure Resource Manager, можно перенести существующие развернутые службы из классической модели развертывания.

## <a name="supported-resources-for-migration"></a>Поддерживаемые при миграции ресурсы
При миграции поддерживаются следующие классические ресурсы IaaS:

* Виртуальные машины
* Группы доступности
* Облачные службы
* Учетные записи хранения
* Виртуальные сети
* VPN-шлюзы;
* шлюзы Express Route _(в той же подписке, в которой находится виртуальная сеть)_;
* группы сетевой безопасности; 
* таблицы маршрутов; 
* Зарезервированные IP-адреса 

## <a name="supported-scopes-of-migration"></a>Поддерживаемые области применения миграции
Существует 4 способа выполнения миграции ресурсов вычисления, хранения и сети. А именно: 

* перенос виртуальных машин (не в виртуальной сети);
* Перенос виртуальных машин (в виртуальной сети)
* Перенос учетных записей хранения
* неприкрепленные ресурсы (группы безопасности сети, таблицы маршрутов и зарезервированные IP-адреса).

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Перенос виртуальных машин (не в виртуальной сети)
В модели развертывания с помощью Resource Manager защита приложений применяется принудительно по умолчанию. В данной модели все виртуальные машины должны находиться в виртуальной сети. Платформа Azure перезапускает (`Stop`, `Deallocate` и `Start`) виртуальные машины в ходе миграции. Для виртуальной сети, в которую перемещаются виртуальные машины, существует два варианта:

* Вы можете отправить к платформе запрос на создание новой виртуальной сети и перенести виртуальную машину в нее.
* Вы можете перенести виртуальную машину в существующую виртуальную сеть в Resource Manager.

> [!NOTE]
> При такой области применения во время миграции, возможно, какое-то время будет запрещено выполнять операции в плоскости управления и плоскости данных.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Перенос виртуальных машин (в виртуальной сети)
Для большинства конфигураций виртуальных машин между классической моделью и моделью развертывания с помощью Resource Manager переносятся только метаданные. Базовые виртуальные машины работают на одном оборудовании, в одной сети и в одном хранилище. При миграции в течение определенного периода, возможно, будет запрещено выполнять операции в плоскости управления. Однако выполнение операций в плоскости данных прерываться не будет. То есть приложения, запущенные в виртуальных машинах (классическая модель), не будут простаивать во время миграции.

Следующие конфигурации сейчас не поддерживаются. В случае добавления поддержки для них в будущем возможен простой некоторых виртуальных машин в этой конфигурации (из-за операций остановки, освобождения и перезапуска).

* В отдельной облачной службе есть несколько групп доступности.
* В отдельной облачной службе есть одна или несколько групп доступности, а также виртуальные машины, которые не входят в группу доступности.

> [!NOTE]
> При такой области применения миграции в течение определенного периода, возможно, будет запрещено выполнять операции в плоскости управления. В некоторых конфигурациях, как описано выше, будет возникать простой операций в плоскости данных.
>
>

### <a name="storage-accounts-migration"></a>Перенос учетных записей хранения
Чтобы обеспечить бесперебойную миграцию, вы можете развернуть виртуальные машины Resource Manager в классической учетной записи хранения. Благодаря этой возможности вычислительные и сетевые ресурсы можно и нужно переносить независимо от учетных записей хранения. После миграции виртуальных машин и виртуальной сети для завершения процесса необходимо перенести учетные записи хранения.

> [!NOTE]
> В модели развертывания с помощью Resource Manager отсутствуют понятия классических образов и дисков. После переноса учетной записи хранения классические образы и диски не будут отображаться в стеке Resource Manager, но резервные виртуальные жесткие диски останутся в учетной записи хранения.
>
>

### <a name="unattached-resources-network-security-groups-route-tables--reserved-ips"></a>Неприкрепленные ресурсы (группы безопасности сети, таблицы маршрутов и зарезервированные IP-адреса)
Группы безопасности сети, таблицы маршрутов и зарезервированные IP-адреса, которые не привязаны к виртуальным машинам и виртуальным сетям, могут быть перенесены независимо друг от друга.

<br>

## <a name="unsupported-features-and-configurations"></a>Неподдерживаемые компоненты и конфигурации
В настоящее время не поддерживаются некоторые компоненты и конфигурации. В следующих разделах приведены рекомендации по ним.

### <a name="unsupported-features"></a>Неподдерживаемые функции
Следующие функции сейчас не поддерживаются. Дополнительно вы можете удалить эти параметры, перенести виртуальные машины, а затем повторно включить параметры в модели развертывания с помощью Resource Manager.

| Поставщик ресурсов | Функция | Рекомендации |
| --- | --- | --- |
| Среда выполнения приложений | Несвязанные диски виртуальных машин. | При миграции учетной записи хранения переносятся большие двоичные объекты виртуальных жестких дисков. |
| Среда выполнения приложений | Образы виртуальных машин. | При миграции учетной записи хранения переносятся большие двоичные объекты виртуальных жестких дисков. |
| Сеть | Списки управления доступом для конечной точки. | Удалите списки управления доступом для конечной точки и повторите попытку миграции. |
| Сеть | Шлюз приложений | Удалите шлюз приложений до начала миграции и повторно создайте его по ее завершении. |
| Сеть | Виртуальные сети с использованием пиринга. | Перенесите виртуальную сеть в Resource Manager с последующей реализацией пиринга. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](../articles/virtual-network/virtual-network-peering-overview.md). | 

### <a name="unsupported-configurations"></a>Неподдерживаемые конфигурации
Следующие конфигурации сейчас не поддерживаются.

| служба | Конфигурация | Рекомендации |
| --- | --- | --- |
| Диспетчер ресурсов |Управление доступом на основе ролей (RBAC) для классических ресурсов |Так как после миграции универсальные коды ресурсов (URI) меняются, рекомендуется спланировать обновления политики RBAC, которые должны быть применены после миграции. |
| Среда выполнения приложений |Несколько подсетей, связанных с одной виртуальной машиной |Обновите конфигурацию подсетей, чтобы в ссылках были указаны только подсети. |
| Среда выполнения приложений |Виртуальные машины, размещенные в виртуальной сети, но без явно назначенной подсети |При необходимости можно удалить виртуальную машину. |
| Среда выполнения приложений |Виртуальные машины с оповещениями и политиками автомасштабирования |По мере выполнения миграции эти параметры будут удалены. Поэтому перед миграцией настоятельно рекомендуется оценить среду. Как вариант, можно настроить параметры оповещений после завершения миграции. |
| Среда выполнения приложений |Расширения виртуальной машины XML (BGInfo 1.*, отладчик Visual Studio, веб-развертывание и удаленная отладка) |Это не поддерживается. Рекомендуется удалить эти расширения из виртуальной машины, чтобы продолжить миграцию, иначе они будут удалены автоматически в процессе миграции. |
| Среда выполнения приложений |Диагностика загрузки при использовании хранилища уровня "Премиум" |Отключите функцию диагностики загрузки для виртуальных машин, прежде чем продолжить миграцию. После завершения переноса можно будет включить диагностику загрузки в стеке Resource Manager. Кроме того, следует удалить большие двоичные объекты, которые используются для хранения снимков экрана и последовательных журналов, чтобы больше не оплачивать их. |
| Среда выполнения приложений | Облачные службы, содержащие веб-роли и рабочие роли | В настоящее время это не поддерживается. |
| Среда выполнения приложений | Облачные службы с несколькими или со множеством групп доступности. |В настоящее время это не поддерживается. Перенесите виртуальные машины в одну группу доступности перед выполнением миграции. |
| Среда выполнения приложений | Виртуальная машина с расширением центра безопасности Azure | Центр безопасности Azure автоматически устанавливает расширения на виртуальные машины, чтобы наблюдать за их безопасности и создавать оповещения. Обычно эти расширения устанавливается автоматически, если для подписки включена политика центра безопасности Azure. Чтобы перенести виртуальные машины, отключите политику центра безопасности в подписке, после чего расширение мониторинга центра безопасности удалится на виртуальных машинах. |
| Среда выполнения приложений | Виртуальная машина с расширением моментальных снимков или архивации | Эти расширения устанавливаются на виртуальной машине, настроенной с помощью службы архивации Azure. Чтобы перенести эти виртуальные машины, следуйте указаниям перечисленным [здесь](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#vault).  |
| Сеть |Виртуальные сети, содержащие виртуальные машины, а также веб-роли и рабочие роли |В настоящее время это не поддерживается. Перенесите рабочие роли и веб-роли в виртуальную сеть перед выполнением миграции. Как только классическая виртуальная сеть будет перенесена, к виртуальной сети Azure Resource Manager можно установить пиринговое подключение из классической виртуальной сети для достижения аналогичной конфигурации, как и прежде.|
| Сеть | Классические каналы ExpressRoute |В настоящее время это не поддерживается. Эти каналы нужно перенести в Azure Resource Manager перед началом миграции IaaS. Дополнительные сведения см. в статье [Перемещение каналов ExpressRoute из классической модели развертывания в модель развертывания с помощью Resource Manager](../articles/expressroute/expressroute-move.md).|
| Служба приложений Azure |Виртуальные сети, содержащие среды службы приложений |В настоящее время это не поддерживается. |
| Azure HDInsight |Виртуальные сети, содержащие службы HDInsight |В настоящее время это не поддерживается. |
| Microsoft Dynamics Lifecycle Services |Виртуальные сети, содержащие виртуальные машины под управлением служб Dynamics Lifecycle Services |В настоящее время это не поддерживается. |
| Доменные службы Azure AD |Виртуальные сети, содержащие доменные службы Azure AD. |В настоящее время это не поддерживается. |
| Azure RemoteApp |Виртуальные сети, содержащие развернутые удаленные приложения Azure RemoteApp. |В настоящее время это не поддерживается. |
| Cлужба управления Azure API  |Виртуальные сети, содержащие развернутые службы управления API Azure. |В настоящее время это не поддерживается. Чтобы перенести виртуальную сеть IaaS, измените виртуальную сеть развернутой службы управления API. Эта операция не влечет за собой простой. |
