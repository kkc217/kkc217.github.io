---
layout: post
title: 처음 배우는 리액트 네이티브 5장
subtitle: 할 일 관리 애플리케이션
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 5.1 프로젝트 준비하기

<span style="color:coral; line-height:0.8">기능</span>
  * `등록`: 할 일 항목을 추가
  * `수정`: 완료되지 않은 할 일 항목을 수정
  * `삭제`: 할 일 항목을 삭제
  * `완료`: 할 일 항목의 완료 상태를 관리
  
<br/>

```
expo init react-native-todo
cd react-native-todo
npm install styled-components prop-types
```

<br/>

<span style="color:coral; line-height:0.8">theme.js</span>

```javascript
export const theme = {
    background: '#101010',
    itemBackground: '#313131',
    main: '#778bdd',
    text: '#cfcfcf',
    done: '#616161',
};
```


<br/>

---


## 5.2 타이틀 만들기
  * 상태바 또는 노치 디자인에 의해 Title 컴포넌트의 일부가 가려지는 문제가 발생함.
  * `iOS`: 'SafeAreaView 컴포넌트'를 사용하면 자동으로 padding값이 적용됨.
  * `안드로이드`: 'StatusBar 컴포넌트'를 사용하면 상태 바의 색도 설정할 수 있음.<br/>('react-native'의 StatusBar를 import 해야 함.)
  
<img src="/assets/images/210818_ch05/statusBar_problem.PNG" style="width:580px; object-fit:contain">


<br/>

---


## 5.3 Input 컴포넌트 만들기

### 1. Dimensions
  * `Input 컴포넌트`가 화면에 꽉 차게 설정됨.
  * 다양한 모바일 기기에 대응해 현재 화면의 크기를 알 수 있는 기능들을 제공함.
  * `Dimensions`: 처음 값을 받아왔을 때의 크기로 고정되어 화면 회전 등의 크기 변화에 대응할 수 없음.
  * `useWindowDimensions`: Hooks 중 하나로, 화면의 크기가 변경되면 화면의 크기, 너비, 높이를 자동으로 업데이트함.
  * 'react-native'로부터 각각을 import 해야 함.

<img src="/assets/images/210818_ch05/Dimensions_problem.PNG" style="width:580px; object-fit:contain">


<br/>


### 2. Input 컴포넌트
  * placeholder 색을 타이틀과 같은 색으로 설정하고, 글자 수를 50자로 제한함.

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
//...
const StyledInput = styled.TextInput.attrs(({ theme }) => ({
    placeholderTextColor: theme.main,
}))`//...`;

const Input = ({ placeholder }) => {
    const width = Dimensions.get('window').width;

    return (
        <StyledInput width={width} placeholder={placeholder} maxLength={50} />
    );
};

export default Input;
```

<br/>

  * TextInput 컴포넌트의 기본값으로 `첫 글자가 대문자`로 나타나고 오타가 자동으로 `수정`되는 기능이 켜져 있음.
  * iOS의 경우, 키보드의 완료 버튼이 return으로 되어 있음.
  * 아이폰의 키보드 색상을 변경

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
//...
const Input = ({ placeholder }) => {
    const width = Dimensions.get('window').width;

    return (
        <StyledInput
            width={width}
            placeholder={placeholder}
            maxLength={50}
            autoCapitalize="none"
            autoCorrect={false}
            returnKeyType="done"
            keyboardAppearance="dark"
        />
    );
};
//...
```


<br/>


### 3. 이벤트
Input 컴포넌트에 입력되는 값을 사용할 수 있도록 이벤트를 등록.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React, { useState } from 'react';
//...

export default function App() {
    const [newTask, setNewTask] = useState('');

    const _addTask = () => {
        alert(`Add: ${newTask}`);
        setNewTask('');
    };

    const _handleTextChange = text => {
        setNewTask(text);
    };

    return (
        <ThemeProvider theme={theme}>
            <Container>
                <StatusBar
                    barStyle="light-content"
                    backgroundColor={theme.background}
                />
                <Title>TODO List</Title>
                <Input
                    placeholder="+ Add a Task"
                    value={newTask}
                    onChangeText={_handleTextChange}
                    onSubmitEditing={_addTask}
                    />
            </Container>
        </ThemeProvider>
    );
}
```

<br/>

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
//...
import PropTypes from 'prop-types';
//...

const Input = ({ placeholder, value, onChangeText, onSubmitEditing }) => {
    const width = Dimensions.get('window').width;

    return (
        <StyledInput
            //...
            value={value}
            onChangeText={onChangeText}
            onSubmitEditing={onSubmitEditing}
        />
    );
};

Input.PropTypes = {
    placeholder: PropTypes.string,
    value: PropTypes.string.isRequired,
    onChangeText: PropTypes.func.isRequired,
    onSubmitEditing: PropTypes.func.isRequired,
};

export default Input;
```

<img src="/assets/images/210818_ch05/Event_picture.PNG" style="width:280px; object-fit:contain">


<br/>

---


## 5.4 할 일 목록 만들기

### 1. 이미지 준비
Google Material Design에서 아이콘으로 사용할 이미지를 다운함.  
`Google Material Design Icon`: https://material.io/resources/icons

<img src="/assets/images/210818_ch05/icons_capture.PNG" style="width:200px; object-fit:contain">



















<br/>


---


### 참고  
* View vs. Fragment  
https://www.reddit.com/r/reactnative/comments/cjoz9g/fragment_vs_view_tag/

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---