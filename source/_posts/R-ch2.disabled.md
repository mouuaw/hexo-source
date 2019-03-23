---
title: R - 데이터 타입
date: 2019-03-12 22:06:05
tags:
- R
---

# 2장 데이터 타입

## 스칼라
스칼라란 단일 차원의 값을 뜻하는 것으로 숫자 1,2,3,... 을 예로 들 수 있다. 반면 평면위의 점 (1,2)는 2차원 값이므로 스칼라에 해당하지 않는다.

### 숫자
정수, 부동소수 등을 숫자형 데이터 타입으로 지원한다.

### NA 
NA는 Not Available 의 약자로 데이터 값이 없음을 뜻한다.
변수에 NA값이 저장되어 있는지는 is.na() 함수로 확인한다.

```r
is.na(
  x # R의 데이터 객체
)

NA가 저장되어 있으면 TRUE, 그렇지 않으면 FALSE를 반환한다.
```

예를들어, 다음은 변수 four에 NA가 저장되어 있음을 확인하는 코드다.

```r
> is.na(four)
[1] TRUE
```

### NULL
NULL은 NULL객체를 뜻하며, 변수가 초기화되지 않았을 때 사용한다. NULL은 NA와 구분해서 생각해야 한다. 어떤 변수에 NULL이 저장되어 있는지는 is.null()을 사용해 판단할 수 있다.

```r
is.null(
  x # R의 데이터 객체
)

반환 값은 NULL이 저장되어 있으면 TRUE, 그렇지 않으면 FALSE다.
```

다음은 is.null을 사용하는 예제이다

```r
> x <- NULL
> is.null(x)
[1] TRUE
> is.null(1)
[1] FALSE
> is.null(NA)
[1] FALSE
> is.na(NULL)
logical(0)
Warning message:
In is.na(NULL) : is.na() applied to non-(list or vector) of type 'NULL'
```

### 문자열
R은 모든 문자를 문자열로 처리한다.

### 진릿값
TRUE, T는 모두 참 값을 의미한다. FALSE, F는 거짓 값을 의미한다. 진리값에는 &(AND), |(OR), !(NOT) 연산자를 사용할 수 있다.

```R
> TRUE & TRUE
[1] TRUE
> TRUE & FALSE
[1] FALSE
> TRUE | TRUE
[1] TRUE
> TRUE | FALSE
[1] TRUE
> !TRUE
[1] FALSE
> !FALSE
[1] TRUE
```

좀 더 엄밀히 말하면 TRUE와 FALSE는 예약어고 T, F는 각각 TRUE와 FALSE로 초기화된 전역 변수다. 따라서 다음과 같이 T에 FALSE를 할당하는 것이 가능하다! 반면 TRUE는 예약어 이므로 FALSE를 할당할 수 없다. 이런 이유로 TRUE, FALSE 대신 T, F라는 축약 표현을 사용할 때는 주의가 필요하다.

```r
> T <- FALSE
> TRUE <- FALSE
Error in TRUE <- FALSE : invalid (do_set) left-hand side to assignment
```

AND나 OR 연산자에는 &, | 외에도 &&와 ||가 있다. 이들의 차이점은 &, | 는 진리값이 저장된 벡터끼리 연산할 때 요소별로 계산을 한다는 점이다.
&&, || 의 경우 벡터의 요소 간 계산을 하기 위함이 아니라 TRUE && TRUE 등의 경우와 같이 두 개의 진리값끼리 연산을 하기 위한 연산자다.

### 팩터
팩터(Factor) 는 범주형 데이터를 표현하기 위한 테이터 타입이다.

범주형 데이터란 데이터가 사전에 정해진 특정 유형으로만 분류되는 경우를 뜻한다.
범주형 테이터는 또 다시 명목형과 순서형으로 구분된다. 명목형 데이터는 값들 간에 크기 비교가 불가능한 경우를 뜻한다. 반면 순서형 데이터는 대,중,소와 같이 값에 순서를 둘 수 있는 경우를 말한다.

