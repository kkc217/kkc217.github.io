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