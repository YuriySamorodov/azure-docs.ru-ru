---
title: "Типы оповещений системы безопасности в центре безопасности Azure | Документация Майкрософт"
description: "В этой статье рассматриваются типы оповещений системы безопасности в центре безопасности Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/22/2017
ms.author: yurid
ms.openlocfilehash: 829657664cf1e37b22d57c62614300a205b5e91c
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/22/2017
---
# <a name="understanding-security-alerts-in-azure-security-center"></a>Основные сведения об оповещениях системы безопасности в центре безопасности Azure
Из этой статьи вы узнаете о различных типах оповещений системы безопасности и связанных аналитических данных, доступных в центре безопасности Azure. Дополнительные сведения об управлении оповещениями и инцидентами см. в статье [Управление оповещениями безопасности в центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md).

Чтобы настроить расширенное обнаружение, обновите центр безопасности Azure до уровня "Стандартный". Бесплатный пробный период составляет 60 дней. Чтобы выполнить обновление, выберите в [политике безопасности](security-center-policies.md) соответствующую **ценовую категорию**. Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Центр безопасности выпустил ограниченную предварительную версию нового набора обнаружений, которые используют записи аудита (общую платформу аудита), для обнаружения вредоносного поведения на компьютерах Linux. Отправьте [нам](mailto:ASC_linuxdetections@microsoft.com) электронное письмо с идентификаторами подписок, чтобы получить предварительную версию.

## <a name="what-type-of-alerts-are-available"></a>Какие типы оповещений доступны?
В центре безопасности Azure используются различные [возможности обнаружения](security-center-detection-capabilities.md), чтобы оповещать клиентов о потенциальных атаках на их среды. Эти оповещения содержат важные сведения о причине их активации, ресурсах, на которые нацелена атака, и ее источнике. Сведения в оповещениях могут отличаться в зависимости от типа аналитических служб, используемых для обнаружения атак. Инциденты также могут содержать дополнительные контекстные сведения, которые могут быть полезны при анализе угроз.  В этой статье приведены сведения о следующих типах оповещений:

* анализ поведения виртуальных машин;
* анализ сети;
* анализ ресурсов.
* контекстные сведения.

## <a name="virtual-machine-behavioral-analysis"></a>Анализ поведения виртуальных машин
Поведенческая аналитика позволяет центру безопасности Azure выявлять скомпрометированные ресурсы. Для этого центр безопасности анализирует журналы событий виртуальных машин, например события создания процессов, входов в систему и др. Кроме того, получаемые данные сверяются с другими сигналами. Это дает возможность получить подтверждающие доказательства масштабной кампании.

> [!NOTE]
> Дополнительные сведения о способах обнаружения угроз в центре безопасности см. в статье [Возможности обнаружения центра безопасности Azure](security-center-detection-capabilities.md).
>

### <a name="crash-analysis"></a>Анализ сбоев
Анализ памяти аварийного дампа — это метод определения сложных вредоносных программ, способных обойти традиционные решения по обеспечению безопасности. Вредоносные программы различных типов пытаются уменьшить вероятность своего обнаружения с помощью антивирусных программ. Для этого они никогда не записывают данные на диск или шифруют записанные на диск программные компоненты. Из-за этого подхода вредоносные программы трудно обнаружить традиционными методами. Зато такую программу можно обнаружить в ходе анализа памяти, так как, функционируя, программа не может не оставлять следов в памяти.

Когда программа дает сбой, аварийный дамп записывает часть памяти в момент сбоя. Вредоносная программа, общее пользование или проблемы системы могут вызвать сбой. Анализируя память в аварийном дампе, центр безопасности может выявить методы, с помощью которых злоумышленник эксплуатирует уязвимости ПО, получает доступ к конфиденциальным данным и тайно пользуется скомпрометированным компьютером. При этом производительность узлов почти не страдает, так как анализ выполняется на сервере центра безопасности.

Следующие поля являются общими для оповещений об аварийном дампе, которые встречаются далее в этой статье.

