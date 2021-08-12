---
layout: post
title: 처음 배우는 리액트 네이티브 3장
subtitle: 컴포넌트
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 0 실습 준비하기
  ```
  expo init react-native-component
  ```


<br/>

---


## 3.1 JSX
  * JavaScript의 확장 기능으로 확장자명도 .js를 사용
  * 객체 생성과 함수 호출을 위한 문법적 편의를 제공하기 위해 만들어짐.
  <br/>

### 1. 하나의 부모
  * JSX에서는 여러 개의 요소를 표현할 경우 반드시 하나의 부모로 감싸야 함.<br/>
  
  <span style="color:coral">잘못된 예시</span>
  ```javascript
  export default function App() {
      return (
        <Text>Hello World!</Text>
        <StatusBar style="auto" />
      );
  }
  ``` 
  <br/><span style="color:coral">옳은 예시</span>
  ```javascript
  export default function App() {
      return (
        <View style={styles.container}>
            <Text>Hello World!</Text>
            <StatusBar style="auto" />
        </View>
      );
  }
  ```
  <br/>

  <span style="color:#f7df1e">View</span>  
  * UI를 구성하는 가장 기본적인 요소
  * \<div>와 비슷한 역할
  * 'react-native'의 View를 import해야 함.
  
  <span style="color:#f7df1e">Fragment</span>  
  * View와 달리 styling이나 layout을 설정하는 목적이 없이 단지 구분하기 위한 목적으로만 사용
  * 'react'의 Fragment를 import해야 함.
  

  <span style="color:#f7df1e">Fragment 단축 문법</span>
  * 따로 import하지 않고 사용할 수 있음.
  ```javascript
  export default function App() {
    return (
      <>
        <Text>Hello world!</Text>
        <StatusBar style="auto" />
      </>
    );
  }
  ```


<br/>


### 1. 하나의 부모

<br/>



---


### 참고  
* View vs. Fragment
https://www.reddit.com/r/reactnative/comments/cjoz9g/fragment_vs_view_tag/

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---