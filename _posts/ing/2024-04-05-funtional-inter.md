---
layout: post
title: "[Java] Funtional Interface"
subtitle: 
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
date: 2024-04-05 11:00:00 +0900
published: false
---

## 1. Funtional interface

> 함수형 인터페이스(Functional Interface)는 오직 1개의 추상 메소드를 갖는 인터페이스를 말한다.

하나의 추상 메소드를 갖기 때문에 Single Abstract Method(SAM)이라고 불리기도 한다.

### 1-1. Why

함수형 인터페이스는 자바의 람다식이 함수형 인터페이스로만 접근이 되기 때문에 사용한다.

인터페이스의 메서드와 람다식이 1:1로 연결이되고, 인터페이스의 추상 메소드의 매개변수(개수와 타입)와 리턴타입과 동일한 람다식을 할당해서 사용할 수가 있다.

### 1-2. How

추상 메소드가 하나만 존재한다면 default 메소드, static 메소드가 존재할 수 있다.

interface 내에 추상 메소드가 하나 뿐이라면 Funtional interface로 판단한다.

인터페이스 상단에 검증과 유지보수를 위해 @Funtionalinterface <u>어노테이션</u>[^ano]을 써주지만, 쓰지 않아도 동작에 문제는 없다. @Funtionalinterface가 붙었지만 형식에 맞지 않는다면 컴파일 에러 메시지를 띄운다.

~~~java
// 에노테이션 작성으로 명시
@Funtionalinterface
public interface FIexample {
  // 추상 메소드는 하나만 존재
  public abstract void greeting();

  // default, static method

  default void do() {
    System.out.println("명령질")
  }

  static void ignore() {
    System.out.println("몰라")
  }
}
~~~

함수형 인터페이스는 아래와 같이 람다식에 사용된다.

~~~java
// 위 코드에서 만든 함수형 인터페이스를 사용한다
FIexample func = new FIexample() {
  @Override
  public void greeting() {
    System.out.println("안녕하세요?");
  }
};

func.greeting();
~~~

~~~java
FIexample func = () -> System.out.println("안녕히가세요?");

func.greeting();
~~~

두가지 코드 결과 값은 갖지만 코드 길이나 가독성 면에서 아래가 훨씬 좋다.

위 코드는 익명 클래스를 사용해서 표현했고, 아래 코드는 미리 정의한 FIexample 데이터 타입을 선언하고 메소드를 실행했다.

추상 메소드가 하나만 정의돼 있기 때문에 람다식이 해당 메소드만 타겟해서 구현한다.

## 2. java.util Package

Java 8부터는 자주 사용하는 메소드를 함수형 인터페이스로 미리 정의한 패키지가 있다.

웬만하면 이 패키지로 람다식을 다 만들 수 있기 때문에 새로 만들어서 사용할 필요는 없다시피하다.

아래는 해당 패키지에 있는 함수형 인터페이스이다.

| Funtional Interfaces | I/O | Method |
| :--- | :--- | :--- |
| **Predicate<T>** | T → boolean | boolean test(T t) |
| **Consumer<T>** | T → void | void accept(T t) |
| **Supplier** | () → T | T get() |
| **Function<T, R>** | T → R | R apply(T t) |
| **Comparator<T, T>** | (T, T) → int | int compare(T o1, T o2) |
| **Runnable** | () → void | void run() |
| **Callable** | () → T | V call() |

아래는 여기서 사용하는 타입의 문자 설명이다.

| Type | Explanation |
| :---: | :---: |
| **<T>** | Type |
| **<R>** | Return Type |
| **<E>** | Element |
| **<K>** | Key |
| **<V>** | Value |
| **<N>** | Number |

해당 인터페이스를 사용한 예제를 살펴보자.

### 2-1. Predicate

~~~java
@FuntionalInterface
public interface Predicate<t> {
  boolean test(T t);
}


___

[^ano]: 어노테이션은 다른 프로그램에게 유용한 정보를 제공하기 위해 사용되는 것. @를 사용하여 작성하며, 해당 타겟에 대한 동작을 수행하는 프로그램 외에는 다른 프로그램에게 영향을 주지 않는다.
