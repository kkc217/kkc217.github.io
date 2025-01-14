---
layout: post
title: 처음 배우는 리액트 네이티브 8장 (1)
subtitle: 내비게이션
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 8.0 실습 준비하기
  ```
  expo init react-native-navigation
  npm install styled-components
  ```


<br/>

---


## 8.1 리액트 내비게이션

  * 리액트 내비게이션 라이브러리에는 리액트 네이티브 앱의 내비게이션을 관리할 수 있도록하는 기능들이 있음.
  * 지원하는 내비게이션: 스택 내비게이션, 탭 내비게이션, 드로어 내비게이션


### 1. 내비게이션의 구조
리액트 내비게이션은 Screen 컴포넌트, Navigator 컴포넌트, NavigationContainer 컴포넌트로 세 가지가 있음.

<span style="color:#027cd4; line-height:1.15; font-weight:bold">Screen 컴포넌트</span>

  * `화면`으로 사용되는 컴포넌트로 name과 component 속성을 지정해야 함.
  * `name`: 화면 이름
  * `component`: 화면으로 사용될 컴포넌트를 전달
  * 화면으로 사용되는 컴포넌트에는 항상 `navigation`과 `route`가 props로 전달됨.

<br/>

<span style="color:#027cd4; line-height:1.15; font-weight:bold">Navigator 컴포넌트</span>

  * 화면을 관리하는 `중간 관리자 역할`
  * 내비게이션을 구성하며, 여러 개의 Screen 컴포넌트를 자식 컴포넌트로 가짐.

<br/>

<span style="color:#027cd4; line-height:1.15; font-weight:bold">NavigationContainer 컴포넌트</span>

  * 내비게이션의 `계층 구조`와 `상태`를 관리함.
  * 모든 내비게이션 구성 요소를 감싼 `최상위 컴포넌트`여야 함.


<br/>


### 2. 설정 우선 순위

  * 리액트 내비게이션에서 설정할 수 있는 속성을 수정하는 방법은 크게 세 가지가 있음.<br/>1. `Navigator 컴포넌트`의 속성으로 수정하는 방법<br/>2. `Screen 컴포넌트`의 속성으로 수정하는 방법<br/>3. 화면으로 사용되는 컴포넌트의 props로 전달되는 `navigation`을 이용해서 수정하는 방법`
  * 2번과 3번은 `해당 화면`에만 적용되지만, 1번은 `자식 컴포넌트`로 존해하는 `모든` 컴포넌트에 적용됨.
  * `3번 → 2번 → 1번` 순으로 우선순위가 높음.<br/><span style="color:coral; font-size:0.9em">! 작은 범위의 설정일수록 우선순위가 높다.</span>


<br/>


### 3. 폴더 구조와 라이브러리 설치

  * src폴더 아래에 `navigations` 폴더와 `screens` 폴더를 생성.

<img src="/assets/images/210904_ch08/ch08_directory_setting.PNG" style="width:180px; object-fit:contain">

<br/>

  * 리액트 내비게이션 라이브러리 설치
  
```
npm install --save @react-navigation/native
```

<br/>

 * 종속성 설치 → 대부분의 내비게이션에 반드시 필요함.

```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

<span style="color:coral; font-weight:bold">! 리액트 내비게이션은 각 기능별로 모듈이 분리되어 있어, 사용하는 내비게이션의 종류에 따라 개별적으로 추가 라이브러리를 설치해야 함.</span>


<br/>

---


## 8.2 스택 내비게이션

  * 일반적으로 가장 많이 사용되는 내비게이션
  * 현재 화면 위에 다른 화면을 쌓으면서 화면을 이동함.
  * 화면 위에 새로운 화면을 쌓으면서(`push`) 이동하기 때문에 이전 화면을 계속 유지함.
  * 가장 위에 있는 화면을 들어내면(`pop`) 이전 화면으로 돌아갈 수 있음.
  * 스택 내비게이션 활용에 필요한 라이브러리를 설치해야 함.

```
npm install @react-navigation/stack
```

<br/>

### 1. 화면 구성

  * `Home 화면`: 첫 화면으로 사용.
  * 화면을 확인하기 위한 텍스트와 다음 화면인 List 화면으로 이동하기 위한 버튼으로 구성함.

