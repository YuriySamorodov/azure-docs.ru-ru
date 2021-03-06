---
title: "Создание правил брандмауэра базы данных Azure для MySQL и управление ими с помощью портала Azure | Документация Майкрософт"
description: "Создание правил брандмауэра базы данных Azure для MySQL и управление ими с помощью портала Azure"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 0604b29fcd9849545886a783ae5bbb2cbb72f2ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-by-using-the-azure-portal"></a>Создание правил брандмауэра базы данных Azure для MySQL и управление ими с помощью портала Azure
Правила брандмауэра уровня сервера позволяют администраторам обращаться к серверу базы данных Azure для MySQL с указанного IP-адреса или диапазона IP-адресов. 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Создание правила брандмауэра на уровне сервера с помощью портала Azure

1. В колонке сервера MySQL в разделе "Параметры" щелкните **Безопасность подключения**, чтобы открыть колонку "Безопасность подключения" базы данных Azure для MySQL.

   ![Портал Azure: щелчок пункта "Безопасность подключения"](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Щелкните на панели инструментов **Добавить мой IP-адрес**, чтобы создать правило с IP-адресом компьютера, определенным системой Azure.

   ![Портал Azure: нажатие кнопки "Добавить Мой IP-адрес"](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Проверьте IP-адрес перед сохранением конфигурации. В некоторых случаях IP-адрес, определенный порталом Azure, может отличаться от IP-адреса, используемого для доступа к Интернету и серверам Azure. Поэтому может потребоваться изменить начальный IP-адрес и конечный IP-адрес для правила, чтобы оно функционировало ожидаемым образом.

   Используйте поисковую систему или другой сетевой инструмент, чтобы проверить собственный IP-адрес (например, выполните поиск текста "what is my IP").

   ![Поиск текста what is my IP в Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Добавьте дополнительные адресные пространства. В правилах брандмауэра базы данных Azure для MySQL можно указать отдельный IP-адрес или диапазон адресов. Если вы хотите ограничить правило, указав отдельный IP-адрес, введите его в полях начального и конечного IP-адресов. Открытие брандмауэра позволит администраторам и пользователям входить в любую базу данных на сервере MySQL, для которой у них есть действительные учетные данные.

   ![Портал Azure: правила брандмауэра ](./media/howto-manage-firewall-using-portal/5-specify-addresses.png)


5. На панели инструментов нажмите кнопку **Сохранить**, чтобы сохранить это правило брандмауэра уровня сервера. Дождитесь подтверждения успешного обновления правил брандмауэра.

   ![Портал Azure: нажатие кнопки "Сохранить"](./media/howto-manage-firewall-using-portal/4-save-firewall-rule.png)

## <a name="manage-existing-server-level-firewall-rules-by-using-the-azure-portal"></a>Управление существующими правилами брандмауэра уровня сервера с помощью портала Azure
Повторите действия для управлению правилами брандмауэра.
* Чтобы добавить текущий компьютер, нажмите кнопку **Добавить мой IP-адрес**.
* Чтобы добавить дополнительные IP-адреса, введите **имя правила**, **начальный IP-адрес** и **конечный IP-адрес** в соответствующих полях.
* Чтобы изменить существующее правило, щелкните любое поле в нем и внесите необходимые изменения.
* Чтобы удалить существующее правило, щелкните многоточие [...] и выберите **Удалить**.
* Щелкните **Сохранить** , чтобы сохранить изменения.

## <a name="next-steps"></a>Дальнейшие действия
- Справка по подключению к серверу базы данных Azure для MySQL доступна в разделе [Библиотеки подключений для базы данных Azure для MySQL](./concepts-connection-libraries.md).
