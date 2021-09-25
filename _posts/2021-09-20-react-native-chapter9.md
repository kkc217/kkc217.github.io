---
layout: post
title: 처음 배우는 리액트 네이티브 9장
subtitle: 채팅 애플리케이션
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---


## 9.1 프로젝트 준비

```
expo init react-native-simple-chat
```

### 1. 내비게이션

채팅 애플리케이션에서 구현할 부분은 크게 세 가지이다.
> 1. 로그인 등 `인증`하는 화면
> 2. 채팅방의 `목록` 등을 확인할 수 있는 화면
> 3. 메시지를 주고받는 화면

<br/>

  * 리액트 내비게이션 설치.

```
npm install @react-navigation/native
```

<br/>

  * 리액트 내비게이션 라이브러리 사용에 필요한 추가 라이브러리 설치.

```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

<br/>

  * 스택 내비게이션과 탭 내비게이션 라이브러리 설치.

```
npm install @react-navigation/stack @react-navigation/bottom-tabs
```


<br/>


### 2. 라이브러리

스타일 작성을 위한 `스타일드 컴포넌트 라이브러리`와 타입 확인을 위한 `prop-types 라이브러리`를 설치.

```
npm install styled-components prop-types
```

<br/>

<span style="color:coral; line-height:0.9">! 아래의 라이브러리들은 각각이 필요할 때에 설치하여 사용할 예정</span>

<span style="color:#f7df1e">expo-image-picker 라이브러리</span>

  * 기기의 `사진`이나 `영상`을 가져올 수 있도록 시스템 UI에 접근할 수 있는 기능을 제공함.<br/>→ 기기의 사진을 선택해 사용자의 사진을 설정 또는 변경하기 위해 사용할 예정

<br/>

<span style="color:#f7df1e">moment 라이브러리</span>

  * `시간`을 다양한 형태로 변경하는 등 시간과 관련된 많은 기능을 제공.
  * 날짜와 관련된 라이브러리 중 가장 널리 알려져 있고 많이 사용되고 있음.<br/>→ 타임스탬프를 사용자가 보기 편한 형태로 변경하기 위해 사용할 예정

<br/>

<span style="color:#f7df1e">react-native-keyboard-aware-scroll-view 라이브러리</span>

  * 키보드가 화면을 가리면서 생기는 불편한 점을 해결하기 위해 사용되는 라이브러리

<br/>

<span style="color:#f7df1e">react-native-gifted-chat 라이브러리</span>

  * 메시지를 주고받는 `채팅 화면`을 쉽게 구현할 수 있도록 돕는 라이브러리


<br/>

---


## 9.2 파이어베이스

  * `인증`, `데이터베이스` 등의 다양한 기능을 제공하는 개발 플랫폼
  * 파이어베이스에서 제공하는 기능들로 서비스에 필요한 `서버`와 `데이터베이스`를 직접 구축하지 않아도 개발이 가능함.
  * [파이어베이스 콘솔](https://console.firebase.google.com/)에서 새 프로젝트를 만들고 firebase.json 파일과 .gitignore 파일을 수정해줌.

<span style="color:coral; line-height:0.8">firebase.json</span>

```
{
    "apiKey": "...",
    "authDomain": "...",
    "databaseURL": "...",
    "projectId": "...",
    "storageBucket": "...",
    "messageingSenderId": "...",
    "appId": "..."
}
```

<br/>

<span style="color:coral; line-height:0.8">.gitignore</span>

```
node_modules/
.expo/
npm-debug.*
*.jks
*.p8
*.p12
*.key
*.mobileprovision
*.orig.*
web-build/

# macOS
.DS_Store

# firebase
firebase.json
```

<br/>

### 1. 인증

이메일과 비밀번호를 이용해 인증할 수 있는 기능을 만들 것이므로 `이메일/비밀번호` 부분만 환성화함.

<img src="/assets/images/210920_ch09/authentication.PNG" style="width:600px; object-fit:contain">


<br/>


### 2. 데이터베이스

`파이어스토어`(Firestore Database)와 `실시간 데이터베이스`(Realtime Database)를 사용할 수 있는데 파이어스토어를 사용함.

<img src="/assets/images/210920_ch09/database.PNG" style="width:600px; object-fit:contain">


<br/>


### 3. 스토리지

  * 서버 코드 없이 사용자의 사진, 동영상 등을 저장할 수 있는 기능을 쉽게 개발할 수 있도록 기능을 제공함.
  * 채팅 애플리케이션에 가입한 사용자의 사진을 저장하고 가져오는 기능을 구현할 예정


<br/>


### 4. 라이브러리 설치

  * 리액트 네이티브에서 파이어베이스를 사용하기 위해서는 라이브러리 설치가 필요함.
  * utils 폴더에 firebase.js 파일을 생성.

```
npm install firebase
```

<br/>

<span style="color:coral; line-height:0.8">utils/firebase.js</span>

```javascript
import * as firebase from 'firebase';
import config from '../../firebase.json';

