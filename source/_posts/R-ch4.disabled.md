---
title: 4장 데이터 조작 1
date: 2019-03-20 22:55:14
tags:
- R
---

## 01 아이리스 데이터

아이리스 데이터는 R에 기본으로 내장되어 있고, 이해하기 쉬우며 크기가 작고 기계 학습에서 인기 있는 분야 중 하나인 분류에 적합한 데이터다. 이런 이유로 아이리스는 R뿐만 아니라 다른 데이터 분석이나 기계학습 관련 라이브러리에서 자주 사용되고 있다.

* 아이리스 데이터

Species(Factor): 붓꽃의 종. setosa, versicolor, virginica 세 가지 값 중 하나
Sepal.Width(Number): 꽃받침의 너비
Sepal.Length(Number): 꽃받침의 길이
Petal.Width(Number): 꽃잎의 너비
Petal.Length(Number): 꽃잎의 길이

```R
> head(iris)

  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1         5.1         3.5         1.4         0.2   setosa
2         4.9         3.0         1.4         0.2   setosa
3         4.7         3.2         1.3         0.2   setosa
4         4.6         3.1         1.5         0.2   setosa
5         5.0         3.6         1.4         0.2   setosa
6         5.4         3.9         1.7         0.4   setosa

> str(iris)

'data.frame':    150 obs. of 5 variables:
 $ Sepal.Length: num 5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num 3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num 1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num 0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ..
```

iris에는 붓꽃 데이터가 데이터 프레임으로 저장되어 있는 반면, iris3에는 3차월 배열 형태로 저장되어 있다.

이외에도 R에는 다양한 데이터 셋이 준비되어 있다. datasets 패키디에 있는 데이터 셋은 R에 기본적으로 포함된 데이터들이며, 이 데이터의 목록은 library(help=datasets) 명령으로 살펴볼 수 있다.

* AirPassenger: 1949년부터 1960년까지의 항공기 승객 수
* airquality: 1973년 5월부터 9월까지의 뉴욕 대기 오염 정도에 대한 기록
* cars: 자동차의 주행 속도에 따른 제동 거리
* mtcars: 1974년 미국 모터 트렌드 매거진에 실린 32개 자동차에 대해 연료 효율을 비롯한 10여 가지 특징을 기록
* Titanic: 타이타닉 호의 생존자 정보를 호실(1등실, 2등실, 3등실), 성별, 나이, 생존 여부로 정리
* InsectSprays: 6종류의 살충제를 사용했을 때 각 살충제에 대해 살아남은 벌레의 수
* Orange: 오렌지 나무의 종류, 연령, 둘레
* swiss: 1888년경 프랑스어를 사용하는 스위스 내 47개 주의 출산율과 사회 경제적 지표(농업 종사자 비율, 군 입대 시험 성적, 교육 등)

이들 데이터를 사용할 때는 'data(데이터 셋 이름)' 명령을 사용한다. 예를들어, mtcars를 살펴보려면 다음과 같은 명령을 사용한다.

```R
> data(mtcars)
> head(mtcars)
```
mtcars 데이터 셋의 상세 내용르 알고 싶다면 ?mtcars 또는 help(mtcars) 명령을 사용한다.

## 02 파일 입출력

### CSV 파일 입출력

```R
read.csv: CSV 파일을 데이터 프레임으로 읽어들인다.
read.csv(
  file, # 파일명
  header=FALSE, # 파일의 첫 행을 헤더로 처리할 것인지 여부
  # 데이터에 결측치가 포함되어 있을 경우 R의 NA에 대응시킬 값을 지정한다.
  # 기본값은 "NA"로, "NA"로 저장된 문자열들은 R의 NA로 저장된다.
  na.strings="NA",
  # 문자열을 팩터로 저장할지 또는 문자열로 저장할지 여부를 지정하는 데 사용한다. 별다른
  # 설정을 하지 않았다면 기본값은 보통 TRUE다.
  stringsAsFactors=default.stringsAsFactors()
)

반환 값은 데이터 프레임이다.

write.csv: 데이터 프레임을 csv로 저장한다.
write.csv(
  x, # 파일에 저장할 데이터 프레임 또는 행렬
  file="", # 데이터를 저장할 파일명
  row.names=TRUE # TRUE면 행 이름을 CSV 파일에 포함하여 저장한다.
)
```

