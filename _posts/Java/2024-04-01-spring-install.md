---
layout: post
title: "[Spring] Spring Installation"
subtitle: 
author: Bonazoey
categories: Java
banner:
  image: "./assets/images/Java.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Java, Eclipse, Spring]
top: false
date: 2024-04-01 09:00:00 +0900
---


오늘은 `Spring`을 설치해보려고 한다.

## Spring?

> Spring makes programming Java quicker, easier, and safer for everybody. Spring’s focus on speed, simplicity, and productivity has made it the world's most popular Java framework.Spring makes programming Java quicker, easier, and safer for everybody. Spring’s focus on speed, simplicity, and productivity has made it the world's most popular Java framework.

라고 공식 홈페이지에 나와있다.

쉽게 말하면 Java를 이용한 프로그래밍을 쉽게 해주는 `Framework`이다. 이클립스에선 일일이 설정해주었던 것을 기본적으로 프레임으로 제공하여 굉장히 쉽고 빠르게 프로젝트 진행이 가능하다.

그렇기에 전세계적으로 많이 이용하는듯..

그럼 먼저 `Eclipse IDE` 내에서 `Spring`을 설치해보려 한다.

## 마켓플레이스

스프링은 따로 다운받아서 사용할 수도 있지만 이클립스 자체적으로 **마켓플레이스**에서 설치해서 사용할 수 있다.


## 설치 과정

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/39c26663-e018-4ee4-a199-d18805c9c6b8)

Eclipse 상단의 `Help > Eclipse Marketplace` 로 들어가준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/43333490-e238-471e-8ae9-22885f0fb96a)

`Spring`을 검색하고 제일 상단에 `Spring tools 3` 를 설치해준다.

### Problem

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/ba0218a4-11a2-49df-b016-7e52a8f06aee)

하지만 설치 도중에 이런 에러메세지가 나왔다.

#### Attempt 1

구글링 해보니 관리자의 권한으로 실행하라는 말이 있어서 해보았지만 무용지물.

두 번째 방법을 사용해보자..

#### Attempt 2

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/b3124d70-c34f-49d2-85d6-26df2471dbf4)

`Help > Install new software`로 들어간다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/d3ee005e-d943-4a23-b559-4b18a9b4a59f)

`https://download.eclipse.org/mylyn/releases/latest/` 를 입력 후 Add를 눌러 추가해주었다.

`Mylyn`은 이클립스 태스크 관리를 위한 서브 시스템이라는데, 자세한 건 잘 모르겠다.

설치 시 오류는 뜨지 않지만, 설치가 진행되지 않은 것 같다.

이클립스 내에서 스프링을 찾아볼 수 없었다.

#### Attempt 3

스프링 4가 나오면서 하위 버전인 3이 호환이 이제 되지 않는 다는 정보를 얻었다.

`Spring tools 4`를 설치해보자

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/86e4df4f-c6fb-4c1c-a044-57ae8675c2a9)

sts 4는 정상적으로 설치된 것을 볼 수 있다.

하지만 New project 어디에서도 스프링 관련 프로젝트 생성하는 항목이 없었다.

검색해보니 이클립스 내에 마켓플레이스에서 설치하는 sts는 이클립스 버전에 따라 호환 문제가 있을 수 있으니 따로 외부에서 설치해서 실행하는 걸 추천하더라.

(물론 지금 이 문제의 원인이라는 것은 아니다)

#### Attempt 4

Spring 사이트에서 직접 sts 4 jar 파일을 다운 받았다.

원하는 디렉토리에 넣고 cmd를 통해서 압축을 해제한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/e2308085-a6e9-4ead-a612-34305c478c58)

`java -version`으로 자바 환경변수가 제대로 설정되어 작동되는지 확인한다.

첫 시도에는 java를 찾지 못 해서 환경변수를 다시 설정해주었는데 Path에서 `%JAVA_HOME%\bin`의 순서를 최상단으로 올렸더니 제대로 인식이 됐다.

그 후 `cd '설치파일이 있는 경로'`로 디렉토리를 이동 후 `java -jar '파일명.jar'` 해주었다.

그러면 압축해제가 좌라락 진행된다.

(사실 그냥 압축프로그램으로 jar 파일 압축 해제 해도 됐을 것 같다)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/0b1d9671-4b79-4c1f-945b-c37a45ecf3cc)

설치 후 실행하고 Install new software 메뉴에서 `Latest Eclipse Release - https://download.eclipse.org/releases/latest`를 선택해준 해당 항목을 체크하고 넘어간다.

이렇게 perspective 목록에 `Java EE`가 추가된 게 보인다.

하지만 Spring 관련 Legacy project를 생성하려했지만 어디에도 보이지 않는다.

내 추측으론 sts 4에는 지원을 하지 않는 것으로 보이는데.. 그렇다면 이클립스에 마켓플레이스에서 sts 4를 설치했을 땐 정상적으로 설치되었던 것이었다.

그렇다면.. sts 4에서 add on으로 sts 3을 설치하면 되지 않을까?

#### Attempt 5

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/7d125340-a893-4ac9-94c8-b0e07e145981)

이클립스 때와 마찬가지로 같은 에러 메세지가 뜬다.

관리자로 실행하면서 다른 에러메세지가 떴다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/c3dcd508-fc56-4a93-9ac6-3e50fb28a1b3)

Mylyn이 없어서 그런 것 같은데.. 다운 받아보자..

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/57e467fb-83c1-4996-8e37-769e817ced66)

인고의 시간..

에러는 뜨지 않았지만 이클립스 때와 마찬가지로 설치가 되진 않았다.

버전을 낮추는 다른 방법이 있는 것 같은데.. 길어질 것 같으니 다음에 계속하겠다.