<span style="color:coral; line-height:0.8">screens/Home.js</span>

```javascript
import React from 'react';
import { Button } from 'react-native';
import styled from 'styled-components/native';

const Container = styled.View`
    align-items: center;
`;

const StyledText = styled.Text`
    font-size: 30px;
    margin-bottom: 10px;
`;

const Home = () => {
    return (
        <Container>
            <StyledText>Home</StyledText>
            <Button title="go to the list screen" />
        </Container>
    );
};

export default Home;
```

<br/>

  * `List 화면`
  * 화면에서 사용할 임시 목록을 만들고, 항목 수만큼 버튼을 생성하도록 만듦.

<span style="color:coral; line-height:0.8">screens/List.js</span>

```javascript
import React from 'react';
import { Button } from 'react-native';
import styled from 'styled-components/native';

const Container = styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
`;

const StyledText = styled.Text`
    font-size: 30px;
    margin-bottom: 10px;
`;

const items = [
    { _id: 1, name: 'React Native' },
    { _id: 2, name: 'React Navigation' },
    { _id: 3, name: 'Hanbit' },
];

const List = () => {
    const _onPress = item => {};

    return (
        <Container>
            <StyledText>List</StyledText>
            {items.map(item => (
                <Button
                    Key={item._id}
                    title={item.name}
                    onPress={() => _onPress(item)}
                />
            ))}
        </Container>
    );
};

export default List;
```

<br/>

  * 목록의 상세 정보를 보여주는 컴포넌트

<span style="color:coral; line-height:0.8">screens/Item.js</span>

```javascript
import React from 'react';
import styled from 'styled-components/native';

const Container = styled.View`
    flex: 1;
    justify-content: center;
    align-items: center;
`;

const StyledText = styled.Text`
    font-size: 30px;
    margin-bottom: 10px;
`;

const Item = () => {
    return (
        <Container>
            <StyledText>Item</StyledText>
        </Container>
    );
};

export default Item;
```

<br/>

  * 앞서 생성한 screens의 컵포넌트를 이용해 스택 내비게이션을 구성.
  * `createStackNavvigator` 함수를 이용해 스택 내비게이션을 생성.
  * 화면을 구성하는 `Screen` 컴포넌트와 Screen 컴포넌트를 관리하는 `Navigator` 컴포넌트가 있음.
  * Navigator 컴포넌트 안에 Screen 컴포넌트를 자식 컴포넌트로 작성하고, 앞서 만든 컴포넌트를 Screen 컴포넌트의 component로 지정.
  * Navigator의 `initialRouteName` 속성을 이용해 첫 번째 화면을 지정할 수 있음.<br/><span style="color:coral; font-weight:bold">! name에는 화면의 이름을 작성하는데, 반드시 서로 서로 다른 값을 가져야 함.</span>

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
import React from 'react';
import { createStackNavigator } from '@react-navigation/stack';
import Home from '../screens/Home';
import List from '../screens/List';
import Item from '../screens/Item';

const Stack = createStackNavigator();

const StackNavigation = () => {
    return (
        <Stack.Navigator initialRouteName="List">
            <Stack.Screen name="Home" component={Home} />
            <Stack.Screen name="List" component={List} />
            <Stack.Screen name="Item" component={Item} />
        </Stack.Navigator>
    );
};

export default StackNavigation;
```

<br/>

<span style="color:coral; line-height:0.8">App.js</span>

```javascript
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import StackNavigation from './navigations/Stack';

const App = () => {
    return (
        <NavigationContainer>
            <StackNavigation />
        </NavigationContainer>
    );
};

export default App;
```

<img src="/assets/images/210904_ch08/StackNavigation_screen_composition.PNG" style="width:200px; object-fit:contain">

<br/>

### 2. 화면 이동

  * Screen 컴포넌트의 component로 지정된 컴포넌트는 화면으로 이용되고, `navigation`이 `props`로 전달됨.
  * navigation의 기능 중 `navigate 함수`는 원하는 화면으로 이동하는 데 사용됨.
  * navigate 함수에 전달되는 화면의 이름은 Screen 컴포넌트의 name값 중 하나를 입력해야 함.

<span style="color:coral; line-height:0.8">screens/Home.js</span>