```R
> (x <- read.csv("a.csv")) # a.csv 파일을 읽어온다.
```
읽어들인 파일은 데이터 프레임형식으로 읽어온다.

다음과 같이 헤더가 없는 csv파일도 존재한다.
```R
1,"Mr. Foo",95
2,"Ms. Bar",97
3,"Mr. Baz",92
```
이렇게 헤더가 없는 경우 header=FALSE 를 지정해준다. 다음과 같이 names() 를 통해서 컬럼 이름을 지정해줘야 한다.
```R
> (x <- read.csv("b.csv"), header=FALSE)
> names(x) <- c("id", "name", "score")
> x
```

문자열 데이터의 경우 별 다른 설정이 없으면 factor로 읽어들인다. as.character() 를 사용해서 문자열로 바꿔주거나 파일을 불러올때 부터 stringsAsFactors를 사용해주면 된다.

```R
> x <- read.csv("a.csv", stringsAsFactors=FALSE)
```

### 파일 데이터에 NA값이 있을 경우

파일 데이터에 정상적인 값이 들어있지 않은경우가 있다. 이럴경우 na.strings() 를 사용하면 된다.
na.strings() 는 주어진 문자열을 R이 인식하는 NA로 바꿔준다.
```R
> x <- read.csv("c.csv", na.strings=c("NIL"))
```

데이터를 CSV 파일로 저장하려면 write.csv()를 사용한다. 다음 예에서는 row.names를 FALSE로 지정하여 행 이름은 제외하고 파일에 저장한다.
```R
> write.csv(x, "d.csv", row.names=FALSE)
```

### 객체의 파일 입출력

바이너리 파일로 R 객체를 저장하고 불러들이는 함수에는 save(), load()가 있다.

```R
save: 메모리에 있는 객체를 파일에 저장한다.
save(
  ..., # 저장할 객체의 이름
  list=character(), # 저장할 객체의 이름을 백터로 지정할 경우 ... 대신 사용
  file # 파일명
)

load: 파일로부터 객체를 메모리로 읽어들인다.
load(
  file # 파일명
)
반환 값은 파일에서 읽어들인 객체의 이름들을 저장한 벡터다.
```

## 03 데이터 프레임의 행과 컬럼 합치기

* rbind(...) 지정한 데이터들을 행으로 취급해 합친다.
* cbind(...) 지정한 데이터들을 컬럼으로 취급해 합친다.

예들 들어, c(1,2,3), c(4,5,6) 이라는 두 벡터는 rbind()를 사용해 각 벡터를 한 행으로 하는 행렬로 합칠 수 있다.

```R
> rbind(c(1,2,3), c(4,5,6))
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
```
마찬가지로 데이터 프레임 역시 rbind()를 사용하여 행을 합칠 수 있다. 

## 04 apply 계열 함수

R에는 벡터, 행렬 또는 데이터 프레임에 임의의 함수를 적용한 결과를 얻기 위한 apply 계열 함수가 있다. 이 함수들은 데이터 전체에 함수를 한 번에 적용하는 벡터 연산을 수행하므로 속도가 빠르다.

### apply()

```R
apply: 배열 또는 행렬에 함수 FUN을 MARGIN 방향으로 적용하여 결과를 벡터, 배열 또는 리스트로 반환한다.
apply(
  X, # 배열 또는 행렬
  MARGIN, # 함수를 적용하는 방향. 1은 행 방향, 2는 열 방향
          # c(1, 2)는 행과 열 방향 모두를 의미
  FUN     # 적용할 함수
)

반환 값은 FUN이 길이 1인 벡터들을 반환한 경우 벡터, 1보다 큰 벡터들을 반환한 경우 행렬, 서로 다른 길이의 벡터를 반환한 경우 리스트다.
```

합을 구하는 함수 sum()을 apply()에 적용하는 예에 대해 알아보자. sum()은 인자로 주어진 값들의 합을 구하는 간단한 함수다.

