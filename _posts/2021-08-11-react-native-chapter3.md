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

  * HTML의 style과는 달리 문자열로 입력하는 것이 아니라 객체 형태로 입력해야 함.
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
  단순히 UI 역할만 하는 것이 아니라 `부모로부터 받은 속성`이나 `자신의 상태`에 따라 표형이 달라지고 다양한 기능을 수행함.
  <br/>

### 1. 내장 컴포넌트

<div style="font-size:1.2em; color:cornflowerblue;">Button 컴포넌트</div>  
  
  * 'react-native'로부터 Button을 import해야 함.
  * `title 속성` : 버튼 내부에 출력되는 텍스트
  * `onPress 속성` : 버튼의 눌렸을 때 호출되는 함수를 지정.
  * `color 속성` : iOS에서는 텍스트 색, 안드로이드에서는 버튼의 바탕색을 나타냄.

```javascript
import React from 'react';
import { Text, View, Button } from 'react-native';

const App = () => {
  return (
    <View
      style={{
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center',
      }}
    >
      <Text style={{ fontSize: 30, marginBottom: 10 }}>Button Component</Text>
      <Button title="Button" onPress={() => alert('Click!')} />
    </View>
  );
};

export default App;
```

<img src="/assets/images/210811_ch03/button_component.PNG" style="width:600px; object-fit:contain">


<br/>


### 2. 커스텀 컴포넌트

<div style="font-size:1.2em; color:cornflowerblue;">MyButton 컴포넌트</div>  

  * Button 컴포넌트는 iOS와 안드로이드에서 다른 모습으로 렌더링된다는 단점을 보완.
  * TouchableOpacity 컴포넌트와 Text 컴포넌트를 사용하여 만듦.
  * TouchableOpacity 컴포넌트에는 `onPress` 속성이 없지만, `TouchableWithoutFeedback 컴포넌트를 상속`받아 onPress 속성을 지정하고 사용할 수 있음.
  
<span style="color:coral; line-height:0.8">MyButton.js</span>

```javascript
import React from 'react';
import { TouchableOpacity, Text } from 'react-native';

const MyButton = () => {
  return (
    <TouchableOpacity
      style={{
        backgroundColor: '#3498db',
        padding: 16,
        margin: 10,
        borderRadius: 8,
      }}
      onPress={() => alert('Click~')}
    >
      <Text style={{ color: 'white', fontSize: 24 }}>My Button</Text>
    </TouchableOpacity>
  );
};

export default MyButton;
``` 

<img src="/assets/images/210811_ch03/my_button_component.PNG" style="width:600px; object-fit:contain">
<br/>


### <span style="color:#cd853f">Plus.</span> eject 명령어 <span style="font-size:11px; opacity:0.6">내보내기</span>
  Expo는 개발자의 생산성을 높여준다는 장점이 있지만, 이로 인해 일부 기능을 사용하는데 제한이 있어 `CLI 프로젝트로 변경`해야 하는 상황이 발생하는데 이 때 사용하는 명령어가 eject이다.  
  <span style="font-size:0.9em; opacity:0.7">! Expo는 JavaScript로 앱을 작성하고, eject는 이를 React Native 코드로 바꿔준다.</span>

  * <span style="color:coral; font-weight:bold">주의</span> 변경된 CLI 프로젝트는 다시 Expo 프로젝트로 돌아올 수 없다.
  * 이와 관련된 내용은 뒷부분이나 따로 빼서 다루도록 하겠다.


<br/>

---


## 3.3 props와 state

### 1. props <span style="font-size:15px">properties</span>
  * 부모 컴포넌트로부터 전달된 속성값 or `상속`받은 속성값
  * 자식 컴포넌트에서는 부모로부터 받은 props를 사용할 수 있지만, `변경은 불가능`함.
  * 변경이 필요하면 props를 설정 및 전달한 부모 컴포넌트에서 변경해야 함.
<br/>

<div style="font-size:1.2em; color:cornflowerblue;">props 전달하고 사용하기</div>  

  * MyButton 컴포넌트에서 title 속성을 지정하면 MyButton 컴포넌트의 props로 title이 전달됨.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
  <MyButton title="Button" />
//...
```  
<br/>

<span style="color:coral; line-height:0.8">MyButton.js</span>

```javascript
//...
  const MyButton = props => {
    return (
      <TouchableOpacity
      //...
      >
        <Text style={{ color: 'white', fontSize: 24 }}>{props.title}</Text>
      </TouchableOpacity>
      );
  };
//...
```  
<br/>


  * 컴포넌트의 태그 사이에 값을 입력해서 전달하는 방법도 있음.
  * props에 children으로 전달됨.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
  <MyButton title="Button" />
  <MyButton title="Button">Children Props</MyButton>
//...
```  
<br/>

<span style="color:coral; line-height:0.8">MyButton.js</span>

```javascript
//...
const MyButton = props => {
  return (
    <TouchableOpacity
      //...
    >
      <Text style={{ color: 'white', fontSize: 24 }}>
        {props.children || props.title}
      </Text>
    </TouchableOpacity>
  );
};
//...
```

<img src="/assets/images/210811_ch03/props_children.PNG" style="width:280px; object-fit:contain">


<br/>


<div style="font-size:1.2em; color:cornflowerblue;">defaultProps</div>  

  * props에 아무것도 넣지 않으면 내용 없이 출력되거나 예상과 다른 결과가 나올 수 있음.
  
<span style="color:coral; line-height:0.8">MyButton.js</span>

```javascript
//...
const MyButton = props => {/*...*/};

MyButton.defaultProps = {
  title: 'Button',
};

export default MyButton;
```  


<br/>


<div style="font-size:1.2em; color:cornflowerblue;">propTypes</div>  

  * 잘못된 props가 전달되었으면 경고 메시지를 출력함.
  * 'Required'여도 defaultProps로 기본값이 설정되어 있다면 값이 없어도 됨.
  * 'prop-types'에서 PropTypes를 import해야 함.

<span style="color:coral; line-height:0.8">prop-types 라이브러리 설치하기</span>

```
npm install prop-types
```

<br/>

<span style="color:coral; line-height:0.8">MyButton.js</span>

```javascript
//...
MyButton.propTypes = {
  title: PropTypes.string.isRequired,
  onPress: PropTypes.func.isRequired,
};
//...
```










<br/>



---


### 참고  
* View vs. Fragment  
https://www.reddit.com/r/reactnative/comments/cjoz9g/fragment_vs_view_tag/

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---