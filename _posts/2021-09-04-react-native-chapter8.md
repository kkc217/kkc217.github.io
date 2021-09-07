---
layout: post
title: 처음 배우는 리액트 네이티브 8장
subtitle: 내비게이션
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 8.0 실습 준비하기
  ```
  expo init react-native-navigation
  npm install styled-components
  ```


<br/>

---


## 8.1 리액트 내비게이션

  * 리액트 내비게이션 라이브러리에는 리액트 네이티브 앱의 내비게이션을 관리할 수 있도록하는 기능들이 있음.
  * 지원하는 내비게이션: 스택 내비게이션, 탭 내비게이션, 드로어 내비게이션


### 1. 내비게이션의 구조
리액트 내비게이션은 Screen 컴포넌트, Navigator 컴포넌트, NavigationContainer 컴포넌트로 세 가지가 있음.

<span style="color:#027cd4; line-height:1.15; font-weight:bold">Screen 컴포넌트</span>

  * `화면`으로 사용되는 컴포넌트로 name과 component 속성을 지정해야 함.
  * `name`: 화면 이름
  * `component`: 화면으로 사용될 컴포넌트를 전달
  * 화면으로 사용되는 컴포넌트에는 항상 `navigation`과 `route`가 props로 전달됨.

<br/>

<span style="color:#027cd4; line-height:1.15; font-weight:bold">Navigator 컴포넌트</span>

  * 화면을 관리하는 `중간 관리자 역할`
  * 내비게이션을 구성하며, 여러 개의 Screen 컴포넌트를 자식 컴포넌트로 가짐.

<br/>

<span style="color:#027cd4; line-height:1.15; font-weight:bold">NavigationContainer 컴포넌트</span>

  * 내비게이션의 `계층 구조`와 `상태`를 관리함.
  * 모든 내비게이션 구성 요소를 감싼 `최상위 컴포넌트`여야 함.


<br/>


### 2. 설정 우선 순위

  * 리액트 내비게이션에서 설정할 수 있는 속성을 수정하는 방법은 크게 세 가지가 있음.<br/>1. `Navigator 컴포넌트`의 속성으로 수정하는 방법<br/>2. `Screen 컴포넌트`의 속성으로 수정하는 방법<br/>3. 화면으로 사용되는 컴포넌트의 props로 전달되는 `navigation`을 이용해서 수정하는 방법`
  * 2번과 3번은 `해당 화면`에만 적용되지만, 1번은 `자식 컴포넌트`로 존해하는 `모든` 컴포넌트에 적용됨.
  * `3번 → 2번 → 1번` 순으로 우선순위가 높음.<br/><span style="color:coral; font-size:0.9em">! 작은 범위의 설정일수록 우선순위가 높다.</span>


<br/>


### 3. 폴더 구조와 라이브러리 설치

  * src폴더 아래에 `navigations` 폴더와 `screens` 폴더를 생성.

<img src="/assets/images/210904_ch08/ch08_directory_setting.PNG" style="width:180px; object-fit:contain">

<br/>

  * 리액트 내비게이션 라이브러리 설치
  
```
npm install --save @react-navigation/native
```

<br/>

 * 종속성 설치 → 대부분의 내비게이션에 반드시 필요함.

```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

<span style="color:coral; font-weight:bold">! 리액트 내비게이션은 각 기능별로 모듈이 분리되어 있어, 사용하는 내비게이션의 종류에 따라 개별적으로 추가 라이브러리를 설치해야 함.</span>







































<br/>


---


## 참고  
* <span style="opacity:0.5">비동기</span>  
https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/

* <span style="opacity:0.5">async &. await</span>  
https://joshua1988.github.io/web-development/javascript/js-async-await/#async--await%EB%8A%94-%EB%AD%94%EA%B0%80%EC%9A%94

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---