```R
> d <- matrix(1:9, ncol=3)
> d
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
> apply(d, 1, sum)
[1] 12 15 18
> apply(d, 2, sum)
[1] 6 15 24
```

행 또는 열의 합 그리고 평균의 계산은 빈번히 사용되므로 관련 함수들이 미리 정의되어 있다. 

```R
rowSums: 숫자 배열 또는 데이터 프레임에서 행의 합을 구한다.
rowSums(
  x, # 배열 또는 숫자를 저장한 데이터 프레임
  na.rm=FALSE, # NA를 제외할지 여부
)
반환 값은 행 방향에 저장된 값의 합이다.

rowMeans: 숫자 배열 또는 데이터 프레임에서 행의 평균을 구한다.
rowMeans(
  x, # 배열 또는 숫자를 저장한 데이터 프레임
  na.rm=FALSE, # NA를 제외할지 여부
)
반환 값은 행 방향에 저장된 값의 평균이다.
```

colSums(), colMeans() 역시 같은 형식으로 사용하면 된다.

### lapply()

```R
lapply: 벡터, 리스트, 표현식, 데이터 프레임 등에 함수를 적용하고 그 결과를 리스트로 반환한다.
lapply(
  X, # 벡터, 리스트, 표현식 또는 데이터 프레임
  FUN, # 적용할 함수
  ... # 추가 인자. 이 인자들은 FUN에 전달된다.
)
반환 값은 X와 같은 길이의 리스트다.
```
리스트 보다는 벡터 또는 데이터 프레임이 사용하기에 직관적인 면이 있으므로 lapply()의 결과를 벡터 또는 데이터 프레임으로 변환할 필요가 있다.
```R
unlist: 리스트 구조를 벡터로 변환한다.
unlist(
  x, # R 객체. 보통 리스트 또는 벡터
  recursive=FALSE, # x에 포함된 리스트 역시 재귀적으로 변환할지 여부
  use.names=TRUE # 리스트 내 값의 이름을 보존할지 여부
)
반환 값은 벡터다.

do.call: 함수를 리스트로 주어진 인자에 적용하여 결과를 반환한다.
do.call(
  what, # 호출할 함수
  args, # 함수에 전달할 인자의 리스트
)
반환 값은 함수 호출 결과다.
```

lapply()는 인자로 리스트를 받을 수 있다.
```R
> (x <- list(a=1:3, b=4:6))
$a
[1] 1 2 3

$b
[1] 4 5 6
> lapply(x, mean)
$a
[1] 2

$b
[1] 5
```

데이터 프레임에도 곧바로 lapply()를 적용할 수 있다.

### sapply()

sapply() 는 lapply()와 유사하지만 리스트 대신 행렬, 벡터 등의 데이터 타입으로 결과를 반환하는 특징이 있는 함수다.

```R
sapply: 벡터, 리스트, 표현식, 데이터 프레임 등에 함수를 적용하고 그 결과를 벡터 또는 행렬로 반환한다.
sapply(
  X, # 벡터, 리스트, 표현식 또는 데이터 프레임
  FUN, # 적용할 함수
  ..., # 추가 인자. 이 인자들은 FUN에 전달된다.
)
반환 값은 FUN의 결과가 길이 1인 벡터들이면 벡터, 길이가 1보다 큰 벡터들이면 행렬이다.
```

lapply()는 결과를 리스트로 반환하지만, sapply()는 벡터를 반환한다.

```R
> lapply(iris[, 1:4], mean)
$Sepal.Length
[1] 5.843333

$Sepal.Width
[1] 3.057333

$Petal.Length
[1] 3.758

$Petal.Width
[1] 1.199333
> sapply(iris[, 1:4], mean)
Sepal.Length  Sepal.Width  Petal.Length  Petal.Width
    5.843333     3.057333      3.758000     1.199333
> class(sapply(iris[, 1:4], mean))  # "numeric"은 숫자를 저장한 벡터를 의미함
[1] "numeric"
```

sapply()에서 반환한 벡터는 as.data.frame()을 사용해 데이터 프레임으로 변환할 수 있다.

