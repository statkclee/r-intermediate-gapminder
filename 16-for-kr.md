---
layout: page
title: 재현가능한 과학적 분석을 위한 중급 R 
subtitle: For 루프
minutes: 20
---


> ## 학습 목표 {.objectives}
>
> * `for` 루프를 이해하고 작성한다.
>

### 반복되는 연산

문제를 해결하려 시도하거나, 분석을 실행할 때,
데이터 다른 그룹 집단 혹은 다른 파일 혹은 변수를 변형해서 
동일한 작업을 반복하는 경우가 있다.

R에 관한 훌륭한 점, 일반적으로 프로그래밍의 대단한 점은 **게으름(lazy)**을 피울 수 있게 한다는 점이다.
컴퓨터가 대신 작업을 수행하게 되면, 왜 반복적인 작업을 우리가 직접 실행해야 되나?

예를 들어, 2007년 `gapminder` 데이터셋에서 각 대륙별로 전체 인구를 계산한다고 가정해보자. 몇가지 방식으로 연산작업을 실행할 수 있지만, 가장 기본적인 접근법은 수작업이다:



~~~{.r}
gap[year == 2007 & continent == "Asia", sum(pop)]
~~~



~~~{.output}
[1] 3811953827

~~~



~~~{.r}
gap[year == 2007 & continent == "Africa", sum(pop)]
~~~



~~~{.output}
[1] 929539692

~~~



~~~{.r}
gap[year == 2007 & continent == "Americas", sum(pop)]
~~~



~~~{.output}
[1] 898871184

~~~



~~~{.r}
gap[year == 2007 & continent == "Europe", sum(pop)]
~~~



~~~{.output}
[1] 586098529

~~~



~~~{.r}
gap[year == 2007 & continent == "Oceania", sum(pop)]
~~~



~~~{.output}
[1] 24549947

~~~

타이핑이 지겨울 수 있지만, 무난히 수행해 낼 수 있다. 
하지만, 각 국가별로 어떤 연산을 수행하는 것을 상상해보라!

상기 작업을 수행할 수 있는 더 총명한 방법은
최근에 익힌 `data.table` 기술을 사용하는 것이다:



~~~{.r}
gap[year == 2007, sum(pop), by=continent]
~~~



~~~{.output}
   continent         V1
1:      Asia 3811953827
2:    Europe  586098529
3:    Africa  929539692
4:  Americas  898871184
5:   Oceania   24549947

~~~

하지만, 문제에 대한 해결책이 명확하지 않거나,
익숙히 사용했던 형태에 들어맞지는 않는다.
그래서, 문제해결 도구상자에 언제라도 믿고 쓸 수 있는 도구 다수를 구비하는 것이 도움이 된다. 

대신에 `for` 루프를 가지고, 각 대륙별로 *반복(iterate)*해서, 동일한 명령을 실행하도록 R에게 지시한다:



~~~{.r}
for (cc in gap[,unique(continent)]) {
  popsum <- gap[continent == cc, sum(pop)]
  print(paste(cc, ":", popsum))
}
~~~



~~~{.output}
[1] "Asia : 30507333902"
[1] "Europe : 6181115304"
[1] "Africa : 6187585961"
[1] "Americas : 7351438499"
[1] "Oceania : 212992136"

~~~

상기 코드는 우선 `in` 연산자 우측에 각각을 샅샅이 찾아서 `cc` 변수에 저장한다.
`for` 루프 *몸통부문* 내부, 즉 괄호 내부(`{`와 `}`)에 속한 코드에서는 
`cc` 값을 접근해서 원하는 작업을 수행한다.
그래서 `cc` 변수는 "Asia" 값을 가장 먼저 갖고서 코드를 실행하고 나서, 루프 처음으로 되돌아간다.
다음으로 `cc` 변수는 "Europe" 값을 갖게 되고 동일한 작업을 반복한다. 계속해서...

만약 수년에 걸쳐 각 대륙별로 전체 인구 변화를 살펴보고자 하면 어떨가?
개별 조건 다수를 루프 돌리는데 "중첩"하면 된다:


~~~{.r}
for (cc in gap[,unique(continent)]) {
  for (yy in gap[,unique(year)]) {
    popsum <- gap[year == yy & continent == cc, sum(pop)]
    print(paste(cc, yy, ":", popsum))
  }
}
~~~



