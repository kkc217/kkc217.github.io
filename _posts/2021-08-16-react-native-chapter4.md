---
layout: post
title: 처음 배우는 리액트 네이티브 4장
subtitle: 스타일링
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 4.0 실습 준비하기
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

<span style="color:coral; line-height:0.8">justifyContent에 설정할 수 있는 값</span>
  * `flex-start`: 시작점에서부터 정렬 (기본값)
  * `flex-end`: 끝에서부터 정렬
  * `center`: 중앙 정렬
  * `space-between`: 컴포넌트 사이의 공간을 동일하게 만들어서 정렬
  * `space-around`: 컴포넌트 각각의 주변 공간을 동일하게 만들어서 정렬
  * `space-evenly`: 컴포넌트 사이와 양 끝에 동일한 공간을 만들어서 정렬

<img src="/assets/images/210816_ch04/justifyContent_and_alignItems.PNG" style="width:600px; object-fit:contain"><br/>
<img src="/assets/images/210816_ch04/justifyContent_picture.PNG" style="width:600px; object-fit:contain"><br/>


<br/>


<div style="font-size:1.2em; color:cornflowerblue;">alignItems</div>
  * flexDirection에서 정한 방향과 `수직`

<span style="color:coral; line-height:0.8">alignItems에 설정할 수 있는 값</span>
  * `flex-start`: 시작점에서부터 정렬 (기본값)
  * `flex-end`: 끝에서부터 정렬
  * `center`: 중앙 정렬
  * `stretch`: alignItems의 방향으로 컴포넌트 확장
  * `baseline`: 컴포넌트 내부의 텍스트 베이스라인을 기준으로 정렬

<img src="/assets/images/210816_ch04/alignItems_picture.PNG" style="width:600px; object-fit:contain"><br/>


<br/>


<div style="font-size:1.2em; color:cornflowerblue;">그림자</div>

  * `shadowColor`: 그림자 색 설정
  * `shadowOffset`: width와 height값을 지정해 그림자 거리 설정
  * `shadowOpacity`: 그림자의 불투명도 설정
  * `shadowRadius`: 그림자의 흐림 반경 설정  
  * 이 속성들은 `iOS에만` 적용되는 속성으로, 안드로이드에서는 `elevation`이라는 속성을 사용해야 함.


<br/>

---


## 4.3 스타일드 컴포넌트
  * 자바스크립트 파일 안에 스타일을 작성하는 `CSS-in-JS` 라이브러리
  * CSS에서 사용하는 속성들을 그대로 사용할 수 있음.
  
### 0. 스타일드 컴포넌트 설치
```
npm install styled-components
```


<br/>