const app = firebase.initializeApp(config);
```


<br/>

---


## 9.3 앱 아이콘과 로딩 화면

로딩 화면과 앱의 아이콘을 변경.

  * 사용할 이미지와 폰트를 미리 불러와서 사용할 수 있도록 `cacheImages`와 `cacheFonts` 함수를 작성하고 이를 이용해 `_loadAssets` 함수를 구성함.<br/>→ 사용하는 환경에 따라 이미지나 폰트가 느리게 적용되는 문제를 개선할 수 있음.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React, { useState } from 'react';
import { StatusBar, Image } from 'react-native';
import AppLoading from 'expo-app-loading';
import { Asset } from 'expo-asset';
import * as Font from 'expo-font';
import { ThemeProvider } from 'styled-components/native';
import { theme } from './theme';

const cacheImages = images => {
    return images.map(image => {
        if (typeof image === 'string') {
            return Image.prefetch(image);
        } else {
            return Asset.fromModule(image).downloadAsync();
        }
    });
};

const cacheFonts = fonts => {
    return fonts.map(fort => Font.loadAsync(font));
};

const App = () => {
    const [isReady, setIsReady] = useState(false);

    const _loadAssets = async () => {
        const imageAssets = cacheImages([require('../assets/splash.png')]);
        const fontAssets = chacheFonts([]);

        await Promise.all([...imageAssets, ...fontAssets]);
    }

    return isReady ? (
        <ThemeProvider theme={theme}>
            <StatusBar barStyle="dark-content" />
        </ThemeProvider>
    ) : (
        <AppLoading
            startAsync={_loadAssets}
            onFinish={() => setIsReady(true)}
            onError={console.warn}
        />
    );
};

export default App;
```

<br/>

  * `로딩 화면`에서 기기의 크기에 따라 주변에 흰색바타잉 보이는 것을 방지하기 위해 로딩 화면의 배경색을 로딩 화면 이미지의 배경색과 동일하게 변경.

<span style="color:coral; line-height:0.8">app.json</span>

```javascript
{
  "expo": {
    //...
    "icon": "./assets/icon.png",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#3679fe"
    },
    //...
  }
}

```


<br/>

---


## 9.4 인증 화면

  * 파이어베이스의 인증 기능을 이용해 `로그인 화면`과 `회원가입 화면`을 만듦.
  * 인증을 위해 이메일과 비밀번호가 필요하므로 로그인 및 회원가입 화면에서 `이메일`과 `비밀번호`를 필수로 입력받음.
  * 회원가입 시 서비스에서 사용할 `이름`과 `프로필 사진`도 받도록 화면을 구성.

### 1. 내비게이션

  * 회원가입 화면으로 이동할 수 있는 버튼이 있는 로그인 화면을 만듦.

<span style="color:coral; line-height:0.8">screens/Login.js</span>

```javascript
import  React from 'react';
import styled from 'styled-components/native';
import { Text, Button } from 'react-native';

const Container = styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
    background-color: ${({ theme }) => theme.background};
`;

const Login = ({ navigation }) => {
    return (
        <Container>
            <Text style={{ fontSize: 30 }}>Login Screen</Text>
            <Button title="Signup" onPress={() => navigation.navigate('Signup')} />
        </Container>
    );
};

export default Login;
```

<br/>

  * 화면을 확인할 수 있는  텍스트가 있는 회원가입 화면을 만듦.

<span style="color:coral; line-height:0.8">screens/Signup.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import { Text } from 'react-native';

const Container = styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
    background-color: ${({ theme }) => theme.background};
`;

const Signup = () => {
    return (
        <Container>
            <text style={{ fontSize: 30 }}>Signup Screen</text>
        </Container>
    );
};

export default Signup;
```

<br/>

<span style="color:coral; line-height:0.8">screens/Index.js</span>

```javascript
import Login from './Login';
import Signup from './Signup';

export { Login, Signup };
```

<br/>

  * 화면 준비는 끝났으니 내비게이션 파일을 작성함.
  * 첫 화면을 로그인 화면으로 하고, 회원가입 화면도 가진 내비게이션을 만듦.
  * `ThemeContext`와 `useContext Hook` 함수를 이용해 theme을 받아와 내비게이션 화면의 배경을 theme에 정의된 배경색과 동일하게 바꿈.
  * 안드로이드와 iOS에서의 `타이틀 위치`를 동일하게 하기 위해 `headerTitleAlign`의 갑승ㄹ center로 설정.

<span style="color:coral; line-height:0.8">navigations/AuthStack.js</span>

```javascript
import React, { useContext } from 'react';
import { ThemeContext } from 'styled-components/native';
import { createStackNavigator } from '@react-navigation/stack';
import { Login, Singup } from '../screens';

