---
layout: post
title: 처음 배우는 리액트 네이티브 6장
subtitle: Hooks
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 6.0 실습 준비하기
  ```
  expo init react-native-hooks
  npm install styled-components
  ```


<br/>

---


## 6.1 useState
useState 함수를 호출하면 파라미터로 전달한 값을 초깃값으로 갖는 `상태 변수`와 그 변수를 수정할 수 있는 `세터 함수`를 `배열`로 반환함.

### 1. useState 사용하기

<span style="color:coral; line-height:0.8">Counter.js</span>

```javascript
import React, { useState } from 'react';
import styled from 'styled-components/native';
import Button from './Button';

const StyledText = styled.Text`
    font-size: 24px;
    margin: 10px;
`;

const Counter = () => {
    const [count, setCount] = useState(0);

    return (
        <>
            <StyledText>count: {count}</StyledText>
            <Button
                title="+"
                onPress={() => {
                    setCount(count + 1);
                }}
            />
            <Button
                title="-"
                onPress={() => {
                    setCount(count - 1);
                }}
            />
        </>
    );
};

export default Counter;
```

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import Counter from './components/Counter';

const Container = styled.View`
    flex: 1;
    background-color: #fff;
    justify-content: center;
    align-items: center;
`;

const App = () => {
    return (
        <Container>
            <Counter />
        </Container>
    );
};

export default App;
```

<img src="/assets/images/210819_ch06/using_useState.PNG" style="width:280px; object-fit:contain">


<br/>


### 2. 세터 함수

<span style="color:coral; line-height:0.8">세터 함수 사용법</span>

  * 세터 함수에 변경될 `상태의 값`을 전달하는 방법 (지금까지 사용한 방법)
  * 세터 함수의 파라미터에 `함수`를 전달하는 방법<br/>함수에는 변경되기 전의 상태 값이 파라미터로 전달되므로 값을 어떻게 수정할지 정의하면 됨.

<br/>

  * 세터 함수는 `비동기`로 동작하기 때문에 상태 변경이 여러 번 일어날 경우 상태가 변경되기 전에 다시 상태에 대한 업데이트가 실행되는 상황이 발생함.

<span style="color:coral; line-height:0.8">Counter.js</span>

```javascript
//...
const Counter = () => {
    const [count, setCount] = useState(0);

    return (
        <>
            <StyledText>count: {count}</StyledText>
            <Button
                title="+"
                onPress={() => {
                    setCount(count + 1);
                    setCount(count + 1);
                    console.log(`count: ${count}`);
                }}
            />
            //...
        </>
    );
};
//...
```

<img src="/assets/images/210819_ch06/setter_func_async.PNG" style="width:280px; object-fit:contain">

<span style="color:coral; font-size:0.9em">! 1씩만 증가하고, 로그를 확인하면 증가되기 전인 현재 상태값이 나타남.</span>

<br/>

  * 상태 변수를 사용하지 않고, 이전 상태값(즉, 함수를 세터 함수의 파라미터로 전달)을 이용하면 됨.

<span style="color:coral; line-height:0.8">Counter.js</span>

```javascript
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
      <>
          <StyledText>count: {count}</StyledText>
          <Button
              title="+"
              onPress={() => {
                  setCount(prevCount => prevCount + 1);
                  setCount(prevCount => prevCount + 1);
                  console.log(`count: ${count}`);
              }}
          />
          //...
      </>
  );
};
//...
```


<br/>

---


## 6.2 useEffect
  * 컴포넌트가 렌더링될 때마다 원하는 작업을 하도록 설정할 수 있음.
  * `첫 번째 파라미터`: 조건을 만족할 때마다 호출될 함수
  * `두 번째 파라미터`: 함수가 호출되는 조건, `배열`로 전달
  * 'react'의 useEffect를 import 해야 함.

### 1. useEffect 사용하기
두 번째 파라미터에 값을 전달하지 않으면 컴포넌트가 컴포넌트가 `렌더링될 때마다` 호출됨.

<span style="color:coral; line-height:0.8">Form.js</span>

```javascript
import React, { useState, useEffect } from 'react';
import styled from 'styled-components/native';