### 1. 스타일드 컴포넌트 사용법

  * `"styled.[컴포넌트 이름]"` 형태 뒤에 벡틱(`)을 사용해 만든 문자열을 붙이고, 그 안에 스타일을 지정하면 됨.
  * styled 뒤에 작성하는 컴포넌트의 이름은 `반드시 존재`하는 컴포넌트를 지정해야 함.
  * `css`를 이용해 재사용 가능한 코드를 관리할 수 있음.
  * 'styled-components/native'의 styled를 import 해야 함.

```javascript
import styled. { css } from 'styled-components/native';

const whiteText = css`
    color: #fff;
    font-size: 14px;
`;
const MyBoldTextComponent = styled.Text`
    ${whiteText}
    font-weight: 600;
`;
const MyLightTextComponent = styled.Text`
    ${whiteText}
    font-weight: 200;
`;
```

<br/>

  * 완성된 컴포넌트를 `상속`받아 이용할 수도 있음.
  * 기존의 형태와 달리 `"styled(컴포넌트 이름)"` 형태로 소괄호로 감싸야 함.

```javascript
import styled from 'styled-components/native';

const StyledText - styled.Text`
    color: #000;
    font-size: 20px;
`;

const ErrorText = styled(StyledText)`
    font-weight: 600;
    color: red;
`;
```


<br/>


### 2. 스타일 적용하기
스타일드 컴포넌트를 사용하면 역할에 맞게 스타일에 이름을 지정할 수 있다는 장점이 있음.

<span style="color:coral; line-height:0.8">Button.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';

const ButtonContainer = styled.TouchableOpacity`
    background-color: #9b59b6;
    border-radius: 15px;
    padding: 15px 40px;
    margin: 10px 0px;
    justify-content: center;
`;
const Title = styled.Text`
    font-size: 20px;
    font-weight: 600;
    color: #fff;
`;

const Button = props => { 
    return (
        <ButtonContainer>
            <Title>{props.title}</Title>
        </ButtonContainer>
    );
};

export default Button;
``` 

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import Button from './components/Button';

const Container = styled.View`
    flex: 1;
    background-color: #ffffff;
    align-items: center;
    justify-content: center;
`;

const App = () => {
    return (
        <Container>
            <Button title="Hanbit" />
            <Button title="React Native" />
        </Container>
    );
};

export default App;
```


<br/>


### 3. props 사용하기
같은 파일 내에서, props로 값을 전달하면 백틱 안에서 props에 접근할 수 있음.

<span style="color:coral; line-height:0.8">Button.js</span>

```javascript
//...
const ButtonContainer = styled.TouchableOpacity`
    background-color: ${props =>
        props.title === 'Hanbit' ? '#3498db' : '#9b59b6'};
    border-radius: 15px;
    padding: 15px 40px;
    margin: 10px 0px;
    justify-content: center;
`;
//...

const Button = props => {
    return (
        <ButtonContainer title={props.title}>
            <Title>{props.title}</Title>
        </ButtonContainer>
    );
};
/...
```

<span style="color:#f7df1e; font-height:0.5">\<ButtonContainer title={props.title}></span>  
`{props.title}`은 Button 컴포넌트를 사용할 때 전달받는 props의 값이고, <br/>`ButtonContainer title=`의 title은 ButtonContainer에 넘겨주는 props의 값이다.


<br/>


### 4. attrs 사용하기
  * 스타일드 컴포넌트에서 사용할 수 있는 기능 중 하나
  * 다른 파일로부터 값을 받아 스타일드 컴포넌트에서 속성을 설정할 때 사용할 수 있음.

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';

const StyledInput = styled.TextInput.attrs(props => ({
    placeholder: 'Enter a text...',
    placeholderTextColor: props.borderColor,
}))`
    width: 200px;
    height: 60px;
    margin: 5px;
    padding 10px;
    border-radius: 10px;
    border: 2px;
    border-color: ${props => props.borderColor};
    font-size: 24px;
`;

const Input = props => {
    return <StyledInput borderColor={props.borderColor} />;
};

export default Input;
```

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import Button from './components/Button';
import Input from './components/Input';

const Container = styled.View`
    flex: 1;
    background-color: #ffffff;
    align-items: center;
    justify-content: center;
`;

const App = () => {
    return (
        <Container>
            <Button title="Hanbit" />
            <Button title="React Native" />
            <Input borderColor="#3498db" />
            <Input borderColor="#9b59b6" />
        </Container>
    );
};

export default App;
```

<img src="/assets/images/210816_ch04/styledComponents_attrs.PNG" style="width:280px; object-fit:contain">


<br/>


### 5. ThemeProvider

  * Context API를 활용해 애플리케이션 전체에서 스타일드 컴포넌트를 이용할 때 `미리 정의`한 값들을 사용할 수 있도록 props로 전달함.
  * 'styled-components/native'의 ThemeProvider를 import 해야 함.

<span style="color:coral; line-height:0.8">theme.js</span>

```javascript
export const lightTheme = {
    background: '#ffffff',
    text: '#ffffff',
    purple: '#9b59b6',
    blue: '#3498db',
};

export const darkTheme = {
    background: '#34495e',
    text: '#34495e',
    purple: '#9b59b6',
    blue: '#3498db',
};
```

<span style="color:coral; line-height:0.8">Button.js</span>

```javascript
//...
const ButtonContainer = styled.TouchableOpacity`
    background-color: ${props =>
        props.title === 'Hanbit' ? props.theme.blue : props.theme.purple};
    //...
`;
//...
const Title = styled.Text`
    font-size: 20px;
    font-weight: 600;
    color: ${props => props.theme.text};
`;
//...
```

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React, { useState } from 'react';
import { Switch } from 'react-native';
import styled, { ThemeProvider } from 'styled-components/native';
import Button from './components/Button';
import Input from './components/Input';
import { lightTheme, darkTheme } from './theme';

const Container = styled.View`
    flex: 1;
    background-color: ${props => props.theme.background};
    align-items: center;
    justify-content: center;
`;

const App = () => {
    const [isDark, setIsDark] = useState(false);
    const _toggleSwitch = () => setIsDark(!isDark);

    return (
        <ThemeProvider theme={isDark ? darkTheme : lightTheme}>
            <Container>
                <Switch value={isDark} onValueChange={_toggleSwitch} />
                <Button title="Hanbit" />
                <Button title="React Native" />
                <Input borderColor="#3498db" />
                <Input borderColor="#9b59b6" />
            </Container>
        </ThemeProvider>
    );
};

export default App;
```

<span style="color:#f7df1e; font-height:0.5"><ThemeProvider theme={isDark ? darkTheme : lightTheme}></span>  
ThemeProvider를 활용해 theme를 지정하면, 미리 정의해둔 색을 `하위 컴포넌트`에서 사용할 수 있음.

<span style="color:#f7df1e; font-height:0.5">background-color: ${props => props.theme.background};</span>  
뒤에 App return하는 부분에서 ThemeProvider에 의해 정의된 theme가 props.theme에 해당됨.
<br/>

<img src="/assets/images/210816_ch04/styledComponents_ThemeProvider.PNG" style="width:600px; object-fit:contain">



<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---