* Dumpfile (файл дампа) — имя файла аварийного дампа.
* Processname (имя процесса) — имя процесса, аварийно завершившего работу.
* Processversion (версия процесса) — версия процесса, аварийно завершившего работу.

### <a name="code-injection-discovered"></a>Обнаружение внедрения кода
Внедрение кода — это вставка исполняемых модулей в выполняемые процессы или потоки.  Такой способ используют вредоносные программы, чтобы получить доступ к данным и скрыть или предотвратить их удаление (например, сохраняемость). Это оповещение означает, что внедренный модуль присутствует в аварийном дампе. Разработчики подлинного программного обеспечения иногда внедряют код без злого умысла, например для изменения или расширения функциональных возможностей существующего приложения или компонента операционной системы.  Чтобы отличить внедренные вредоносные модули от невредоносных, центр безопасности проверяет, соответствуют ли внедренные модули профилю подозрительного поведения. В оповещении результат такой проверки отображается в поле Signature (подпись), а также отражается в уровне серьезности, описании оповещения и шагах по исправлению. 

Это оповещение содержит такие дополнительные поля:

- Address (Адрес) — расположение внедренного модуля в памяти.
- Imagename (Имя образа) — имя внедренного модуля. Обратите внимание, что это поле может быть пустым, если имя образа не указано в образе.
- Signature (Подпись) — указывает, соответствует ли внедренный модуль профилю подозрительного поведения. 

В следующей таблице приведены примеры результатов и их описание.

| Значение подписи                      | Описание                                                                                                       |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Suspicious reflective loader exploit (Подозрительный эксплойт загрузчика с отражением) | Это подозрительное поведение часто связано с загрузкой внедренного кода независимо от загрузчика ОС. |
| Suspicious injected exploit (Подозрительно внедренный эксплойт)          | Означает вредоносное поведение, которое часто связано с внедрением кода в память.                                       |
| Suspicious injecting exploit (Подозрительное внедрение эксплойта)         | Означает вредоносное поведение, которое часто связано с использованием внедренного кода в памяти.                                   |
| Suspicious injected debugger exploit (Подозрительное внедрение эксплойта отладчика) | Означает вредоносное поведение, которое часто связано с обнаружением или обходом отладчика.                         |
| Suspicious injected remote exploit (Подозрительное внедрение удаленного эксплойта)   | Означает вредоносное поведение, которое часто связано со сценариями контроля и управления (C2).                                 |

Ниже приведен пример такого оповещения.

![Оповещение о внедрении кода](./media/security-center-alerts-type/security-center-alerts-type-fig21.png)

### <a name="suspicious-code-segment"></a>Подозрительный сегмент кода
Подозрительный сегмент кода указывает на то, что сегменты кода выделены нестандартными способами, которые используются, например, при внедрении эксплойта с отражением и скрытии процесса.  Кроме того, оповещение позволяет проверить дополнительные характеристики сегмента кода, чтобы предоставить контекст относительно возможностей и поведения этого сегмента.

Это оповещение содержит такие дополнительные поля:

- Address (Адрес) — расположение внедренного модуля в памяти.
- Size (Размер) — размер подозрительного сегмента кода.
- Stringsignatures (Подписи строк) — в этом поле перечислены возможности интерфейсов API, имена функций которых содержатся в сегменте кода. Некоторые примеры возможностей:
    - дескрипторы раздела образа, динамическое выполнение кода для 64-разрядной версии системы, выделение памяти и выполнение функций загрузчика, возможность внедрения удаленного кода, возможность перехвата управления, считывание переменных среды, считывание памяти произвольного процесса, запрос или изменение разрешений маркера доступа, сетевое взаимодействие по протоколу HTTP или HTTPS и сетевое взаимодействие через сокеты.
