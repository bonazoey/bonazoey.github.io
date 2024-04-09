---
layout: post
title: "[Java] XML to Java (Parsing)"
subtitle: 까막눈이 아닙니다.
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
date: 2024-04-09 10:00:00 +0900
---

기본적으로 파일 형식이 다른 것 끼리는 호환이 되지 않는다.

생 XML 문서의 데이터의 구조를 Java에서 읽을 수 없다.

그렇기 때문에 Java든 어떤 프로그램이든 XML 문서를 읽기 위해선 **파싱 단계**를 거쳐야 하는데

[여기](https://bonazoey.github.io/basic/2024/04/05/xml.html){: target="_blank"}에서 말한 것 처럼 파싱의 단계는 `XML 문서의 문자열 → XML DOM 객체 → 객체에서 정보 추출`의 과정을 거친다.

따라서 이번 포스팅에선 `XML to Java` 파싱에 대해 알아볼 것이다.

## XML Data

먼저 XML 데이터는 총 3가지 방법으로 받을 수 있다.

1. 다른 시스템으로 받은 응답, 소스 내부의 문자열(String)

2. 시스템 내부 XML파일을 불러올 경우

3. 외부 사이트에서 제공하는 XML 데이터를 URL로 불러오는 경우

### 1. 문자열 파싱

~~~java
package test;

import java.io.ByteArrayInputStream;
import java.io.InputStream;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class Test {

	public static void main(String[] args) throws Exception {
		
		String sample = "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\r\n"
				+ "<customer id=\"cus\">\r\n"
				+ "    <online>\r\n"
				+ "        <name>고객1</name>\r\n"
				+ "        <age>28</age>\r\n"
				+ "        <gender>Male</gender>\r\n"
				+ "    </online>\r\n"
				+ "</customer>";
		InputStream is = new ByteArrayInputStream(sample.getBytes("UTF-8"));
				
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder build = factory.newDocumentBuilder();
		Document doc = build.parse(is);
		
		Element ele = doc.getDocumentElement();
		
		System.out.println(ele.getAttribute("id"));
	}

}
~~~

일단 문자열 안에 `"`를 써주기 위해서는 역슬래시`＼`를 붙여 반드시 `"＼"` 이 형태로 만들어줘야한다.

`sample` 변수에 xml 데이터를 할당해주었고, `InputStream`을 통해 데이터를 받았다.

`InputStream` 클래스의 주석을 살펴보면 다음과 같이 적혀있다.

~~~java
/**
 * This abstract class is the superclass of all classes representing
 * an input stream of bytes.
 *
 * <p> Applications that need to define a subclass of <code>InputStream</code>
 * must always provide a method that returns the next byte of input.
 *
 * @author  Arthur van Hoff
 * @see     java.io.BufferedInputStream
 * @see     java.io.ByteArrayInputStream
 * @see     java.io.DataInputStream
 * @see     java.io.FilterInputStream
 * @see     java.io.InputStream#read()
 * @see     java.io.OutputStream
 * @see     java.io.PushbackInputStream
 * @since   1.0
 */
~~~

이 클래스는 `input stream of bytes(데이터의 입력 통로)`를 대표하는 수퍼클래스라고 정의돼 있다.

여기서 `@see`안에 있는 클래스들은 이 것과 관련된 클래스들 같고, 여기서 `ByteArrayInputStream` 생성자를 통해 입력 받은 문자열을 InputStream 타입으로 만들어주었다.

뒤의 `build` 객체에서 사용하는 `parse` 메소드는 문자열 그대로를 매개변수로 받을 수 없고 `InputStream` 타입으로 받을 수 있기 때문이다. 여기서 캐릭터셋에 문제가 있을 경우 `getBytes()` 안에 `"UTF-8"`을 넘겨주면 된다.

그 후 `DocumentBuilderFactory`를 사용해 XML DOM 파서 생성용 인스턴스인 `factory`를 만들어 준다.

`DocumnetBuilder`는 DOM 문서용 인스턴스를 얻기 위한 클래스로 factory를 통해 생성한다.

쉽게 말하면 `factory`에서 DOM 문서 인스턴스를 얻어주는 `build`를 생성하고 `build`를 통해 XML데이터를 파싱해서 `doc`에 DOM형태의 데이터로 담아준다.

그렇게 얻은 `doc`에서 루트 요소를 가져온뒤 `id` 속성을 가져와 출력을 해보았다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/0348de9b-ab96-410b-9286-84a603a2aaf1)

콘솔에 제대로 출력된다.

### 2. 내부 XML 파일

이번엔 프로젝트 폴더 내부에 xml 예제 파일을 아래와 같이 구성하였다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a0b172f3-5ccf-4182-9361-04878be581b1)

이 후 코드는 위와 비슷하지만 `InputStream`을 통해 데이터를 가져오는 부분은 생략됐다.

~~~java
package test;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class Test {

	public static void main(String[] args) throws Exception {

		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder build = factory.newDocumentBuilder();
		Document doc = build.parse("./xmlsrc/sample.xml");
		
		Element ele = doc.getDocumentElement();
		
		System.out.println(ele.getAttribute("id"));
	}

}

~~~

### 3. 외부 URL

~~~java
package test;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class Test {

	public static void main(String[] args) throws Exception {
				
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder build = factory.newDocumentBuilder();
		Document doc = build.parse("http://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=108");
		
		Element ele = doc.getDocumentElement();
		
		System.out.println(ele.getAttribute("version"));
	}

}
~~~

외부 URI을 사용할 경우 그대로 위의 방법과 같고 파일 경로 대신에 URI를 넣어주면 된다.

여기서는 루트 요소의 `version` 속성의 속성값 `2.0`이 출력된다.

[예제 URI](https://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=108){: target="_blank"}

## End..

이번엔 단순히 XML을 Java에서 사용하기 위해 파싱하는 과정을 작성하고, 테스트를 위해 속성값을 출력해보았다.

하지만 이것은 시작에 불과할 뿐 내가 원하는 데이터로 조건을 걸어주고 정제를 하고 json으로 변환시키고 하기 위해선 추가 단계들이 남았다.
