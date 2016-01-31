---
layout: page
title: 재현가능한 과학적 분석을 위한 중급 R 
---

R과 이미 친숙하지만, 프로그래밍 개념을 갖추지 못한 과학연구원을 위한 중급 학습과정이다.

본 학습과정의 목적은 이미 R을 경험한 과학연구원에게 코드를 좀더 효율적이고, 모듈방으로, 재사용가능하게 
작성할 뿐만 아니라, 효율적인 자료분석을 위한 팩키지를 작성하는 방법을 전달함에 있다.

다양한 제3자 팩키지가 워크샵을 통해 사용된다. 여기에서 소개되는 팩키지가 필히 가장 최고는 아니고, 포괄적인 것도 아니다. 하지만, 유용하다고 판단되는 팩키지로, 주로 사용편의성에 초점을 맞춰 선택되었다.

본 워크샵에서 통계학은 다루지는 않는다.

> ## Prerequisites {.prereq}
>
> Be familar with RStudio, project creation, and variables, and 
> the basic data structures and types in R.
>

## 선행 이수 학습

다음 학습주제는 [R 학습](https://statkclee.github.io/r-novice-gapminder/index-kr.html),
[초급 R(영문)](http://swcarpentry.github.io/r-novice-gapminder)을 통해 R 초급 지식을 전달했고,
본 워크샵에 필요한 사전 지식이 된다:

|   한국어(Korean)      |    영어(English)            |
|--------------------------------|-----------------------------------|
|1. [R과 RStudio 소개][01]           | 1. [Introduction to R and RStudio] [01-en] |
|2. [RStudio로 프로젝트 관리][02]      | 2. [Project management with RStudio] [02-en] |
|3. [도움말 찾기][03]                 | 3. [Seeking help] [03-en] |
|4. [자료형과 자료구조][04]             | 4. [Data structures] [04-en] |
|5. [자료구조: 데이터프레임][05]         | 5. [Data frames and reading in data] [05-en] |
|6. [데이터 부분집합 만들기][06]         | 6. [Subsetting data] [06-en] |
|7. [벡터화][09]                     | 7. [Vectorisation] [06-en] |
|8. [데이터 파일에 저장하기][11]         | 8. [Writing data] [11-en] |


## 학습 주제

### 1부 수업 (~ 4 시간)

|   한국어(Korean)      |    영어(English)            |
|--------------------------------|-----------------------------------|
| 1.  [Data.table](14-data-table-kr.html) | 1.  [Data.table](14-data-table.html) |
| 2.  [데이터 재형성](15-reshape2-kr.html)    | 2.  [Reshaping data](15-reshape2.html) |
| 3.  [For 루프](16-for-kr.html)            | 3.  [For loops](16-for.html) |
| 4.  [Apply 함수](17-apply-kr.html)        | 4.  [Apply functions](17-apply.html) |
| 5.  [제어 흐름][10]                     | 5.  [Control flow][10-en] |

### 2부 수업 (~ 3 시간)

|   한국어(Korean)      |    영어(English)            |
|--------------------------------|-----------------------------------|
| 6.  [R 마크다운](18-rmd-kr.html)        | 6.  [R markdown](18-rmd.html)             |
| 7.  [plyr][12]                       | 7.  [plyr][12-en]                         |
| 8.  [병렬 for 루프](19-foreach-kr.html) | 8.  [Parallel for loops](19-foreach.html) |
| 9.  [함수][07]                         | 9.  [Functions][07-en]                   |
| 10. [요약정리][15]                      | 10. [Wrapping up][15-en]                  |
                                      

## 선택 추가 학습 

|   한국어(Korean)      |    영어(English)            |
|--------------------------------|-----------------------------------|
| 8.  [ggplot2][08] | 8.  [ggplot2][08-en] |

## 추가 학습교재       

*   [참고문헌](reference.html)
*   [강사 안내서](instructors.html)

[01]: http://statkclee.github.io/r-novice-gapminder/01-rstudio-intro-kr.html
[02]: http://statkclee.github.io/r-novice-gapminder/02-project-intro-kr.html
[03]: http://statkclee.github.io/r-novice-gapminder/03-seeking-help-kr.html
[04]: http://statkclee.github.io/r-novice-gapminder/04-data-structures-part1-kr.html
[05]: http://statkclee.github.io/r-novice-gapminder/05-data-structures-part2-kr.html
[06]: http://statkclee.github.io/r-novice-gapminder/06-data-subsetting-kr.html
[07]: http://statkclee.github.io/r-novice-gapminder/07-functions-kr.html
[08]: http://statkclee.github.io/r-novice-gapminder/08-plot-ggplot2-kr.html
[09]: http://statkclee.github.io/r-novice-gapminder/09-vectorisation-kr.html
[10]: http://statkclee.github.io/r-novice-gapminder/10-control-flow-kr.html
[11]: http://statkclee.github.io/r-novice-gapminder/11-writing-data-kr.html
[12]: http://statkclee.github.io/r-novice-gapminder/12-plyr-kr.html
[13]: http://statkclee.github.io/r-novice-gapminder/13-dplyr-kr.html
[14]: http://statkclee.github.io/r-novice-gapminder/14-tidyr-kr.html
[15]: http://statkclee.github.io/r-novice-gapminder/15-wrap-up-kr.html

[01-en]: http://swcarpentry.github.io/r-novice-gapminder/01-rstudio-intro.html
[02-en]: http://swcarpentry.github.io/r-novice-gapminder/02-project-intro.html
[03-en]: http://swcarpentry.github.io/r-novice-gapminder/03-seeking-help.html
[04-en]: http://swcarpentry.github.io/r-novice-gapminder/04-data-structures-part1.html
[05-en]: http://swcarpentry.github.io/r-novice-gapminder/05-data-structures-part2.html
[06-en]: http://swcarpentry.github.io/r-novice-gapminder/06-data-subsetting.html
[07-en]: http://swcarpentry.github.io/r-novice-gapminder/07-functions.html
[08-en]: http://swcarpentry.github.io/r-novice-gapminder/08-plot-ggplot2.html
[09-en]: http://swcarpentry.github.io/r-novice-gapminder/09-vectorisation.html
[10-en]: http://swcarpentry.github.io/r-novice-gapminder/10-control-flow.html
[11-en]: http://swcarpentry.github.io/r-novice-gapminder/11-writing-data.html
[12-en]: http://swcarpentry.github.io/r-novice-gapminder/12-plyr.html
[13-en]: http://swcarpentry.github.io/r-novice-gapminder/13-dplyr.html
[14-en]: http://swcarpentry.github.io/r-novice-gapminder/14-tidyr.html
[15-en]: http://swcarpentry.github.io/r-novice-gapminder/15-wrap-up.html