const StyledTextInput = styled.TextInput.attrs({
    autoCapitalize: 'none',
    autoCorrect: false,
})`
    border: 1px solid #757575;
    padding: 10px;
    margin: 10px 0;
    width: 200px;
    font-size: 20px;
`;

const StyledText = styled.Text`
    font-size: 24px;
    margin: 10px;
`;

const Form = () => {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');

    useEffect(() => {
        console.log(`name: ${name}, email: ${email}\n`);
    });

    return (
        <>
            <StyledText>Name: {name}</StyledText>
            <StyledText>Email: {email}</StyledText>
            <StyledTextInput
                value={name}
                onChangeText={text => setName(text)}
                placeholder="name"
            />
            <StyledTextInput
                value={email}
                onChangeText={text => setEmail(text)}
                placeholder="email"
            />
        </>
    );
};

export default Form;
```

<img src="/assets/images/210819_ch06/useEffect_no_parameter.PNG" style="width:600px; object-fit:contain">

name 또는 email의 텍스트를 입력할 때마다 렌더링되어 useEffect의 첫번째 파라미터로 보낸 함수가 실행됨.


<br/>


### 2. 특정 조건에서 실행하기
두 번째 파라미터에 상태 변화를 감지해 렌더링할 변수를 배열로 전달.

<span style="color:coral; line-height:0.8">Form.js</span>

```javascript
//...
const Form = () => {
    //...
    useEffect(() => {
        console.log(`name: ${name}, email: ${email}\n`);
    }, [email]);
    //...
};
//...
```

<img src="/assets/images/210819_ch06/useEffect_second_parameter.PNG" style="width:600px; object-fit:contain">

email의 텍스트가 변하면 렌더링 되지만, name의 텍스트는 변해도 렌더링 되지 않음.


<br/>


### 3. 마운트될 때 실행하기
두 번째 파라미터에 빈 배열을 전달하면 컴포넌트가 `마운트`될 때 함수가 실행됨.

<span style="color:coral; line-height:0.8">Form.js</span>

```javascript
//...
const Form = () => {
    //...
    useEffect(() => {
        console.log(`\n===== Form Component Mount =====\n`);
    }, []);
    //...
};
//...
```

<img src="/assets/images/210819_ch06/useEffect_work_mounted.PNG" style="width:600px; object-fit:contain">

Form 컴포넌트가 마운트될 때만 함수가 실행되고, name 또는 email의 텍스트가 변해도 함수는 실행되지 않음.


<br/>


### 4. 언마운트될 때 실행하기
실행 조건은 빈 배열 + 정리 함수(retrun)에 실행할 함수 전달

<span style="color:coral; line-height:0.8">Form.js</span>

```javascript
//...
const Form = () => {
    //...
    useEffect(() => {
        console.log(`\n===== Form Component Mount =====\n`);
        return () => console.log(`\n===== Form Component Unmount =====\n`);
    }, []);
    //...
};
//...
```

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React, { useState } from 'react';
import styled from 'styled-components/native';
import Counter from './components/Counter';
import Form from './components/Form';
import Button from './components/Button';
//...
const App = () => {
    const [isVisible, setIsVisible] = useState(true);

    return (
        <Container>
            <Button
                title={isVisible ? 'Hide' : 'Show'}
                onPress={() => setIsVisible(prev => !prev)}
            />
            {isVisible && <Form />}
        </Container>
    );
};

export default App;
```

<img src="/assets/images/210819_ch06/useEffect_work_unmounted.PNG" style="width:600px; object-fit:contain">

Button 컴포넌트의 onPress에 의해 Form 컴포넌트가 언마운트 될 때 return으로 전달한 함수가 실행됨.


