---
layout: post
title: 처음 배우는 리액트 네이티브 7장
subtitle: Context API
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 7.0 실습 준비하기
  ```
  expo init react-native-context
  npm install styled-components
  ```


<br/>

---


## 7.1 전역 상태 관리
  * 리액트 네이티브 앱에서 데이터는 부모 컴포넌트에서 자식 컴포넌트로 전달됨.<br/>But, 최상위 컴포넌트로부터 여러 자식 컴포넌트를 거친 컴포넌트에 props로 전달하려면 비효율적이며 관리가 어려움.
  * Context API를 이용하면 필요한 컴포넌트에서 데이터를 바로 받아올 수 있음.


<br/>


## 7.2 Context API

   * `createContext` 함수를 이용해 Context를 생성할 수 있음.
   * createContext의 파라미터로 Context의 기본값을 지정할 수 있음.
   * 'react'의 createContext를 import 해야 함.

<span style="color:coral; line-height:0.8">contexts/User.js</span>

```javascript
import { createContext } from 'react';

const UserContext = createContext({ name: 'BeomCheol Kim'});

export default UserContext;
```

### 1. Consumer

  * Context의 내용을 읽고 사용하게 해줌.
  * 상위 컴포넌트 중 `가장 가까운` 곳에 있는 Provider 컴포넌트가 절달한 데이터를 이용함.
  * 상위 컴포넌트에 Provider가 없다면 createContext로 전달한 `기본값`을 사용함.

<span style="color:coral; line-height:0.8">components/User.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import UserContext from '../contexts/User';

const StyledText = styled.Text`
    font-size: 24px;
    margin: 10px;
`;

const User = () => {
    return (
        <UserContext.Consumer>
            {value => <StyledText>Name: {value.name}</StyledText>}
        </UserContext.Consumer>
    );
};

export default User;
```

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import User from './components/User';

const Container = styled.View`
    flex: 1;
    background-color: #ffffff;
    justify-content: center;
    align-items: center;
`;

const App = () => {
    return (
        <Container>
            <User />
        </Container>
    );
};

export default App;
```

<img src="/assets/images/210823_ch07/Consumer.PNG" style="width:200px; object-fit:contain">


<br/>


### 2. Provider

  * 하위 컴포넌트에 Context의 변화를 알리는 역할을 함.
  * value를 받아서 `모든 하위 컴포넌트`에 전달하고, 하위 컴포넌트는 Provider 컴포넌트의 value가 변경될 때마다 `다시 렌더링`됨.
  * value를 전달받는 하위 컴포넌트의 수는 제한이 없음.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
import UserContext from './contexts/User';
//...
const App = () => {
    return (
        <UserContext.Provider value={{ name: 'BeomCheol '}}>
            <Container>
                <User />
            </Container>
        </UserContext.Provider>
    );
};
//...
```

<br/>

<span style="color:coral; line-height:0.8">components/User.js</span>

```javascript
//...
const User = () => {
    return (
        <UserContext.Provider value={{ name: 'React Native' }}>
            <UserContext.Consumer>
                {value => <StyledText>Name: {value.name}</StyledText>}
            </UserContext.Consumer>
        </UserContext.Provider>
    );
};
//...
```

<img src="/assets/images/210823_ch07/Provider.PNG" style="width:200px; object-fit:contain">


<br/>


### 3. Context 수정하기

  * Provider 컴포넌트의 value에 전역적으로 관리할 상태 변수와 상태를 변경하는 함수를 함께 전달하는 `UserProvider` 컴포넌트를 생성.
  * Provider와 비슷하지만 하위에 있는 Consumer 컴포넌트로부터 데이터와 변경할 수 있는 `함수`도 파라미터로 전달받음.

<span style="color:coral; line-height:0.8">contexts/User.js</span>

```javascript
import React, { createContext, useState } from 'react';

const UserContext = createContext({
    user: { name: ''},
    dispatch: () => {},
});

const UserProvider = ({ children }) => {
    const [name, setName] = useState('BeomCheol Kim');

    const value = { user: { name }, dispatch: setName};
    return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
};

const UserConsumer = UserContext.Consumer;

export { UserProvider, UserConsumer };
export default UserContext;
```

<br/>

* UserProvider 컴포넌트의 value로 전달되는 세터 함수를 이용해, 입력되는 값으로 Context의 값을 변경하는 Input 컴포넌트를 만듦.

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
import React, { useState } from 'react';
import styled from 'styled-components/native';
import { UserConsumer } from '../contexts/User';

const StyledInput = styled.TextInput`
    border: 1px solid #606060;
    width: 250px;
    padding: 10px 15px;
    margin: 10px;
    font-size: 24px;
`;

const Input = () => {
    const [name, setName] = useState('');
    return (
        <UserConsumer>
            {({ dispatch }) => {
                return (
                    <StyledInput
                        value={name}
                        onChangeText={text => setName(text)}
                        onSubmitEditing={() => {
                            dispatch(name);
                            setName('');
                        }}
                        placeholder="Enter a name..."
                        autoCapitalize="none"
                        autoCorrect={false}
                        returnKeyType="done"
                    />
                );
            }}
        </UserConsumer>
    );
};

export default Input;
```


<br/>


## 7.3 useContext

  * Consumer 컴포넌트의 자식 함수로 전달되던 값과 동일한 데이터를 반환함.
  * Consumer 컴포넌트에서는 반드시 `리액트 컴포넌트`를 반환하는 함수를 넣어야 하지만, useContext를 이용하면 그럴 필요가 없음.
 
<span style="color:coral; line-height:0.8">components/User.js</span>

```javascript
import React, {useContext } from 'react';
import styled from 'styled-components/native';
import UserContext from '../contexts/User';
//...
const User = () => {
    const { user } = useContext(UserContext);
    return <StyledText>Name: {user.name}</StyledText>;
};
//..
```

<br/>

<span style="color:coral; line-height:0.8">Input.js</span>

```javascript
import React, { useState, useContext } from 'react';
import styled from 'styled-components/native';
import UserContext from '../contexts/User';
//...
const Input = () => {
    const [name, setName] = useState('');
    const { dispatch } = useContext(UserContext);

    return (
        <StyledInput
            value={name}
            onChangeText={text => setName(text)}
            onSubmitEditing={() => {
                dispatch(name);
                setName('');
            }}
            placeholder="Enter a name..."
            autoCapitalize="none"
            autoCorrect={false}
            returnKeyType="done"
        />
    );
};
//...
```


<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---