~~~{.output}
[1] "Asia 1952 : 1395357351.99999"
[1] "Asia 1957 : 1562780599"
[1] "Asia 1962 : 1696357182"
[1] "Asia 1967 : 1905662900"
[1] "Asia 1972 : 2150972248"
[1] "Asia 1977 : 2384513556"
[1] "Asia 1982 : 2610135582"
[1] "Asia 1987 : 2871220762"
[1] "Asia 1992 : 3133292191"
[1] "Asia 1997 : 3383285500"
[1] "Asia 2002 : 3601802203"
[1] "Asia 2007 : 3811953827"
[1] "Europe 1952 : 418120846"
[1] "Europe 1957 : 437890351"
[1] "Europe 1962 : 460355155"
[1] "Europe 1967 : 481178958"
[1] "Europe 1972 : 500635059"
[1] "Europe 1977 : 517164531"
[1] "Europe 1982 : 531266901"
[1] "Europe 1987 : 543094160"
[1] "Europe 1992 : 558142797"
[1] "Europe 1997 : 568944148"
[1] "Europe 2002 : 578223869"
[1] "Europe 2007 : 586098529"
[1] "Africa 1952 : 237640501"
[1] "Africa 1957 : 264837738"
[1] "Africa 1962 : 296516865"
[1] "Africa 1967 : 335289489"
[1] "Africa 1972 : 379879541"
[1] "Africa 1977 : 433061021"
[1] "Africa 1982 : 499348587"
[1] "Africa 1987 : 574834110"
[1] "Africa 1992 : 659081517"
[1] "Africa 1997 : 743832984"
[1] "Africa 2002 : 833723916"
[1] "Africa 2007 : 929539692"
[1] "Americas 1952 : 345152446"
[1] "Americas 1957 : 386953916"
[1] "Americas 1962 : 433270254"
[1] "Americas 1967 : 480746623"
[1] "Americas 1972 : 529384210"
[1] "Americas 1977 : 578067699"
[1] "Americas 1982 : 630290920"
[1] "Americas 1987 : 682753971"
[1] "Americas 1992 : 739274104"
[1] "Americas 1997 : 796900410"
[1] "Americas 2002 : 849772762"
[1] "Americas 2007 : 898871184"
[1] "Oceania 1952 : 10686006"
[1] "Oceania 1957 : 11941976"
[1] "Oceania 1962 : 13283518"
[1] "Oceania 1967 : 14600414"
[1] "Oceania 1972 : 16106100"
[1] "Oceania 1977 : 17239000"
[1] "Oceania 1982 : 18394850"
[1] "Oceania 1987 : 19574415"
[1] "Oceania 1992 : 20919651"
[1] "Oceania 1997 : 22241430"
[1] "Oceania 2002 : 23454829"
[1] "Oceania 2007 : 24549947"

~~~

#### For 혹은 Apply? 두번째 지옥의 순환.


