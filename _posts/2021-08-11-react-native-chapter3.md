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
  
  <span style="color:coral; line-height:0.8">잘못된 예시</span>
  ```javascript
  export default function App() {
      return (
        <Text>Hello World!</Text>
        <StatusBar style="auto" />
      );
  }
  ``` 
  <br/><span style="color:coral; line-height:0.6">옳은 예시</span>
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

  <span style="color:#f7df1e; font-weight:bold; font-size:1.1em; font-height:0.5">View</span>
  * UI를 구성하는 가장 기본적인 요소
  * \<div>와 비슷한 역할
  * 'react-native'의 View를 import해야 함.  
  <br/>

  <span style="color:#f7df1e; font-weight:bold; font-size:1.1em; font-height:0.5">Fragment</span>
  * View와 달리 styling이나 layout을 설정하는 목적이 없이 단지 구분하기 위한 목적으로만 사용
  * 'react'의 Fragment를 import해야 함.  
  <br/>

  <span style="color:#f7df1e; font-weight:bold; font-size:1.1em; font-height:0.5">Fragment 단축 문법</span>
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


### 2. 자바스크립트 변수
  JSX 내부에서는 자바스크립트 변수를 전달해 사용할 수 있음.
```javascript
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { Text, View } from 'react-native';

export default function App() {
  const name = "BeomCheol";
  return (
    <View>
      <Text>My name is {name}</Text>
      <StatusBar style="auto" />
    </View>
  );
}
```


<br/>


### 3. 자바스크립트 조건문
자바스크립트의 조건문을 이용할 수 있지만, 복잡한 조건인 경우 `JSX 밖에서` 조건에 따른 값을 설정하고 `JSX 내에서` 사용하는 조건문에서는 최대한 간단하게 작성하는 것이 깔끔함.  
<br/>

<div style="font-size:1.2em; color:cornflowerblue;">if 조건문</div>
즉시실행함수 형태로 작성해야 함.

```javascript
export default function App() {
  const name = 'BeomCheol';
  return (
    <View>
      <Text>
        {(() => {
          if (name === 'Hanbit') return 'My name is Hanbit';
          else if (name === 'BeomCheol') return 'My name is BeomCheol';
          else return 'My name is React Native';
        })()}
      </Text>
    <StatusBar style="auto" />
    </View>
  );
}
```

<br/>

<div style="font-size:1.2em; color:cornflowerblue;">삼항 연산자</div>

```javascript
export default function App() {
  const name = 'BeomCheol';
  return (
    <View>
      <Text>
        My name is {name === 'BeomCheol' ? 'BeomCheol Kim' : 'React Native'}
      </Text>
    <StatusBar style="auto" />
    </View>
  );
}
```

<br/>

<div style="font-size:1.2em; color:cornflowerblue;">AND 연산자와 OR 연산자</div>

  * AND 연산자: 앞의 조건이 참일 때 뒤의 내용이 나타나고, 거짓인 경우 나타나지 않음.
  * OR 연산자: `앞의 조건이 거짓인 경우` 내용이 나타나고, 참인 경우 나타나지 않음.

```javascript
export default function App() {
  const name = 'BeomCheol';
  return (
    <View>
      {name === 'BeomCheol' && (
        <Text>My name is BeomCheol</Text>
      )}
      {name !== 'BeomCheol' && (
        <Text>My name is no BeomCheol</Text>
      )}
    <StatusBar style="auto" />
    </View>
  );
}
```

<br/>

<div style="font-size:1.2em; color:cornflowerblue;">null과 undefined</div>

  null은 허용하지만, undefined는 `오류가 발생`함.

<br/>

<div style="font-size:1.2em; color:cornflowerblue;">주석</div>

  JSX의 주석은 {/* \*/}를 사용해야하지만, 태그 안에서는 자바스크립트처럼 //나 /* \*/를 사용할 수 있음.

<br/>

<div style="font-size:1.2em; color:cornflowerblue;">스타일링</div>

  * HTML의 style과는 달리 문자열로 입력하는 거시 아니라 객체 형태로 입력해야 함.
  * 카멜 표기법(camel case)으로 작성해야 함.
  
```javascript
export default function App() {
  return (
    <View
      style={{
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center',
      }}
    >
      <Text>Hi! React Native!</Text>
    </View>
  );
}
```


<br/>

---


## 3.2 컴포넌트



<br/>



---


### 참고  
* View vs. Fragment  
https://www.reddit.com/r/reactnative/comments/cjoz9g/fragment_vs_view_tag/

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---