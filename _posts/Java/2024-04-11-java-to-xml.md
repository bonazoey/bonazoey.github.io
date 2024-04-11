---
layout: post
title: "[Java] Java to XML"
subtitle: 의좋은 형제
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
date: 2024-04-11 09:00:00 +0900
---

저번 포스팅에선 XML 문서를 XML DOM으로 파싱하여 Java로 변환하는 과정을 보았다.

이번엔 반대로 Java에서 XML로 파싱하는 방법을 다룰 것이다.

저번엔 get 메소드를 알아보았다면 이번엔 create, append, remove, 등의 메소드를 알아볼 것이다.

## 기본 메소드

기본적으로 사용하는 메소드들이 있고 이외에도 많은 메소드들이 존재한다.

| 메소드 | 설명 |
| :---: | :--- |
| createElement(String tagName) | Element 객체를 생성 |
| createAttribute(String name) | Att 객체를 생성 |
| createTextNode(data) | Text 객체를 생성 |
| createCDATASection(data) | CDATASection 객체를 생성 |
| appendChild(Node newChild) | newChild를 자식 노드의 끝에 추가 |
| removeChild(Node oldChild) | oldChild를 자식 노드에서 삭제 |
| insertBefore(Node newChild, Node oldChild) | oldChild 앞에 newChild를 삽입 |
| replaceChild(Node newChild, Node oldChild) | oldChild를 newChild로 교체 |

## to XML

아래는 기본적으로 [여기](https://bonazoey.github.io/java/2024/04/09/xml-to-java.html){: target="_blank"}에서 설명했던 것과 같이, 빌더 인스턴스를 생성하고 `doc` Document 데이터 타입까지 만드는 과정이다.

~~~java
package test;

import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileOutputStream;
import java.nio.charset.StandardCharsets;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.OutputKeys;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;

import org.w3c.dom.Document;
import org.w3c.dom.Element;

public class Java2XML {

	public static void main(String[] args) throws Exception {
		
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder builder = factory.newDocumentBuilder();
		
		Document doc = builder.newDocument();
	}
}
~~~

### 1. 데이터 생성 및 삽입

아래는 생성한 `doc`에 원하는 XML 데이터를 삽입하는 과정이다.

~~~java
// 루트 요소 생성
Element ele = doc.createElement("class");
ele.setAttribute("value", "학급");

// 자식 요소 생성
Element teacher = doc.createElement("teacher");
teacher.setAttribute("name", "김복남");
teacher.setTextContent("1반 담임입니다.");

Element teacher2 = doc.createElement("teacher");
teacher2.setAttribute("name", "박봉팔");
teacher2.setTextContent("2반 담임입니다.");

Element teacher3 = doc.createElement("teacher");
teacher3.setAttribute("name", "이창필");
teacher3.setTextContent("3반 담임입니다.");

Element teacher4 = doc.createElement("teacher");
teacher4.setAttribute("name", "류정숙");
teacher4.setTextContent("4반 담임입니다.");

// 루트 요소 추가
doc.appendChild(ele);
// 자식 요소 추가
ele.appendChild(teacher);
// 한 요소가 중복 추가되지 않는다
ele.appendChild(teacher);
ele.appendChild(teacher2);
ele.appendChild(teacher3);
ele.appendChild(teacher4);
// 요소 삭제
ele.removeChild(teacher3);
// 요소 대체, 대체 시 새로 바뀌는 요소가 이미 존재한다면 그 요소는 없어진다
ele.replaceChild(teacher2, teacher);
~~~

여기서 주의할 것이, 한 번 들어간 요소는 중복되어 들어가지 않는다. `teacher` 데이터를 `appendChild`를 통해 두 번 삽입 한다고 해도 실제로 출력해보면 하나만 존재한다.

`replaceChild`도 마찬가지다. 다른 요소로 대체 시 이미 해당 요소가 존재할 때, 중복 변수가 삽입이 불가능하므로 기존에 있던 데이터가 없어진다.

### 2. XML 데이터 출력

~~~java
ByteArrayOutputStream os = new ByteArrayOutputStream();

DOMSource src = new DOMSource(doc);
StreamResult rs = new StreamResult(os);

TransformerFactory tsFactory = TransformerFactory.newInstance();
Transformer ts = tsFactory.newTransformer();

// 인코딩
ts.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
// 들여쓰기
ts.setOutputProperty(OutputKeys.INDENT, "yes");
ts.transform(src, rs);

System.out.println(new String(os.toByteArray(), StandardCharsets.UTF_8));
~~~

데이터를 편집한 `doc` 변수를 DOM 트리 형식으로 변환해준다.

`TransformerFactory`는 `DocumentBuilderFactory`와 반대의 역할을 한다. 후자는 XML에서 자바로 파싱하는데 사용하였다면, 전자는 XML로 변환하기 위해 사용한다.

`StreammResult`는 데이터가 스트림을 통과 후 나오는 결과에 대한 정보들을 정의할 수 있는데, 여기서는 `ByteArrayOutputStream` 데이터 타입으로 넣어줬고 프린트 시 `toByteArray()`를 사용하여 문자로 출력된다.

아래는 출력 화면이다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/9b4630d5-8f32-49af-9693-fc3af42a0a09)

### 3. XML 파일 생성

~~~java
ByteArrayOutputStream os = new ByteArrayOutputStream();

DOMSource src = new DOMSource(doc);

TransformerFactory tsFactory = TransformerFactory.newInstance();
Transformer ts = tsFactory.newTransformer();

FileOutputStream fo = new FileOutputStream(new File("./xmlsrc/output.xml"));
StreamResult rs = new StreamResult(fo);

ts.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
ts.setOutputProperty(OutputKeys.INDENT, "yes");
ts.transform(src, rs);

fo.close();
~~~

위와 동일하나 변환 정보인 `rs`를 단순 문자열에서 원하는 경로에 해당 데이터로 XML 문서를 작성하는 것으로 바꾸어주었다.

실행 후 `package explorer`를 refresh 하면 아래와 같이 XML 문서가 작성된 것을 볼 수 있다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/479d76b4-e445-489e-943d-8d67dc5353c1)

아래는 생성된 XML 문서이다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/b23e3d9d-b4c8-4170-a22a-cdd94ca326e0)
