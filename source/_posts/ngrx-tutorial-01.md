---
title: Ngrx 튜토리얼 - Store
date: 2019-01-10 16:52:19
tags:
- Ngrx
- Angular

---

## 출처 

해당 사이트의 설명을 대부분 참조했고 제 나름대로의 설명을 추가했습니다.
[https://coursetro.com](https://coursetro.com/posts/code/151/Angular-Ngrx-Store-Tutorial---Learn-Angular-State-Management)

## 프로젝트 생성

angular-cli를 이용해서 프로젝트를 생성합니다.

```bash
ng new ngrx-tut
cd ngrx-tut
```

프로젝트에 ngrx/store 를 설치합니다.
```bash
npm install @ngrx/store --save
```

작업을 위해 프로젝트를 띄워봅시다

```bash
ng serve
```
별 다른 작업을 하지않았다면 다음 링크로 프로젝트를 볼 수 있습니다.
[http://localhost:4200](http://localhost:4200)

## Model 생성

다음 경로에 파일을 생성하고 모델을 정의해봅시다.
/src/app/models/tutorial.model.ts
```javascript
export interface Tutorial {
  name: string;
  url: string;
}
```

## Action 생성

ngrx에서 사용되는 Action은 두가지 요소를 가지고 있습니다.

1. type: 보통 Action의 이름을 정의합니다. 
2. payload: Action에 필요한 parameters를 저장할 수 있는 객체입니다.

Action는 마치 Event의 이름들을 정의해놓은 느낌이 있습니다. 여기서 정의한 Action을 Reducer에서 사용하며 Action.type을 통해 각각의 동작을 정의해주게 됩니다.

다음 경로에 Action을 생성해봅시다.
/src/app/actions/tutorial.actions.ts
```javascript
import { Injectable } from '@angular/core'
import { Action } from '@ngrx/store'
import { Tutorial } from '../models/tutorial.model'

export const ADD_TUTORIAL = '[TUTORIAL] Add'
export const REMOVE_TUTORIAL = '[TUTORIAL] Remove'

export class AddTutorial implements Action {
  readonly type = ADD_TUTORIAL

  constructor(public payload: Tutorial) {}
}

export class REmoveTutorial implements Action {
  readonly type = REMOVE_TUTORIAL

  constructor(public payload: number) {}
}

export type Actions = AddTutorial | RemoveTutorial
```

## Reducer 생성

앞서 말했던것처럼 Reducer는 입력된 Action의 type을 통해 해야 할 동작을 명시해줍니다. MVC의 Controller 느낌이랑 비슷합니다.

다음 경로에 Reducer를 생성해봅시다.
/src/app/reducers/tutorial.reducer.ts
```javascript
import { Action } from '@ngrx/store'
import { Tutorial } from '../models/tutorial.model';
import * as TutorialActions from '../actions/tutorial.actions';

// Section 1
const initialState: Tutorial = {
  name: 'Initial Tutorial',
  url: 'http://google.com'
}

export function tutorialReducer(
  state: Tutorial[] = [initialState], 
  action: TutorialActions.Actions
){
  // Section 2
  switch(action.type) {
    case TutorialActions.ADD_TUTORIAL:
      return [...state, action.payload];
    case TutorialActions.REMOVE_TUTORIAL:
      state.splice(action.payload, 1)
      return state;
    
    default:
      return state;
  }
}
```

* Section 1
  초기값으로 사용하기 위한 데이터를 만들어줍니다. 실제로는 데이터가 없을때의 Model을 정의해놓는등 다양하게 활용할 수 있습니다.

* Section 2
  switch 문에 action.type을 이용해서 동작 방식을 결정합니다. 여기선 case 안에 로직을 넣어놧지만 상황에 따라 로직을 분리해서 사용하는것도 좋습니다.

## App State 생성

State 는 '상태' 라는 말로 번역하기는 좀 이해하기 어려운듯 합니다. 기존의 '변수'라는 말과 상당히 유사하게 사용됩니다. component 기반 개발을 하다보면 각각 다른 위치에 있는 component가 같은 데이터를 공유하는 상황이 종종 생기는데, 이를 컴포넌트 자체에서 처리하는건 굉장히 복잡합니다. 

그래서 SingleTon 패턴 형식으로 데이터를 다루게 되는데 여기서 사용되는 App State도 그러한 종류 중 하나로 보면 될듯합니다.

다음 경로에 app.state.ts를 만들어 봅시다.
/src/app/app.state.ts
```javascript
import { Tutorial } from './models/tutorial.model';

export interface AppState {
  readonly tutorial: Tutorial[];
}
```

## app.module.ts 변경

이제 지금까지 만든 store를 등록하는 과정입니다. 
app.module.ts의 import 에 StoreModule을 추가해 봅시다.
/src/app/app.module/ts
```javascript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { StoreModule } from '@ngrx/store';
import { tutorialReducer } from './reducers/tutorial.reducer';

@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    StoreModule.forRoot({
      tutorial: tutorialReducer
    }),
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Component 생성

tutorial reducer를 동작할 component를 만들어봅시다

```bash
ng g c read
ng g c create
```

## Read Component

해당 경로에 read 컴포넌트를 다음과 같이 작성해줍니다.
/src/app/read/read.component.ts

```javascript
import { Component, OnInit } from '@angular/core';
import { Observable } from 'rxjs';
import { Tutorial } from '../models/tutorial.model';
import { AppState } from '../app.state';
import { Store } from '@ngrx/store';
import * as TutorialActions from '../actions/tutorial.actions';

@Component({
  selector: 'app-read',
  templateUrl: './read.component.html',
  styleUrls: ['./read.component.scss']
})
export class ReadComponent implements OnInit {
  // Section 1
  tutorials: Observable<Tutorial[]>;
  // Section 2
  constructor(
    private store: Store<AppState>
  ) { 
    this.tutorials = this.store.select('tutorial')
  }

  ngOnInit() {}

}
```

* Section 1
tutorials라는 Observable을 정의했습니다. 차후에 HTML에서 사용될 변수입니다.

* Section 2
store.select를 통해 tutorial을 불러왔습니다. app.module.ts의 StoreModule.forRoot({}) 에 정의해둔 reducer를 불러오는 동작을 합니다.

이제 HTML을 작성해봅시다.
/src/app/read/read.component.html
```html
<div class="right" *ngIf="tutorials">
  <h3>Tutorials</h3>
  <ul>
    <li *ngFor="let tutorial of tutorials | async">
      <a [href]="tutorial.url" target="_blank">{{ tutorial.name }}</a>
    </li>
  </ul>
</div>
```

HTML과 Angular 문법이기 때문에 딱히.... 여기서 설명드리지 않겟습니다.
그래도 조금 설명 하자면 *ngFor 는 javascript의 for...of 반복문을 생각하시면 됩니다.
async는 tutorials가 Observable이기 때문에 AsyncPipe를 사용해서 자동으로 subscribe 해줍니다.

이제 app.component에서 read.component 를 사용해봅시다.
 /src/app/app.component.html
```html
<app-create></app-create>
<app-read></app-read>
```

css도 조금 추가해주겟습니다.

/src/styles.css
```css
body, html {
    margin: 0;
    padding: 0;
    font-family: 'Arial';
}

.left, .right {
    float:left;
    width: calc(50% - 6em);
    padding: 3em;
}

input[type="text"] {
    width: 100%;
    padding: 5px;
    margin-bottom: 10px;
}
```
여기까지가 등록된 리스트를 보는 방법입니다.

## Create Component

ngrx의 동작은 action -> dispatch -> reducer -> state 변경 이런 순 입니다. 
이런 동작을 잘 알아보기 위해 Create 동작을 해보겟습니다.

다음과 같이 Create component 를 작성해봅시다
/src/app/create/create.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Store } from '@ngrx/store';
import { AppState } from '../app.state';
import * as TutorialActions from "../actions/tutorial.actions";

@Component({
  selector: 'app-create',
  templateUrl: './create.component.html',
  styleUrls: ['./create.component.scss']
})
export class CreateComponent implements OnInit {

  constructor(
    private store: Store<AppState>
  ) { }

  addTutorial(name, url){
    this.store.dispatch(new TutorialActions.AddTutorial({name, url}))
  }

  ngOnInit() {
  }

}
```

Create Html 입니다.
/src/app/create/create.component.html
```html
<div class="left">

  <input type="text" placeholder="name" #name>
  <input type="text" placeholder="url" #url>

  <button (click)="addTutorial(name.value,url.value)">Add a Tutorial</button>
</div>
```

CreateComponent 에서 addTutorial() 을 사용하면 TutorialActions.AddTutorial의 action을 통해 해당하는 reducer 동작을 합니다.

## Remove 구현

remove를 구현하기 위해 read.component.html 을 다음과 같이 바꿔봅시다.

```html
<div class="right" *ngIf="tutorials">
  <h3>Tutorials</h3>
  <ul>
    <li *ngFor="let tutorial of tutorials | async; let i = index">
      <a [href]="tutorial.url" target="_blank">{{ tutorial.name }}</a>
      <button (click)="delTutorial(i)">delete</button>
    </li>
  </ul>
</div>
```

delTutorial을 수행하는 button이 추가되었고 for...of 안에 index를 사용하는 구문이 추가되었습니다.

이제 read.component.ts 와 tutorial.reducer.ts에 remove 동작을 구현해봅시다.

/src/app/read/read.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Observable } from 'rxjs';
import { Tutorial } from '../models/tutorial.model';
import { AppState } from '../app.state';
import { Store } from '@ngrx/store';
import * as TutorialActions from '../actions/tutorial.actions';

@Component({
  selector: 'app-read',
  templateUrl: './read.component.html',
  styleUrls: ['./read.component.scss']
})
export class ReadComponent implements OnInit {

  tutorials: Observable<Tutorial[]>;

  constructor(
    private store: Store<AppState>
  ) { 
    this.tutorials = this.store.select('tutorial')
  }

  ngOnInit() {
  }
  
  // 요 부분을 추가해주세요
  delTutorial(index){
    this.store.dispatch(new TutorialActions.RemoveTutorial(index))
  }

}

```

/src/app/reducers/tutorial.reducer.ts
```javascript
import { Action } from '@ngrx/store'
import { Tutorial } from '../models/tutorial.model';
import * as TutorialActions from '../actions/tutorial.actions';

const initialState: Tutorial = {
  name: 'Initial Tutorial',
  url: 'http://google.com'
}

export function tutorialReducer(
  state: Tutorial[] = [initialState], 
  action: TutorialActions.Actions
){
  switch(action.type) {
    case TutorialActions.ADD_TUTORIAL:
      return [...state, action.payload];

    // remove action을 사용합니다
    case TutorialActions.REMOVE_TUTORIAL:
      state.splice(action.payload, 1)
      return state;
    
    default:
      return state;
  }
}
```

splice를 사용해 리스트를 삭제하고 있습니다. http통신을 통한 list를 제어할 경우 화면상의 빠른 동작을 위해 리스트 전체를 다시 불러오는게 아니라 삭제된 부분만 클라이언트에서 삭제하는 경우가 많습니다. 

다음번엔 @ngrx/effects 를 공부해보겠습니다.