* 팩터 관련 함수

```r
factor( # 팩터 값을 생성한다
  X, # 팩터로 표현하고자 하는 값
  levels, # 값의 레벨
  ordered # TRUE면 순서형, FALSE면 명목형 데이터를 뜻한다. 기본값은 FALSE
)
반환 값은 팩터형 데이터 값이다.

nlevels( # 팩터에서 레벨의 개수를 반환한다.
  x # 팩터 값 
)
반환 값은 팩터 값의 레벨 개수다.

levels( # 팩터에서 레벨의 목록을 반환한다.
  x # 팩터 값
)
반환 값은 팩터에서 레벨의 목록이다.

is.factor( # 주어진 값이 팩터인지를 판단한다.
  x # R 객체
)
반환 값은 x가 팩터면 TRUE, 그렇지 않으면 FALSE다.

ordered( # 순서형 팩터를 생성한다.
  x # 팩터로 표현하고자 하는 값(주로 문자열 백터로 지정)
)

is.ordered( # 순서형 팩터인지를 판단한다.
  x # R 객체
)
반환 값은 x가 순서형 팩터면 TRUE, 그렇지 않으면 FALSE다.
```

## 벡터
벡터는 다른 프로그래밍 언어에서 흔히 접하는 배열의 개념으로, 한 가지 스칼라 데이터 타입의 데이터를 저장할 수 있다. 예를 들면, 숫자만 저장하는 배열, 문자열만 저장하는 배열이 벡터에 해당한다.

### 벡터 생성
벡터는 c()를 사용해 생성하고, names()를 사용해 이름을 부여할 수 있다. 아래는 벡터 관련 함수이다.

```r
주어진 값들을 모아 벡터를 생성한다.
c(
  ... # 벡터로 모을 객체들
)

names: 객체의 이름을 반환한다.
names(
  x # 이름을 얻어올 R 객체
)

names<-: 객체에 이름을 저장한다.
names(
  x # 이름을 저장할 R 객체
) <- value # 저장할 이름
```

### 벡터 데이터 접근
벡터 데이터에 접근하는 문법을 알아보자

* x[n] : 벡터의 n번째 요소,
* x[-n] : 벡터의 n번째 요소를 제외한 나머지
* x[idx_vector] : idx_vector에 지정된 요소를 얻어옴
* x[start:end] : start 부터 end 까지의 값을 반환함

벡터의 길이 관련 함수
```r
length: 객체의 길이를 반환한다.
length(
  x # R 객체, 팩터, 배열, 리스트를 지정한다.
)

NROW: 배열의 행 또는 열의 수를 반환한다
NROW(
  x # 벡터, 배열 또는 데이터 프레임
)
```

### 벡터 연산

```r
identical: 객체가 동일한지를 판단한다.
identical(x, y)

union: 합집합을 구한다.
union(x, y)

intersect: 교집합을 구한다.
intersect(x, y)

setdiff: 차집합을 구한다.
setdiff(x, y)

setequal: x와 y가 같은 집합인지 판단한다.
setequal(x, y)
```

다음은 벡터 연산자들이다.

* value %in% x : 벡터에 value가 저장되어 있는지 판단함
* x + n : 벡터의 모든 요소에 n을 더한 벡터를 구함

### 연속된 숫자로 구성된 벡터

```r
seq: 시퀀스를 생성한다.
seq(
  from, # 시작 값
  to, # 끝 값
  by # 증가치
)
from부터 to 까지의 값을 by 간격으로 저장한 숫자 벡터를 반환한다.

seq_along: 주어진 객체의 길이만큼 시퀀스를 생성한다.
seq_along(
  along.with # 이 인자 길이만큼 시퀀스를 생성한다.
)
반환 값은 along.with의 길이가 N일 때, 1 부터 N까지의 숫자를 저장한 벡터다.
```

### 반복된 값을 저장한 벡터

