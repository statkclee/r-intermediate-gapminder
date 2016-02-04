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
  popsum <- gap[year == yy & continent == cc, sum(pop)]
  print(paste(cc, ":", popsum))
}
~~~



~~~{.output}
Error in eval(expr, envir, enclos): 객체 'yy'를 찾을 수 없습니다

~~~

This construct tells R to go through each thing on the right of the `in` 
operator and store it in the variable `cc`. Inside the *body* of the `for` loop,
i.e. any lines of code that fall between the curly braces (`{` and `}`), we can 
then access the value of `cc` to do whatever we like. So first, `cc` will
hold the value "Asia", then it will run the line of code, and return back to the
top of the loop. Next `cc` will hold the value "Europe", and do the same thing,
and so on. 

What if we want to look at the change in total population for each continent
over the years? We can "nest" for loops to iterate through multiple separate
conditions:


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

#### For or Apply? The second circle of hell.

> We made our way into the second Circle, here live the gluttons.
> -- [The R inferno](http://www.burns-stat.com/pages/Tutor/R_inferno.pdf)

One of the biggest things that trips up novices and experienced R users alike, 
is building a results object (vector, list, matrix, data frame) as your for 
loop progresses. For example:


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

"Growing" a results object like this is bad practice. At each iteration, R needs
to talk to the computer's operating system to ask for the right amount of memory
for your new results object. Like all diplomatic negotiations, this can take a 
while (at least in computer time!). As a result, you might find that your for 
loops seem to take forever when you start working with bigger datasets or more
complex calculations.

It's much better to tell R how big your results object will be up front, that 
way R only needs to ask the computer for the right amount of memory once:


~~~{.r}
# First lets calculate the number of rows we need:
nresults <- gap[,length(unique(continent))] * gap[,length(unique(year))] 
results <- data.frame(
  continent=character(length=nresults), 
  year=numeric(length=nresults), 
  popsum=numeric(length=nresults)
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
~~~



~~~{.output}
Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

Warning in `[<-.factor`(`*tmp*`, iseq, value = "Asia"): invalid factor
level, NA generated

~~~



~~~{.r}
results
~~~



~~~{.output}
   continent year     popsum
1       <NA> 1952 1395357352
2       <NA> 1957 1562780599
3       <NA> 1962 1696357182
4       <NA> 1967 1905662900
5       <NA> 1972 2150972248
6       <NA> 1977 2384513556
7       <NA> 1982 2610135582
8       <NA> 1987 2871220762
9       <NA> 1992 3133292191
10      <NA> 1997 3383285500
11      <NA> 2002 3601802203
12      <NA> 2007 3811953827
13      <NA> 1952  418120846
14      <NA> 1957  437890351
15      <NA> 1962  460355155
16      <NA> 1967  481178958
17      <NA> 1972  500635059
18      <NA> 1977  517164531
19      <NA> 1982  531266901
20      <NA> 1987  543094160
21      <NA> 1992  558142797
22      <NA> 1997  568944148
23      <NA> 2002  578223869
24      <NA> 2007  586098529
25      <NA> 1952  237640501
26      <NA> 1957  264837738
27      <NA> 1962  296516865
28      <NA> 1967  335289489
29      <NA> 1972  379879541
30      <NA> 1977  433061021
31      <NA> 1982  499348587
32      <NA> 1987  574834110
33      <NA> 1992  659081517
34      <NA> 1997  743832984
35      <NA> 2002  833723916
36      <NA> 2007  929539692
37      <NA> 1952  345152446
38      <NA> 1957  386953916
39      <NA> 1962  433270254
40      <NA> 1967  480746623
41      <NA> 1972  529384210
42      <NA> 1977  578067699
43      <NA> 1982  630290920
44      <NA> 1987  682753971
45      <NA> 1992  739274104
46      <NA> 1997  796900410
47      <NA> 2002  849772762
48      <NA> 2007  898871184
49      <NA> 1952   10686006
50      <NA> 1957   11941976
51      <NA> 1962   13283518
52      <NA> 1967   14600414
53      <NA> 1972   16106100
54      <NA> 1977   17239000
55      <NA> 1982   18394850
56      <NA> 1987   19574415
57      <NA> 1992   20919651
58      <NA> 1997   22241430
59      <NA> 2002   23454829
60      <NA> 2007   24549947

~~~

As you can see, this involves a lot more work. Most R users will even go so far
to tell you that for loops are bad, and that you should use something called
`apply` instead! We'll cover this in the next lesson, and later we'll show you
another method, `foreach` which also handles object creation for you.

For loops are most useful when you're performing a series of calculations where
each iteration depends on the results of the last (for example a random walk).


> #### Tip: While loops {.callout}
>
>
> Sometimes you will find yourself needing to repeat an operation until a certain
> condition is met. You can do this with a `while` loop.
> 
> 
> ~~~{.r}
> while(this condition is true){
>   do a thing
> }
> ~~~
> 
> As an example, here's a while loop 
> that generates random numbers from a uniform distribution (the `runif` function)
> between 0 and 1 until it gets one that's less than 0.1.
> 
> ~~~ {.r}
> z <- 1
> while(z > 0.1){
>   z <- runif(1)
>   print(z)
> }
> ~~~
> 
> `while` loops will not always be appropriate. You have to be particularly careful
> that you don't end up in an infinite loop because your condition is never met.
>

> #### Challenge 1 {.challenge}
>
> Write a script that loops through the `gapminder` data by continent and prints 
> out the mean life expectancy in 1952.
>

> #### Challenge 3 {.challenge}
>
> Modify the script so that it loops through the years as well as the continents.
>

> #### Challenge 4 {.challenge}
>
> Write a for loop that performs a random walk for 100 steps, then plot the 
> result.
>
> Hint: You can use `sign(rnorm(1))` in the body of the loop to randomly choose 
> a direction (forward or backward) at each iteration. 
>
> Hint: You will want to store the resulting position (starting at 0) after each
> iteration for plotting purposes.
>
> Hint: give the `plot` function the indices 0:100 as the x axis, and the 
> stored positions as the y axis. specify the 'type' argument as "l" to draw
> a the path.
>

