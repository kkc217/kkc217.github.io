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


### 2. IconButton 컴포넌트 & Task 컴포넌트

<span style="color:coral; line-height:0.8">images.js</span>

```javascript
import CheckBoxOutline from '../assets/icons/check_box_outline.png';
import CheckBox from '../assets/icons/check_box.png';
import DeleteForever from '../assets/icons/delete_forever.png';
import Edit from '../assets/icons/edit.png';

export const images = {
    uncompleted: CheckBoxOutline,
    completed: CheckBox,
    delete: DeleteForever,
    update: Edit,
};
```

<br/>

<span style="color:coral; line-height:0.8">IconButton.js</span>

```javascript
import React from 'react';
import { TouchableOpacity } from 'react-native';
import styled from 'styled-components/native';
import PropTypes from 'prop-types';
import { images } from '../images';

const Icon = styled.Image`
    tint-color: ${({ theme }) => theme.text};
    width: 30px;
    height: 30px;
    margin: 10px;
`;

const IconButton = ({ type, onPressOut }) => {
    return (
        <TouchableOpacity onPressOut={onPressOut}>
            <Icon source={type} />
        </TouchableOpacity>
    );
};

IconButton.propTypes = {
    type: PropTypes.oneOf(Object.values(images)).isRequired,
    onPressOut: PropTypes.func,
};

export default IconButton;
```

<br/>

<span style="color:coral; line-height:0.8">Task.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';
import PropTypes from 'prop-types';
import IconButton from './IconButton';
import { images } from '../images';

const Container = styled.View`
    flex-direction: row;
    align-items: center;
    background-color: ${({ theme }) => theme.itemBackground};
    border-radius: 10px;
    padding: 5px;
    margin: 3px 0px;
`;

const Contents = styled.Text`
    flex: 1;
    font-size: 24px;
    color: ${({ theme}) => theme.text};
`;

const Task = ({ text }) => {
    return (
        <Container>
            <IconButton type={images.uncompleted} />
            <Contents>{text}</Contents>
            <IconButton type={images.update} />
            <IconButton type={images.delete} />
        </Container>
    );
};

Task.propTypes = {
    text: PropTypes.string.isRequired,
};

export default Task;
```

<img src="/assets/images/210818_ch05/IconButton_and_Task.PNG" style="width:280px; object-fit:contain">


<br/>

---


## 5.5 기능 구현하기

### 1. 추가 기능

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
export default function app() {
    //...
    const _addTask = () => {
        const ID = Date.new().toString();
        const newTaskObject = {
            [ID]: { id: ID, text: newTask, completed: false },
        };
        setNewTask('');
        setTasks({ ...tasks, ...newTaskObject });
    };
    //...
}
```

<span style="color:#f7df1e; font-height:0.5">setTasks({ ...tasks, ...newTaskObject });</span>  
기존의 목록 tasks는 유지한 상태에서 새로운 항목이 추가되도록 구성

<img src="/assets/images/210818_ch05/add_function.PNG" style="width:600px; object-fit:contain">


<br/>


### 2. 삭제 기능

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
export default function App() {
    //...
    const _addTask = () => {//...
        };

    const _deleteTask = id => {
        const currentTasks = Object.assign({}, tasks);
        delete currentTasks[id];
        setTasks(currentTasks);
    };
    //...
    return (
        <ThemeProvider theme={theme}>
            <Container>
                //...
                <List width={width}>
                    {Object.values(tasks)
                        .reverse()
                        .map(item => (
                            <Task key={item.id} item={item} deleteTask={_deleteTask} />
                        ))}
                </List>
            </Container>
        </ThemeProvider>
    );
}
```

<br/>

<span style="color:coral; line-height:0.8">Task.js</span>

```javascript
//...
const Task = ({ item, deleteTask }) => {
    return (
        <Container>
            <IconButton type={images.uncompleted} />
            <Contents>{item.text}</Contents>
            <IconButton type={images.update} />
            <IconButton type={images.delete} id={item.id} onPressOut={deleteTask} />
        </Container>
    );
};

Task.propTypes = {
    item: PropTypes.object.isRequired,
    deleteTask: PropTypes.func.isRequired,
};

export default Task;
```

<br/>

<span style="color:coral; line-height:0.8">IconButton.js</span>

```javascript
//...
const IconButton = ({ type, onPressOut, id }) => {
    const _onPressOut = () => {
        onPressOut(id);
    };

    return (
        <TouchableOpacity onPressOut={_onPressOut}>
            <Icon source={type} />
        </TouchableOpacity>
    );
};

IconButton.defaultProps = {
    onPressOut: () => {},
};

IconButton.propTypes = {
    type: PropTypes.oneOf(Object.values(images)).isRequired,
    onPressOut: PropTypes.func,
    id: PropTypes.string,
};