```javascript
//...
const Home = ({ navigation }) => {
    return (
        <Container>
            <StyledText>Home</StyledText>
            <Button
                title="go to the list screen"
                onPress={() => navigation.navigate('List')}
            />
        </Container>
    );
};
//...
```

<img src="/assets/images/210904_ch08/stack_navigation_navigate_func.PNG" style="width:400px; object-fit:contain">

<br/>

  * 이동하는 화면이 이전 화면의 `상세 화면`이라면 어떤 내용을 렌더링해야 하는지 전달받아야 함.<br/>→ navigate 함수의 두 번째 파라미터에 객체를 전달해 이동하는 화면에 필요한 정보를 함께 전달할 수 있음.

<span style="color:coral; line-height:0.8">screens/List.js</span>

```javascript
//...
const List = ({ navigation }) => {
    const _onPress = item => {
        navigation.navigate('Item', { id: item._id, name: item.name });
    };
    //...
};
//...
```

<br/>

<span style="color:coral; line-height:0.8">screens/Item.js</span>

```javascript
//...
const Item = ({ route }) => {
    return (
        <Container>
            <StyledText>Item</StyledText>
            <StyledText>ID: {route.params.id}</StyledText>
            <StyledText>Name: {route.params.name}</StyledText>
        </Container>
    );
};
//...
```

<img src="/assets/images/210904_ch08/stack_navigation_screen_move.PNG" style="width:600px; object-fit:contain">

<br/>

### 3. 화면 배경색 수정하기

 * 각 screen 컴포넌트에서 배경색을 지정할 수 있음.
 * But, 아래 화면 처럼 일부분만 색이 바뀜.<br/>→ 내비게이션은 화면 전체를 차지하고 있지만, 화면으로 사용된 컴포넌트의 영역이 전체를 차지하고 있지 않아서 생기는 문제

<img src="/assets/images/210904_ch08/stack_navigation_change_color.PNG" style="width:200px; object-fit:contain">

<br/>

  * 화면 컴포넌트가 전체 영역을 차지하도록 스타일에 `flex: 1`을 설정해도 되지만, 상황에 따라 화면 전체를 차지 못 할 수도 있음.
  * 리액트 내비게이션의 설정을 수정해 해결 가능. (stack navigation 전체의 색을 변경.)
  * `cardStyle`을 이용하면 스택 내비게이션의 화면 배경색을 수정할 수 있음.
  * 화면마다 설정하지 않고 Navigator 컴포넌트의 `screenOptions`에 설정해 화면 전체에 적용되도록 할 수 있음.

<img src="/assets/images/210904_ch08/stack_navigation_change_all_color.PNG" style="width:200px; object-fit:contain">

<br/>

### 4. 헤더 수정하기

`헤더` → 뒤로 가기 버튼을 제공하거나 타이틀을 통해 현재 화면을 알려주는 역할을 함.


