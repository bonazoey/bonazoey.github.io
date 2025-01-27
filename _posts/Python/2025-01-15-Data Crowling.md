---
layout: post
title: "[Data Analysis] 금융 데이터 수집"
subtitle: 도적질도 잘 합니다.
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
tags: [Python, Selenium, BeautifulSoup, Crowling]
top: 1
published: false
---

## 1. 금융 데이터 수집 방법

1. 파이썬 모듈 (패키지)

편하지만 주어진 데이터, 포맷만 가능

2. 웹 크롤링

사이트에서 직접 수집. 파이썬/웹 지식 필요

3. API

파이썬/웹 지식 필요, 많은 경우 API가 주어지지 않음

## 2. 데이터 수집

### 2.1. 티커 수집

1. 코스피, 코스닥 전 종목 코드(티커) 수집

pykrx 라이브러리를 설치한다.

pykrx 모듈은 네이버와 [KRX](https://data.krx.co.kr/)에서 주가 정보를 스크래핑하기 위해 만들어진 모듈이다.

이외엔 사용이 어려운듯..

FinanceDateReader나 yfinance 모듈도 사용가능하지만 해당 모듈은 상장폐지가 된 종목이 있다면 과거의 기록을 볼 수 없다.

~~~python
# 내장 패키지가 아니기 때문에 아나콘다 프롬프트에서 인스톨해준다.
pip install pykrx

# 프롬프트가 아닌 주피터에서 바로 설치하고 싶다면 매직커맨드를 사용한다(!)
!pip install pykrx
~~~

여기서 pip는 python install package의 약자이며 패키지 인스톨 매니저이다.

파이썬 버전에 따라 pip 또는 pip3을 사용한다.

~~~python
# pykrx 의 서브모듈인 stock 사용
from pykrx import stock

# 코스피와 코스닥 전 종목 티커 가져오는 메소드
def get_stock_codes(date):
    # KOSPI 종목 티커 가져오기
    kospi_tickers = stock.get_market_ticker_list(market = "KOSPI", date)
    # KOSDAQ 종목 티커 가져오기
    kosdaq_tickers = stock.get_market_ticker_list(market = "KOSDAQ", date)

    return kospi_tickers + kosdaq_tickers

# 출력하면 티커가 주르륵 나옴
get_stock_codes('2024-01-01')

# stock 모듈의 get_marcket_ticker_list 함수를 사용하면 현재 상장폐지가 되었더라도 이전 시점의 티커를 불러올 수 있다.
stock.get_market_ticker_list("2020-01-01", market = 'KOSPI')
~~~

2. 종목 별 종가 데이터 수집

~~~python
import pandas as pd
import FinanceDataReader as fdr

# 종가 반환 함수
def get_stock_prices(stock_code, start_date, end_date)
  df = fdr.DateReader(stock_code, start_date, end_date)

  return df['Close'] # 종가만 반환

# 입력 기간 동안의 시가, 고가, 저가, 종가, 거래량, 변화율이 들어있다. (시리즈)
get_stock_prices('027410', '2020-01-01', '2024-12-31')

# 사실 위처럼 따로 메소드를 만들지 않고 바로 사용해도 됨. 같은 결과임
fdr.DateReader(stock_code, start_date, end_date)['Close']
~~~

beautifulsoup은 파싱 라이브러리

selenium은 크롤링 라이브러리