> 두번째 악순환으로 들어가면, 대식가가 살고 있다.  
> We made our way into the second Circle, here live the gluttons.  
> -- [The R inferno](http://www.burns-stat.com/pages/Tutor/R_inferno.pdf)

R 초보자나 유경험자가 저지르는 가장 커다란 실수 중에 하나는 
루프가 돌때마다 결과 객체(벡터, 리스트, 행렬, 데이터프레임)를 생성해 나가는 것이다. 예를 들어:


~~~{.r}
results <- data.frame(continent=character(), year=numeric(), popsum=numeric())
for (cc in gap[,unique(continent)]) {
  for (yy in gap[,unique(year)]) {
    popsum <- gap[year == yy & continent == cc, sum(pop)]
    this_result <- data.frame(continent=cc, year=yy, popsum=popsum)
    results <- rbind(results, this_result)
  }
}
results
~~~



~~~{.output}
   continent year     popsum
1       Asia 1952 1395357352
2       Asia 1957 1562780599
3       Asia 1962 1696357182
4       Asia 1967 1905662900
5       Asia 1972 2150972248
6       Asia 1977 2384513556
7       Asia 1982 2610135582
8       Asia 1987 2871220762
9       Asia 1992 3133292191
10      Asia 1997 3383285500
11      Asia 2002 3601802203
12      Asia 2007 3811953827
13    Europe 1952  418120846
14    Europe 1957  437890351
15    Europe 1962  460355155
16    Europe 1967  481178958
17    Europe 1972  500635059
18    Europe 1977  517164531
19    Europe 1982  531266901
20    Europe 1987  543094160
21    Europe 1992  558142797
22    Europe 1997  568944148
23    Europe 2002  578223869
24    Europe 2007  586098529
25    Africa 1952  237640501
26    Africa 1957  264837738
27    Africa 1962  296516865
28    Africa 1967  335289489
29    Africa 1972  379879541
30    Africa 1977  433061021
31    Africa 1982  499348587
32    Africa 1987  574834110
33    Africa 1992  659081517
34    Africa 1997  743832984
35    Africa 2002  833723916
36    Africa 2007  929539692
37  Americas 1952  345152446
38  Americas 1957  386953916
39  Americas 1962  433270254
40  Americas 1967  480746623
41  Americas 1972  529384210
42  Americas 1977  578067699
43  Americas 1982  630290920
44  Americas 1987  682753971
45  Americas 1992  739274104
46  Americas 1997  796900410
47  Americas 2002  849772762
48  Americas 2007  898871184
49   Oceania 1952   10686006
50   Oceania 1957   11941976
51   Oceania 1962   13283518
52   Oceania 1967   14600414
53   Oceania 1972   16106100
54   Oceania 1977   17239000
55   Oceania 1982   18394850
56   Oceania 1987   19574415
57   Oceania 1992   20919651
58   Oceania 1997   22241430
59   Oceania 2002   23454829
60   Oceania 2007   24549947

~~~

결과 객체를 "키워나가는" 것은 나쁜 습관이다.
매번 반복을 할 때마다, R이 컴퓨터 운영체제에 신규 결과 객체에 대해서 메모리 할당을 요청해야 된다.
모든 정치협상과 마찬가지로, 시간(적어도 컴퓨터 시간)이 걸린다! 결과적으로, 
더 큰 데이터셋 혹은 더 복잡한 연산 작업을 수행할 때, `for` 루프를 돌리면 시간이 엄청 오래 걸리는 것을 볼 수 있다.

따라서, 결과 객체 크기를 사전에 R에게 정보를 제공하는 것이 훨씬 낫다.
이렇게 함으로써, 적절한 메모리 용량을 한번만 R이 컴퓨터에 요청하면 된다:


~~~{.r}
# First lets calculate the number of rows we need:
nresults <- gap[,length(unique(continent))] * gap[,length(unique(year))] 
results <- data.frame(
  continent=character(length=nresults), 
  year=numeric(length=nresults), 
  popsum=numeric(length=nresults), stringsAsFactors=FALSE
)
# Instead of iterating over values, we need to keep track of indices so we know
# which row to insert or new results into at each iteration. 
# `seq_along` will create a sequence of numbers based on the length of the 
# vector. So instead of c("Asia", "Americas", "Europe", "Africa", "Oceania"),
# ii will store c(1,2,3,4,5)
continents <- gap[,unique(continent)]
years <- gap[,unique(year)]
# We also need to keep track of which row to insert into. We could do fancy 
# math based on our indices, but this is hard to get right and can lead to hard
# to detect errors. Its much easier to just keep track of this manually. 
this_row <- 1
for (ii in seq_along(continents)) {
  for (jj in seq_along(years)) {
    # Now we need to look-up the appopriate values based on our indices
    cc <- continents[ii]
    yy <- years[jj]
    popsum <- gap[year == yy & continent == cc, sum(pop)]
    results[this_row,] <- list(cc, yy, popsum)
    # Increment the row counter
    this_row <- this_row + 1
  }
}
results
~~~



~~~{.output}
   continent year     popsum
1       Asia 1952 1395357352
2       Asia 1957 1562780599
3       Asia 1962 1696357182
4       Asia 1967 1905662900
5       Asia 1972 2150972248
6       Asia 1977 2384513556
7       Asia 1982 2610135582
8       Asia 1987 2871220762
9       Asia 1992 3133292191
10      Asia 1997 3383285500
11      Asia 2002 3601802203
12      Asia 2007 3811953827
13    Europe 1952  418120846
14    Europe 1957  437890351
15    Europe 1962  460355155
16    Europe 1967  481178958
17    Europe 1972  500635059
18    Europe 1977  517164531
19    Europe 1982  531266901
20    Europe 1987  543094160
21    Europe 1992  558142797
22    Europe 1997  568944148
23    Europe 2002  578223869
24    Europe 2007  586098529
25    Africa 1952  237640501
26    Africa 1957  264837738
27    Africa 1962  296516865
28    Africa 1967  335289489
29    Africa 1972  379879541
30    Africa 1977  433061021
31    Africa 1982  499348587
32    Africa 1987  574834110
33    Africa 1992  659081517
34    Africa 1997  743832984
35    Africa 2002  833723916
36    Africa 2007  929539692
37  Americas 1952  345152446
38  Americas 1957  386953916
39  Americas 1962  433270254
40  Americas 1967  480746623
41  Americas 1972  529384210
42  Americas 1977  578067699
43  Americas 1982  630290920
44  Americas 1987  682753971
45  Americas 1992  739274104
46  Americas 1997  796900410
47  Americas 2002  849772762
48  Americas 2007  898871184
49   Oceania 1952   10686006
50   Oceania 1957   11941976
51   Oceania 1962   13283518
52   Oceania 1967   14600414
53   Oceania 1972   16106100
54   Oceania 1977   17239000
55   Oceania 1982   18394850
56   Oceania 1987   19574415
57   Oceania 1992   20919651
58   Oceania 1997   22241430
59   Oceania 2002   23454829
60   Oceania 2007   24549947

~~~

지금까지 살펴봤듯이, 엄청난 작업이 관여되어 있다.
R사용자 대부분은 `for` 루프가 나쁘다고까지 말하고, 대신에 `apply`를 사용해야 된다고 말하기까지 한다!
`apply`는 다음 학습에서 다룰 예정이고, 추후 또다른 방법을 소개할 것이다: `foreach`는 객체 생성을 사용자를 
대신하여 처리한다.

`for` 루프는 각 반복이 마지막 결과에 의존하는 연속 계산을 실행할 때 가장 유용하다. (예를 들어, 랜덤 워크)

> #### Tip: `while` 루프 {.callout}
>
> 종종 특정 조건이 만족될 때까지 연산작업을 반복할 필요가 있다.
> 이런 경우 `while` 루프를 사용한다.
> 
> 
> ~~~{.r}
> while(조건이 참){
>   연산 작업을 수행한다.
> }
> ~~~
>
> 예제로, `while` 루프가 있는데 균등분포(`runif` 함수)에서 0과 1사이 난수를 생성하는데, 
> 0.1보다 작은 값이 나올 때까지 반복한다.
> 
> ~~~ {.r}
> z <- 1
> while(z > 0.1){
>   z <- runif(1)
>   print(z)
> }
> ~~~
> 
> `while` 루프가 항상 적절한 것은 아니다.
> 무한루프에 빠지지 않도록 특히 주의를 기울여야 된다. 왜냐하면, 조건이 절대로 만족되지 않을 수가 있다.
>

> #### 도전과제 1 {.challenge}
>
> `gapminder` 데이터를 대륙별로 샅샅이 돌려 1952년 평균 기대수명을 출력하는 스크립트를 작성한다.
>

> #### 도전과제 2 {.challenge}
>
> 연도 뿐만 아니라 대륙도 샅샅이 돌리도록 스크립트를 변경한다.
>

> #### 도전과제 3 {.challenge}
>
> 100번 난보(랜덤워크, random walk)를 걷는 `for` 루프를 작성하고 결과를 도식화하시오.
>
> **힌트:** `sign(rnorm(1))` 함수를 `for` 루프 몸통부문에 사용해서 
> 매번 반복루프를 돌 때마다 임의로 방향을 선택하게 한다.
>
> **힌트:** 도식화를 위해서 매번 반복을 돈 다음에 결과값을 저장한다. (저장 시작값은 0)
>
> **힌트:** x-축에 0:100 인덱스를 두고, y-축은 저장된 위치정보로 두고서 `plot` 함수로 도식화한다.
> `type` 인자를 `l`로 설정해서 경로를 선분으로 연결한다.
