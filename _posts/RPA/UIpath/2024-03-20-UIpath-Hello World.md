---
layout: post
title: "[UIpath] 'Hello World'"
subtitle: 이 사람은 로봇입니다.
author: Bonazoey
categories: RPA
banner:
  image: "./assets/images/planet.gif"
  video: "./assets/images/planet.gif"
  loop: true
  volume: 0
  start_at: 0
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [RPA, UIpath]
top: 1
---

## 기초지식

### RPA란?

> : RPA(Robotic Process Automation)는 디지털 시스템 및 소프트웨어와 사람 사이의 상호 작용을 에뮬레이션하는 소프트웨어 로봇을 쉽게 빌드, 구현, 및 관리할 수 있도록 해주는 소프트웨어 기술.

다시 말해 로봇을 통해 자동화 시키는 프로세스를 말하는데, 여기서 로봇은 Physical한 요소가 아니라 Software 요소를 말한다.

대표적인 RPA tool로는 `UIpath`, `Automation Anywhere`, `Blue Prism`, 등이 있는데 그 중 **'UIpath'** 에 대해 포스팅하려 한다.

### UIpath?

> 전세계에서 가장 많이 사용하는 RPA 플랫폼

아무래도 가장 많이 사용하는 툴이다 보니 최적화는 물론, 기능 등이 많이 탑재 돼있을 것 같다.

> 찍먹해보니 굉장히 쉬운 작업

Spring Framework를 사용할 때에 비하면 굉장히 가시적이고 직관적인 디자인.

직접적인 코드 작성보다는 UI를 통한 시퀀스 만들기 느낌이다. Hello World를 찍어보면서 매크로 프로그램 같다는 느낌도 들었다.

> 사실 이것을 사용한다는 직장

제곧내

## "Hello World"

메인 프로젝트 실행 시 실행창을 열어 `notepad`를 실행 후 `"Hellow World"`를 작성하는 시퀀스를 만들어보려고 한다.

### 프로젝트 생성

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/776de243-69d2-4e78-9abb-13862b7e8d41)

`UIpath Studio`를 설치하고 실행시켰을 때 우측 상단에 프로젝트 만들 수 있는 화면이 뜬다.

잽싸게 만들어보자

### 프로젝트 화면

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/3d09a511-370a-4b57-9fa2-dae47e57d1bd)

프로젝트 생성 시 만들어 지는 화면인데, 하나하나 간단히 뜯어보자.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/35e84b58-811e-4f41-ab4b-853e495f2968)

좌측 화면이다. 여기선 프로젝트 목록과 해당 프로젝트에 대한 요소들을 살펴볼 수 있다.

아래의 액티비티 탭을 누르게 되면 `main 시퀀스`에 넣을 수 있는 여러가지 `기능`들이 표시가 된다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/342978e3-cfdb-418f-8791-fa7b11557c0a)

우측 화면이다. 여기선 main 시퀀스에 놓은 `activity`에 대한 `속성`을 관리할 수 있다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/cab0ea19-9dc6-4d15-8fcc-d786f4c9f9b3)

가운데 화면이다. 여기선 `activity`를 구성하여 실제로 구현할 `main 시퀀스`를 만든다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/03635b39-2a99-4d6d-9914-489149362560)

아래 화면이다. 여기선 `출력 결과`를 나타내는 것 같다. `console`이라고 생각하면 될듯.

### 액티비티 추가

#### Send Hotkey

`Send Hotkey`는 단축키를 보내주는 액티비티이다.

이를 통해 `Win + r` 단축키로 실행창을 열어보자

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/1a8c4dc9-27b6-4ee5-b632-959c953937ed)

하지만 액티비티에 아무리 Hotkey를 검색해도 `Send Hotkey`가 나오질 않았다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/b2b5f164-c199-4988-975e-991a5fcdf24b)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/257466ed-a00d-4d7d-953b-e1f297e83b73)

이 문제는 액티비티 `필터`에 `클래식`을 체크해주어서 해결했다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/48d4e34a-23bd-42ab-bcb7-dba5e07a6b8f)

액티비티를 시퀀스에 드래그 후 Win 키와 r을 입력해주었다.

#### Type into

`Type into`는 텍스트를 작성해주는 액티비티이다.

이를 통해 실행창을 연 후 notepad를 작성해주었다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/eb13a8ad-1cdf-41f8-aa79-6642d4838e5d)

그리고 다시 Send Hotkey 액티비티를 통해 enter 후 notepad를 F5를 통해 실행하였다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/28fc5357-3afc-4a09-ae95-09d44fab6601)

아이고! notepad와 같은 문자열을 작성해줄 땐 <b>큰따옴표</b>로 감싸주는 것을 잊지말도록하자..

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/9b89762e-a78f-4876-8493-65c72157566b)

다시 정상적으로 실행하였지만 notepad가 실행될 것이라는 기대와 다르게 "notepad" 뒤에 "enter"가 문자열로 그대로 붙었다.

이 문제는 enter를 실행하는 Send Hotkey 액티비티의 `속성`을 수정해주면 해결할 수 있는데

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/9f9fbb0e-14d5-4550-817d-6ba3b4bfae0a)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/fa015da6-3375-44e8-9569-207b8e8304f6)

해당 액티비티를 클릭한 후 오른쪽 속성 창에서 `특수키`를 `True`로 변경해주면 해결된다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a370a463-a1a2-431a-a92b-480cd49b0cd0)

이렇게 정상적으로 notepad가 실행되는 것을 확인했다.

그 후 다시 Type into 액티비티로 "Hello World"를 작성해주면 끝!

## 실행결과

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/dbf1479e-7f7e-47e8-8d36-57ac1569f9ca)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/0826fc21-2280-4a12-9aaa-ff3945cabc30)

**Ta-Da!**