sapply()의 func에 반환값이 1개 값인 경우, sapply()의 결과는 이들 값을 모은 벡터가 된다. 하지만 sapply()에 func의 반환값이 1개 이상인 경우 행렬을 반환한다. 

sapply()는 한 가지 타입만 저장 가능한 벡터나 행렬을 반환 하므로 func의 반환값은 데이터 타입이 섞여있으면 안된다.

### tapply()
tapply()는 그룹별로 함수를 적용하기 위한 apply 계열 함수다.

```r
tapply: 벡터 등에 저장된 데이터를 주어진 기준에 따라 그룹으로 묶은 뒤 각 그룹에 함수를 적용하고 그 결과를 반환한다.
tapply(
  X, # 벡터
  INDEX, # 데이터를 그룹으로 묶을 색인, 팩터를 지정해야 하며 팩터가 아닌 타입이 지정되면 팩터로 형 변환된다.
  func, # 각 그룹마다 적용할 함수
  ..., # 추가 인자. 이 인자들은 func에 전달된다.
)
반환값은 배열이다.
```

다음과 같은 예를 생각해보자. 1부터 10까지의 숫자가 있고 이들이 모두 한 그룹에 속해 있을때, 이 그룹에 속한 데이터의 합을 구하면 55가 될 것이다.
```r
> tapply(1:10, rep(1, 10), sum)
1
55
```

이번에는 1부터 10까지 숫자를 홀수별, 짝수별로 묶어서 합을 구해보자. INDEX에 홀수와 짝수별로 다른 팩터 값이 주어지도록 %% 2 를 사용했다.

```r
> tapply(1:10, 1:10 %% 2 == 1, sum)
FALSE TRUE
30    25
```

### mapply()

```r
mapply: 함수에 리스트 또는 벡터로 주어진 인자를 적용한 결과를 반환한다.
mapply(
  func, # 실행할 함수
  ..., # 적용할 인자
)
... 에 주어진 여러 데이터가 있을 때 func에 이들 데이터 각각의 요소들을 인자로 전달해 실행한 결과를 반환한다.
```

난수를 생성하는 함수를 사용하여 예제를 만들어보자

* 난수 생성 함수

```r
rnorm(n, mean=0, sd=1) 평균이 n, 표준 편차가 sd인 정규 분포를 따르는 난수 n개 발생
runinf(n, min=0, max=1) 최솟값이 min, 최댓값이 max인 균등 분포를 따르는 난수 n개 발생
rpois(n, lambda) 람다 값이 lambda인 포아송 분포를 따르는 난수 n개 발생
rexp(n, rate=1) 람다가 rate인 지수 분포를 따르는 난수 n개 발생
```

다음은 평균 0, 표준 편차 1을 따르는 난수 10개를 발생시킨다.

```r
> rnorm(10, 0, 1)
```

rnorm()을 다음 세 가지 조합에 대해 호출할 필요가 있다고 해보자.

n: 1, 2, 3
mean: 0, 10, 100
sd: 1, 1, 1

이를 수행하기 위해 rnorm()을 세 번 호출해도 되지만 또 다른 방법은 mapply()를 활용하는 것이다.

```r
> mapply(rnorm,
  c(1, 2, 3), # n
  c(0, 10, 100), # mean
  c(1, 1, 1) # sd
)
1
[1] -0.1700506

2
[1] 8.321710 7.564312

3
[1] 99.45474 100.10412 100.80828
```

## 05 데이터를 그룹으로 묶은 후 함수 호출하기

데이터 분석에는 데이터 전체에 대해 함수를 호출하기보다는 그룹별로 나눈 뒤 각 그룹별로 연산하는 일이 흔하다. 이런 목적에 특화된 패키지들이 있는데, doBy는 그중 잘 알려진 패키지다.

doBy 패키지에는 summaryBy(), orderBy(), sampleBy()와 같이 특정 값에 따라 데이터를 처리하는 유용한 함수들이 있다.

* summaryBy() 데이터 프레임을 컬럼 값에 따라 그룹으로 묶은 후 요약 값 계산
* orderBy() 지정된 컬럼 값에 따라 데이터 프레임을 정렬
* sampleBy() 데이터 프레임을 특정 컬럼 값에 따라 그룹으로 묶은 후 각 그룹에서 샘플(sample) 추출

