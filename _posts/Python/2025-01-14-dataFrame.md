---
layout: post
title: 데이터 분석
subtitle: 데이터 프레임이라는 머시기..
author: Bonazoey
categories: Python
banner:
  image: "./assets/images/coding-robot.gif"
  video: "./assets/images/coding-robot.gif"
  loop: true
  volume: 0
  start_at: 0
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Python, 파이썬]
top: 1
published: true
---

파이썬 툴은 Anaconda를 설치하였고, Jupyter lab이용한다.

Jupyter lab을 사용하는 이유는 기존에 많이 사용하는 IDE (VScode, Pycharm, 등등)은 전체 코드를 실행해야하는 반면

Jypyter lab은 코드 한 줄씩 실행이 가능하여 코드 테스트가 쉽고, 데이터를 다루기에 적합하기 때문이다.

---

## 라이브러리

~~~python
import pandas as pd # 데이터 프레임?
import numpy as np # 연산?
imper as sns # 그래프?
~~~

## 이전 파트

.head()
.tail()
.mean()
.sum()
.agg()
.read_csv()
where
sns.boxplot
\ # 메서드 체이닝

## 데이터 결합 파트 3 챕터 5

### 열 추가

데이터를 가로로 추가하는 것(열 추가)은 'merge' 함수를 사용한다.

~~~python
test1 = pd.DataFrame({'id' : [1, 2, 3, 4, 5], 'midterm' : [60, 80, 70, 90, 85]})
test2 = pd.DataFrame({'id' : [1, 2, 3, 4, 5], 'midterm' : [70, 83, 65, 95, 80]})

total = pd.merge(test1, test2, how = 'left', on = 'id')

total
~~~

how 파라미터에 결합하는 방향을 적는다 'left, right 등' 첫 변수 데이터 테이블인 test1의 왼쪽 방향으로 병합이 되며 'on' 파라미터로 넘긴 컬럼명을 기준으로 합쳐진다.

### 행 추가

데이터를 세로로 추가하는 것(행 추가)은 'concat' 함수를 사용한다.

~~~python
group_a = pd.DataFrame({'id' : [1, 2, 3, 4, 5], 'test' : [60, 80, 70, 90, 85]})
group_b = pd.DataFrame({'id' : [6, 7, 8, 9, 10], 'test' : [70, 83, 65, 95, 80]})

group_all = pd.concat([group_a, group_b])

group_all
~~~

들어간 변수 순서에 따라 차례로 아래에 붙는다.

각 데이터프레임의 인덱스를 무시하고 새로 인덱스가 정의되길 원한다면 'ignore_index' 파라미터 값을 True로 넘기면 된다.

사실 인덱스는 데이터 분석 시 사용하지 않으므로 굳이 재정의할 필요는 없다.

~~~python
group_all = pd.concat([group_a, group_b], ignore_index = True)
~~~

## 데이터 정제

### 결측치 : 누락 값

결측치가 있다면 제거하거나 채워넣는 방법이 있다.

1.1. 결측치 제거

~~~python
import numpy as np

np.nan # 결측치

df = pd.DataFrame({'sex' L ['M', 'F', np.nan, 'M', 'F'], 'score' : [5, 4, 3, 4, np.nan]})

df

pd.isna(df) # 결측치 확인 : .isna() 함수를 사용하면 결측치인 경우 'True', 아닌 경우 'False'로 결과가 나온다.
pd.isna(df).sum() # 결측치 빈도 확인 : .sum() 함수를 사용하면 어떤 변수에 몇 개의 결측치가 있는지 나타난다.
~~~

결측치가 있는 행을 제거하려면 '.dropna()' 함수를 사용하면 된다.

~~~python
df_nomiss = df.dropna(subset = ['score', 'sex']) # 해당 변수에서 결측치 제거
df_nomiss2 = df.dropna() # 모든 행에 대해 결측치 제거
~~~

'subset' 파라미터에 결측치를 제거하고 싶은 변수를 넣어주면 해당 열에 결측치가 존재하면 그 행을 제거한다.

모든 변수에 대해 결측치를 제거하게 되면 분석에 사용할 행까지 제거할 가능성이 높으므로 보통 사용하지 않는다.

날려도 크게 상관 없다면 (데이터가 많음) 사용하기도 한다.

pandas에서 결측치를 제거하지 않고 연산을 진행하면 자동으로 결측치를 제거하고 연산한다. (mean, su, agg, ...) 

