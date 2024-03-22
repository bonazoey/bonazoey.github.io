---
layout: post
title: "[Leetcode] 561. Array Partition"
subtitle: 이 사람은 로봇입니다.
author: Bonazoey
categories: Alogorithm
banner:
  image: "./assets/images/coding-robot.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [RPA, UIpath]
top: 1
---

## 문제

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a9c840fb-747e-495f-92cc-deaabd5c5329)

[문제 링크](https://leetcode.com/problems/array-partition/)

주어진 배열의 원소 들을 두 개씩 쌍을 이루어 그 쌍 중 최소값들의 합이 최대값이 되는 알고리즘

## 접근 방법

선택된 값들의 합이 최대가 되기 위해선.. 가장 큰 최소값들이 선택이 되어야 한다.

그렇다면.. 정렬을 한 뒤 홀수 값만 뽑으면 되지 않을까???

몇 가지 테스트를 해보자

### 테스트

> `[1, 2, 3, 4]` 라는 배열이 있을 경우

* 오름/내림차순 정렬로 나눌 시

`(1, 2)`, `(3, 4)` 일 때 1과 3이므로 합은 **4**

* 쌍들의 원소의 합의 차가 최소일 때로 나눌 시

`(1, 3)`, `(2, 4)` 일 때 1과 2이므로 합은 **3**

* 쌍의 원소의 차가 최대일 때로 나눌 시

`(1, 4), (2, 3)` 일 때 1과 2이므로 합은 **3**

### 결론

테스트 결과 오름/내림차순 일 때 두 원소씩 쌍을 이루면 그 쌍 중 최소값의 합이 최대가 된다는 결과가 나왔다.

그럼 코드를 작성해보자

## 코드 작성

### Python

> 주어진 코드

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/c4ba8169-3f31-48e2-8152-b5ed05c24196)

> 작성 코드

~~~python
class Solution:
    def arrayPairSum(self, nums: List[int]) -> int:
        sum = 0
        nums.sort()
        for i in range(0, len(nums), 2):
            sum += nums[i]

        return sum
~~~

> 코드 설명

주어진 리스트 nums를 `.sort()`를 통해 정렬해준다.

그 후 반복문을 통해서 0번 째 index부터 len(nums)(리스트의 마지막)까지 값을 뽑아서 sum 변수에 더해주는데, 여기서 반복문 안의 변수 i는 index `2만큼 hop`하게 출력했다.

그렇게 되면 정렬된 리스트 안의 홀수번 쨰(두 수가 쌍을 이룰 시 최소값) 값들의 합이 출력된다.

### Java

> 주어진 코드

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/73888bf0-abdf-41cb-b0a0-9008541903c6)

> 작성 코드

~~~java
class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        int sum = 0;
        for(int i = 0; i < nums.length/2; i++) {
            sum += nums[2*i];
        }
        return sum;
    }
}
~~~

> 코드 설명

주어진 리스트 nums를 내장 클래스 `Arrays`의 `sort 메소드`로 정렬해준다.

그 후 반복문을 통해서 0번 째 index부터 nums 리스트의 절반만큼 반복문을 실행해준다. 이 때 반복문 변수 i는 1씩 증가한다.

여기서 ***파이썬과 다르게*** sum 변수에 `nums[2*i]`번 째 값을 더해줌으로써 2만큼 hop해주었다.

## 회고

조금만 생각해보면 풀 수 있는 간단한 문제였던 것 같다.