### summaryBy() 
summaryBy() 는 그룹별로 그룹을 특징짓는 통계적 요약 값을 계산하는 함수다. 기본적으로 제공되는 summary() 함수와 비교해보자

```r
summary: 다양한 모델링 함수의 결과에 대한 요약 결과를 반환한다.
summary(
  object # 요약할 객체
)
반환 값은 요약 결과며, 데이터 타입은 object 타입에 따라 다르다

summaryBy: 포뮬러에 따라 데이터를 그룹으로 묶고 요약한 결과를 반환한다.
summaryBy(
  formula, # 요약을 수행할 포뮬러
  data=parent.frame() # 포뮬러를 적용할 데이터
)
반환 값은 데이터 프레임이다.
```

summary() 는 결과값으로 최솟값, 1사분위수, 중앙값, 평균, 3사분위수, 최댓값을 보여준다. 팩터인 species는 각 레벨마다 몇 개의 값이 있는지를 보여준다.

doBy 패키지의 summaryBy()는 원하는 컬럼의 값을 특정 조건에 따라 요약하는 목적으로 사용한다. 예를 들어, Sepal.Length 와 Sepal.Width를 Species에 따라 살펴보려면 다음과 같이 하면 된다.

```R
> summaryBy(Sepal.Width + Sepal.Length ~ Species, iris)
     Species Sepal.Width.mean Sepal.Length.mean
1     setosa            3.428             5.006
2 versicolor            2.770             5.936
3  virginica            2.974             6.588
```

위 코드에서 'Sepal.Length + Sepal.Length ~ Species' 부분은 포뮬러(수식) 이라고 하는데, 처리할 데이터를 일종의 수학 공식처럼 표현하는 방법이다. 이 예에서는 Sepal.Width와 Sepal.Length를 + 로 연결해 이 두가지에 대한 값을 결과에 각 컬럼으로 놓고, 각 행에는 ~ Species를 사용해 Species를 놓았다. 즉, 위 결과는 Sepal.Width와 Sepal.Length를 Species별로 요약한 것이다.

### orderBy()
orderBy()는 데이터 프레임을 정렬하는 목적으로 사용한다. orderBy()역시 base 패키지의 order()함수에 대응한다.

```R
order: 데이터를 정렬하기 위한 순서를 반환한다.
order(
  ..., # 정렬할 데이터
  # na.last는 NA값을 정렬한 결과의 어디에 둘 것인지를 제어한다. 기본값인 na.last=TRUE는
  # NA 값을 정렬한 결과의 마지막에 둔다. na.last=FALSE는 정렬한 값의 처음에 둔다.
  # na.last=NA는 NA 값을 정렬 결과에서 제외한다.
  na.last=TRUE,
  decreasing=FALSE # 내림차순 여부
)
반환 값은 원 데이터에 지정하면 정렬된 결과가 나오도록 하는 색인이다.

orderBy: 포뮬러에 따라 데이터를 정렬한다.
orderBy(
  formula, # 정렬할 기준을 지정한 포뮬러
          # ~의 좌측은 무시하며, ~ 우측에 나열한 이름에 따라 데이터가 정렬된다.
  data, # 정렬할 데이터
)
반환값은 order()와 동일하다.
```

다음 예는 아이리스 데이터를 Sepal.Length에 따라 정렬했을 때 61행이 가장 처음에 해당함을 보여준다

```R
> order(iris$Sepal.Width)
[1] 61 63 69 120 42 ...
...
```
orderBy()는 order()와 유사하지만 정렬할 데이터를 포뮬러로 지정할 수 있다는 점이 편리하다. 다음 예는 모든 데이터를 Sepal.Width로 배열한다. orderBy()에서 ~의 좌측은 무시하므로 적지 않는다.

```R
> orderBy(~ Sepal.Width, iris)
  Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
61          5.0         2.0          3.5         1.0 versicolor
63          6.0         2.2          4.0         1.0 versicolor
69          6.2         2.2          4.5         1.5 versicolor
120         6.0         2.2          5.0         1.5 virginica
42          4.5         2.3          1.3         0.3    setosa
...
```

