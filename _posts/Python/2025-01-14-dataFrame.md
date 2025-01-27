---
layout: post
title: "[Data Analysis] 데이터 프레임"
subtitle: "주피터라는 머시기.. 와 함께"
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

## 1. 패키지

해당 포스트의 사용 패키지

~~~python
import pandas as pd # 데이터 핸들링 패키지
import numpy as np # 요약통계량 연산
import seaborn as sns # 그래프
~~~

### 1.1. 설치

아나콘다 프롬프트에 `conda install pandas` 라고 입력하면 해당 패키지가 설치됨

## 2. 함수

### 2.1. 내장 함수

앞에 패키지 명을 쓰지 않고 바로 사용할 수 있다.

`info()` : 해당 데이터의 정보를 볼 수 있다. (데이터 타입 등)

`sum()` : 합

`mean()` : 평균

.

.

.

`agg(1 = (2, 3))` : 요약통계량 구하기, 2 변수의 3 연산 값을 1 변수에 대입

`head()` : 상위 데이터 n개 추출, 파라미터 없을 시 5개 추출

`tail()` : 하위 데이터 n개 추출, 파라미터 없을 시 5개 추출

### 2.2. 패키지 함수

패키지명 뒤에 함수명이 온다.

해당 패키지에 무슨 함수가 있는지 모르면 검색해보자..

`pd.DataFrame()` : 데이터 프레임화 하기

`pd.read_csv()` : csv 파일 불러오기

`?.to_csv()` : csv로 저장

`pd.read_excel()` : excel 파일 불러오기

`np.where(1, 2, 3)` : 1 조건에 해당할 경우 2, 아닐 경우 3

`sns.boxplot()` : 박스 플롯으로 나타내기

`sns.countplot(data = 1, x = 2, hue = 3)` : 카운트 플롯으로 나타내는데 ..

.

.

### 2.3. 기타

