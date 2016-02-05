---
layout: page
title: 재현가능한 과학적 분석을 위한 중급 R 
subtitle: Apply 함수
minutes: 20
---


> ## 학습 목표 {.objectives}
>
> * *apply* 함수를 사용해서 작업을 효율적으로 자동화하는 방법을 학습한다.
> * `apply`, `lapply`, `sapply`, `tapply`, 
>   `mapply` 함수간 차이를 이해한다.
>

### 벡터화 작업 자동화

앞서 `for` 루프를 소개했다: 많은 프로그래밍 언어에서 공통된 기본 프로그래밍 구성체(construct).
R에는 작업을 자동화하는 더 최적화된 방식이 있는데 `for` 루프보다 속도가 더 빠를 뿐만 아니라, 결과 객체를 사전에 정의해야되는 고통도 함께 가져가 버렸다.

접하게 되는 가장 흔한 함수가 `lapply`가 되는데, 이 함수는 `sapply`와 매우 밀접하게 연관되어 있다.

다음 `for` 루프를 살펴보자:


~~~{.r}
for (cc in gap[,unique(continent)]) {
  popsum <- gap[year == 2007 & continent == cc, sum(pop)]
  print(paste(cc, ":", popsum))
}
~~~

상기 함수는 각 대륙별로 전체 인구를 계산하고 나서 결과를 출력한다. 바로 출력하는 대신에 결과를 저장하려면, 사전에 벡터를 미리 생성하고 나서 결과를 저장하거나, `apply` 계열 함수 하나를 선택하면 세부적인 사항은 알아서 자동 수행한다:


~~~{.r}
results <- lapply(gap[,unique(continent)], function(cc) {
  popsum <- gap[year == 2007 & continent == cc, sum(pop)]
  popsum
})
names(results) <- gap[,unique(continent)]
results
~~~



~~~{.output}
$Asia
[1] 3811953827

$Europe
[1] 586098529

$Africa
[1] 929539692

$Americas
[1] 898871184

$Oceania
[1] 24549947

~~~

`lapply` 함수는 벡터(혹은 리스트)를 첫번째 인자(이번 경우에는 대륙명 벡터)로 받고, 두번째 인자로 함수를 받는다.
`lapply` 함수는 첫번째 인자에 들어있는 모든 요소에 대해 연산작업을 수행한다. 이런 점은 `for` 루프와 매우 유사하다: 먼저 `cc` 변수에 첫번째 대륙명 "Asia"를 저장하고 나서, 함수 몸통에 있는 코드를 실행한다. 그리고 나서 `cc` 변수에 두번째 대륙명을 저장하고 함수몸통을 실행한다. 이를 모든 요소에 반복한다. 함수 몸통에 담긴 코드는 정확하게 `for` 루프와 동일하게 간주해도 좋다. 마지막 행 결과가 `lapply` 함수에 반환되는데, 이때 반환되는 결과는 리스트로 결합된다.

`sapply` 함수는 `lapply` 함수와 동일한데, 차이점은 결과객체를 단순화함에 있다. `lapply` 함수 대신에 `sapply` 함수로 동일한 코드를 실행하면, 벡터로 결과가 반환된다:


~~~{.r}
results <- sapply(gap[,unique(continent)], function(cc) {
  popsum <- gap[year == 2007 & continent == cc, sum(pop)]
  popsum
})
names(results) <- gap[,unique(continent)]
results
~~~



~~~{.output}
      Asia     Europe     Africa   Americas    Oceania 
3811953827  586098529  929539692  898871184   24549947 

~~~

### apply

`apply` 함수는 행렬 데이터에 유용하다:
행렬 행 혹은 열 방향으로 루프를 돌릴 수 있게 해 준다.



~~~{.r}
# create some dummy data
r <- matrix(rnorm(10*4), nrow=10)
colnames(r) <- letters[1:4]
rownames(r) <- LETTERS[1:10]
~~~


~~~{.r}
# 각 행에 대한 최대값을 구한다:
apply(r, 1, max)
~~~



~~~{.output}
         A          B          C          D          E          F 
 1.7186295  1.5747280  0.9143232  0.8146741  2.0507913  0.4780862 
         G          H          I          J 
-0.1877886  0.2429086  0.2726646  0.5065971 

~~~



~~~{.r}
# 각 칼럼에 대한 최대값을 구한다:
apply(r, 2, max)
~~~



~~~{.output}
       a        b        c        d 
2.050791 1.624339 1.718630 1.894572 

~~~

> ### 평균과 합계 {.callout}
>
> R에는 행과 열에 대한 평균 혹은 합계를 계산하는 내장 함수가 들어있다: 
> columns: `colSums`, `colMeans`, `rowSums`, `rowMeans`. 
> 자체 `apply` 함수를 작성하는 것보다 이들이 더 빠르다!
>

> ### `apply` 함수 출력 {.callout}
>
>  `apply`에 들어가는 함수가 단일값이 아닌 벡터를 출력할 때,
> 행에 걸쳐 함수를 실행할 때 조차도 결과는 항상 칼럼으로 결합된다!
>

### mapply

`mapply` 함수를 사용해서 다른 인수 조합을 갖는 함수를 실행할 수 있다. 예제를 살펴보자:


~~~{.r}
a <- 1:4
b <- 4:1
mapply(rep, a, b)
~~~



~~~{.output}
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4

~~~

상기 코드를 실행하면 다음과 결과가 같다:


~~~{.r}
rep(a[1], b[1])
~~~



~~~{.output}
[1] 1 1 1 1

~~~



~~~{.r}
rep(a[2], b[2])
~~~



~~~{.output}
[1] 2 2 2

~~~



~~~{.r}
rep(a[3], b[3])
~~~



~~~{.output}
[1] 3 3

~~~



~~~{.r}
rep(a[4], b[4])
~~~



~~~{.output}
[1] 4

~~~

혹은,  다음 `lapply` 문장과 동일하다:


~~~{.r}
lapply(1:4, function(ii) {
  rep(a[ii], b[ii])
})
~~~



~~~{.output}
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4

~~~

### tapply

`tapply` 함수는 벡터 내부 서로 다른 그룹집단에 함수를 실행할 수 있게 해준다. 이번 학습 첫번째 예제로 되돌아 가서, `tapply` 함수를 사용해서 2007년 각 대륙별로 총인구를 계산할 수 있다:



~~~{.r}
gap2007 <- gap[year == 2007] # first filter by the year
tapply(
  gap2007[,pop], # The column of population counts from the data frame
  gap2007[,continent], # The column of continent labels for each entry
  sum # The function to apply to the population vector within each continent
)
~~~



~~~{.output}
    Africa   Americas       Asia     Europe    Oceania 
 929539692  898871184 3811953827  586098529   24549947 

~~~