### sampleBy()
sampleBy()는 데이터를 그룹으로 묶은 후 각 그룹에서 샘플을 추출하는 함수다. 이 함수는 sample()에 대응하므로 함께 알아보도록 하자

```R
sample: 샘플링을 수행한다.
sample(
  x, # 샘플을 뽑을 데이터 벡터. 만약 길이 1인 숫자 n이 지정되면 1:n에서 샘플이 선택된다.
  size, # 샘플의 크기
  replace=FALSE, # 복원 추출 여부
  prob # 데이터가 뽑힐 가중치. 예를 들어, x=c(1,2,3) 에서 2개의 샘플을 뽑되 각 샘플이 뽑힐 확률을 50%, 20%, 30%로 하고자 한다면 size=2, prob=c(5,2,3) 을 지정한다.
)
반환 값은 샘플을 저장한 길이 size인 벡터다

sampleBy: 포뮬러에 따라 데이ㅓ를 그룹으로 묶은 후 샘플을 추출한다.
sampleBy(
  formula, # ~ 우측에 나열한 이름에 따라 데이터가 그룹으로 묶인다.
  frac=0.1, # 추출할 샘플의 비율이며 기본값은 10%
  replace=FALSE, # 복원 추출 여부
  data=parent.frame(), # 데이터를 추출할 데이터 프레임
  systematic=FALSE # 계통 추출을 사용할지 여부
)
반환 값은 데이터 프레임이다.
```

샘플링은 주어진 데이터를 훈련 데이터와 테스트 데이터로 분리하는데 유용하게 사용할 수 있다. 훈련 데이터로부터 모델을 만든 뒤 테스트 데이터에 모델을 적용하면 모델의 정확성을 평가할 수 있다.

평가를 올바르게 하려면 훈련 데이터와 테스트 데이터에 Species값 별로 데이터의 수가 균일한 것이 좋다. 바로 이런 경우에 sampleBy()가 유용하다.

다음은 아이리스 데이터에서 각 Species별로 10%의 데이터를 추출한 예다.

```R
> sampleBy(~ Species, frac=0.1, data=iris)
               Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
setosa.11               5.4         3.7          1.5         0.2     setosa
setosa.29               5.2         3.4          1.4         0.2     setosa
setosa.39               4.4         3.0          1.3         0.2     setosa
setosa.41               5.0         3.5          1.3         0.3     setosa
setosa.46               4.8         3.0          1.4         0.3     setosa
versicolor.55           6.5         2.8          4.6         1.5 versicolor
versicolor.74           6.1         2.8          4.7         1.2 versicolor
versicolor.82           5.5         2.4          3.7         1.0 versicolor
versicolor.90           5.5         2.5          4.0         1.3 versicolor
versicolor.100          5.7         2.8          4.1         1.3 versicolor
virginica.112           6.4         2.7          5.3         1.9  virginica
virginica.122           5.6         2.8          4.9         2.0  virginica
virginica.123           7.7         2.8          6.7         2.0  virginica
virginica.135           6.1         2.6          5.6         1.4  virginica
virginica.140           6.9         3.1          5.4         2.1  virginica
```

## 06 데이터 분리 및 병합

```R
split: 주어진 기준에 따라 데이터를 분리한다.
split(
  x, # 분리할 벡터 또는 데이터 프레임
  f # 분리할 기준을 저장한 팩터
)
반환 값은 분리된 데이터를 저장한 리스트다.

subset: 조건을 만족하는 벡터, 행렬, 데이터 프레임의 일부를 반환한다.
subset(
  x, # 일부를 취할 객체
  subset, # 데이터를 취할 것인지 여부
  select # 데이터 프레임의 경우 선택하고자 하는 컬럼
)
반환 값은 조건을 만족하는 데이터다.

merge(
  x, # 병합할 데이터 프레임
  y, # 병합할 데이터 프레임
)
반환 값은 병합된 결과다.
```

