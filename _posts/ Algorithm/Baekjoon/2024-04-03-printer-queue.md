---
layout: post
title: "[Baekjoon] 1966. 프린터 큐"
subtitle: 그냥 프린터 안 쓸게요..
author: Bonazoey
categories: Algorithm
banner:
  image: "./assets/images/Agrt.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Algorithm, Baekjoon]
date: 2024-04-03 09:00:00 +0900
top: false
---

## 문제

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/6e3ff348-031c-496d-956f-697ce4981fc6)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/93e52c37-5005-466d-8b46-05c4d58ce3f9)

[문제 링크](https://www.acmicpc.net/problem/1966){: target="_blank"}

## 접근 방법

프린트할 순서마다 우선순위도 함께 돌다가 max값에 만나면 우선순위와 프린터 순서 모두 지우는 방식으로 접근,

원하는 순서에 도달했을 시 max값 만날 시 증가시켰던 cnt를 반환

## 코드 작성

### Java

~~~java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Deque;
import java.util.List;
import java.util.Scanner;

public class Main {

	public static void main(String[] args) {

		Scanner sc = new Scanner(System.in);
		int T = sc.nextInt();
		sc.nextLine();

		for (int i = 0; i < T; i++) {

			String[] nums = sc.nextLine().split(" ");
			String[] tmp = sc.nextLine().split(" ");
			Deque<Integer> prior = new ArrayDeque<Integer>();
			Deque<Integer> orders = new ArrayDeque<Integer>();
			List<Integer> maxVal = new ArrayList<Integer>();
			int cnt = 1;

			int m = Integer.parseInt(nums[1]);
			for (int j = 0; j < Integer.parseInt(nums[0]); j++) {
				prior.add(Integer.parseInt(tmp[j]));
				maxVal.add(Integer.parseInt(tmp[j]));
				orders.add(j);
			}
			while (true) {
				Collections.sort(maxVal, Collections.reverseOrder());
				if (prior.getFirst() == maxVal.get(0)) {
					if (orders.getFirst() == m) {
						System.out.println(cnt);
						break;
					}
					prior.removeFirst();
					orders.removeFirst();
					maxVal.remove(0);
					cnt++;
				} else {
					prior.addLast(prior.pollFirst());
					orders.addLast(orders.pollFirst());
				}
			}
		}
		sc.close()
	}
}
~~~

> 코드 설명

일단 테스트 케이스의 종류 T값을 받아주고, 첫 째줄을 나눠서 배열 nums에, 둘 째줄을 나눠서 배열 tmp에 담아준다.

nums[0] 만큼 반복문을 돌아주는데, 이 때 목표하는 순서 값 nums[1]은 Integer로 형변환 한 후 m에 넣어준다.

그리고 tmp 배열은 deque 타입 prior와 ArrayList 타입 maxVal에 담아주는데, 찾아보니 Deque 클레스엔 max 값을 뽑는 메소드가 따로 존재하지 않는 것 같았다. 그래서 Collections.sort( *, Collections.reverseOrder())를 통해 내림차순 정렬 후 첫 값만 뽑으려 했으나.. 정렬도 먹히지 않는 것.

그래서 maxVal이라는 ArrayList 변수에 담아서 sort후 max를 뽑아주었다.

orders는 우선순위가 부여된 순서 목록이기 때문에 prior와 함께 순회할 예정이라 앞 뒤로 데이터 입출력이 가능한 Deque 타입으로 설정했다.

prior의 첫 값이 max 값이면서 타겟으로 하는 값이면 cnt 초기값 1을 바로 출력한다.

하지만 prior의 값이 max 값이긴 하지만 원하는 값이 아니라면 cnt를 1 증가시키고 prior와 orders에 첫 원소를 삭제한다.

만약 prior의 첫 값이 max 값이 아니라면 prior와 orders에서 첫 값으 뽑아서 뒤로 넣어준다.

반복문이 한 번 끝나고 다시 시작할 때마다 maxVal을 다시 정렬해주고 max값을 다시 뽑아준다.

### Python

~~~python
from collections import deque

T = int(input())

for _ in range(T):

    N, M = map(int, input().split())
    priority = deque(map(int, input().split()))
    idx = deque(range(0, N))
    cnt = 0

    while True:
        if priority[0] == max(priority):
            cnt += 1

            if idx[0] == M:
                print(cnt)
                break
            else:
                del priority[0]
                del idx[0]
        else:
            priority.rotate(-1)
            idx.rotate(-1)
~~~

> 코드 설명

위와 같지만 파이썬엔 입력받은 문자열을 split하면서 int로 바로 변환하여 변수에 담을 수 있었다.

또 Java에는 없는 rotate 메소드나 max값을 뽑는 기능이 있어서 추가 변수 할당이 필요 없기 때문에 코드가 굉장히 짧아 질 수 있었다.

## 회고

난이도가 조금 있는 문제였는데, 처음 풀었을 땐 아예 진행이 안 됐지만 다시 풀었을 땐 그래도 코드를 완성시키긴 했기에.. 성장함을 느낀다.. 나는.. 자란다..
