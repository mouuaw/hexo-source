---
title: ionic issue 02
date: 2019-01-15 21:24:37
tags:
- 문제해결
- ionic
---

## 이슈
ionic3 에서 ngrx를 사용하려 했으나 에러와 함께 작동되지 않았다.

같은 예제를 Angular에서 돌려봣을때 문제가 없었다.

## 문제점

ionic3버젼에서 Angular5를 사용하고 있었고 @ngrx/store를 그냥 깔면 7버젼이 깔려 서로 호환이 안된다는걸 알게되었다.

## 해결

@ngrx/store@5 라는 명령어를 이용해서 5버젼을 깔면 ionic3에서도 ngrx/store를 사용할 수 있다. 아무래도 Angular 와 ngrx 의 버젼이 같아야 호환이 되는듯하다.