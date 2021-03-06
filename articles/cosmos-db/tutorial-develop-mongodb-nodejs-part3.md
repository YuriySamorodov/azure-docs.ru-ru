---
title: "Руководство по MongoDB, Angular и Node — часть 3 | Документация Майкрософт"
description: "Часть 3 серии руководств по созданию в Azure Cosmos DB приложения MongoDB с помощью Angular, Node и тех же API, которые используются для MongoDB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.openlocfilehash: 1fb8d7b9f1014f37f0f3afa20605fce10c45a967
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-3-build-the-ui-with-angular"></a>Создание приложения MongoDB с помощью Angular и Azure Cosmos DB. Часть 3. Создание пользовательского интерфейса с помощью Angular

Из этого руководства в нескольких частях вы узнаете, как создать приложение [API MongoDB](mongodb-introduction.md), написанное на Node.js с использованием Express и Angular, и подключить его к базе данных Azure Cosmos DB.

Часть 3 руководства основана на [части 2](tutorial-develop-mongodb-nodejs-part2.md). Здесь рассматриваются следующие задачи:

> [!div class="checklist"]
> * создание пользовательского интерфейса Angular;
> * использование каскадных таблиц стилей для оформления;
> * локальное тестирование приложения.

## <a name="video-walkthrough"></a>Видеоруководство

