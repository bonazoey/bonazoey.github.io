---
layout: post
title: "[Leetcode] 49. Group Anagrams"
subtitle: 우린 모두 흙으로 돌아간다.
author: Bonazoey
categories: Algorithm
banner:
  image: "./assets/images/Agrt.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [Algorithm, Leetcode]
date: 2024-04-04 10:00:00 +0900
top: false
---

## 1. 문제

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/7aa15230-29bd-4ecb-bbb7-144ee543125c)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/ea5c80e3-2897-4a70-8bb4-b4b46e23c515)

[문제 링크](https://leetcode.com/problems/group-anagrams/description/){: target="_blank"}

문자열을 정렬시켰을 때 같은 값이 나오는 것들 끼리 그룹으로 묶는다.

## 2. 접근 방법

2차원 배열을 만들고 `strs` 있는 원소들을 하나씩 정렬해가며, 정렬한 원소가 만들어놓은 2차원 배열안에 같은 원소가 있을 경우 하나하나 비교해가며 넣어주면 어떨까?

물론 strs의 길이가 짧다면 가능하겠지만 제약조건에 보면 최대 10⁴개 들어가고 한 원소의 길이도 100까지나 가능하다..

시간복잡도상 시간초과가 무조건 나는 접근 방법이다.

그럼 어떻게 해야할까..

### 2-2. 결론

고민 끝에 검색을 해봤는데 `딕셔너리`[^dic]를 활용하여 문제를 해결하면 된다. 시간복잡도 관련 문제는 거의 key, value 쌍의 딕셔너리를 사용하는 것 같다.

## 3. 코드 작성

### ● Python

> 작성 코드

~~~python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ana = defaultdict(list)

        for i in strs:
            ana["".join(sorted(i))].append(i)

        return ana.values()
~~~

> 코드 설명

일단 먼저 반복문을 통해 뽑아낸 원소에 sort를 해주는데 이때 `*.sort()`가 아닌 `sorted(*)`를 해준다.

.sort()는 리스트의 메소드이고, sorted()는 내장 함수이다. 둘의 차이는 간단하게 말하자면 전자는 **적용 값을 리턴하지 않고** 후자는 **원본은 유지하고 적용 값을 리턴**한다는 것이다.

따라서 반복문 안에서 `print(i.sort())`를 했을 때 `None` 값이 출력되는 걸 볼 수 있다. 하지만 원본인 i를 출력하면 정렬되어있다. 우리는 출력값을 바로 사용할 예정이므로 sorted()를 사용해 정렬해준다.

그리고 정렬된 문자열의 값이 key인 곳에 value를 넣어준다. 이때 정렬된 i는 `['a', 'e', 't']` 형식으로 리스트로 출력되기 때문에 `"".join()`을 통해 문자열로 합쳐준다.

이렇게 하면 정렬시 같은 값의 문자열을 가진 원소는 해당 key 해당하는 곳에 value로 들어가게 된다.

또, 딕셔너리는 `defaultdict`을 사용해주었는데, 데이터 타입에 맞는 알맞은 디폴트값을 설정해주는 것이다. 예를 들면 만약 아무 값이 없다면 `int = 0`, `string = ""`, 그리고 여기에 해당되는 list 는 `[]`를 출력한다.

마지막으로 value 값만 출력하면 끝!

### ● Java

> 작성 코드

~~~java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

		Map<String, List<String>> ana = new HashMap<String, List<String>>();

		
		for (String i : strs) {
			
			char[] arr = i.toCharArray();
			Arrays.sort(arr);
			String str = new String(arr);
			List<String> list = new ArrayList<String>();
			
			if (ana.get(str) == null) {
				list.add(i);
				ana.put(str, list);
			} else {
				list = ana.get(str);
				list.add(i);
				ana.put(str, list);
			}
		}
		List<List<String>> answer = new ArrayList<List<String>>(ana.values());
		return answer;
    }
}
~~~

> 코드 설명

파이썬의 딕셔너리와 비슷한 기능을 하는 `HashMap`[^map]을 사용하였고 문자열 정렬은 `char[]` 변수에 문자열을 `toCharArray()`로 하나씩 나누어 넣고 `Arrays.sort()`로 정렬해주었다. `Arrays.sort()`는 리스트에 대한 정렬만 가능하기 때문에 문자열을 char[]로 나누어준 것이다. 그렇게 정렬된 배열을 `str` 변수를 생성하면서 넣어주어 문자열로 다시 변환해주었다.

그리고 HashMap의 value 값에 들어갈 `list` 변수를 생성하였고 key가 `str`일 때 value의 유무에 따라 조건문을 걸어주었다. 그 뒤는 파이썬의 과정과 같다.

마지막으로 리턴 타입이 `List<List<String>>`이므로 현재 `ana.values()`의 데이터 타입은 Collections<List<String>>`이기 때문에 return할 answer를 생성해줄때 ana.values()를 변환해주었다.

## 회고

파이썬의 딕셔너리, 자바의 맵 개념을 모른다면 시간복잡도의 폭발적인 증가로 인해 풀 수 없는 문제였다.

난이도가 적지는 않았으며, 이또한 역시 파이썬이 더 간결하고 자바가 신경 쓸 게 더 많았다.

___

[^dic]: Dictionary data type : key, value 쌍으로 이루어진 자료형이다. 예를 들면 {key: value, 문자: 한글, 숫자: 1}

[^map]: HashMap data type : 자바에도 Dictionary 타입이 있긴하지만 메소드가 제한적이어서, 보통 같은 기능을 하는 HashMap을 사용하는듯.