## 07 데이터 정렬
```R
sort: 벡터를 정렬한다.
sort(
  x, # 정렬할 벡터
  decreasing=FALSE, # 내름차순 여부
  na.last=NA
  # na.last는 NA값을 정렬한 결과의 어디에 둘 것인지를 제어한다.
  # na.last=TRUE는 NA값을 정렬한 결과의 마지막에 두고,
  # na.last=FALSE는 정렬한 값의 처음에 둔다
  # 기본값은 na.last=NA는 NA값을 정렬 결과에서 제외한다.
)
반환 값은 정렬된 벡터다
```

sort()는 값을 정렬한 결과를 반환할 뿐, 인자로 받은 벡터 자체를 변경하지 않는다.

### order()
order()는 주어진 인자를 정렬하기 위한 각 요소의 index를 반환한다.

## 08 데이터 프레임 컬럼 접근

### with()

```R
with: 데이터 환경에서 주어진 표현식을 평가한다
with(
  data, # 환경(environment)을 만들 데이터
  expr, # 평가할 표현식
  ... , # 이후 함수들에 전달될 인자
)
반환 값은 expr의 평가값이다.
```

```r
> print(mean(iris$Sepal.Length))
[1] 5.843333
> print(mean(iris$Sepal.Width))
[1] 3.057333

# with 명령을 사용하면 값에 바로 접근할 수 있다.

> with(iris, {
  print(mean(Sepal.Length))
  print(mean(Sepal.Width))
})
[1] 5.843333
[1] 3.057333
```

### within()

```r
within: 데이터 환경에서 주어진 표현식을 평가한다.
within(
  data, # 환경을 만들 데이터
  expr, # 평가할 표현식 expr의 예어는 코드 블록 {...} 을 들 수 있다.
  ... # 이후 함수들에 전달될 인자
)
반환 값은 expr의 평가에 따라 수정된 데이터다
```

다음은 벡터에서 결측치를 중앙값으로 치환하는 예다

```R
> x <- within(x , {
  val <- iselse(is.na(val), median(val, na.rm=TRUE, val))
})
```
위 코드에서 median() 함수 호출 시에 na.rm=TRUE를 지정했다. 이는 NA값이 포함된 채로 median()을 호출하면 결과로 NA가 나오기 때문이다.


### attach(), detach()
attach(), detach()는 함수 호출 후 모든 코드에서 컬럼들을 직접 접근할 수 있게한다.

```r
attach: 데이터를 R 검색 경로에 추가하여 변수명으로 바로 접근할 수 있게 한다.
attach(
  what # 이름으로 곧바로 접근하게 할 데이터 프레임 또는 리스트
)

detach: 데이터를 R 검색 경로에서 제거한다.
detach(
  what # 제거할 객체
)

search: R 객체에 대한 검색 경로를 반환한다.
search()
반환 값은 R 객체를 검색하는 검색 경로다.
```

```r
> Sepal.Width
Error: object 'Sepal.Width' not found
> attach(iris)
> head(Sepal.Width)
[1] 3.5 3.0 3.2 3.1 3.6 3.9
> detach(iris)
> Sepal.Width
Error: object 'Sepal.Width' not found
```

## 09 조건에 맞는 데이터의 색인 찾기

```r
which: 조건이 참인 색인을 반환한다.
which(
  x # 논릿값 벡터 또는 배열
)
반환 값은 논릿값이 참인 색인이다.

which.max: 최댓값의 위치를 반환한다.
which.max(
  x # 숫자 벡터
)
반환 값은 최댓값이 저장된 색인이다.

which.min: 최솟값의 위치를 반환한다.
which.min(
  x # 숫자 벡터
)
반환 값은 최솟값이 저장된 색인이다.
```

예를 들어, iris 데이터에서 Species가 setosa인 행은 subset()으로 다음과 같이 찾을 수 있다.

```r
> subset(iris, Species == "setosa")
   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1           5.1         3.5          1.4         0.2  setosa
2           4.9         3.0          1.4         0.2  setosa
3           4.7         3.2          1.3         0.2  setosa
4           4.6         3.1          1.5         0.2  setosa
5           5.0         3.6          1.4         0.2  setosa
...
```

which()의 경우 조건을 만족하는 행의 index를 반환한다.
```r
> which(iris$Species == "setosa")
 [1] 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
[31] 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50
```
min, max를 사용하면 index중에서 최솟값 최댓값을 찾을 수 있다.

