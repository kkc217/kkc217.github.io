---
layout: post
title: 처음 배우는 리액트 네이티브 2장
subtitle: 리액트 네이티브 시작하기
categories: ReactNative
tags: [React Native, 창업 동아리, 스터디]
---

## 2.1 개발 환경 준비하기

### 1. Node.js 설치
  이전에 설치했었기에 그대로 사용함.  
  <img src="/assets/images/210810_ch02/nodeJs_version_check.PNG">

### 2. 파이썬 설치
  파이썬 2를 설치해야 한다지만, 파이썬 3도 잘 동작해 이전에 설치했던 파이썬 3 그대로 사용함.  
  <img src="/assets/images/210810_ch02/python_version_check.PNG">

### 3. 자바 설치
  기존의 상위 버전을 그대로 사용할 경우 오류가 발생하여 JAVA 14.0.2를 설치하여 사용함.  
  <img src="/assets/images/210810_ch02/java_version_check.PNG" style="width:450px; object-fit:contain">

### 4. 안드로이드 스튜디오 설치
    https://bit.ly/android-ide-download
  
  위 링크에서 안드로이드 스튜디오를 설치하고 환경 변수를 설정함.  
  <img src="/assets/images/210810_ch02/AndroidStudio_home.PNG">  
  <img src="/assets/images/210810_ch02/AndroidStudio_user_variable_change.PNG" style="width:450px; object-fit:contain">
  <img src="/assets/images/210810_ch02/AndroidStudio_system_variable_change.PNG" style="width:450px; object-fit:contain">

### 5. 안드로이드 스튜디오 에뮬레이터로 가상 기기 만들기
  <img src="/assets/images/210810_ch02/AndroidStudio_emulator.PNG" style="width:300px; object-fit:contain">

### 6. 에디터(Visual Studio Code) 설치
  기존에 사용하던 VScode를 사용.


<br/>

---


## 2.2 리액트 네이티브 프로젝트 만들기
  Expo를 이용하는 방법과 리액트 네이티브 CLI를 이용하는 방법이 있음.

### 1. Expo
  * 리액트 네이티브 처음 시작하는 사람도 쉽게 사용할 수 있으며, 완성된 프로젝트를 배포 및 관리할 수   있는 다양한 기능을 제공함.
  * But, Expo에서 제공하는 API만 사용할 수 있음. + 네이티브 모듈을 추가로 만들어서 사용하는 것이 <span style="color:coral">불가능</span>함.

  <br/>

  &nbsp;&nbsp;&nbsp;&nbsp;<div style="font-size:1.2em">(1) <span style="color:cornflowerblue;">expo-cli 설치</span></div>
  ```
  npm install --global expo-cli
  ```

  &nbsp;&nbsp;&nbsp;&nbsp;<div style="font-size:1.2em">(2) <span style="color:cornflowerblue;">Expo 프로젝트 생성</span></div>
```
expo init my-first-expo  //'my-first-expo'라는 이름의 프로젝트 생성
```
  \>> <span style="color:#00d4f4">expo init</span> : Expo 프로젝트 생성 명령어  
  <img src="/assets/images/210810_ch02/create_first_expo_project.PNG" style="width:450px; object-fit:contain">  
  <span style="font-size:0.9em">! blank 선택지로 생성</span>

  &nbsp;&nbsp;&nbsp;&nbsp;<div style="font-size:1.2em">(3) <span style="color:cornflowerblue;">Expo 프로젝트 실행</span></div>
  ```
  cd my-first-expo
  npm start
  ```
  <img src="/assets/images/210810_ch02/expo_run_cmd.PNG" style="width:600px; object-fit:contain">  

<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---