export default IconButton;
```

<img src="/assets/images/210818_ch05/delete_function.PNG" style="width:280px; object-fit:contain">


<br/>


### 3. 완료 기능
완료한 항목은 수정 기능을 사용하지 않도록 수정 버튼을 없애고, 흐리게까지 만듦.

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
export default function App() {
    //...
    const _deleteTask = id => {
        //...
    };

    const _toggleTask = id => {
        const currentTasks = Object.assign({}, tasks);
        currentTasks[id]['completed'] = !currentTasks[id]['completed'];
        setTasks(currentTasks);
    };
    //...
    return (
        <ThemeProvider theme={theme}>
            <Container>
                //...
                <List width={width}>
                    {Object.values(tasks)
                        .reverse()
                        .map(item => (
                            <Task
                            key={item.id}
                            item={item}
                            deleteTask={_deleteTask}
                            toggleTask={_toggleTask}
                        />
                        ))}
                </List>
            </Container>
        </ThemeProvider>
    );
}
```

<span style="color:#f7df1e; font-height:0.5">Object.assign({}, tasks);</span>  
{}에 tasks를 복사함. (열거할 수 있는 하나 이상의 속성들을 목표 객체로 복사해주는 함수)

<br/>

<span style="color:coral; line-height:0.8">Task.js</span>

```javascript
//...
const Contents = styled.Text`
    flex: 1;
    font-size: 24px;
    color: ${({ theme, completed }) => (completed ? theme.done : theme.text)};
    text-decoration-line: ${({ completed }) =>
        completed ? 'line-through' : 'none'};
`;

const Task = ({ item, deleteTask, toggleTask }) => {
    return (
        <Container>
            <IconButton
                type={item.completed ? images.completed : images.uncompleted}
                id={item.id}
                onPressOut={toggleTask}
                completed={item.completed}
            />
            <Contents completed={item.completed}>{item.text}</Contents>
            {item.completed || <IconButton type={images.update} />}
            <IconButton
                type={images.delete}
                id={item.id}
                onPressOut={deleteTask}
                completed={item.completed}
            />
        </Container>
    );
};

Task.propTypes = {
    item: PropTypes.object.isRequired,
    deleteTask: PropTypes.func.isRequired,
    toggleTask: PropTypes.func.isRequired,
};

export default Task;
```

<br/>

<span style="color:coral; line-height:0.8">IconButton.js</span>

```javascript
//...
const Icon = styled.Image`
    tint-color: ${({ theme, completed }) =>
        completed ? theme.done : theme.text};
    width: 30px;
    height: 30px;
    margin: 10px;
`;

const IconButton = ({ type, onPressOut, id, completed }) => {
    const _onPressOut = () => {
        onPressOut(id);
    };

    return (
        <TouchableOpacity onPressOut={_onPressOut}>
            <Icon source={type} completed={completed} />
        </TouchableOpacity>
    );
};
//...
IconButton.propTypes = {
    type: PropTypes.oneOf(Object.values(images)).isRequired,
    onPressOut: PropTypes.func,
    id: PropTypes.string,
    completed: PropTypes.bool,
};

export default IconButton;
```

<img src="/assets/images/210818_ch05/toggle_function.PNG" style="width:280px; object-fit:contain">


<br/>


### 4. 수정 기능

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
//...
export default function App() {
    //...
    const _toggleTask = id => {
        //...
    };

    const _updateTask = item => {
        const currentTasks = Object.assign({}, tasks);
        currentTasks[item.id] = item;
        setTasks(currentTasks);
    };
    //...
    return (
        <ThemeProvider theme={theme}>
            <Container>
                //...
                <List width={width}>
                    {Object.values(tasks)
                        .reverse()
                        .map(item => (
                            <Task
                            key={item.id}
                            item={item}
                            deleteTask={_deleteTask}
                            toggleTask={_toggleTask}
                            updateTask={_updateTask}
                        />
                        ))}
                </List>
            </Container>
        </ThemeProvider>
    );
}
```

<br/>

<span style="color:coral; line-height:0.8">Task.js</span>

```javascript
import React, { useState } from 'react';
import styled from 'styled-components/native';
import PropTypes from 'prop-types';
import IconButton from './IconButton';
import { images } from '../images';
import Input from './Input';
//...
const Task = ({ item, deleteTask, toggleTask, updateTask }) => {
    const [isEditing, setIsEditing] = useState(false);
    const [text, setText] = useState(item.text);

    const _handleUpdateButtonPress = () => {
        setIsEditing(true);
    };

    const _onSubmitEditing = () => {
        if (isEditing) {
            const editedTask = Object.assign({}, item, { text });
            setIsEditing(false);
            updateTask(editedTask);
        }
    };

    return isEditing ? (
        <Input
            value={text}
            onChangeText={text => setText(text)}
            onSubmitEditing={_onSubmitEditing}
        />
    ) : (
        <Container>
            <IconButton
                type={item.completed ? images.completed : images.uncompleted}
                id={item.id}
                onPressOut={toggleTask}
                completed={item.completed}
            />
            <Contents completed={item.completed}>{item.text}</Contents>
            {item.completed || (
                <IconButton
                    type={images.update}
                    onPressOut={_handleUpdateButtonPress}
                    />
            )}
            //...
        </Container>
    );
};

Task.propTypes = {
    item: PropTypes.object.isRequired,
    deleteTask: PropTypes.func.isRequired,
    toggleTask: PropTypes.func.isRequired,
    updateTask: PropTypes.func.isRequired,
};

export default Task;
```

<img src="/assets/images/210818_ch05/edit_function.PNG" style="width:280px; object-fit:contain">























<br/>


---


## 참고  
* Object.assign()
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---