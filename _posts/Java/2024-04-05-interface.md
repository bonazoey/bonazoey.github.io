---
layout: post
title: "[Java] Interface"
subtitle: 명령을 따라야 합니다?
author: Bonazoey
categories: Java
banner:
  image: "./assets/images/java.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Java, Interface]
top: false
date: 2024-04-05 10:00:00 +0900
---

## 1. Interface

> 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스

다시 말해 인터페이스는 하나의 표준화 문서 같은 것이다. 

### 1-1. Why

[추상 클래스](https://bonazoey.github.io/java/2024/04/05/abstract.html){: target="_blank"}와 비슷한데, 추상 클래스는 자식 클래스에서 사용할 기능들을 구현해놓고 추가로 구현할 기능을 추상 메소드로 남겨 자식 클래스에서 구현함으로써 확장을 하는 느낌이라면,

인터페이스는 해당 인터페이스를 사용하는 클래스에서 구현해야하는 공통 기능들을 강제로 정해주는 느낌이다. 그렇기 때문에 표준화가 가능함.

### 1-2. How

인터페이스는 클래스가 아니기 때문에 `interface` 선언을 해주고, 구성은 추상 메소드 외에 `final 변수(상수)`, `efault 메소드`, `static 메소드`, `private 메소드`도 가능하다.

인터페이스 내에 추상 메소드에 public abstract를 선언해도 되고 생략해도 된다.

~~~java
// interface를 선언하여 생성한다. 
interface Rubber {
  void strech();
}

interface Metal {
  void hard();
  void heavy();
} 
~~~

인터페이스는 다른 인터페이스들을 상속 받을 수 있으며 `extends`를 사용하고 다중 상속 또한 가능하다.

~~~java
// 인터페이스는 extends를 통해 다른 인터페이스를 상속 받는다. 다중 상속 가능.
interface Wheel extends Rubber, Metal {
  void roll();
  void boom();
}
~~~

상속 받은 클래스에선 클래스명 뒤에 `implements + 인터페이스명`으로 상속을 하며 다중 상속이 가능하다.

아마 자식 클래스가 추상 부모 클래스 처럼 미리 만들어놓은 기능의 확장이 아니라, 해당되는 인터페이스 기능의 표준이기 때문에 겹치는 일이 없기 때문이 아닐까

~~~java
// implements를 통해 인터페이스 상속이 가능하다.
class Car implements Wheel {

  @Override
  void strech() {
    System.out.println("늘어난다");
  }

  @Override
  void hard() {
    System.out.println("딱딱하다");
  }
  @Override
  void heavy() {
    System.out.println("무겁다");
  }

  @Override
  void roll() {
    System.out.println("굴러간다");
  }

  @Override
  void boom() {
    System.out.println("터진다");
  }
}

~~~

## 2. Summary

아래는 추상 클래스와 인터페이스의 목적 비교표 이다.

| 구분| Interface | Abstract Class |
| :---: | :---: | :---: |
| 목적 | 객체의 기능을 모아둔 표준 문서와 비슷하다, 인터페이스의 모든 추상메소드 구현 목적 | 자식클래스에서 필요로하는 기능 대부분을 구현하고 상속 받아 할 수 있게 함. 다형성 구현 목적 |
