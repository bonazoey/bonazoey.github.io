---
layout: post
title: "[Java] No grammar constraint (DTD or XML Schema)"
subtitle: 좀 정의해줘라
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
date: 2024-04-11 11:00:00 +0900
---

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a2a0b34d-8cfa-4a37-b9d0-c85baab1cbc8)


이클립스에서 XML 파일을 만들고 보면 XML 선언부 아래에 `Bind to grammars/schema...`라고 뜬다.

이 뜻은 DTD나 XSD가 정의돼있지 않고 네임스페이스도 명시되지 않았다는 뜻인데, 해결 방법은 3가지가 있다.

## 1. java에서 유효성 검사 무시

~~~java
DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
DocumentBuilder builder = factory.newDocumentBuilder();
Document doc = builder.parse("./xmlsrc/xpathsample.xml");

// 스키마 유효성 검사 무시
factory.setValidating(false);
// 네임스페이스 유효성 검사 무시
factory.setNamespaceAware(false);
~~~

`DocumentBuilderFactory`에서 기본값으로 유효성 검사를 실시하기 때문에 유효성 검사 무시 설정을 해주면 스키마/네임스페이스에 대한 검사를 하지 않는다.

## 2. XML 파서에게 알리기

~~~xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
~~~

XML 선언부에 `standalone`을 `no`로 설정해준다.

`standalone`은 XML 파서에게 XML 문서의 독립성을 알리는 것이다. 사실 유효성 검사는 자바에서 설정해야되기 때문에 이걸 설정한다고 해도 유효성 검사는 실시된다.

사실 방법은 2개다. 어그로 죄송..

## 3. 스키마 정의하기

그냥 스키마를 정의해주는 방법이 있다.

예제 XML 문서의 스키마를 정의해보자.

~~~xsd
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
			targetNamespace="http://www.example.com/"
			xmlns:this="http://www.example.com/">
	
	<!-- 루트요소 school 정의 -->
	<xs:element name="school">
		<xs:complexType>
			<xs:sequence>
				<xs:element ref="this:class" minOccurs="1" maxOccurs="unbounded"/>
			</xs:sequence>
		</xs:complexType>
	</xs:element>
	
	<!-- class 요소 정의 -->
	<xs:element name="class">
		<xs:complexType>
			<xs:sequence>
				<xs:element ref="this:student" minOccurs="1" maxOccurs="unbounded"/>
			</xs:sequence>
			<xs:attribute name="value" type="xs:integer" use="required"/>
		</xs:complexType>
	</xs:element>
	
	<!-- student 요소 정의 -->
	<xs:element name="student">
		<xs:complexType>
			<xs:attribute name="name" type="xs:string" use="required"/>
		</xs:complexType>
	</xs:element>
</xs:schema>
~~~

루트요소에서 `ref` 속성을 통해 아래의 class 요소 정의를 참조해줬다. `ref` 속성은 `targetNamespace`가 정의돼있고 해당 네임스페이스가 명시돼지 않았을 경우 에러가 뜬다.

따라서 꼭 네임스페이스를 명시하고 사용해줘야한다. 그렇기에 접두사 `this`를 사용하여 해당 XSD의 class 요소를 참조할 수 있었다.

또 class 요소 안에 student를 참조하지 않고 포함시켜서 넣었을 경우 XML에서 네임스페이스를 지정하지 않아야 에러가 뜨지 않았고 지정하기 위해선 student 요소를 밖으로 빼고 참조하는 식으로 했어야했다.

`xs:attribute`는 복합요소이기 때문에 `xs:complexType`에 넣어주었고 그 하위 태그에는 `xs:sequence`로 시작하였다.

해당 요소가 1개 이상인 것은 `minOccurs="1"`, `maxOccurs="unbounded"` 속성을 주었고, 꼭 필요한 값은 `use="required"`를 통해 명시해주었다.

이 스키마를 XML 문서에 적용시키게 되면 아래와 같아진다.

~~~XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<sc:school xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:sc="http://www.example.com/"
	xsi:schemaLocation="http://www.example.com/ xpathxsd.xsd">

	<sc:class value="1">
		<sc:student name="김복수"></sc:student>
		<sc:student name="도우찬"></sc:student>
		<sc:student name="성숙희"></sc:student>
	</sc:class>
	<sc:class value="2">
		<sc:student name="강참모"></sc:student>
		<sc:student name="서소라"></sc:student>
		<sc:student name="함도영"></sc:student>
	</sc:class>
	<sc:class value="3">
		<sc:student name="가정식"></sc:student>
		<sc:student name="김가람"></sc:student>
		<sc:student name="김필창"></sc:student>
		<sc:student name="우미영"></sc:student>
	</sc:class>
</sc:school>
~~~

사실 적용한 XSD가 하나 뿐이어서 충돌할 일이 없기 때문에 네임스페이스를 명시하지 않아도 되지만 예제로 다 명시해보았다.

이로써 `Well-formed XML`에서 `Valid XML`문서가 되었다.

더이상 `No grammar constraint`도 뜨지 않는다.