<br/>

---


## 6.3 useRef
  * 웹에서 getElementById 또는 querySelector를 사용해 특정 DOM을 선택하는 것처럼 동작하게 해줌.
  * ref로 지정하면 생성된 변수에 값이 `저장`되는 것이 아니라 변수의 `.current 프로퍼티`에 해당 값을 담는 것
  * useState로 생성된 상태와 달리 useRef의 내용이 변경돼도 컴포넌트는 다시 `렌더링`되지 않음.
  * 'react'의 useRef를 import 해야 함.

<span style="color:coral; line-height:0.8">Form.js</span>

```javascript
import React, { useState, useEffect, useRef } from 'react';
//...
const Form = () => {
    //...
    const refName = useRef(null);
    const refEmail = useRef(null);

    useEffect(() => {
        console.log(`\n===== Form Component Mount =====\n`);
        refName.current.focus();
        return () => console.log(`\n===== Form Component Unmount =====\n`);
    }, []);

    return (
        <>
            //...
            <StyledTextInput
                //...
                placeholder="name"
                ref={refName}
                returnKeyType="next"
                onSubmitEditing={() => refEmail.current.focus()}
            />
            <StyledTextInput
                //...
                placeholder="email"
                ref={refEmail}
                returnKeyType="done"
            />
        </>
    );
};

export default Form;
```

<img src="/assets/images/210819_ch06/useRef.PNG" style="width:600px; object-fit:contain">

Form 컴포넌트가 렌더링 되면 name을 입력하도록 focus가 설정되고, name을 입력한 후 next를 누르면 email을 입력하도록 focus가 설정됨.


<br/>

---


## 6.4 useMemo
  * 반복 수행을 제거해 성능을 최적화할 수 있음.
  * `첫 번째 파라미터`: 실행할 함수
  * `두 번째 파라미터`: 함수 실행 조건을 배열로 전달
  * 'react'의 useMemo를 import 해야 함.

<span style="color:coral; line-height:0.8">Length.js</span>

```javascript
import React, { useState, useMemo } from 'react';
import styled from 'styled-components/native';
import Button from './Button';

const StyledText = styled.Text`
    font-size: 24px;
`;

const getLength = text => {
    console.log(`Target Text: ${text}`);
    return text.length;
}

const list = ['JavaScript', 'Expo', 'Expo', 'React Native'];

let idx = 0;
const Length = () => {
    const [text, setText] = useState(list[0]);

    const _onPress = () => {
        ++idx;
        if (idx < list.length) setText(list[idx]);
    };
    const length = useMemo(() => getLength(text), [text]);

    return (
        <>
            <StyledText>Text: {text}</StyledText>
            <StyledText>Length: {length}</StyledText>
            <Button title="Get Length" onPress={_onPress} />
        </>
    );
};

export default Length;
```

<img src="/assets/images/210819_ch06/useMemo.PNG" style="width:600px; object-fit:contain">

상태 변수 text의 값이 변할 때만 'useMemo'가 실행되어 length가 재설정됨.<br/>'Expo'라는 단어가 두개 연속으로 있을 때도 useMemo에 의해 한번만 실행됨.

### <span style="color:#cd853f">Plus.</span> useEffect vs. useMemo
  * useEffect는 함수 실행 결과로 아무것도 `반환`하지 않음.
  * useMemo는 첫 번째 파라미터로 전달된 함수의 return 값을 반환함.
  
```javascript
//useEffect: 사용 불가!!
const length = useEffect(() => getLength(text), [text]);

//useMemo: 사용 가능!!
const length = useMemo(() => getLength(text), [text]);
```












































<br/>


---


## 참고  
* <span style="opacity:0.5">Object.assign()</span>  
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign

* <span style="opacity:0.5">async-storage 오류</span>  
https://stackoverflow.com/questions/56029007/nativemodule-asyncstorage-is-null-with-rnc-asyncstorage

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---