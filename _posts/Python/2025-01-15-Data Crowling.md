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

pykrx 라이브러리를 설치한다. pykrx 모듈은 네이버와 [KRX](https://data.krx.co.kr/)에서 주가 정보를 스크래핑하기 위해 만들어진 모듈이다.

이외엔 사용이 어려운듯..

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
def get_all_tickers(date):
    # KOSPI 종목 티커 가져오기
    kospi_tickers = stock.get_market_ticker_list(market = "KOSPI", date)
    # KOSDAQ 종목 티커 가져오기
    kosdaq_tickers = stock.get_market_ticker_list(market = "KOSDAQ", date)

    return kospi_tickers + kosdaq_tickers
~~~

2. 종목 별 주가 데이터 수집

pykrx 패키지

beautifulsoup은 파싱 라이브러리

selenium은 크롤링 라이브러리