> [!VIDEO https://www.youtube.com/embed/MnxHuqcJVoM]

## <a name="prerequisites"></a>Предварительные требования

Перед переходом к этой части руководства убедитесь, что выполнены все задачи из [части 2](tutorial-develop-mongodb-nodejs-part2.md).

> [!TIP]
> В этом руководстве изложены пошаговые инструкции по созданию приложения. Готовое приложение можно скачать из [репозитория angular-cosmosdb](https://github.com/Azure-Samples/angular-cosmosdb) на GitHub.

## <a name="build-the-ui"></a>Создание пользовательского интерфейса

1. В Visual Studio Code нажмите кнопку "Остановить", ![Кнопка "Остановить" в Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/stop-button.png) чтобы остановить выполнение приложения Node.

2. В командной строке Windows или окне терминала Mac введите приведенную ниже команду, чтобы создать компонент Heroes. В этом коде g = создать, c = компонент, heroes = имя компонента. Используется организация неструктурированного файла (--flat), чтобы для него не создавалась вложенная папка.

    ```
    ng g c heroes --flat 
    ```

    В окне терминала отображается подтверждение для новых компонентов.

    ```bash
    installing component
      create src\client\app\heroes.component.ts
      update src\client\app\app.module.ts 
    ```

    Давайте рассмотрим файлы, которые были созданы и обновлены. 

3. В Visual Studio Code в области **Обозреватель** перейдите к новой папке **src\client\app** и откройте новый файл **heroes.component.ts**, созданный на шаге 2. Этот файл компонента TypeScript был создан предыдущей командой.

    > [!TIP]
    > Если в Visual Studio Code не отображается папка приложения, нажмите клавиши CMD + SHIFT + P на компьютере Mac или Ctrl + Shift + P на компьютере Windows, чтобы открыть палитру команд. Затем введите *Reload Window* (Перезагрузить окно), чтобы применить изменение системы.

    ![Открытие файла heroes.component.ts](./media/tutorial-develop-mongodb-nodejs-part3/open-folder.png)

4. В той же папке откройте файл **app.module.ts** и обратите внимание, что компонент `HeroesComponent` добавлен к объявлениям в строке 5 и импортирован в строку 10.

    ![Открытие файла app-module.ts](./media/tutorial-develop-mongodb-nodejs-part3/app-module-file.png)

    Теперь, когда у вас есть компонент Heroes, создайте файл для его HTML-кода. Мы создали минимальное приложение, поэтому код HTML содержится в том же файле, что и TypeScript. Но теперь нам нужно отделить его и создать отдельный файл.

5. В области **Обозреватель** щелкните правой кнопкой мыши папку **app**, выберите пункт **Создать файл**и присвойте новому файлу имя *heroes.component.html*.

6. В файле **heroes.component.ts** удалите строки 5–9 

    ```ts
    template: `
        <p>
          heroes Works!
        </p>
      `,
      ```
      и замените их следующим кодом:
  
    ```ts
    templateUrl: './heroes.component.html',
    ```

    для ссылки на новый HTML-файл.
 
    > [!TIP]
    > Чтобы ускорить разработку, вы можете использовать в Visual Studio Code расширения и фрагменты кода, предоставленные Джоном Папа (John Papa) в наборе основных компонентов Angular. 
    > 1. Нажмите кнопку **Расширения** ![Кнопка "Расширения" в Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/extensions-button.png).
    > 2. Введите в поле поиска *angular essentials*.
    > 3. Щелкните **Install**(Установить). 
    > 4. Нажмите кнопку **Перезагрузить**, чтобы использовать новые расширения.
    > Их также можно скачать по адресу [http://jpapa.me/angularessentials](http://jpapa.me/angularessentials). 
    > ![Расширение в наборе основных компонентов Angular](./media/tutorial-develop-mongodb-nodejs-part3/angular-essentials-extension.png)

7. Вернитесь к файлу **heroes.component.html** и скопируйте в него указанный ниже код. `<div>` — это контейнер для всей страницы. В контейнере содержится список компонентов Hero, которые нам нужно создать, чтобы при щелчке компонента можно было выбрать, изменить или удалить его в пользовательском интерфейсе. В HTML-коде есть несколько стилей, которые можно применить, чтобы выделить выбранный компонент. Также предусмотрена область редактирования для добавления нового компонента Hero или изменения существующего. 

    ```html
    <div>
      <ul class="heroes">
        <li *ngFor="let hero of heroes" (click)="onSelect(hero)" [class.selected]="hero === selectedHero">
          <button class="delete-button" (click)="deleteHero(hero)">Delete</button>
          <div class="hero-element">
            <div class="badge">{{hero.id}}</div>
            <div class="name">{{hero.name}}</div>
            <div class="saying">{{hero.saying}}</div>
          </div>
        </li>
      </ul>
      <div class="editarea">
        <button (click)="enableAddMode()">Add New Hero</button>
        <div *ngIf="selectedHero">
          <div class="editfields">
            <div>
              <label>id: </label>
              <input [(ngModel)]="selectedHero.id" placeholder="id" *ngIf="addingHero" />
              <label *ngIf="!addingHero" class="value">{{selectedHero.id}}</label>
            </div>
            <div>
              <label>name: </label>
              <input [(ngModel)]="selectedHero.name" placeholder="name" />
            </div>
            <div>
              <label>saying: </label>
              <input [(ngModel)]="selectedHero.saying" placeholder="saying" />
            </div>
          </div>
          <button (click)="cancel()">Cancel</button>
          <button (click)="save()">Save</button>
        </div>
      </div>
    </div>
    ```

8. Теперь, когда мы получили код HTML, нужно добавить его в файл **heroes.component.ts**, чтобы обеспечить взаимодействие с шаблоном. Новый код, помещенный в **heroes.component.ts** ниже, добавляет шаблон в наш файл компонента. Добавлен конструктор, который позволяет получать некоторые компоненты Hero и инициализировать компонент службы Hero, чтобы получить все данные. Кроме того, этот код добавляет все необходимые методы для обработки событий в пользовательском интерфейсе. Вы можете скопировать указанный ниже код поверх существующего кода в файле **heroes.component.ts**. 

    ```ts
    import { Component, OnInit } from '@angular/core';

    @Component({
      selector: 'app-heroes',
      templateUrl: './heroes.component.html'
    })
    export class HeroesComponent implements OnInit {
      addingHero = false;
      heroes: any = [];
      selectedHero: Hero;
    
      constructor(private heroService: HeroService) {}
    
      ngOnInit() {
       this.getHeroes();
      }

      cancel() {
        this.addingHero = false;
        this.selectedHero = null;
      }

      deleteHero(hero: Hero) {
        this.heroService.deleteHero(hero).subscribe(res => {
          this.heroes = this.heroes.filter(h => h !== hero);
          if (this.selectedHero === hero) {
            this.selectedHero = null;
          }
        });
      }

      getHeroes() {
        return this.heroService.getHeroes().subscribe(heroes => {
          this.heroes = heroes;
        });
      }

      enableAddMode() {
        this.addingHero = true;
        this.selectedHero = new Hero();
      }

      onSelect(hero: Hero) {
        this.addingHero = false;
        this.selectedHero = hero;
      }

      save() {
        if (this.addingHero) {
          this.heroService.addHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
            this.heroes.push(hero);
          });
        } else {
          this.heroService.updateHero(this.selectedHero).subscribe(hero => {
            this.addingHero = false;
            this.selectedHero = null;
          });
        }
      }
    }
    ```

9. В **обозревателе**откройте файл **app/app.module.ts** и обновите строки 13 (добавьте запятую) и 14, чтобы добавить функцию импорта для `FormsModule`. Раздел импорта теперь должен выглядеть следующим образом:

    ```
    imports: [
      BrowserModule,
      FormsModule
    ],
    ```

10. Добавьте функцию импорта для нового модуля FormsModule в строке 3. 

    ```
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { FormsModule } from '@angular/forms';
    ```

## <a name="use-css-to-set-the-look-and-feel"></a>Использование каскадных таблиц стилей для оформления

1. В области обозревателя откройте файл **src/client/styles.scss**.

2. Скопируйте следующий код в файл **styles.scss**, заменив его текущее содержимое.

    ```css
    /* You can add global styles to this file, and also import other style files */

    * {
      font-family: Arial;
    }
    h2 {
      color: #444;
      font-weight: lighter;
    }
    body {
      margin: 2em;
    }
    
    body,
    input[text],
    button {
      color: #888;
      // font-family: Cambria, Georgia;
    }
    button {
      font-size: 14px;
      font-family: Arial;
      background-color: #eee;
      border: none;
      padding: 5px 10px;
      border-radius: 4px;
      cursor: pointer;
      cursor: hand;
      &:hover {
        background-color: #cfd8dc;
      }
      &.delete-button {
        float: right;
        background-color: gray !important;
        background-color: rgb(216, 59, 1) !important;
        color: white;
        padding: 4px;
        position: relative;
        font-size: 12px;
      }
    }
    div {
      margin: .1em;
    }

    .selected {
      background-color: #cfd8dc !important;
      background-color: rgb(0, 120, 215) !important;
      color: white;
    }

    .heroes {
      float: left;
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      li {
        cursor: pointer;
        position: relative;
        left: 0;
        background-color: #eee;
        margin: .5em;
        padding: .5em;
        height: 3.0em;
        border-radius: 4px;
        width: 17em;
        &:hover {
          color: #607d8b;
          color: rgb(0, 120, 215);
          background-color: #ddd;
          left: .1em;
        }
        &.selected:hover {
          /*background-color: #BBD8DC !important;*/
          color: white;
        }
      }
      .text {
        position: relative;
        top: -3px;
      }
      .saying {
        margin: 5px 0;
      }
      .name {
        font-weight: bold;
      }
      .badge {
        /* display: inline-block; */
        float: left;
        font-size: small;
        color: white;
        padding: 0.7em 0.7em 0 0.5em;
        background-color: #607d8b;
        background-color: rgb(0, 120, 215);
        background-color:rgb(134, 183, 221);
        line-height: 1em;
        position: relative;
        left: -1px;
        top: -4px;
        height: 3.0em;
        margin-right: .8em;
        border-radius: 4px 0 0 4px;
        width: 1.2em;
      }
    }

    .header-bar {
      background-color: rgb(0, 120, 215);
      height: 4px;
      margin-top: 10px;
      margin-bottom: 10px;
    }

    label {
      display: inline-block;
      width: 4em;
      margin: .5em 0;
      color: #888;
      &.value {
        margin-left: 10px;
        font-size: 14px;
      }
    }

    input {
      height: 2em;
      font-size: 1em;
      padding-left: .4em;
      &::placeholder {
          color: lightgray;
          font-weight: normal;
          font-size: 12px;
          letter-spacing: 3px;
      }
    }

    .editarea {
      float: left;
      input {
        margin: 4px;
        height: 20px;
        color: rgb(0, 120, 215);
      }
      button {
        margin: 8px;
      }
      .editfields {
        margin-left: 12px;
      }
    }
    ``` 
3. Сохраните файл. 

## <a name="display-the-component"></a>Отображение компонента

Теперь, когда у нас есть компонент, нужно обеспечить его отображение на экране. Давайте изменим компоненты по умолчанию в файле **app.component.ts**.

1. В области обозревателя откройте **client/app/app.component.ts**.

2. В строках 6–8 измените заголовок на Heroes и добавьте имя компонента, который мы создали в **heroes.components.ts** (app-heroes), чтобы добавить ссылку на этот новый компонент. Раздел шаблона должен выглядеть, как показано ниже: 

    ```ts
    template: `
      <h1>Heroes</h1>
      <div class="header-bar"></div>
      <app-heroes></app-heroes>
    `,
    ```

3. В **heroes.components.ts** есть и другие компоненты, на которые добавляются ссылки так же, как и на компонент Hero. Их также нужно создать. В командной строке Angular CLI используйте указанную ниже команду, чтобы создать модель Hero и файл с именем **hero.ts**, где g = создать, cl = класс и hero = имя класса.

    ```bash
    ng g cl hero
    ```

    В окне терминала отображается подтверждение для нового класса.

    ```bash
    installing class
    create src\client\app\hero.ts
    ```

4. В области обозревателя откройте **src\client\app\hero.ts**.

5. В **hero.ts** замените содержимое файла указанным ниже кодом, который добавляет класс Hero с идентификатором, именем и репликой. 

    ```ts
      export class Hero {
      id: number;
      name: string;
      saying: string;
    }
    ```

6. Вернитесь к файлу **heroes.components.ts** и обратите внимание, что в строке `selectedHero: Hero;` (строка 10) термин `Hero` подчеркнут красной линией. 

7. Щелкните левой кнопкой мыши термин `Hero`, и в Visual Studio отобразится значок лампочки в левой части блока кода. 

    ![Значок лампочки в Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/light-bulb.png)

8. Щелкните значок лампочки и выберите **Import Hero from "client/app/hero".** (Импортировать компонент Hero из client/app/hero.) или **Import Hero from "./hero".** (Импортировать компонент Hero из ./hero.). Сообщение зависит от настроек.

    В строке 2 появится новая строка кода. Если в строке 2 есть ссылка на client/app/hero, измените ее таким образом, чтобы ссылка указывала на файл Hero в локальной папке (./hero). Строка 2 должна выглядеть так:

   ```
   import { Hero } from "./hero";
   ``` 

    Теперь модель готова, но нам по-прежнему необходимо создать службу.

## <a name="create-the-service"></a>Создание службы

1. В командной строке Angular CLI введите указанную ниже команду, чтобы создать службу Hero в **app.module.ts**, где g = создать, s = служба, hero = имя службы, -m = поместить в app.module.

    ```bash
    ng g s hero -m app.module
    ```

    Результат свидетельствует о том, что файл **hero.service.ts** создан, а файл **app.module.ts** обновлен.
  
    ```bash
    installing service
      create src\client\app\hero.service.ts
      update src\client\app\app.module.ts
    ```
    
    В app.module.ts добавлены следующие строки кода (строки 6 и 17):
    
    ```typescript
    import { HeroService } from './hero.service';
    ...
        providers: [HeroService],
    ```

2. В Visual Studio Code откройте файл **hero.service.ts** и скопируйте в него следующий код, заменив содержимое файла.

    ```ts
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    
    import { Hero } from './hero';
    
    const api = '/api';

    @Injectable()
    export class HeroService {
      constructor(private http: HttpClient) {}

      getHeroes() {
        return this.http.get<Array<Hero>>(`${api}/heroes`)
      }

      deleteHero(hero: Hero) {
        return this.http.delete(`${api}/hero/${hero.id}`);
      }

      addHero(hero: Hero) {
        return this.http.post<Hero>(`${api}/hero/`, hero);
      }

      updateHero(hero: Hero) {
        return this.http.put<Hero>(`${api}/hero/${hero.id}`, hero);
      }
    }
    ```

    Этот код использует последнюю версию HttpClient для Angular — модуль, который необходимо указать. Приступим к этой задаче.

3. В Visual Studio Code откройте **app.module.ts** и импортируйте HttpClientModule, включив его в раздел imports.

    ```ts
    imports: [
      BrowserModule,
      FormsModule,
      HttpClientModule
    ],
    ```

4. В **app.module.ts** добавьте инструкцию импорта HttpClientModule в список операций импорта.

    ```ts
    import { HttpClientModule } from '@angular/common/http';
    ```

5. В Visual Studio Code вернитесь к **heroes.components.ts**. Обратите внимание, что в строке `constructor(private heroService: HeroService) {}` (строка 13), термин `HeroService` подчеркнут красной линией. Щелкните `HeroService`, и в левой части блока кода отобразится значок лампочки. Щелкните его и выберите **Import HeroService from "./hero.service ".** (Импортировать HeroService из ./hero.service.) или **Import HeroService from "client/app/hero.service ".** (Импортировать HeroService из client/app/hero.service.).

    Если щелкнуть значок лампочки, в строку 2 вставляется новая строка кода. Если в строке 2 есть ссылка на папку client/app/hero.service, измените ее таким образом, чтобы ссылка указывала на файл Hero в локальной папке (./hero.service). Строка 2 должна выглядеть так:
    
    ```javascript
    import { HeroService } from "./hero.service"
    ```

6. Сохраните все файлы в Visual Studio Code.

## <a name="build-the-app"></a>Сборка приложения

1. В командной строке введите указанную ниже команду, чтобы выполнить сборку приложения Angular. 

    ```bash
    ng b
    ``` 

    При наличии проблем в окне терминала отображаются сведения о файлах, которые нужно исправить. После сборки новые файлы помещаются в папку **dist**. При необходимости вы можете просмотреть новые файлы в папке **dist**.

    Теперь давайте запустим приложение.

2. В Visual Studio Code нажмите кнопку **Отладка** слева ![значок отладки в Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png), а затем — кнопку **Начать отладку** ![значок начала отладки в Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part3/start-debugging-button.png).

3. Теперь откройте браузер и перейдите по адресу **localhost:3000**. Как видите, приложение выполняется локально.

     ![Приложение Hero, которое выполняется в локальной среде](./media/tutorial-develop-mongodb-nodejs-part3/azure-cosmos-db-mongodb-mean-app.png)

## <a name="next-steps"></a>Дальнейшие действия

В этой части руководства мы выполнили следующую задачу:

> [!div class="checklist"]
> * создали пользовательский интерфейс Angular;
> * выполнили локальное тестирование приложения.

Теперь можно приступить к следующей части руководства, чтобы создать учетную запись Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Создание учетной записи Azure Cosmos DB с помощью Azure CLI](tutorial-develop-mongodb-nodejs-part4.md)