`\` : 메서드 체이닝, 가독성을 위해 줄 바꿈할 때 사용한다.

## 3. 데이터 추출

### 3.1. 조건 추출

query 함수는 조건에 해당하는 데이터를 추출할 수 있다.

~~~python
exam.query('nclass == 1') # nclass 변수가 1인 데이터만 추출
exam.query('math > 50') # math가 50보다 큰 데이터만 추출
exam.query('nclass in [1, 3, 5]') # nclass 변수가 1, 3, 5인 데이터만 추출
.
.
~~~

### 3.2. 변수 추출

원하는 변수 추출은 '[]'를 사용한다

~~~python
exam['math'] # 시리즈 형태로 추출
exam[['math']] # 데이터 프레임 형태로 추출
~~~

### 3.3. 변수 제외

해당 변수 제외하고 추출하고 싶다면 `drop()` 함수를 사용한다.

~~~python
exam.drop(columns = ['math', 'english']) # math와 english 변수를 제외 하고 추출
~~~

### 3.4. 혼합 추출

변수 추출 시 조건을 주고싶으면 그냥 같이 쓰면 됨

~~~python
exam.query('nclass == 2')['math'] # nclass가 2인 데이터의 math 변수만 추출
~~~

## 4. 데이터 정렬

~~~python
exam.sort_values('math') # math 변수 오름차순으로 정렬
exam.sort_values('math', ascending = False) # math 변수 내림차순으로 정렬

# 여러 변수에 대해 정렬도 똑같다.
exam.sort_values(['nclass', 'math'], ascending = [True, False]) # nclass 변수에 대해 오름차순, math 변수에 대해 내림차순 정렬
~~~

## 5. 파생 변수

데이터 프레임에 새로운 변수를 추가하려면 assign을 사용하면 된다.

~~~python
# 변수 math, english, exam의 합을 total이라는 변수에 담아 exam 데이터 프레임에 추가
exam.assign(total = exam['math'] + exam['english'] + exam['science']) 
~~~

함수 중첩 사용도 가능하다

~~~python
# numpy 패키지 임포트
import numpy as np
# numpy의 where에 해당하는 값을 test 변수에 넣고 exam 데이터 프레임에 추가함
exam.assign(test = np.where(exam['science'] >= 60, 'pass', 'fail'))
~~~

람다도 이용 가능함!

~~~python
# 둘이 같음
exam.assign(total = exam['math'] + exam['english'] + exam['science'])
exam.assign(total = lambda x: x['math'] + x['english'] + x['science'])
~~~

아래는 람다 이용해야만 함..

~~~python
exam.assign(total = exam['math'] + exam['english'] + exam['science'],
            mean = exam['total'] / 3)
# keyError : 'total' -> 아직 total이라는 변수가 할당되지 않아 찾을 수 없어 에러가 뜸

exam.assign(total = exam['math'] + exam['english'] + exam['science'],
            mean = lambda x: x['total'] / 3)
# 위와 같이 람다를 이용하면 assign에 의해 total이 추가된 변수인 것을 인식하고 데이터 편집이 가능

exam.assign(total = lambda x: x['math'] + x['english'] + x['science'],
            mean = lambda x: x['total'] / 3)
# 결국은 이렇게 사용함
~~~

## 6. 데이터 요약

`agg` 함수를 이용해 요약통계량을 구한다

~~~python
exam.agg(mean_math = ('math', 'mean')) # math 평균 산출
~~~

### 6.1. 집단별 요약

`groupby()` 함수를 사용해 집단별로 구할 수 있다.

~~~python
exam.groupby('nclass') \ # nclass 별로 분리
    .agg(mean_math = ('math', 'mean')) # math 평균 산출
~~~

한 번에 구할 수도 있다.

~~~python
exam.groupby('nclass') \ # nclass 별로 분리
    .agg(mean_math = ('math', 'mean'), # math 평균 산출
        sum_math = ('math', 'sum'), # math 합 산출
        median_math = ('math', 'median'), # math 중앙값 산출
        n = ('nclass', 'count')) # 빈도 (학생 수)
~~~

요약통계량으로 구한 변수를 출력하면 데이터 프레임으로 나타나지 않고 인덱스의 형태로 나타나게 된다. groupby 함수에 as_index 파라미터를 False로 지정해주면 인덱스가 아닌 데이터 프레임으로 나타낼 수 있다.

~~~python
exam.groupby('nclass', as_index = False) \
    .agg(mean_math = ('math', 'mean'))
~~~

#### 6.1.1. 하위 집단별로 나누기

groupby 시 하위 집단으로 또 나눌 수 있음

~~~python
mpg.groupby(['manufacturer', 'drv']) \ # manufacturer로 그룹화 후 그 안에서 drv로 하위 그룹화
    .agg(mean_cty = ('cty', 'mean'))
~~~

### 6.2. 다른 방법

`agg()` 함수를 사용하지 않고 요약통계량을 구할 수 있다.

~~~python
mpg.groupby('drv') \
    .agg(n = ('drv', 'count'))

mpg['drv'].value_counts()

# 위와 아래 출력 값은 똑같이 나오지만 `query` 함수를 사용할 때 문제가 될 수 있다.
# `query` 함수는 데이터 프레임에만 사용할 수 있는데 아래의 값의 경우 데이터 프레임이 아닌 시리즈 형태로 나오기 때문이다.

mpg['drv'].value_counts() \
          .to_frame('n') \
          .query('n > 100')

# 뭐 이런식으로 `to_frame()`을 사용하여 데이터 프레임화 한 후 사용할 수 있긴하다.
~~~

### 6.3. 자주 사용하는 요약통계량 함수

`sum()` : 합

`mean()` : 평균

`max()` : 최대값

`min()` : 최소값

`len()` : 개수

`std()` : 표준편차

`count()` : 빈도(개수)

## 7. 데이터 시각화 (그래프)

`seaborn` 패키지를 사용한다.

~~~python
import seaborn as sns
~~~

### 7.1. 산점도

나이와 소득처럼 연속 값으로 된 두 변수의 관계를 표현할 때 사용한다.

`sns.scatterplot()` 함수를 사용한다.

~~~python
sns.scatterplot(data = mpg, x = 'displ', y = 'hwy') \ # mpg 변수에 담긴 데이터의 x 값에 'displ' y 값에 'hwy'로 타나낸다.
    .set(xlim = [3, 6], ylim = [10, 30]) # x 값의 범위를 3 ~ 6으로 고정, y 값의 범위를 10 ~ 30으로 고정

# 'hue' 파라미터를 사용하면 변수의 값들을 해당 값으로 구분해줄 수 있다.
sns.scatterplot(data = mpg, x = 'displ', y = 'hwy', hue = 'drv') # 'drv' 변수에 따라 색으로 구분
~~~

### 7.2. 막대그래프

성별과 소득처럼 집단 간 차이를 표현할 때 사용한다.

`sns.barplot()` 함수를 사용한다

~~~python
# 그룹화한 데이터를 만들어준다.
df_mpg = mpg.groupby('drv', as_index = False) \ # 'drv' 변수가 인덱스로 나타나지 않게 하기 위해 'as_index = False'를 파라미터로 넘겨준다
            .agg(mean_hwy = ('hwy', 'mean'))
sns.barplot(data = df_mpg, x = 'drv', y = 'mean_hwy') # 말 안 해도 알겨

# 막대 그래프를 정렬하고 싶다면 df_mpg 변수를 정렬해주면 된다.
df_mpg = df_mpg.sort_values('mean_hwy', ascending = False)
sns.barplot(data = df_mpg, x = 'drv', y = 'mean_hwy')
~~~

### 7.3. 선 그래프

시계열 데이터(환율, 주가지수 등 경제지표처럼 시간에 따라 변하는 데이터)를 표현할 때 사용한다.

`sns.lineplot()` 함수를 사용한다.

~~~python
sns.lineplot(data = economics, x = 'date', y = 'unemploy')

economics.info() # 변수 타입을 확인해보면 date가 object 타입으로 되어있어 그래프 표현 시 매끄럽게 표현이 되지 않는다.
economics['date2'] = pd.to_datetime(economics['date']) # pandas 패키지의 to_datetime() 함수를 사용하여 데이터 타입을 datetime으로 바꿔준 후 사용한다.

# 연, 월, 일 추출
economics['date2'].dt.year
economics['date2'].dt.month
economics['date2'].dt.day
# 'date' 변수는 안 됨. datetime 타입으로 변경한 'date2'만 가능하다.
# 'date' 변수에 사용 시 : ...AttributeError : Can only use .dt accessor with datetimelike values 에러가 발생

# 그래프에 연도 별로 추출하기
economics['year'] = economics['date2'].dt.year # 연도만 뽑은 데이터를 year 변수에 할당
sns.lineplot(data = economics, x = 'year', y = 'unemploy') # 연도별로 선 그래프가 나타남

# 신뢰 구간 제거
sns.lineplot(data = economics, x = 'year', y = 'unemploy', errorbar = None) # errorbar = None으로 신뢰구간을 제거하고 나타낸다.
~~~

### 7.4. 상자 그림

분포를 알 수 있는 그래프로 평균만 볼 때보다 데이터 특징을 좀 더 자세히 볼 수 있다.

데이터 분석 그래프 중 가장 중요하다.

`sns.boxplot()` 함수를 사용한다.

* 상자의 가로 선분 : 중앙값

* 상자의 아랫면 : 상위 25% 지점 (1분위 수)

* 상재의 윗면 : 상위 75% 지점 (3분위 수)

* 상자 밖 가로 선분 : 최대값과 최소값

* 상자 밖 점 : 극단치

#### 7.4.1. 극단치 구하는 법

* 4분위 거리 (IQR) = 1분위 수 - 3분위 수

* 1.5 IQR = IQR * 1.5 

* 아래 극단치 = 1분위 수 - 1.5 IQR 보다 작은 값

* 위 극단치 = 3분위 수 + 1.5 IQR 보다 큰 값


~~~python
# 값을 수동으로 정렬하기
sns.boxplot(data = mpg, x = 'drv', y = 'hwy', order = ['f', 'r', '4']) # order 파라미터를 이용해 그래프 순서를 정렬해준다.

# 값에 따른 정렬 자동으로 하기
drv_order = mpg.groupby('drv')['hwy'].median().sort_values(ascending = False) # 중앙값 별로 내림차순으로 정렬. 인덱스로 나타난다.
drv_order.index # 해당 변수의 인덱스를 출력 (이 값을 order 파라미터의 값으로 넣어주면 됨

sns.boxplot(data = mpg, x = 'drv', y = 'hwy', order = drv_order.index) # drv_order의 중앙값에 따라 내림차순으로 자동으로 정렬되어 나타난다.
~~~

## 8. 데이터 결합 (파트 3 챕터 5)

### 8.1. 열 추가

데이터를 가로로 추가하는 것(열 추가)은 'merge' 함수를 사용한다.

~~~python
test1 = pd.DataFrame({'id' : [1, 2, 3, 4, 5], 'midterm' : [60, 80, 70, 90, 85]})
test2 = pd.DataFrame({'id' : [1, 2, 3, 4, 5], 'midterm' : [70, 83, 65, 95, 80]})

total = pd.merge(test1, test2, how = 'left', on = 'id')

total
~~~

how 파라미터에 결합하는 방향을 적는다 'left, right 등' 첫 변수 데이터 테이블인 test1의 왼쪽 방향으로 병합이 되며 'on' 파라미터로 넘긴 컬럼명을 기준으로 합쳐진다.

### 8.2. 행 추가

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

## 9. 데이터 정제

### 9.1. 결측치 : 누락 값

결측치가 있다면 제거하거나 채워넣는 방법이 있다.

#### 9.1.1. 결측치 제거

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

#### 9.1.2. 결측치 대체

결측치가 많아 데이터 손실이 많은 경우 사용한다

대표값 (평균, 최빈값) 등으로 대체 하거나 다른 변수들에 따른 추정값으로 대체하는 법이 있다.

~~~python
exam = pd.read_csv('exam.csv') # 데이터 불러오기
exam.loc[[2, 7, 14], ['math']] = np.nan # 첫 매개변수는 선택할 인덱스, 두 번째는 해당 열이 된다. 그곳에 결측치 할당.
exam

exam['math'] = exam['math].fillna(exam['math'].mean()) # 결측치를 평균값으로 대체하기
~~~

'.fillna()' 함수를 이용하여 해당 변수의 결측치에 일괄적으로 값을 할당할 수 있다.

### 9.2. 이상치

이상치는 정상 범주에서 벗어난 값이다.

|종류|예|해결방법|
|---|---|---|
|존재할 수 없는 값|성별 변수에 3|결측처리|
|극단적인 값|몸무게 변수에 200|정상범위 기준 정해서 결측 처리|

'.value_counts()' 함수를 이용하여 변수에 존재하는 값 별로 카운트 할 수 있으며 이상치 확인이 쉬워진다.

추가로 '.sort_index()' 함수로 인덱스 별로 정렬을하면 보기가 더 편하다.

#### 9.2.1. 존재할 수 없는 값

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

#### 9.2.2. 극단적인 값

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

## 10. ChatGPT 4.0

### 10.1. Advanced Data Analysis

ChatGPT 4.0 부터 파일 업로드, 다운로드 기능이 있기 때문에 데이터 파일을 업로드 하고 필요한 분석을 요청하면 실행된 코드와 결과물을 보여준다.

즉, 위에 말한 거 몰라도 일반인들도 다 할 수 있음.

하지만 할루시네이션[^hal]은 항상 조심해야한다. 

분석 목표만 요청한다면 할루시네이션 위험이 높아진다. 따라서 단계별로 작업을 하고 오류를 점검하는 게 좋다. 또 프롬프트에 오류 점검해달라고 추가 요청을 한다.

이점 유의해서 AI 사용하도록

### 10.2. Browsing

ChatGPT 4.0 부터 최신 정보를 검색하여 답변을 줄 수 있다.


## 11. Communities

아래는 데이터 분석 네트워킹 커뮤니티이다.

[데이터 분석 커뮤니티](https://fb.com/groups/datacommunity)

[지피터스](https://gpters.org)

---
[^hal]: 데이터에 오류 가능성을 염두해두지 않거나, 없는 데이터를 그럴듯하게 생성해서 분석
