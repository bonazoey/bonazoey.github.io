---
layout: post
title: "[Baekjoon] 9012. 괄호"
subtitle: 웃음 - 안 웃음 = 0
author: Bonazoey
categories: Algorithm
banner:
  image: "./assets/images/Agrt.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Algorithm, Baekjoon]
date: 2024-04-02 09:00:00 +0900
top: false
---

## 문제

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a4fe264a-005e-4a06-b96e-5a2971709c8b)

[문제 링크](https://www.acmicpc.net/problem/9012){: target="_blank"}

VPS 일까 아닐까 찾는 문제

## 접근 방법

`(`와 `)`가 만나는 순간, 그러니까 문자열 내에서 `()`가 있다면 없애주는 걸 반복하면 되지 않을까?

그러다가 더이상 `()`가 남지 않았을 때 문자열 길이를 확인하고 0보다 크다면 `NO`, 작다면 `YES`를 출력하면 될 것 같다.

## 코드 작성

### Java

~~~java
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {
		int T;
		String text;
		Scanner sc = new Scanner(System.in);
		T = sc.nextInt();
		sc.nextLine();

		for (int i = 0; i < T; i++) {
			text = sc.nextLine();
			while (text.contains("()")) {
				text = text.replace("()", "");
			}
			if (text.length() > 0) {
				System.out.println("NO");
			} else {
				System.out.println("YES");
			}
		}
		sc.close();
	}
}

~~~

> 코드 설명

먼저 T값을 받고 T값 만큼 반복문을 돌아준다.

반복문을 돌면서 text를 입력 받는데 while문을 돌면서 만약 입력 받은 text에 "()"이 존재한다면 "()"를 빈 문자열로 대체하고 다시 text에 넣어준다. 그걸 반복하다가 더이상 바꿀 "()"가 존재하지 않을 시 길이를 체크 후 `YES, NO`를 출력 후 break로 빠져나온다.

### Python

~~~python
T = int(input())

for _ in range(T):
    text = str(input())

    while '()' in text:
        text = text.replace('()', '')

    if len(text) > 0:
        print('NO')
    else:
        print('YES')
~~~

> 코드 설명

위와 같다.

## 회고

복잡하지 않고 간단한 쉬운 문제!
