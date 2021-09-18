---
layout: post
title: 처음 배우는 리액트 네이티브 8장 (2)
subtitle: 내비게이션
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 8.3 탭 내비게이션

  * 보통 화면 위나 아래에 위치하며, 탭에 있는 버튼을 누르면 버튼과 연결된 화면으로 이동하는 방식으로 동작함.
  * 추가 라이브러리를 설치해야 함.

```
npm install @react-navigation/bottom-tabs
```

<br/>

### 1. 화면 구성

  * `createBottomTabNavigator 함수`를 이용해 탭 내비게이션을 생성.
  * 스택 내비게이션과 동일하게 `Navigator 컴포넌트`와 `Screen 컴포넌트`가 있음.
  * 컴포넌트(화면)들을 Screen 컴포넌트의 `component`로 지정해 화면으로 사용하고 `Navigator 컴포넌트`로 감싸줌.
  * `initialRouteNavme` 속성을 이용해 첫 번째 화면을 설정할 수 있음.

<span style="color:coral; line-height:0.8">screens/TabScreens.js</span>

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
`;

export const Mail = () => {
    return (
        <Container>
            <StyledText>Mail</StyledText>
        </Container>
    );
};

export const Meet = () => {
    return (
        <Container>
            <StyledText>Meet</StyledText>
        </Container>
    );
};

export const Settings = () => {
    return (
        <Container>
            <StyledText>Settings</StyledText>
        </Container>
    );
};
```

<br/>

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
import React from 'react';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Mail, Meet, Settings } from '../screens/TabScreens';

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
    return (
        <Tab.Navigator initialRouteName="Settings">
            <Tab.Screen name="Mail" component={Mail} />
            <Tab.Screen name="Meet" component={Meet} />
            <Tab.Screen name="Settings" component={Settings} />
        </Tab.Navigator>
    );
};

export default TabNavigation;
```

<img src="/assets/images/210919_ch08/tab_navigation_setting.PNG" style="width:200px; object-fit:contain">


<br/>


### 2. 탭 바 수정하기

#### <span style="font-size:1.2em; color:cornflowerblue;">- 버튼 아이콘 설정하기</span>

  * `tabBarIcon`을 이용해 탭 버튼에 아이콘을 렌더링할 수 있음.
  * tabBarIcon에 컴포넌트를 반환하는 `함수`를 지정하면 아이콘이 들어갈 자리에 해당 컴포넌트를 렌더링함.
  * tabBarIcon에 설정된 함수에는 `color`, `size`, `focused`값을 포함한 객체가 파라미터로 전달됨.
  * Screen 컴포넌트마다 tabBarIcon에 `MaterialCommunityIcons 컴포넌트`를 반환하는 함수를 지정함.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
import React from 'react';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Mail, Meet, Settings } from '../screens/TabScreens';
import { MaterialCommunityIcons } from '@expo/vector-icons';

const TabIcon = ({ name, size, color }) => {
    return <MaterialCommunityIcons name={name} size={size} color={color} />;
};

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
    return (
        <Tab.Navigator initialRouteName="Settings">
            <Tab.Screen
                name="Mail"
                component={Mail}
                options={
                    tabBarIcon: props => TabIcon({ ...props, name: 'email' }),
                }
            />
            <Tab.Screen
                name="Meet"
                component={Meet}
                options={
                    tabBarIcon: props => TabIcon({ ...props, name: 'video' }),
                }
            />
            <Tab.Screen
                name="Settings"
                component={Settings}d
                options={
                    tabBarIcon: props => TabIcon({ ...props, name: 'settings' }),
                }
            />
        </Tab.Navigator>
    );
};

export default TabNavigation;
```

<br/>

  * Screen 컴포넌트마다 탭 버튼 아이콘을 지정하지 않고, 한 곳에서 모든 버튼의 아이콘을 관리할 수도 있음.
  * `Navigator` 컴포넌트의 `screenOptions` 속성을 사용하면 됨.
  * screenOptions에 `객체`를 반환하는 함수를 설정하고 함수로 전달되는 route를 이용함.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
import React from 'react';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { Mail, Meet, Settings } from '../screens/TabScreens';
import { MaterialCommunityIcons } from '@expo/vector-icons';

const TabIcon = ({ name, size, color }) => {
    return <MaterialCommunityIcons name={name} size={size} color={color} />;
};

const Tab = createBottomTabNavigator();

const TabNavigation = () => {
    return (
        <Tab.Navigator
            initialRouteName="Settings"
            screenOptions={({ route }) => ({
                tabBarIcon: props => {
                    let name = '';
                    if (route.name === 'Mail') name = 'email';
                    else if (route.name === 'Meet') name = 'video';
                    else name = 'settings';
                    return TabIcon({ ...props, name});
                },
            })}
        >
            <Tab.Screen name="Mail" component={Mail}/>
            <Tab.Screen name="Meet" component={Meet}/>
            <Tab.Screen name="Settings" component={Settings}/>
        </Tab.Navigator>
    );
};

