---
layout: post
title: 처음 배우는 리액트 네이티브 4장
subtitle: 스타일링
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 0 실습 준비하기
  ```
  expo init react-native-style
  ```
<br/>

### <span style="color:#cd853f">Plus.</span> <span style="color:#f7df1e">android studio 에뮬레이터 캡처</span>
  'ctrl + s'로 기기 화면만 캡처할 수 있음.
<img src="/assets/images/210816_ch04/emulator_capture.PNG" style="width:200px; object-fit:contain">


<br/>

---


## 4.1 스타일링
인라인 스타일을 적용하는 방법과 스타일시트에 정의된 스타일을 사용하는 방법이 있음.

### 1. 인라인 스타일링
  * HTML의 인라인 스타일링처럼 컴포넌트에 `직접` 스타일을 입력하는 방식
  * HTML은 `문자열` 형태로 스타일을 입력하지만, 리액트 네이티브에서는 `객체` 형태로 전달함.
  
```javascript
//...
const App = () => {
    <View
        style={{
            flex: 1,
            backgroundColor: '#fff',
            alignItems: 'center',
            justifyContent: 'center',
        }}
    >
    //...
}
//...
```


<br/>


### 2. 클래스 스타일링
  * 스타일시트에 스타일을 정의하고, 컴포넌트에서는 정의된 스타일의 이름으로 적용하는 방법
  * CSS 클래스를 이용하는 방법과 유사
  * 'react-native'의 StyleSheet를 import 해야 함.

```javascript
import React from 'react';
import { StyleSheet, View, Text } from 'react-native';

const App = () => {
    return (
        <View style={styles.container}>
            <Text style={styles.text}>Inline Styling - Text</Text>
        </View>
    );
};

const styles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#fff',
        alignItems: 'center',
        justifyContent: 'center',
    },
    text: {
        padding: 10,
        fontSize: 26,
        fontWeight: '600',
        color: 'black',
    },
});

export default App;
```


<br/>


### 3. 여러 개의 스타일 적용
  * `배열`을 이용해 style 속성에 여러 개의 스타일을 적용하면 됨.
  * 뒤에 오는 스타일이 앞에 있는 스타일을 덮음.
  * 인라인 스타일과 클래스 스타일 방식을 혼용해도 됨.
  
```javascript
//...
const App = () => {
    return (
        <View>
            <Text style={styles.text, { color: 'green' }}>Inline Styling - Text</Text>
            <Text style={[styles.text, styles.error]}>Inline Styling - Error</Text>
        </View>
    );
};
```


<br/>


### 4. 외부 스타일 이용
  * 외부 파일에 스타일을 정의하고 여러 개의 파일에서 스타일을 공통으로 사용할 수도 있음.
  * 각각의 스타일시트마다 export 해주고 불러올 때도 각각 불러옴.

<span style="color:coral; line-height:0.8">styles.js</span>

```javascript
import { StyleSheet } from 'react-native';

export const viewStyles = StyleSheet.create({
    container: {
        flex: 1,
        backgroundColor: '#fff',
    },
});

export const textStyles = StyleSheet.create({
    text: {
        padding: 10,
        fontSize: 26,
    },
    error: {
        fontWeight: '400',
        color: 'red',
    },
});
``` 
<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
import { viewStyles, textStyles } from './styles';

const App = () => {
    return (
        <View style={viewStyles.container}>
            <Text style={[textStyles.text, { color: 'green' }]}>
                Inline Styling - Text
            </Text>
            <Text style={[textStyles.text, textStyles.error]}>
                Inline Styling - Error
            </Text>
        </View>
    );
};

export default App;
```


<br/>

---


## 4.2 리액트 네이티브 스타일

### 1. flex와 범위
  * 화면의 범위를 정하는 속성에는 width와 height가 있음.
  * But, width와 height만 사요할 경우 디바이스에 따라 구성이 달라질 수 있음.
  * flex는 `비율`로 크기가 설정됨.<br/><span style="font-size:0.9em; opacity:0.6">0일 때: 설정된 width와 height값에 따라 크기가 결정됨.<br/>양수일 때: flex값에 비례하여 크기가 조정됨.</span>


<br/>


### 2. 정렬

<div style="font-size:1.2em; color:cornflowerblue;">flexDirection</div>

  * 컴포넌트는 항상 위에서 아래로 쌓이지만, `쌓이는 방향`을 원하는대로 변경할 수 있음.
  * 자기 자신이 쌓이는 방향이 아니라 `자식 컴포넌트`가 쌓이는 방향 (쌓이는 순서)
  
<span style="color:coral; line-height:0.8">flexDirection에 설정할 수 있는 값</span>
  * `column`: 세로 방향으로 정렬 (기본값)
  * `column-reverse`: 세로 방향 역순 정렬
  * `row`: 가로 방향으로 정렬
  * `row-reverse`: 가로 방향 역순 정렬

<img src="/assets/images/210816_ch04/flexDirection_picture.PNG" style="width:600px; object-fit:contain">


<br/>


<div style="font-size:1.2em; color:cornflowerblue;">justifyContent</div>

  * 쌓이는 순서는 flexDirection에서 설정한대로 하고, 컴포넌트 정렬 방법을 설정함.
  * flexDirection에서 결정한 방향과 `동일한` 방향으로 정렬함.






