```r
rep: 주어진 값을 반복한다.
rep(
  x, # 반복할 값이 저장된 벡터
  times, # 전체 벡터의 반복 횟수
  each # 개별 값의 반복 횟수
)
반환 값은 반복된 값이 저장된 x와 같은 타입의 객체다.
```

## 리스트
R에서 리스트는 다른 언어의 해시 테이블이나 딕셔너리와 비슷하다. 키,값 형태의 데이터를 담는 연관배열의 형태를 하고있다.

### 리스트 생성

```r
list: 리스트 객체를 생성한다.
list(
  key1 = value1,
  key2 = value2,
  ...
)
반환 값은 key1에 value1, key2에 value2 등을 저장한 리스트다.
```

### 리스트 데이터 접근

* x$key : 리스트에서 키 값 key에 해당하는 값
* x[n] : 리스트에서 n번째 데이터의 서브리스트
* x : 리스트에서 n번째 저장된 값

## 행렬

### 행렬 생성

```r
matrix: 행렬을 생성한다.
matrix(
  data, # 행렬을 생성할 데이터 벡터
  nrow, # 행의 수
  ncol, # 열의 수
  byrow=FALSE, # TRUE로 설정하면 행우선, FALSE일 경우 열 우선으로 데이터를 채운다.
  dimnames=NULL # 행렬의 각 차원에 부여할 이름
)
반환 값은 행렬이다.

dimnames: 객체의 각 차원에 대한 이름을 가져온다.
dimnames(
  x # R 객체
)
반환 값은 객체 x의 각 차원에 대한 이름이다.

dimnames<- :객체의 차원에 이름을 설정한다.
dimnames(
  x # R 객체
) <- value # 차원에 부여할 이름

rownames: 행렬의 행 이름을 가져온다
rownames(
  x # 2차원 이상의 행렬과 유사한 객체
)
반환 값은 행 이름이다.

rownames<-: 행렬의 행 이름을 설정한다.
rownames(
  x # 2차원 이상의 행렬과 유사한 객체
) <- value # NULL 또는 x 와 같은 길이의 문자열 벡터

colnames: 행렬의 열 이름을 가져온다.
colnames(
  x # 2차원 이상의 행렬과 유사한 객체
)
반환 값은 열 이름이다.

colnames <- : 행렬의 열 이름을 설정한다.
colnames(
  x # 2차원 이상의 행렬과 유사한 객체
) <- value # NULL 또는 x와 같은 길이의 문자열 벡터
```

### 행렬 데이터 접근

* A[ridx, cidx] : 행렬 A의 ridx행, cidx열에 저장된 값.

### 행렬 연산

* A + x : 행렬 A의 모든 값에 스칼라 x를 더한다(사칙연산 동일)
* A + B : 행렬 A와 행렬 B의 합을 구한다.
* A %*% B : 행렬 A와 행렬 B의 곱을 구한다.

다음은 행렬 연산과 관련된 함수들이다.

```r
t 행렬 또는 데이터 프레임의 전치 행렬을 구한다
t(
  x # 행렬 또는 데이터 프레임
)
반환 값은 x의 전치 행렬이다.

solve 수식 a %*% x = b 에서 x를 구한다
solve(
  a, # 행렬
  b  # 벡터 또는 행렬
)
반환 값은 x다. b를 지정하지 않으면 a의 역행렬을 구한다.

nrow 배열의 행의 수를 구한다.
nrow(
  x # 벡터, 배열 또는 데이터 프레임
)
반환 값은 x의 행의 수다.

ncol 배열의 열의 수를 구한다.
ncol(
  x # 벡터, 배열 또는 데이터 프레임
)
행렬의 열의 수를 반환한다.

dim 객체의 차원 수를 구한다.
dim(
  x # 행렬, 배열 또는 데이터 프레임
)
행렬의 차원수를 반환한다.

dim<- 객체의 차원 수를 지정한다.
dim(
  x # 행렬, 배열 또는 데이터 프레임
) <- value # 객체의 차원
```