export default TabNavigation;
```

<img src="/assets/images/210919_ch08/tab_navigation_button_icon.PNG" style="width:200px; object-fit:contain">

<span style="color:coral; font-size:0.8em; font-weight:bold">! "setting"이라는 아이콘을 불러오지 못함. (자세한 원인은 모르겠음.)</span>


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 라벨 수정하기</span>

  * 버튼 아래에 렌더링되는 `라벨`은 Screen 컴포넌트의 `name` 값을 기본값으로 사용함.
  * 탭 버튼의 라벨은 `tabBarLabel`을 이용해서 변경할 수 있음.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
//...
const TabNavigation = () => {
    return (
        <Tab.Navigator initialRouteName="Settings">
            <Tab.Screen
                name="Mail"
                component={Mail}
                options={
                    tabBarLabel: 'Inbox',
                    tabBarIcon: props => TabIcon({ ...props, name: 'email' }),
                }
            />
            //...
        </Tab.Navigator>
    );
};
//...
```

<br/>

  * 라벨을 버튼 아이콘의 아래가 아닌 아이콘 `옆에` 렌더링되도록 변경할 수 있음.
  * `labelPosition`을 사용하며, 속성값으로 `below-icon`과 `beside-icon`만 설정할 수 있음.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
//...
const TabNavigation = () => {
    return (
        <Tab.Navigator
            initialRouteName="Settings"
            tabBarOptions={ labelPosition: 'beside-icon' }
        >
            //...
        </Tab.Navigator>
    );
};
//...
```

<img src="/assets/images/210919_ch08/tab_navigation_label_position.PNG" style="width:200px; object-fit:contain">

<br/>

  * 라벨을 렌더링하지 않고 아이콘만 사용할 수도 있음.
  * `showLabel`을 이용하면 됨.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
//...
const TabNavigation = () => {
    return (
        <Tab.Navigator
            initialRouteName="Settings"
            tabBarOptions={ labelPosition: 'beside-icon', showLabel: false }
        >
            //...
        </Tab.Navigator>
    );
};
//...
```

<img src="/assets/images/210919_ch08/tab_navigation_label_invisible.PNG" style="width:200px; object-fit:contain">


<br/>


#### <span style="font-size:1.2em; color:cornflowerblue;">- 스타일 수정하기</span>

  * `tabBarOptions` 속성에 `style`의 값으로 스타일 객체를 설정하여 변경할 수 있음.
  * `activeTintColor`와 `inactiveTintColor`를 이용해 아이콘이 선택되어 활성화된 상태의 색과 선택되지 않아 비활성화된 상태의 색을 설정할 수 있음.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
//...
const TabNavigation = () => {
    return (
        <Tab.Navigator
            initialRouteName="Settings"
            tabBarOptions={
                labelPosition: 'beside-icon',
                showLabel: false,
                style: {
                    backgroundColor: '#54b7f9',
                    borderTopColor: '#ffffff',
                    borderTopWidth: 2,
                },
                activeTintColor: '#ffffff',
                inactiveTintColor: '#0B92E9',
            }
        >
            //...
        </Tab.Navigator>
    );
};
//...
```

<br/>

  * 버튼의 아이콘을 설정하기 위해 barTabIcon에 설정한 함수에는 파라미터로 size, color, focused를 가진 객체가 전달됨.
  * `focused`는 버튼의 선택된 상태를 나타내는 값임.
  * focused를 사용해 버튼의 활성화 상태에 따라 다른 버튼을 렌더링하거나 스타일을 변경할 수 있음.

<span style="color:coral; line-height:0.8">navigations/Tab.js</span>

```javascript
//...
const TabNavigation = () => {
    return (
        <Tab.Navigator
            initialRouteName="Settings"
            tabBarOptions={
                labelPosition: 'beside-icon',
                showLabel: false,
                style: {
                    backgroundColor: '#54b7f9',
                    borderTopColor: '#ffffff',
                    borderTopWidth: 2,
                },
                activeTintColor: '#ffffff',
                inactiveTintColor: '#cfcfcf',
            }
        >
            <Tab.Screen
                name="Mail"
                component={Mail}
                options={
                    tabBarLabel: 'Inbox',
                    tabBarIcon: props =>
                        TabIcon({
                            ...props,
                            name: props.focused ? 'email' : 'email-outline',
                        }),
                }
            />
            <Tab.Screen
                name="Meet"
                component={Meet}
                options={{
                    tabBarIcon: props =>
                        TabIcon({
                            ...props,
                            name: props.focused ? 'video' : 'video-outline',
                        }),
                }}
            />
            <Tab.Screen
                name="Settings"
                component={Settings}
                options={
                    tabBarIcon: props =>
                        TabIcon({
                            ...props,
                            name: props.focused ? 'settings' : 'settings-outline',
                        }),
                }
            />
        </Tab.Navigator>
    );
};
//...
```


<br/>


<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---