#### <span style="font-size:1.2em; color:cornflowerblue;">- 타이틀 수정하기</span>

  * 헤더의 타이틀은 Screen 컴포넌트의 name 속성을 기본값으로 사용함.<br/><span style="color:coral">! 간편하게 바꿀 수는 있지만, name 속성을 사용한 곳을 찾아다니며 모두 수정해야함.</span>
  * `headerTitle` 속성을 이용하면 됨.
  * 모든 화면에서 같은 타이틀이 나타나도록 수정하려면 `Navigator 컴포넌트`의 `screen Options` 속성에 `headerTitle`을 지정하면 됨.

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
const StackNavigation = () => {
    return (
        <Stack.Navigator
            //...
        >
            <Stack.Screen name="Home" component={Home} />
            <Stack.Screen
                name="List"
                component={List}
                options={{ headerTitle: 'List Screen' }}
            />
            <Stack.Screen name="Detail" component={Item} />
        </Stack.Navigator>
    );
};
//...
```

<img src="/assets/images/210904_ch08/changing_title_name.PNG" style="width:200px; object-fit:contain">

<br/>

#### <span style="color:#cd853f">Plus.</span> 안드로이드 스튜디오 에뮬레이터 오류

기존에 사용하던대로 안드로이드 스튜디오 에뮬레이터를 사용하려는데 에뮬레이터가 이전에 썼던 화면에서 멈춰 추가적인 동작이 전혀 안 됨.<br/>→ 에뮬레이터에 저장되어 있던 기존의 데이터를 지워서 해결함.

<img src="/assets/images/210904_ch08/android_studio_AVD_error_fix_1.PNG" style="width:500px; object-fit:contain">

<br/>

<img src="/assets/images/210904_ch08/android_studio_AVD_error_fix_2.PNG" style="width:500px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 스타일 수정하기</span>

  * `headerStyle`: 배경색 등을 수정
  * `headerTitleStyle`: 헤더의 타이틀 컴포넌트의 스타일을 수정

<img src="/assets/images/210904_ch08/header_style_change.PNG" style="width:200px; object-fit:contain">

<span style="color:coral; font-size:0.8em">! 헤더의 타이틀이 iOS에서는 중앙에 있는 반면, 안드로이드는 중앙으로 정렬되지 않음.</span>

<br/>

  * `headerTitleAlign` 속성으로 헤더의 타이틀 정렬을 설정할 수 있음.
  * `left`와 `center` 두 가지 값만 설정 가능. (iOS-center, 안드로이드-left가 기본값)

<img src="/assets/images/210904_ch08/header_title_align.PNG" style="width:200px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 타이틀 컴포넌트 변경</span>

  * 타이틀에 문자열이 아닌 다른 것을 렌더링할 수 있음.
  * `headerTitle` 속성에 컴포넌트를 반환하는 함수를 지정<br/>→ 타이틀 컴포넌트를 반환하는 컴포넌트로 변경할수 있음.
  * headerTitle에 함수가 설정되면 해당 함수의 `파라미터로` style과 tintColor 등이 포함된 `객체`가 전될됨.
  * `style`: headerTitleStyle에 설정된 값
  * `tintColor`: headerTitleColor에 지정된 값

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
<MaterialCommunityIcons name="react" style={style} />
//... 
```

<img src="/assets/images/210904_ch08/stack_navigation_title_component_change.PNG" style="width:200px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 버튼 수정하기</span>

  * 이전 화면으로 이동하는 뒤로 가기 버튼이 자동으로 생김.
  * `안드로이드` - 버튼의 타이틀을 보여주지 않음.
  * `iOS` - 이전 화면의 타이틀을 버튼의 타이틀로 보여줌.
  * `headerBackTitleVisible` 속성을 이용해 안드로이드에서도 버튼의 타이틀을 보여줄 수 있음.

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
<Stack.Screen
  name="List"
  component={List}
  options={{
      headerTitle: 'List Screen',
      headerBackTitleVisible: true,
  }}
/>
//... 
```

<img src="/assets/images/210904_ch08/button_headerBackTitleVisibel.PNG" style="width:200px; object-fit:contain">

<br/>

  * 이전 화면의 이름이 아닌 다른 값을 이용할 수도 있음.
  * `headerBackTitle` 속성을 이용하면 됨.

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
<Stack.Screen
  name="List"
  component={List}
  options={{
      headerTitle: 'List Screen',
      headerBackTitleVisible: true,
      headerBackTitle: 'Prev',
  }}
/>
//... 
```

<img src="/assets/images/210904_ch08/button_headerBackTitle.PNG" style="width:200px; object-fit:contain">

