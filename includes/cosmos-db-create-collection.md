Теперь вы можете использовать средство обозреватель данных на портале Azure для создания базы данных и коллекции. 

1. Щелкните **Обозреватель данных** > **Новая коллекция**. 
    
    Справа отобразится область **Добавление коллекции** (вам может потребоваться прокрутить вправо, чтобы увидеть ее).

    ![Колонка "Добавление коллекции" в обозревателе данных на портале Azure](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. На странице **Добавление коллекции** введите параметры для новой коллекции.

    Настройка|Рекомендуемое значение|Описание
    ---|---|---
    Идентификатор базы данных|Задачи|Введите *Задача* в качестве имени новой базы данных. Имена баз данных должны быть длиной от 1 до 255 символов и не могут содержать символы /, \\, #, ? или пробел.
    Идентификатор коллекции|Items|Введите *Элементы* в качестве имени новой коллекции. Для идентификаторов коллекций предусмотрены те же требования к символам, что и для имен баз данных.
    Емкость хранилища| Фиксированный (10 ГБ)|Измените значение на **Фиксированный (10 ГБ)**. Это значение представляет емкость хранилища базы данных.
    Пропускная способность|400 ЕЗ|Укажите для пропускной способности 400 единиц запросов в секунду. Чтобы сократить задержку, позже вы можете увеличить масштаб пропускной способности.
    Ключ секции|/category|Ключ секции, который равномерно распределяет данные в каждой секции. Для создания высокопроизводительной коллекции важно выбрать правильный ключ раздела. См. дополнительные сведения о [проектировании для секционирования](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

    Нажмите кнопку **ОК**.

    В обозревателе данных отобразится новая база данных и коллекция.

    ![Обозреватель данных на портале Azure с новой базой данных и коллекцией](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)