const Stack = createStackNavigator();

const AuthStack = () => {
    const theme = useContext(ThemeContext);
    return (
        <Stack.Navigator
            initialRouterName="Login"
            screenOptions={
                headerTitleAlign: 'center',
                cardStyle: { backgroundColor: theme.backgroundColor },
            }
        >
            <Stack.Screen name="Login" component={Login} />
            <Stack.Screen name="Signup" component={Signup} />
        </Stack.Navigator>
    );
};

export default AuthStack;
```

<br/>

<span style="color:coral; line-height:0.8">navigations/Index.js</span>

```javascript
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import AuthStack from './AuthStack';

const Navigation = () => {
    return (
        <NavigationContainer>
            <AuthStack />
        </NavigationContainer>
    );
};

export default Navigation;
```

<br/>

<span style="color:coral; line-height:0.8">navigations/App.js</span>

```javascript
//...
import Navigation from './navigations';
//...
const App = () => {
    //...
    return isReady ? (
        <ThemeProvider theme={theme}>
            <StatusBar barStyle="dark-content" />
            <Navigation />
        </ThemeProvider>
    ) : (
        //...
    );
};
//...
```

<img src="/assets/images/210920_ch09/authentication_navigation.PNG" style="width:500px; object-fit:contain">


<br/>


### <span style="color:#cd853f">Plus.</span> AppLoading Error

  * 책에 쓰여진대로 AppLoading 컴포넌트를 사용할 경우 아래 사진과 같이 오류가 발생함.
  * expo SDK 40부터 AppLoading이 기본 컴포넌트에서 빠져서 생기는 오류.

```javascript
import { AppLoading } from 'expo';
```

<img src="/assets/images/210920_ch09/AppLoading_error.PNG" style="width:250px; object-fit:contain">

<br/>

<span style="color:coral">해결 방법</span> → 따로 패키지를 다운받아서 사용해야 함.

```
expo install expo-app-loading
```

```javascript
import AppLoading from 'expo-app-loading'
```


<br/>


### 2. 로그인 화면

  * 이메일과 비밀번호를 입력받을 화면을 만듦.
  * `로고`를 렌더링하는 컴포넌트, 사용자의 `입력`을 받는 컴포넌트, `클릭`과 `이벤트`가 발생하는 컴포넌트가 필요.

#### <span style="font-size:1.2em; color:cornflowerblue;">- Image 컴포넌트</span>

  * url을 전달받아 원격에 있는 이미지를 렌더링함.
  * 로그인 화면에서는 Image 컴포넌트를 이용해 앱의 로고를 렌더링함.
  * `theme.js`파일에 Image 컴포넌트의 배경색을 정의함.

<span style="color:coral; line-height:0.8">theme.js</span>

```javascript
//...
export const theme = {
    background: colors.white,
    text: colors.black,

    imageBackground: colors.grey_0,
};

```

<br/>

<span style="color:coral; line-height:0.8">components/Image.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import PropTypes from 'prop-types';

const Container = styled.View`
    align-self: center;
    margin-bottom: 30px;
`;

const StyledImage = styled.Image`
    background-color: ${({ theme }) => theme.imageBackground};
    width: 100px;
    height: 100px;
`;

const Image = ({ url, imageStyle }) => {
    return (
        <Container>
            <StyledImage source={{ uri: url }} style={imageStyle} />
        </Container>
    );
};

Image.propTypes = {
    uri: PropTypes.string,
    imageStyle: PropTypes.object,
};

export default Image;
```

<br/>

<span style="color:coral; line-height:0.8">components/index.js</span>

```javascript
import Image from './Image';

export { Image };
```

<br/>

<span style="color:coral; line-height:0.8">screens/Login.js</span>

```javascript
//...
const Login = ({ navigation }) => {
    return (
        <Container>
            <Image />
            <Button title="Signup" onPress={() => navigation.navigate('Signup')} />
        </Container>
    );
};
//...
```


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 로고 적용하기</span>

  * 로고 이미지를 파이어베이스 스토리지에 업로드해 불러오는 방식으로 사용.

  <span style="color:coral; font-size:0.8em; font-weight:bold">! 이미지 주소의 쿼리 스트링에서 token 부분은 제외하고 사용해야 함.

<span style="color:coral; line-height:0.8">utils/images.js</span>