<span style="color:coral; line-height:0.9">! 버튼의 타이틀에 빈 값을 주고 싶은 경우 headerBackTitleVisible을 이용해서 보이지 않도록 해야 함.</span>


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 버튼 스타일 수정하기</span>

  * `headerBackTitleStyle`을 이용해 글자의 색, 크기 등 다양한 스타일을 지정할 수 있음.<br/>But, 버튼의 타이틀에만 적용됨.
  * `headerTintColor`를 이용하면 버튼의 타이틀과 이미지의 색을 동일하게 변경할 수 있음.
  * headerTintColor에 지정된 색은 버튼뿐만 아니라 헤더의 타이틀에도 적용됨.
  * headerTintColor보다 `TitleStyle` 또는 `headerBackTitleStyle`의 우선순위가 높으므로 겹치지 않도록 잘 사용해야 함.

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
headerTitleStyle: { fontSize: 24 },
headerTintColor: '#e74c3c',
//... 
```

<img src="/assets/images/210904_ch08/button_style_change.PNG" style="width:200px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 버튼 컴포넌트 변경</span>

  * 뒤로 가기 버튼의 아이콘을 변경할 수 있음.
  * `headerBackImage`에 컴포넌트를 반환하는 `함수`를 전달하면 됨.

<span style="color:coral; line-height:0.8">navigations/Stack.js</span>

```javascript
//...
headerBackImage: ({ tintColor }) => {
    const style = {
        marginRight: 5,
        marginLeft: Platform.OS === 'ios' ? 11 : 0,
    };
    return (
        <MaterialCommunityIcons
            name="keyboard-backspace"
            size={30}
            color={tintColor}
            style={style}
        />
    );
},
//... 
```

<br/>

  * 헤더의 왼쪽 버튼 전체를 변경하고 싶다면 `headerLeft`에 컴포넌트를 반환하는 `함수`를 지정.
  * `headerRight`에 컴포넌트를 반환하는 `함수`를 지정하면 헤더의 오른쪽에 원하는 컴포넌트를 렌더링할 수 있음.

  <br/>
  
<span style="color:#f7df1e">useLayoutEffect Hook</span>  

  * useEffect Hook와 사용법과 동작 방식이 거의 같음.
  * useEffect Hook과 달리 컴포넌트가 업데이트된 직후 화면이 렌더링되기 전에 실행됨.
  * 따라서 `화면을 렌더링하기 전`에 변경할 부분이 있거나 수치 등을 측정해야 하는 상황에서 많이 상요됨.

<span style="color:#f7df1e">headerLeft</span>

  * 파라미터로 다양한 값들이 전달됨.
  * `onPress`는 뒤로가기 버튼 기능이 전달됨.<br/>→ 뒤로 가기 버튼의 기능을 그대로 기용하고 싶은 경우 유용하게 사용 가능.

<span style="color:#f7df1e">headerRight</span>  

  * 함수의 파라미터에 `tintColor`만 전달되므로 `onPress`에 원하는 행동을 정의해줘야 함.
  * `popToTop` 함수는 현재 쌓여 있는 모든 화면을 내보내고 첫 화면으로 돌아가는 기능<br/>→ 이 함수를 onPress에 전달하여 첫 화면으로 돌아가도록 작성함.

<br/>

<span style="color:coral; line-height:0.8">screens/Item.js</span>

```javascript
import React, { useLayoutEffect } from 'react';
import styled from 'styled-components/native';
import { MaterialCommunityIcons } from '@expo/vector-icons';
//...
const Item = ({ navigation, route }) => {
    useLayoutEffect(() => {
        navigation.setOptions({
            headerBackTitleVisible: false,
            headerTintColor: '#ffffff',
            headerLeft: ({ onPress, tintColor }) => {
                return (
                    <MaterialCommunityIcons
                        name="keyboard-backspace"
                        size={30}
                        style={{ marginLeft: 11 }}
                        color={tintColor}
                        onPress={onPress}
                    />
                );
            },
            headerRight: ({ tintColor }) => (
                <MaterialCommunityIcons
                    name="home-variant"
                    size={30}
                    style={{ marginRight: 11 }}
                    color={tintColor}
                    onPress={() => navigation.popToTop()}
                />
            ),
        });
    }, []);

    return (
        <Container>
            <StyledText>Item</StyledText>
            <StyledText>ID: {route.params.id}</StyledText>
            <StyledText>Name: {route.params.name}</StyledText>
        </Container>
    );
};
//...
```

<img src="/assets/images/210904_ch08/button_component_change.PNG" style="width:200px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 헤더 감추기</span>

`headerMode`나 `headerShown` 속성을 이용해 헤더를 감출 수 있음.

<span style="color:#f7df1e">headerMode</span>

  * `Navigator 컴포넌트`의 속성으로 헤더를 렌더링하는 방법을 설정하는 속성
  * `float`: 헤더가 상단에 유지되면 하나의 헤더를 사용함. (iOS)
  * `screen`: 각 화면마다 헤더를 가지며 화면 변경과 함께 나타나거나 사라짐. (안드로이드)
  * `none`: 헤더가 렌더링되지 않음.

<span style="color:#f7df1e">headerShown</span>
`Navigator 컴포넌트`의 `screenOptions`에 설정하면 전체 화면의 헤더가 보이지 않도록 설정할 수 있음.


<br/>


<div style="font-size:13px; text-align:right">
<br/><br/>
continue.</div>

---