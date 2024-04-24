---
layout: post
title: "[Java] No grammar constraint (DTD or XML Schema)"
subtitle: 파싱이랑 뭐가 다르죠?
author: Bonazoey
categories: Java
banner:
  image: "./assets/images/java.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Java, XML]
top: false
date: 2024-04-12 10:00:00 +0900
published: false
---

### 사전 준비

지금까진 결과값 출력을 위해서 `java project`로 진행했지만, Java 6 ~ 9 버전과 달리 Java 11 버전을 사용하고 있는 지금 버전에선 마샬링을 위한 `JAXB` 라이브러리가 내장돼있지 않기 때문에 외부에서 가져와야한다.

따라서 `pon.xml`의 `dependancy`에 의존성 주입을 해주어야 하는데, 기존에 사용하는 자바 프로젝트는 pom.xml이 존재하지 않는다.

따라서 `Spring MVC project`로 프로젝트를 시작하려 했지만 내가 사용하는 `Spring Tools Suite 4.0`에는 기존 3.0 버전의 `Legacy project` 템플릿이 제공되지 않아 시작할 수 없었다.

그래서 pom.xml이 포함된 틀을 제공하는 다른 프로젝트로 시작했는데 그것이 `Maven project`이다.

## 1. Marshalling

> 한 객체의 메모리에서 표현방식을 저장 또는 전송에 적합한 다른 데이터 형식으로 변환하는 과정이다.

상이한 프로그램 간의 데이터 전달에 이용되며, 마샬링은 <u>`직렬화`</u>[^seri]와 유사하다.

직렬화는 주소값을 타고 넘어가서 실제 의미하는 값을 private한 데이터로 전부 변환하는 작업(**byte stream으로 바꾸는 작업**)이며, 마샬링은 **변환하는 일련의 과정**을 뜻한다.

그러므로.. 마샬링 안에 직렬화가 하위로 들어가지 않을까

반대 과정을 `언마샬링`(unmarshalling; 또는 디마샬링)이라고 하며 마찬가지로 역직렬화와 비슷하다.


## 2. JAXB

> JAXB(Java Architecture for XML Binding)는 자바 클래스를 XML로 표현하는 자바 API이다.

JAXB의 기능은 마샬링, 언마샬링 두 가지 기능이 있다. 이 API를 통해 마샬링을 구현해 볼 것이다.

### 2-1. Annotation

JAXB는 어노테이션을 이용하여 XML 관련 요소를 정의한다.

아래는 사용되는 어노테이션이다.

| 어노테이션 | 설명 |
| :---: | :--- |
| @XmlRootElement | XML의 Root Element 명을 정의한다. |
| @XmlElement | XML의 Element 명을 정의한다. |
| @XmlType | XML 스키마 이름과 namespace를 정의합니다. propOrder 속성을 이용해서 XML 표현 시 요소들의 표현 순서를 정의한다. |
| @XmlElementWrapper | 다른 XML 요소들을 감싸는 역할을 한다. List 같은 컬렉션 객체들을 XML 변환할 때 사용할 수 있다. |

### 2-2. 마샬링 (java Object → XML)

마샬링 시 `@XmlRootElement`와 `@XmlElement`를 통해 XML 문서로 매핑되는 요소들을 만들어야한다.

이때 만드는 해당 클래스는 VO 클래스와 굉장히 유사하다.

~~~java
package test.marshalling;

import javax.xml.bind.annotation.XmlElement;
import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement
public class Person {
	
	private String name;
	private int age;

	public Person() {
		super();
	}

	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	@XmlElement
	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@XmlElement
	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

}
~~~
___

[^seri]: 직렬화 또는 시리얼라이제이션(serialization)은 컴퓨터 과학의 데이터 스토리지 문맥에서 데이터 구조나 오브젝트 상태를 동일하거나 다른 컴퓨터 환경에 저장(하고 나중에 재구성할 수 있는 포맷으로 변환하는 과정이다.