1.2. 결측치 대체

결측치가 많아 데이터 손실이 많은 경우 사용한다

대표값 (평균, 최빈값) 등으로 대체 하거나 다른 변수들에 따른 추정값으로 대체하는 법이 있다.

~~~python
exam = pd.read_csv('exam.csv') # 데이터 불러오기
exam.loc[[2, 7, 14], ['math']] = np.nan # 첫 매개변수는 선택할 인덱스, 두 번째는 해당 열이 된다. 그곳에 결측치 할당.
exam

exam['math'] = exam['math].fillna(exam['math'].mean()) # 결측치를 평균값으로 대체하기
~~~

'.fillna()' 함수를 이용하여 해당 변수의 결측치에 일괄적으로 값을 할당할 수 있다.

### 이상치

이상치는 정상 범주에서 벗어난 값이다.

|종류|예|해결방법|
|---|---|---|
|존재할 수 없는 값|성별 변수에 3|결측처리|
|극단적인 값|몸무게 변수에 200|정상범위 기준 정해서 결측 처리|

'.value_counts()' 함수를 이용하여 변수에 존재하는 값 별로 카운트 할 수 있으며 이상치 확인이 쉬워진다.

추가로 '.sort_index()' 함수로 인덱스 별로 정렬을하면 보기가 더 편하다.

#### 존재할 수 없는 값

~~~python
outlier['score'] = np.where(outlier['score'] > 5, np.nan, outlier['score'])
~~~

위 코드는 'outlier' 데이터 테이블의 'score' 점수가 5 이상인 것이 있다면 NaN으로 처리하고 아니라면 원래 값 그대로 가져게 한다.

~~~python
outlier.dropna(subset = ['sex', 'score']) \ # 결측치 제거
  .groupby('sex') \ # sex 별 분리
  .agg(mean-score = ('score', 'mean')) # score 평균 구하기
~~~

그 후 위처럼 결측치를 제거하고 성별 그룹을 묶어서 평균을 구한다.

#### 극단적인 값

판단 기준은 논리적 판단 (예 : 성인 몸무게는 40~150kg 사이), 통계적 판단(boxplot에서 극단치인 값)이 있다.

극단치를 구하는 법은 상자그림에서 1사분위수와 3사분위수를 이용한 iqr로 판단한다.

~~~python
pct25 = mpg['hwy'].quantile(.25) # 1사분위수
pct75 = mpg['hwy'].quantile(.75) # 4사분위수
iqr = pct75 - pct 25 # IQR

pct25 - 1.5 * iqr # 기준값 하한
pct75 + 1.5 * iqr # 기준값 상한
~~~

해당 하한과 상한을 벗어난 값은 극단치가 된다.

~~~python
# 하한 4.5, 상한 40.5
mpg['hwy'] = np.where((mpg['hwy'] < 4.5 | (mpg['hwy'] > 40.5), np.nan, mpg['hwy']) # 극단치 결측처리

mpg['hwy'].isna().sum() # 결측치 빈도확인
~~~

위와 같이 상한, 하한을 이용하여 결측치를 제거한 뒤 '2.1. 존재할 수 없는 값'과 마찬가지로 분석하면 된다.

## ChatGPT 4.0

### Advanced Data Analysis

ChatGPT 4.0 부터 파일 업로드, 다운로드 기능이 있기 때문에 데이터 파일을 업로드 하고 필요한 분석을 요청하면 실행된 코드와 결과물을 보여준다.

즉, 위에 말한 거 몰라도 일반인들도 다 할 수 있음.

하지만 할루시네이션[^hal]은 항상 조심해야한다. 

분석 목표만 요청한다면 할루시네이션 위험이 높아진다. 따라서 단계별로 작업을 하고 오류를 점검하는 게 좋다. 또 프롬프트에 오류 점검해달라고 추가 요청을 한다.

이점 유의해서 AI 사용하도록

### Browsing

ChatGPT 4.0 부터 최신 정보를 검색하여 답변을 줄 수 있다.


## Communities

아래는 데이터 분석 네트워킹 커뮤니티이다.

[데이터 분석 커뮤니티](https://fb.com/groups/datacommunity)

[지피터스](https://gpters.org)

---
[^hal]: 데이터에 오류 가능성을 염두해두지 않거나, 없는 데이터를 그럴듯하게 생성해서 분석