```r
> which.min(iris$Sepal.Length)
[1] 14
> which.max(iris$Sepal.Length)
[1] 132
```

## 10 그룹별 연산
doBy가 데이터를 그룹별로 나눈 후 특정 계산을 적용하기 위한 함수들의 패키지인 반면 aggregate() 는 좀 더 일반적인 그룹별 연산을 위한 함수다. aggregate()를 사용하면 데이터를 그룹으로 묶은 후 임의의 함수를 그룹에 적용할 수 있다.

```r
aggregate: 데이터를 분할하고 각 그룹마다 요약치를 계산한다.
aggregate(
  x, # R 객체
  by, # 그룹으로 묶을 값의 리스트
  func # 그룹별로 요약치 계산에 사용할 함수
)

aggregate(
  formula, # y ~ x 형태로 y는 계산에 사용될 값이며, x는 그룹으로 묶을 때 사용할 기준 값
  data, # formula를 적용할 데이터
  func
)

입력이 데이터 프레임인 경우, 반환 값은 그룹 값과 그룹의 요약치를 저장한 데이터 프레임이다.
```

아이리스 데이터 에서 종별 Sepal.Width의 평균 길이를 구하는 예를 다른 함수와 비교해보자.

```r
> aggregate(Sepal.Width ~ Species, iris, mean)
     Species Sepal.Width
1     setosa       3.428
2 versicolor       2.770
3  virginica       2.974

> tapply(iris$Sepal.Length, iris$Species, mean)
   setosa versicolor virginica
    5.006      5.936     6.588
```

## 11 편리한 처리를 위한 데이터의 재표현

보통 데이터 기록을 다음과 같이 한다.

A | B | C
--|---|--
3 | 5 | 4
2 | 3 | 5
9 | 2 | 7

하지만 데이터를 활용하기 위해선 다음과 같은 형태가 처리하기 쉽다.

Medicine | Value
---------|------
A | 3
A | 2
A | 9
B | 5
B | 3
B | 2
C | 4
C | 5
C | 7

이와 같은 방식으로 정리된 형태의 데이터를 'Tidy Data'라고 부른다. Tidy Data는 조작이 편하고 모델링이 편하며 시각화가 쉬운 장접이 있다. Tidy Data의 정의는 다음과 같다.
* 각 변수는 하나의 컬럼에 해당한다.
* 각 관찰은 한 행에 해당한다.
* 한 관찰 유형은 하나의 테이블을 형성한다.

스프레드시트 형태로 정리된 데이터와 Tidy Data 형태의 데이터 의 변환은 stack(), unstack()으로 수행할 수 있다.
```r
stack: 다수의 벡터를 하나의 벡터로 합치면서 관측값이 온 곳을 팩터로 명시한다.
stack(
  x # 리스트 또는 데이터 프레임
)
반환 값은 데이터 프레임이며, values에는 x가 하나로 합쳐진 값들이 저장된다. ind에는 관측값이 온 곳을 팩터로 명시한다.

unstack: stack()의 역 연산
unstack(
  x,
  form # ~ 왼쪽에는 관측값, 오른쪽에는 관측값이 온 곳을 표현하는 팩터를 명시한다.
)
```
### MySql 및 RMySQL 환경 설정
mysql 설치과정 생략

```r
# mysql을 설치한다.
> install.packages("RMySQL", type="source")

# rmysql을 불러온다.
> library(RMySQL)
```

### RMySQL을 사용한 MySQL 입출력

```R
dbConnect: 데이터베이스에 접속한다.
dbConnect(
  drv, # 데이터베이스 드라이버 
  user, # 사용자 이름
  password, # 비밀번호
  dbname, # 데이터베이스 이름
  host # 호스트
)
반환 값은 데이터 베이스 접속 객체다.

dbListTables: 데이터베이스의 테이블 목록을 얻는다.
dbListTables(
  conn # 데이터베이스 접속
)
반환 값은 데이터베이스의 테이블 목록이다.

dbGetQuery(
  conn, # 데이터베이스 접속
  statement # 수행할 질의
)
반환 값은 질의 결과를 저장한 데이터 프레임이다.
```