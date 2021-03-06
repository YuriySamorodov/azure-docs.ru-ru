Очень важно обеспечить безопасность виртуальной машины для выполняемых приложений. Для защиты виртуальных машин можно применить одну или несколько служб или функций Azure, которые обеспечивают безопасный доступ к виртуальным машинам и безопасное хранение данных. Из этой статьи вы узнаете, как обеспечить защиту своих виртуальных машин и приложений.

## <a name="antimalware"></a>Защита от вредоносных программ;

Спектр современных угроз, с которыми сталкиваются пользователи облачных сред, расширяется быстро, что вынуждает искать средства эффективной защиты, обеспечивающие соответствие требованиям к безопасности и нормативным требованиям. [Антивредоносное ПО Майкрософт для Azure](../articles/security/azure-security-antimalware.md) предоставляет бесплатную защиту в реальном времени, которая помогает обнаруживать и устранять вирусы, шпионское ПО и другие вредоносные программы. Вы можете настроить предупреждения, уведомляющие о попытках установки или запуска известных вредоносных или нежелательных программ на виртуальной машине.

## <a name="azure-security-center"></a>Центр безопасности Azure

[Центр безопасности Azure](../articles/security-center/security-center-intro.md) позволяет предотвращать и обнаруживать угрозы на виртуальных машинах, а также реагировать на эти угрозы. Центр безопасности предоставляет встроенные функции мониторинга безопасности и управления политиками для подписок Azure, помогает выявлять угрозы, которые в противном случае могли бы оказаться незамеченными, и взаимодействует с широким комплексом решений по обеспечению безопасности.

## <a name="encryption"></a>Шифрование

Для повышения уровня безопасности и соответствия требованиям [виртуальных машин Windows](../articles/virtual-machines/windows/encrypt-disks.md) и [Linux](../articles/virtual-machines/linux/encrypt-disks.md) содержимое виртуальных дисков в Azure можно зашифровать. Виртуальные диски на виртуальных машинах Windows шифруются в неактивном состоянии с помощью Bitlocker. А виртуальные диски на виртуальных машинах Linux шифруются в неактивном состоянии с помощью dm-crypt. 

В Azure за шифрование виртуальных дисков плата не взимается. Криптографические ключи хранятся в хранилище ключей Azure с применением защиты программного обеспечения. В качестве альтернативы можно импортировать или создать ключи аппаратных модулей безопасности (HSM), сертифицированных по стандартам уровня 2 FIPS 140-2. Эти криптографические ключи используются для шифрования и расшифровки виртуальных дисков, подключенных к виртуальной машине. Вы сохраняете контроль над этими криптографическими ключами и можете проводить аудит их использования. Субъект-служба Azure Active Directory предоставляет безопасный механизм для выдачи этих криптографических ключей при включении и отключении виртуальных машин.

## <a name="key-vault-and-ssh-keys"></a>Хранилище ключей и ключи SSH

Секреты и сертификаты могут моделироваться как ресурсы и предоставляться решением [Key Vault](../articles/key-vault/key-vault-whatis.md). Создать хранилища ключей для [виртуальных машин Windows](../articles/virtual-machines/windows/key-vault-setup.md) можно с помощью Azure PowerShell, а для [виртуальных машин Linux](../articles/virtual-machines/linux/key-vault-setup.md) — с помощью Azure CLI. Вы также можете создать ключи для шифрования.

Политики доступа к хранилищу ключей предоставляют доступ отдельно к ключам, секретам и сертификатам. Например, можно предоставить пользователю доступ только к ключам, но не предоставить разрешения на секреты. Тем не менее разрешения на доступ к ключам и секретам или сертификатам находятся на уровне хранилища. Другими словами, [политика доступа к хранилищу ключей](../articles/key-vault/key-vault-secure-your-key-vault.md) не поддерживает разрешения на уровне объектов.

При подключении к виртуальным машинам необходимо использовать шифрование с открытым ключом, чтобы обеспечить более высокий уровень безопасности при входе на виртуальные машины. Этот процесс предполагает обмен открытыми и закрытыми ключами с использованием команды Secure Shell (SSH), что позволяет аутентифицировать себя, не вводя имя пользователя и пароль. Пароли подвержены атакам методом подбора, особенно на виртуальных машинах с подключением к Интернету, таких как веб-серверы. С помощью пары ключей Secure Shell (SSH) можно создавать [виртуальные машины Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md), использующие ключи SSH для проверки подлинности, что позволяет обойтись без использования паролей для входа. Кроме того, с помощью ключей SSH можно подключаться с [виртуальной машины Windows](../articles/virtual-machines/linux/ssh-from-windows.md) к виртуальной машине Linux.

## <a name="policies"></a>Политики

С помощью [политик Azure Resource Manager](../articles/azure-resource-manager/resource-manager-policy.md) можно определить требуемое поведение [виртуальных машин Windows](../articles/virtual-machines/windows/policy.md) и [Linux](../articles/virtual-machines/linux/policy.md) вашей организации. С помощью политик организация может обеспечить выполнение различных норм и правил во всей организации. Обязательные для выполнения стандарты поведения помогают снизить риск, что способствует успешной деятельности организации.

## <a name="role-based-access-control"></a>Контроль доступа на основе ролей

С помощью [управления доступом на основе ролей (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) вы можете распределить обязанности внутри команды и предоставить пользователям виртуальной машины доступ на уровне, который им необходим для выполнения поставленных задач. Вместо того чтобы предоставлять всем неограниченные разрешения для виртуальной машины, можно разрешить только определенные действия. Вы можете настроить управление доступом для виртуальной машины на [портале Azure](../articles/active-directory/role-based-access-control-configure.md) с помощью [Azure CLI](https://docs.microsoft.com/cli/azure/role) или [Azure PowerShell](../articles/active-directory/role-based-access-control-manage-access-powershell.md).


## <a name="next-steps"></a>Дальнейшие действия
- Выполните действия для мониторинга безопасности виртуальной машины в центре безопасности Azure для [Linux](../articles/virtual-machines/linux/tutorial-azure-security.md) или [Windows](../articles/virtual-machines/windows/tutorial-azure-security.md).