## 배열
행렬이 2차원 데이터라면 배열은 다차원 데이터다. 예를 들어, 2x3 차원의 데이터를 행렬로 표현한다면 2x3x4 차원의 데이터는 배열로 표현한다.

### 배열 생성

```r
array 배열을 생성한다.
array(
  data=NA, # 데이터를 저장한 벡터
  dim=length(data), # 배열의 차원. 이값을 지정하지 않으면 1차원 배열이 생성된다.
  dimnames=NULL # 차원의 이름
)
배열을 반환한다.
```

### 배열의 데이터 접근
배열의 데이터 접근 방법은 행렬의 경우와 차원의 수만 다를 뿐이지 차이가 없다. 따라서 색인, 이름 등오로 데이터를 접근할 수 있고 dim()을 사용해 데이터의 차원을 알 수 있다.

```r
> (x <- array(1:12, dim=c(2,2,3)))
> x[1,1,1]
> dim(x)
```

## 데이터 프레임
데이터 프레임은 처리할 데이터를 마치 엑셀의 스프레드시트와 같이 표 형태로 정리한 모습을 하고 있다. 데이터 프레임의 각 열에는 관측값의 이름이 저장되고, 각 행에는 매 관측 단위마다 실제 얻어진 값이 저장된다.

### 데이터 프레임 생성
```r
data.frame 데이터 프레임을 생성한다.
data.frame(
  # value 또는 tag=value로 표현된 데이터 값. '...'은 가변 인자를 의미한다.
  ...,
  # 주어진 문자열을 팩터로 저장할 것인지 또는 문자열로 저장할 것인지를 지정하는 인자.
  # 기본값은 보통 TRUE다. 따라서 이 인자를 지정하지 않으면 문자열은 팩터로 저장된다.
  stringsAsFactors=default.stringsAsFactors()
)
데이터 프레임을 반환한다.

str 임의의 R 객체의 내부 구조를 보인다.
str(
  object # 구조를 살펴볼 R 객체
)
```

### 데이터 프레임 사용 문법
* d$colname : 데이터 프레임 d에서 컬럼 이름이 colname인 데이터를 접근한다.
* d$colname <- y : 데이터 프레임 d에서 컬럼 이름이 colname인 컬럼에 데이터 y를 저장한다. 만약 colname이 d에 없는 새로운 이름이라면 새로운 컬럼이 추가된다.


### 데이터 프레임 접근

* d$colname : 데이터 프레임 d의 컬럼 이름 colname에 저장된 데이터
* d[m, n, drop=TRUE] : 데이터 프레임 d의 m행 n컬럼에 저장된 데이터

### 유틸리티 함수

```r
head 객체의 처음 부분을 반환한다.
head(
  x, # 객체
  n=6L # 반환할 결과 값의 크기
)
x의 앞부분을 n만큼 잘라낸 데이터다.

tail 객체의 뒷부분을 반환한다.
tail(
  x, # 객체
  n=6L # 반환할 결과 값의 크기
)
x의 뒷부분을 n만큼 잘라낸 데이터다.

View 데이터 뷰어를 호출한다.
View(
  x, # 데이터 프레임으로 강제 형 변환한 뒤 뷰어로 볼 데이터
  title # 뷰어 윈도우의 제목
)
```

## 타입 판별

* class(x) : 객체 x의 클래스
* str(x) : 객체 x의 내부 구조
* is.factor(x) : 팩터여부
* is.numeric(x) : 숫자를 저장한 백터인가
* is.character(x) : 문자열을 저장한 벡터인가
* is.matrix(x) : 행렬인가
* is.array(x) : 배열인가
* is.data.frame(x) : 데이터 프레임인가

## 타입 변환

* as.factor(x) : 팩터로 변환
* as.numeric(x) : 숫자형 벡터로 변환
* as.character(x) : 문자형 벡터로 변환
* as.matrix(x) : 행렬로 변환
* as.array(x) : 배열로 변환
* as.data.frame(x) : 데이터 프레임으로 변환