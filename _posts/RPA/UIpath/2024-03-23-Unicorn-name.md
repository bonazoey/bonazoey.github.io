---
layout: post
title: "[UIpath] Get Unicorn name (Excel to Email)"
subtitle: 이 사람은 로봇일까요?.
author: Bonazoey
categories: RPA
banner:
  image: "./assets/images/planet.gif"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: gold"
tags: [RPA, UIpath]
top: false
date: 2024-03-23 09:00:00 +0900
---

## 개요

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/e5f7629f-c96a-491e-b646-2211f194f904)

엑셀 파일에 있는 `이름`, `탄생월`을 통해서 사이트에서 Unicorn name을 확인한 후 해당 Unicorn name을 메일로 전송하는 automatication을 구현할 것이다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/c9dbd3a9-7abd-422d-aec6-648a35a14c17)

자세한 과정이다.

## 프로젝트 진행

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/59ac67ca-eadf-4c9b-849d-01404daf9bb2)

빈 프로젝트를 생성한 후 `홈 > 도구 > UIpath Extension > Excel 추가 기능` 순으로 진행해준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/2b4243bb-5dba-4c91-8b8e-3be0d4c1fd80)

해당 기능이 설치가 안 돼있다면 쉽게 설치해주면 된다.

추가로 크롬 확장 기능이 설치가 안 돼있다면 이것 또한 설치하자.

### Get Unicorn Name

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/fb90afa3-72ff-4f31-b579-6a4c4eaf1d1e)

액티비티에서 `Use Application/Browswer`을 시퀀스에 추가하고

그 안에 `Use Excel File`을 추가한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/c14375a9-627d-4474-93ca-93af97c51f28)

그리고 주어진 `Use Application/Browswer` 액티비티 안의 **자동화할 애플리케이션 표시**를 누르면 자동을 브라우저 창으로 전환된다.

그 후 기존에 제공된 [사이트](https://www.rpasamples.com/findunicornname){: target="_blank"}를 클릭해준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a0405b6f-703b-40eb-ae08-3d30da30ee54)

성공적으로 선택됐다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/11211ce2-a3cf-4463-9178-6b7639800c3a)

그리고 또 기존에 제공된 해당 데이터가 들어있는 엑셀 파일도 `Use Excel File` 액티비티 안에 넣어주자

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a464f4e4-6af2-4cd3-9fa1-791de4148c2b)

그 후 `Type into` 액티비티를 `Use Excel File` 액티비티 안에 놓아준 후 **표시할 애플리케이션**을 눌러주면 창이 전환된다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/c4a79fa6-6ccd-4897-9d54-0918784a3b9c)

그리고 Unicorn name 사이트에서 텍스트 박스를 타켓, 그리고 질문에 앵커를 설정한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/28a18742-2dc8-4dff-9552-3f1e116e0279)

입력 창에 `Excel > Excel에서 표시`를 눌러주면 엑셀 창으로 전환된다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/52450ebe-7148-4a84-b914-1114ef6f9160)

설정할 데이터 셀을 누른 후 comfirm한다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/6618c947-a0fb-4e1b-b21b-87288c1e9510)

제대로 진행됐다면 `Select item` 액티비티를 그 다음 시퀀스로 놓아준 후 위와 동일하게 브라우저와 상응하는 엑셀 데이터를 설정해준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/5627600c-2f6e-47df-9162-6b8c9e5c0a0b)

그 후 `Click` 액티비티를 다음 단계로 설정하고 **Get Unicorn Name** 버튼을 타겟으로 설정한다.

여기까지 하게되면 자동으로 엑셀에 있는 데이터가 사이트 내에 입력이 되고 Unicorn Name을 얻는 과정이 진행된다.

#### Problem

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/237c111f-5046-4c16-9535-c64541965a1b)

그런데 문제가 생겼다. ***David***가 들어가야하는 칸에 시스템 설정인 한글로 ***ㅇㅁ퍙***이 들어가는 게 아닌가..

#### Solving

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/065644b0-af86-4cda-ae8a-b2dae1a244bd)

이 문제를 구글링해본 결과 해당 액티비티의 `속성 > 옵션 > 입력 모드 > Chromium API` 로 바꿔주면 간단히 해결된다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/63996e61-f5b9-41f2-9cdb-9ec492977931)

그 후 다음 액티비티로 `Get Text`를 놓아준 후 생성된 Unicorn Name에 타겟과 앵커를 적절히 걸어준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/b808a5a3-e547-41c0-9458-e5ee38a92945)

그리고 받은 데이터는 변수 사용을 눌러 **MyUnicornName**에 담아준다.

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/90229649-d351-4995-8d08-16dba3b3a8a0)

그럼 데이터 관리자에 새로 변수가 생긴 것이 보인다.

### Send to Email

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/8936f279-ff40-4682-9124-5d824e006d35)

이제 사용할 이메일 매체를 선택한다. 나는 `Use Gmail`을 선택했고 계정을 선택해준 후 이 액티비티 내에 `Send Email` 액티비티를 놓아준다.

그리고 수신인과 제목, 보낼 텍스트(MyUnicornName 변수)를 설정해준다.

초안으로 저장 체크박스는 해제하게 되면 이메일이 다이렉트로 전달되게 된다.

#### Problem

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/765c0bc3-0521-4276-98fa-820fffe6a8b5)

그런데 이메일을 보내는 과정에서 이러한 에러가 뜬다.

#### Solving

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/12ea2550-4b3a-4c44-9588-fc6b355025aa)

구글링 결과 에러에 나온 패키지가 없어서 나타난 것으로 패키지 관리에서 해당 패키지를 설치하면 된다.

#### Problem & Solving

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/845ca52b-19a7-4a8d-822d-07fc8a0afda7)

..는 무슨.. 새로운 에러가 또 떴다.

권한 관련 문제였으므로 Google Console API 사이트로 이동해서 사용하는 계정 로그인 후 Gmail 이용하는 작업이니 해당 권한을 부여해주면 된다.

(사실,, 프로젝트 실행할 때 구글 로그인 창이 뜨는데 그때 권한을 덜 부여해서인지,, 동시에 테스트해서 정확한 원인은 모르겠다)

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/434a5d5b-cd53-4e38-b89a-31af17e9ec0e)

메일이 정상적으로 온 것을 확인할 수 있다.

## 프로젝트 결과

![image](https://github.com/bonazoey/bonazoey.github.io/assets/142956374/a5641a87-f245-47ce-b76f-396ac8c0dbfc)

**Ta-Da!**
