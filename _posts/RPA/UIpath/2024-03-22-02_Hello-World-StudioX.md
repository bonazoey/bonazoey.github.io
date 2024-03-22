---
layout: post
title: "[UIpath] 'Hello World' (StudioX)"
subtitle: 이 사람은 인사합니다.
author: Bonazoey
categories: RPA
banner:
  image: "./assets/images/planet.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [RPA, UIpath]
top: false
---

## 서론

> UIpath Academy 수강자.

`UIpath Academy` 수업을 하나씩 듣고 있는데 여기선 Hello World를 `StudioX`를 통해 `메세지 박스`로 찍어내고 있다. 기존에 `Studio`로 notepad로 실행해봤으니 이번 것도 해보려고 한다.

## 과정

### 프로젝트 시작
![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/0ccdb5e5-e5ce-4d5b-9133-b16b22e099cf)

StudioX 실행시 나오는 화면이다. 새로운 프로젝트를 만들어주자! (Hellow는 못 본 척 해주자..)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/73b860a3-b91a-45d8-a914-3ebbb69a60bf)

Studio로 프로젝트 생성 때와 달리 몇 가지 옵션이 더 있는 게 보인다. 크게 다를 건 없으니 프로젝트를 생성하겠다.

### 프로젝트 화면

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/701b53a5-feda-4d5e-b265-8d0fcc80a9ab)

오른쪽 액티비티 탭에 MS와 호환을 위해 Studio에는 없던 몇 가지 추가된 것이 보인다.

### Hello World

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/80f2abbc-0933-4126-95f2-6b5a8f6f88c2)

액티비티에 `message box`를 검색해 시퀀스에 추가해준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/ef839fc7-7173-41d9-8be2-130d70fddb93)

시퀀스에 추가한 모습인데 message box는 JS에 alert창을 띄우는 것과 비슷한 역할을 한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/d3091273-43b7-48de-a033-f219f8de7f49)

텍스트창을 눌러서 텍스트를 선택한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/43c5e95f-67e1-4d43-91a4-0db8663f8cd5)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/ee83986c-da5e-497c-8349-22bc9acd6a70)

Hello world를 작성 후 publish 한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/ef39b085-4a46-40b3-8445-64332719bf7c)

게시 옵션 탭에서 오케스트레이터 어느 영역에 게시할지 선택 가능하다.

**다음**을 누른 후에 나오는 인증서는 지금은 무시해도 된다.

그냥 게시해주자!

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/67049f7a-4e4b-453f-856b-7ff86b5c950d)

이후 UIpath assistant를 실행하면 오케스트레이터에 게시된 프로젝트를 실행할 수 있다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/49190c6b-c975-4c30-844f-b19f90c3c5b6)

**Ta-Da!** 정상적으로 실행하는 걸 확인했다.

## 결과

Studio로 작성했던 Hello world 프로젝트에선 Studio 개발 환경 내에서 Hello World를 실행하는 것 까지 진행해봤지만

이번엔 Orchestrator에 직접 게시도 하였고, UIpath assistant를 통해 anttend robot에 명령을 하여 자동화된 프로젝트를 통해 Hello World 메세지 박스를 띄우는 작업까지 진행했다.

Java 때와 마찬가지로 지금은 굉장히 쉽다고 느껴지지만 배우면 배울수록 갈 길이 멀다고 느껴질 것 같은 느낌..
