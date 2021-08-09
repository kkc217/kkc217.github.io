---
layout: post
title: 처음 배우는 리액트 네이티브 1장
subtitle: 리액트 네이티브란?
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## React Native
  사용자 인터페이스를 만드는 리액트에 기반을 두고 제작됨.  
  But, 리액트와 달리 iOS와 안드로이드에서 동작하는 네이티브 <span style="color:coral">모바일 애플리케이션</span>을 만드는 **자바스크립트 프레임워크**



### 장점
  * 작성된 코드 대부분 플랫폼 간 공유가 가능해서 두 플랫폼(iOS, 안드로이드)을 동시에 개발할 수 있음.  
  * 변경된 코드를 저장하기만 해도 자동으로 변경된 내용이 적용된 화면을 확인할 수 있는 패스트 리프레쉬 기능을 제공함.
  * 코드바나 아이오닉은 웹뷰를 이용해 렌더링하는 방식으로 성능이 떨어진다는 단점이 있음.
    But, 리액트 네이티브는 각 플랫폼에서 그에 맞는 네이티브 앨리먼트로 전환되기에 큰 성능 저하 없이 개발이 가능함.

### 단점
  * 안드로이드나 iOS에서 업데이트를 통해 새로운 API를 제공하더라고 리액트 네이티브가 이를 지원하기까지 시간이 걸림.
  * 유지보수가 어려움.
  * 업데이트가 너무 잦음.


<br/>

---


## 동작 방식

### 1. 브릿지 <span style="font-size:15px">bridge</span>
  <span style="color:coral">자바스크립트</span> 코드를 이용해 네이티브 계층과 통신할 수 있도록 연결하는 역할을 함.

  - <span style="font-size:17px">JavaScript Thread</span>  
      <span style="opacity:0.5">자바스크립트 코드가 실행되는 곳 (보통 리액트 코드)</span>
  - <span style="font-size:17px">Main Thread</span>  
      <span style="opacity:0.5">UI를 담당.</span>
  - <span style="font-size:17px">Shadow Thread</span>  
      <span style="opacity:0.5">레이아웃을 계산하는 데 사용하는 백그라운드 스레드</span>
  - <span style="font-size:17px">Native Modult</span>  
      <span style="opacity:0.5">각 모듈마다 자체 스레드가 있음.</span>  

<kbd style="color:coral">&#8594;</kbd> 기기와 통신하는 모든 자바스크립트의 기능을 분리된 스레드로 처리하여 성능이 향상됨.


### 2. 가상 DOM  
  가상 DOM이 존재하여 데이터에 변화가 있는 경우 가상 DOM과 실제 DOM을 비교하여 차이점이 있는 부분만 실제 DOM에 적용함.

  
### 3. JSX <span style="font-size:15px">JavaScript XML</span>
  자바스크립트 코드보다 가독성이 더 뛰어나며, 오류 검사에도 장점이 있음.


<br/><br/>
---
---