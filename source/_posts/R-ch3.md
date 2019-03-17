---
title: R-R프로그래밍
date: 2019-03-17 01:57:44
tags:
- R
---

## 01 R의 특징
(생략)

## 02 흐름 제어(조건문과 반복문)

### if
```r
if(cond) {
  cond가 참일 때 실행할 문장
}else {
  cond가 거짓일 때 실행할 문장 
}
```

다수의 TRUE, FALSE 데이터를 한 번에 처리한다면 ifelse() 함수를 고려할 수 있다.

```r
ifelse(
  test, # 참, 거짓을 저장한 객체
  yes, # test가 참일 때 선택할 값
  no # test가 거짓일 때 선택할 값
)
test에 다수의 TRUE, FALSE가 저장되어 있을 때 TRUE에 대해서는 yes 값을, FALSE에 대해서는 no 값을 선택하여 반환한다.
```

### 반복문

```r
for(i in data) {
  i를 사용한 문장
}
data에 들어있는 각각의 값을 변수 i에 할당하면서 각각에 대해 블록 안의 문장을 수행한다.

while(cond) {
  조건이 참일 때 수행할 문장
}
조건 cond가 참일 때 블록 안의 문장을 수행한다.

repeat {
  반복해수 수행할 문장
}
블록 안의 문장을 반복해서 수행한다. repeat은 다른 언어의 do-while에 해당한다.
```

반복문 내 블록에서는 break, next 문을 사용해 반복의 수행을 조정할 수 있다.

* break : 반복문을 종료한다.
* next: 현재 수행 중인 반복문 블록의 수행을 중단하고 다음 반복을 시작한다.

## 03 연산

### 수치 연산

* +, -, *, /: 사칙 연산
* n %% m: n 을 m으로 나눈 나머지
* n %/% m: n 을 m으로 나눈 몫
* n^m: n의 m승
* exp(n): e의 n승
* log(x, base=exp(1)): logbase(x), 만약 base가 지정되지 않으면 loge(x)를 계산
* log2(x), log10(x): 각각 log2(x), log10(x)를 계산
* sin(x), cos(x), tan(x): 삼각함수

### 벡터 연산

벡터 연산은 벡터 또는 리스트를 한 번에 연산하는 것을 말한다. 벡터 연산이 중요한 이유는 for 문 등을 사용해 값을 하나씩 처리해나가는 대신 벡터나 리스트를 한 번에 처리하는 것이 더 효율적이고 편리하기 때문이다.

예)
```r
> x <- c(1,2,3,4,5)
> x + x
[1] 2 4 6 8 10
> x == x
[1] TRUE TRUE TRUE TRUE TRUE
> x == c(1,2,3,5,5)
[1] TRUE TRUE TRUE FALSE TRUE
> c(T, T, T) & c(T, F, T)
[1] TRUE FALSE TRUE
```

R의 함수들은 기본적으로 이러한 벡터 기반 연산을 지원한다.

예)
```r
> x <- c(1,2,3,4,5)
> sum(x)
[1] 15
> mean(x)
[1] 3
> median(x)
[1] 3
```

### NA의 처리
NA는 값이 기록되지 않았거나 관칙되지 않은 경우 데이터에 저장되는 값으로 '결측치'라고 부른다.

데이터에 NA가 포함되어 있을 경우 연산 결과가 NA로 바뀐다.

이러한 문제점을 해결하기 위해 많은 R 함수에서 na.rm을 함수 인자로 받는다. na.rm은 NA값이 있을 때 해당하는 값을 연산에서 제외할 것인지를 지정하는 데 사용한다.

```r
> NA + 1
[1] NA
> sum(c(1,2,3,NA))
[1] NA
> sum(c(1,2,3,NA), na.rm=TRUE)
[1] 6
```

NA값에 따라 처리를 다르게 하는 함수들이 존재한다
* na.fail(object, ...) : object에 NA가 포함되어 있으면 실패한다
* na.omit(object, ...) ; object에 NA가 포함되어 있으면 이를 제외한다.
* na.exclude(object, ...) : object에 NA가 포함되어 있으면 이를 제외한다는 점에서 na.omit과 동일하다. 그러나 naresid, napredict를 사용하는 함수에서 NA로 제외한 행을 결과에 다시 추가한다는 점이 다르다.
* na.pass(object, ...) : object에 NA가 포함되어 있더라도 통과시킨다.

## 04 함수의 정의

### 기본 정의

함수의 정의는 다음과 같다
```r
function_name <- function(인자, 인자, ...) {
  함수 본문
  return(반환 값) # 반환 값이 없다면 생략
}
```

### 가변길이 인자
R의 함수는 가변인자를 인자로 받을 수 있다.

```r
> f <- function(...) {
  args <- list(...)
  for(a in args) {
    print(a)
  }
}
> f('3', '4')
[1] "3"
[1] "4"
```
### 주첩 함수
함수 안에 또 다른 함수를 정의하여 사용할 수 있다. 이를 중첩함수 라고 부른다.

```r
> f <- function(x, y) {
  print(x)
  g <- function(y) {
    print(y)
  }
  g(y)
}
> f(1,2)
[1] 1
[1] 2
```