```javascript
const prefix =
    'https://firebasestorage.googleapis.com/v0/b/react-native-simple-chat-75301.appspot.com/o';

export const images = {
    logo: `${prefix}/logo.png?alt=media`,
};
```

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
import { images } from './utils/images';
//...
const App = () => {
    const [isReady, setIsReady] = useState(false);

    const _loadAssets = async () => {
        const imageAssets = cacheImages([
            require('../assets/splash.png'),
            ...Object.values(images),
        ]);
        //...
    };
    //...
};
//...
```

<br/>

<span style="color:coral; line-height:0.8">screens/Login.js</span>

```javascript
//...
import { images } from '../utils/images';
//...
const Login = ({ navigation }) => {
    return (
        <Container>
            <Image url={images.logo} imageStyle={{ borderRadius: 8 }} />
            <Button title="Signup" onPress={() => navigation.navigate('Signup')} />
        </Container>
    );
};
//...
```

<img src="/assets/images/210920_ch09/login_logo.PNG" style="width:250px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- Input 컴포넌트</span>

아이디와 비밀번호를 `입력`받을 수 있도록 함.

<span style="color:coral; line-height:0.8">theme.js</span>

```javascript
//...
export const theme = {
    background: colors.white,
    text: colors.black,

    imageBackground: colors.grey_0,
    label: colors.grey_1,
    inputPlaceholder: colors.grey_1,
    imputBorder: colors.grey_1,
};
```

<br/>

<span style="color:coral; line-height:0.8">comoponents/Input.js</span>

```javascript
import React, { useState, forwardRef } from 'react';
import styled from 'styled-components/native';
import PropTypes from 'prop-types';

const Container = styled.View`
    flex-direction: column;
    width: 100%;
    margin: 10px 0;
`;

const Label = styled.Text`
    font-size: 14px;
    font-weight: 600;
    margin-bottom: 6px;
    color: ${({ theme, isFocused }) => (isFocused ? theme.text : theme.label)};
`;

const StyledTextInput = styled.TextInput.attrs(({ theme }) => ({
    placeholderTextColor: theme.inputPlaceholder,
}))`
    background-color: ${({ theme }) => theme.background};
    color: ${({ theme}) => theme.text};
    padding: 20px 10px;
    font-size: 16px;
    border: 1px solid
        ${({ theme, isFocused }) => (isFocused ? theme.text : theme.inputBorder)};
    border-radius: 4px;
`;

const Input = forwardRef(
    (
        {
            label,
            value,
            onChangeText,
            onSubmitEditing,
            onBlur,
            placeholder,
            isPassword,
            returnKeyType,
            maxLength,
        },
        ref
    ) => {
    const [isFocused, setIsFocused] = useState(false);

    return (
        <Container>
            <Label isFocused={isFocused}>{label}</Label>
            <StyledTextInput
                ref={ref}
                isFocused={isFocused}
                value={value}
                onChangeText={onChangeText}
                onSubmitEditing={onSubmitEditing}
                onFocus={() => setIsFocused(true)}
                onBlur={() => {
                    setIsFocused(false);
                    onBlur();
                }}
                placeholder={placeholder}
                secureTextEntry={isPassword}
                returnKeyType={returnKeyType}
                maxLength={maxLength}
                autoCapitalize="none"
                autoCorrect={false}
                textContentType="none" // iOS only
                underlineColorAndroid="transparent" // Android only
            />
        </Container>
        );
    }
);

Input.defaultProps = {
    onBlur: () => {},
};

Input.propTypes = {
    label: PropTypes.string.isRequired,
    value: PropTypes.string.isRequired,
    onChangeText: PropTypes.func.isRequired,
    onSubmitEditing: PropTypes.func.isRequired,
    onBlur: PropTypes.func,
    placeholder: PropTypes.string,
    isPassword: PropTypes.bool,
    returnKeyType: PropTypes.oneOf(['done', 'next']),
    maxLength: PropTypes.number,
};

export default Input;
```

<br/>

<span style="color:coral; line-height:0.8">comoponents/index.js</span>

```javascript
import Image from './Image';
import Input from './Input';

export { Image, Input };
```

<br/>

<span style="color:coral; line-height:0.8">screens/Login.js</span>

```javascript
import React, { useState, useRef } from 'react';
//...
const Login = ({ navigation }) => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const passwordRef = useRef();

    return (
        <Container>
            <Image url={images.logo} imageStyle={{ borderRadius: 8 }} />
            <Input
                label="Email"
                value={email}
                onChangeText={text => setEmail(text)}
                onSubmitEditing={() => passwordRef.current.focus()}
                placeholder="Email"
                returnKeyType="next"
            />
            <Input
                ref={passwordRef}
                //...
            />
        </Container>
    );
};
//...
```































































  

<br/>


---


## 참고  
* <span style="opacity:0.5">AppLoading Error</span>  
[https://sinawi.tistory.com/350](https://sinawi.tistory.com/350)


<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---