- Imagedetected (Обнаруженный образ) — это поле указывает, внедрен ли образ среды предустановки в процесс, в котором обнаружен подозрительный сегмент кода, и с какого адреса запускается внедренный модуль.
- Shellcode (Код оболочки) — это поле указывает на присутствие поведения, которое часто используется в атакующем коде вредоносных программ для получения доступа к дополнительным функциям операционной системы, требующих особой защиты. 

Ниже приведен пример такого оповещения.

![Оповещение о подозрительном сегменте кода](./media/security-center-alerts-type/security-center-alerts-type-fig22.png)

### <a name="shellcode-discovered"></a>Обнаружение кода оболочки
Код оболочки — это атакующий код, который выполняется, когда вредоносная программа воспользовалась уязвимостью программного обеспечения. Такое оповещение означает, что анализ с помощью аварийного дампа обнаружил исполняемый код, поведение которого типично для атакующего кода вредоносных программ. Хотя невредоносная программа тоже может так себя вести, такое поведение нетипично для обычного программного обеспечения.

Это оповещение о коде оболочки содержит дополнительное поле:

* Address (адрес) — расположение кода оболочки в памяти.

Ниже приведен пример такого оповещения.

![Оповещение о коде оболочки](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Обнаружение перехватов модулей
Windows использует библиотеки динамической компоновки (DLL), чтобы разрешать программному обеспечению использовать стандартные функции системы Windows. Перехват DLL происходит, когда вредоносная программа изменяет порядок загрузки библиотек DLL, чтобы загрузить вредоносный атакующий код в память, где может выполняться произвольный код. Это предупреждение означает, что анализ аварийного дампа обнаружил модуль с одинаковым именем, который загружается из двух разных расположений. Один из путей загрузки — стандартное расположение двоичных файлов Windows.

Разработчики безопасного программного обеспечения иногда изменяют порядок загрузки библиотек DLL без злого умысла, например для инструментирования, расширения возможностей операционной системы или приложений Windows. Чтобы отличить вредоносные изменения от потенциально неопасных изменений в порядке загрузки библиотек DLL, центр безопасности Azure проверяет, соответствуют ли загруженные модули подозрительному профилю. В оповещении результат такой проверки отображается в поле Signature (подпись), а также отражается в уровне серьезности, описании оповещения и шагах по исправлению. Чтобы выяснить, является модуль безопасным или вредоносным, проанализируйте копию перехваченного модуля на диске. Например, проверьте цифровую подпись файла или запустите сканирование на вирусы.

Кроме стандартных полей, описанных в разделе "Обнаружение кода оболочки", это оповещение содержит следующие поля:

* Signature (подпись) — указывает, соответствует ли перехваченный модуль профилю подозрительного поведения.
* Hijackedmodule (перехваченный модуль) — имя перехваченного модуля системы Windows.
* Hijackedmodulepath (путь к перехваченному модулю) — путь к перехваченному модулю системы Windows.
* Hijackingmodulepath (путь к модулю-перехватчику) — путь к модулю-перехватчику.

Ниже приведен пример такого оповещения.

![Оповещение о перехвате модулей](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Обнаружение маскирующихся модулей Windows
Вредоносные программы могут использовать стандартные имена двоичных файлов и модулей Windows (например, Svchost.exe и Ntdll.dll), чтобы *смешаться во множестве* файлов и скрыть природу вредоносных программ от системных администраторов. Такое оповещение в ходе анализа аварийного дампа означает, что в файле дампа обнаружены модули, которые используют имена модулей системы Windows, но не отвечают другим критериям, характерным для модулей Windows. Анализ хранящейся на диске копии маскирующегося модуля даст больше информации, которая поможет помочь определить его истинный характер. Анализ может состоять из таких этапов:

* Убедитесь, что рассматриваемый файл является частью безопасного программного обеспечения.
* Проверьте цифровую подпись файла.
* Проверьте файл на наличие вирусов.

Кроме стандартных полей, описанных в разделе "Обнаружение кода оболочки", это оповещение содержит дополнительные поля.

* Details (подробные сведения) — указывает, допустимы ли метаданные модуля и был ли модуль загружен из системного расположения.
* Name (имя) — имя маскирующегося модуля Windows.
* Path (путь) — путь маскирующегося модуля Windows.

Оповещение также содержит некоторые поля из заголовка модуля PE, например Checksum (контрольная сумма) и Timestamp (метка времени). Эти поля отображаются, только если они присутствуют в модуле. Дополнительную информацию об этих полях см. в [спецификациях Microsoft PE и COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Ниже приведен пример такого оповещения.

![Оповещение о маскирующихся модулях Windows](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Обнаружение измененных системных двоичных файлов
Вредоносные программы могут изменять основные системные двоичные файлы, чтобы получить скрытый доступ к данным или тайно храниться в поврежденной системе. Такое оповещение в ходе анализа аварийного дампа означает, что обнаружено изменение базовых двоичных файлов Windows в памяти или на диске.

Разработчики подлинного программного обеспечения иногда изменяют системные модули в памяти без злого умысла, например для перенаправления или обеспечения совместимости приложений. Чтобы отличить вредоносные модули от потенциально неопасных, центр безопасности Azure проверяет, соответствуют ли измененные модули подозрительному профилю. Результат этой проверки отражается в уровне серьезности, описании оповещения и шагах по исправлению.

Кроме стандартных полей, описанных в разделе "Обнаружение кода оболочки", это оповещение содержит дополнительные поля.

* Modulename (имя модуля) — имя измененного системного двоичного файла.
* Moduleversion (версия модуля) — версия измененного системного двоичного файла.

Ниже приведен пример такого оповещения.

![Оповещение о системном двоичном файле](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Выполнение подозрительных процессов
Центр безопасности обнаруживает, что на целевой виртуальной машине выполняется подозрительный процесс, и активирует оповещение. Во время обнаружения ищется не конкретное имя, а параметр исполняемого файла. Поэтому, даже если злоумышленник переименует исполняемый файл, центр обеспечения безопасности все равно сможет обнаружить подозрительный процесс.

Ниже приведен пример такого оповещения.

![Оповещение о подозрительном процессе](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domains-accounts-queried"></a>Запрос к нескольким учетным записям домена
Центр безопасности может обнаружить несколько попыток запроса учетных записей домена Active Directory. Обычно злоумышленники осуществляют такие попытки при разведке сети. Злоумышленники могут использовать этот метод для определения пользователей, учетных записей администраторов домена, компьютеров, выполняющих роль контроллеров домена, а также возможных доверительных отношений домена с другими доменами.

Ниже приведен пример такого оповещения.

![Оповещения о нескольких учетных записях домена](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

### <a name="local-administrators-group-members-were-enumerated"></a>Перечислены участники группы локальных администраторов

Центр безопасности создаст оповещение, если инициировано событие безопасности 4798, в Windows Server 2016 и Windows 10. Это происходит, если перечислены группы локальных администраторов, что обычно делают злоумышленники при разведке сети. Злоумышленники могут использовать этот метод для запроса идентификации пользователей с привилегиями администратора.

Ниже приведен пример такого оповещения.

![Локальный администратор](./media/security-center-alerts-type/security-center-alerts-type-fig14-new.png)

### <a name="anomalous-mix-of-upper-and-lower-case-characters"></a>Аномальное сочетание символов верхнего и нижнего регистра

Центр безопасности создает оповещение, когда обнаруживает сочетание символов верхнего и нижнего регистра в командной строке. Некоторые злоумышленники могут использовать этот метод, чтобы обойти правила на основе хэша или с учетом регистра.

Ниже приведен пример такого оповещения.

![Аномальное сочетание](./media/security-center-alerts-type/security-center-alerts-type-fig15-new.png)

### <a name="suspected-kerberos-golden-ticket-attack"></a>Возможная атака с помощью "золотого билета" Kerberos

Злоумышленник может использовать взломанный ключ [krbtgt](https://technet.microsoft.com/library/dn745899.aspx) для создания "золотых билетов" Kerberos, которые позволяют олицетворять любого пользователя. Центр безопасности активирует оповещение при обнаружении этого типа действия.

> [!NOTE] 
> Дополнительные сведения о "золотом билете" Kerberos см. в [руководстве по устранению рисков кражи учетных данных Windows 10](http://download.microsoft.com/download/C/1/4/C14579CA-E564-4743-8B51-61C0882662AC/Windows%2010%20credential%20theft%20mitigation%20guide.docx).

Ниже приведен пример такого оповещения.

!["Золотой билет"](./media/security-center-alerts-type/security-center-alerts-type-fig16-new.png)

### <a name="suspicious-account-created"></a>Создана подозрительная учетная запись

Центр безопасности активирует оповещение при создании учетной записи, которая напоминает существующую учетную запись с правами администратора. Этот метод могут использовать злоумышленники для создания фальшивой учетной записи, которую человек не может заметить во время ручной проверки.
 
Ниже приведен пример такого оповещения.

![Подозрительная учетная запись](./media/security-center-alerts-type/security-center-alerts-type-fig17-new.png)

### <a name="suspicious-firewall-rule-created"></a>Создано подозрительные правило брандмауэра

Злоумышленники могут попытаться обойти систему безопасности узла, создав пользовательские правила брандмауэра, которые позволяют вредоносным приложениям взаимодействовать с командами и элементами управления или запускать атаки по сети через взломанный узел. Центр безопасности создает оповещение, если он обнаруживает новое правило брандмауэра, созданное из исполняемого файла в подозрительном расположении.
 
Ниже приведен пример такого оповещения.

![Правило брандмауэра](./media/security-center-alerts-type/security-center-alerts-type-fig18-new.png)

### <a name="suspicious-combination-of-hta-and-powershell"></a>Подозрительное сочетание HTA и PowerShell

Центр безопасности активирует оповещение, если он обнаруживает, что узел приложения Microsoft HTML (HTA) запускает команды PowerShell. Злоумышленники используют этот метод для запуска вредоносных скриптов PowerShell.
 
Ниже приведен пример такого оповещения.

![HTA и PS](./media/security-center-alerts-type/security-center-alerts-type-fig19-new.png)


## <a name="network-analysis"></a>анализ сети;
Функция обнаружения угроз сети в центре безопасности автоматически собирает информацию о безопасности из трафика IPFIX Azure. Она анализирует эту информацию, часто сравнивая сведения из разных источников и определяя угрозы.

### <a name="suspicious-outgoing-traffic-detected"></a>Обнаружение подозрительного исходящего трафика
Сетевые устройства можно обнаружить и профилировать так же, как и другие системы. Злоумышленники обычно начинают со сканирования или перебора портов. В следующем примере обнаружен подозрительный трафик Secure Shell (SSH) с виртуальной машины. В этом сценарии возможна SSH-атака на внешний ресурс методом подбора или перебора портов.

![Оповещение о подозрительном исходящем трафике](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Это оповещение содержит сведения, с помощью которых можно идентифицировать ресурс, используемый для проведения атаки. Это оповещение содержит сведения, с помощью которых можно идентифицировать скомпрометированный компьютер, время обнаружения, а также протокол и порт, которые были использованы. Эта страница также содержит список действий, которые помогут устранить проблему.

### <a name="network-communication-with-a-malicious-machine"></a>Сетевое взаимодействие с вредоносным компьютером
Используя каналы Майкрософт для аналитики угроз, центр безопасности Azure может обнаружить скомпрометированные компьютеры, которые взаимодействуют с вредоносными IP-адресами. В большинстве случаев этот IP-адрес является центром контроля и управления. В нашем примере центр безопасности обнаружил, что обмен данными выполнялся с помощью вредоносной программы Pony Loader (также известной как [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![оповещение о сетевом взаимодействии](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Это оповещение содержит информацию, которая позволяет определить ресурс, использованный для проведения атаки, атакованный ресурс, атакованный IP-адрес, IP-адрес злоумышленника и время обнаружения.

> [!NOTE]
> Настоящие IP-адреса удалены с этого снимка экрана в целях конфиденциальности.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Обнаружение возможной исходящей атаки типа "отказ в обслуживании"
Обнаружив аномальный сетевой трафик, исходящий из одной виртуальной машины, центр безопасности может активировать оповещение о возможной атаке типа "отказ в обслуживании".

Ниже приведен пример такого оповещения.

![Исходящая атака типа "отказ в обслуживании"](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Анализ ресурсов
При анализе ресурсов в центре безопасности основное внимание уделяется службам PaaS (платформа как услуга), например службам, интегрированным с [системой обнаружения угроз для базы данных SQL Azure](../sql-database/sql-database-threat-detection.md). На основе анализа результатов в этих областях центр безопасности выдает оповещение о ресурсах.

### <a name="potential-sql-injection"></a>Потенциальная атака путем внедрения кода SQL
Внедрение кода SQL — это атака, во время которой вредоносный код вставляется в строки, которые позже будут переданы экземпляру SQL Server для анализа и выполнения. Так как SQL Server выполняет все полученные запросы, являющиеся синтаксически правильными, все процедуры, создающие инструкции SQL, необходимо проверять на предмет уязвимости к внедрению. Чтобы определять подозрительные события, которые могут происходить в базах данных SQL Azure, система обнаружения угроз SQL использует машинное обучение, анализ поведения и обнаружение аномалий. Например:

* попытка бывшего сотрудника получить доступ к базе данных;
* атака путем внедрения кода SQL;
* необычная попытка пользователя получить доступ к рабочей базе данных из домашней системы.

![Оповещение о потенциальной атаке путем внедрения кода SQL](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Используя сведения из этого оповещения, можно идентифицировать атакованный ресурс, время обнаружения и состояние атаки. Оповещение также содержит ссылку на следующие этапы выявления.

### <a name="vulnerability-to-sql-injection"></a>Уязвимость к атакам путем внедрения кода SQL
Это оповещение активируется при обнаружении ошибки приложения в базе данных. Оно может указывать на возможную уязвимость к атакам путем внедрения кода SQL.

![Оповещение о потенциальной атаке путем внедрения кода SQL](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Обычный вход из неизвестного расположения
Это оповещение активируется, когда на сервере обнаруживается попытка доступа с незнакомого IP-адреса, который не использовался за последний период.

![Оповещение о необычном доступе](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="contextual-information"></a>Контекстные сведения
В ходе исследования аналитикам требуется дополнительный контекст, чтобы выяснить характер угрозы и способ ее предотвращения.  Например, обнаружена аномалия сети, но без информации о других событиях в сети или о целевом ресурсе трудно понять, какие действия нужно предпринять. Поэтому инцидент безопасности может включать артефакты, связанные события и сведения, которые будут полезны исследователям. Доступность дополнительных сведений зависит от типа обнаруженной угрозы и конфигурации среды. Они могут быть недоступны для некоторых инцидентов безопасности.

Доступные дополнительные сведения отображаются в окне инцидента безопасности под списком оповещений. Эти сведения могут содержать следующее:

- события очистки журналов;
- название самонастраивающегося устройства, подключенного через неизвестное устройство;
- неактивные оповещения. 

![Оповещение о необычном доступе](./media/security-center-alerts-type/security-center-alerts-type-fig20.png) 


## <a name="see-also"></a>См. также
Из этой статьи вы узнали о различных типах оповещений системы безопасности в центре безопасности. Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Обработка инцидентов в центре безопасности Azure](security-center-incident.md)
* [Возможности обнаружения центра безопасности Azure](security-center-detection-capabilities.md)
* [Руководство по планированию использования центра безопасности Azure и работе в нем](security-center-planning-and-operations-guide.md)
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md). Часто задаваемые вопросы об использовании этой службы.
* [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/). Публикации блога, посвященные вопросам безопасности и соответствия требованиям в Azure.