## 05 스코프
코드에 기술한 변수의 사용범위를 정하는것을 스코프라고 한다. R에서는 대부분의 현대적인 프로그래밍 언어가 그러하듯이 문법적 스코프를 사용하며, 문법적 스코프는 변수가 정의된 블록 내부에서만 변수를 접근할 수 있는 구칙을 말한다.

다음은 변수n을 전역으로 선언하고 함수 내부에서 사용한 예다
```r
> n <- 1
> f <- function() {
  print(n)
}
> f()
[1] 1

> f <- function() {
  n <- 2 # 함수 내부에 같은 이름의 변수가 선언될 경우 내부 지역변수가 우선된다.
  print(n)
}
> f()
[1] 2
```

R객체를 메모리에서 삭제하거나 객체를 나열하는 함수를 알아보자

```r
rm: 지정한 환경에서 객체를 삭제한다
rm(
  ..., # 삭제할 객체의 목록
  list=character(), # 삭제할 객체를 나열한 벡터
  envir=as.environment(pos) # 객체를 삭제할 환경
)

ls: 객체를 나열한다
ls(
  name, # 객체를 나열할 환경의 이름
  envir # name 대신 직접 환경을 지정할 경우 사용
)
```

따라서 rm(list = ls())는 메모리에 있는 모든 객체를 삭제하는 명령이 된다.

함수 내부에 선언한 변수는 외부에서 접근할 수 없다.

```r
> f <- function() {
  n <- 1
}
> f()
> n
Error : object 'n' not found
```

함수 내부에서 전역변수에 값을 할당하려면 <<- 을 사용해야 한다

```r
> b <- 0
> f <- function() {
  a <- 1
  g <- function() {
    a <<- 2
    b <<- 2
    print(a)
    print(b)
  }
  g()
  print(a)
  print(b)
}
> f()
[1] 2
[1] 2
[1] 2
[1] 2
```
## 06 값에 의한 전달
R에서는 모든 것이 객체다. 또, 객체는 함수 호출 시 일반적으로 값으로 전달된다. 예외상황도 있지만 대부분 객체가 값으로 넘어간다. 값으로 전달된 객체는 복제되어 넘겨지기 때문에 함수 안에서 객체를 변경해도 원래 객체는 변경되지 않는다.

```r
> f <- function(df2) { ## 이상한데
  df2$a <- c(1,2,3)
}
> df <- data.frame(a=c(4,5,6))
> f(df)
> df
a
1 4
2 5
3 6
```

## 07 객체의 불변성
R의 객체는 불변이다. 객체라는 용어는 메모리에 할당된 데이터 구조들을 뜻한다.
R에는 객체의 복사 추적 관련 함수가 존재한다

```r
trancemem: 객체의 복사를 추적한다
trancemem(
  x # 추적할 r 객체
)

untrancemem: 객체 복사 추적을 중단한다
untrancemem(
  x # 추적을 중단할 R 객체
)
```

다음은 trancemem()을 사용하여 리스트 a를 추적하게 한 다음, 리스트 a의 값을 수정하자 a가 복사됨을 보여준다.

```r
> a <- list()
> trancemem(a)
[1] "<0x081c534>"
> a$b <- c(1,2,3) # 메모리 복사가 발생
trancemem[0x081cd534 -> 0x07e773df4]:
> untrancemem(a)
```

## 08 모듈 패턴
모듈 패턴이랑 외부에서 접근할 수 없는 데이터와 그 데이터를 제어하기 위한 함수로 구성된 구조물을 말한다.
모듈 패턴을 알아보기 위해 '큐' 를 모듈로 구현해보자

```r
> q <- c()
> q_size <- 0
> enqueue < function(data) {
  q <<- c(q, data)
  q_size <<- q_size + 1
}
> dequeue <- function() {
  first <- q[1]
  q <<- q[-1]
  q_size <<- q_size - 1
  return(first)
}
> size <- function() {
  return(q_size)
}
```

사용은 다음과 같다

```r
> enqueue(1)
> enqueue(3)
> enqueue(5)
> print(size()) # 큐 의 길이
[1] 3
> print(dequeue())
[1] 1
> print(dequeue())
[1] 3
> print(dequeue())
[1] 5
> print(size())
[1] 0
```

### 큐 모듈화

앞에 작성한 큐 코드는 q 변수가 전역으로 선언되어 있어서 외부에서 데이터를 조작할 수 있다. 이때 무결성이 깨지는 문제가 생기므로 이를 모듈화 해서 방지해보자.

```r
> queue <- function() {
  q <- c()
  q_size <- 0

  enqueue <- function(data) {
    q <<- c(q, data)
    q_size <<- q_size + 1
  }

  dequeue <- function() {
    first <- q[1]
    q <<- q[-1]
    q_size <<- q_size - 1
    return(first)
  }

  size <- function() {
    return(q_size)
  }

  return(list(enqueue=enqueue, dequeue=dequeue, size=size))
}

이렇게 만든 queue()는 다음과 같이 사용할 수 있다.

```r
> q <- queue()
> q$enqueue(1)
> q$enqueue(3)
> q$size()
[1] 2
> q$dequeue()
[1] 1
> q$dequeue()
[1] 3
> q$size()
[1] 0
```