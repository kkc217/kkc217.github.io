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
  * But, Expo에서 제공하는 API만 사용할 수 있음. + 네이티브 모듈을 추가로 만들어서 사용하는 것이 `불가능`함.

  <br/>


  <div style="font-size:1.2em">(1) <span style="color:cornflowerblue;">expo-cli 설치</span></div>

  ```
  npm install --global expo-cli
  ```
  <br/>


  <div style="font-size:1.2em">(2) <span style="color:cornflowerblue;">Expo 프로젝트 생성</span></div>

  ```
  expo init my-first-expo  //'my-first-expo'라는 이름의 프로젝트 생성
  ```
  \>> <span style="color:#00d4f4">expo init</span> : Expo 프로젝트 생성 명령어  
  <img src="/assets/images/210810_ch02/create_first_expo_project.PNG" style="width:600px; object-fit:contain">  
  <span style="font-size:0.9em; opacity:0.7">! blank 선택지로 생성</span>
  <br/><br/>


  <div style="font-size:1.2em">(3) <span style="color:cornflowerblue;">Expo 프로젝트 실행</span></div>

  ```
  cd my-first-expo
  npm start
  ```
  <img src="/assets/images/210810_ch02/expo_run_cmd.PNG" style="width:640px; object-fit:contain">  


### <span style="color:#cd853f">Plus.</span> eject 명령어 <span style="font-size:11px; opacity:0.6">내보내기</span>
  Expo는 개발자의 생산성을 높여준다는 장점이 있지만, 이로 인해 일부 기능을 사용하는데 제한이 있어 `CLI 프로젝트로 변경`해야 하는 상황이 발생하는데 이 때 사용하는 명령어가 eject이다.  
  <span style="font-size:0.9em; opacity:0.7">! Expo는 JavaScript로 앱을 작성하고, eject는 이를 React Native 코드로 바꿔준다.</span>

  * <span style="color:coral; font-weight:bold">주의</span> 변경된 CLI 프로젝트는 다시 Expo 프로젝트로 돌아올 수 없다.
  * 이와 관련된 내용은 뒷부분이나 따로 빼서 다루도록 하겠다.


<br/>

---


### 2. 리액트 네이티브 CLI
  * Expo와 반대의 장단점을 가짐.
  * 필요한 기능이 있을 경우 모듈을 `직접 만들어 사용`할 수 있음.

  <br/>


  <div style="font-size:1.2em">(1) <span style="color:cornflowerblue;">프로젝트 생성</span></div>

  ```
  npx react-native init MyFirstCLI  //'<yFirstCLI'라는 이름의 프로젝트 생성
  ```
  \>> <span style="color:#00d4f4">npx react-native init</span> : CLI 프로젝트 생성  
  <div style="font-size:0.9em"><span style="color:coral; font-weight:bold">주의</span> 리액트 네이티브 CLI로 프로젝트를 생성할 때는 프로젝트 이름으로 `영문`과 `숫자`만 입력할 수 있음.</div>  
  <br/>


  <div style="font-size:1.2em">(2) <span style="color:cornflowerblue;">프로젝트 실행</span></div>

  ```
  cd MyFirstCLI
  npm run android //또는 npm run ios
  ```
  명령 프롬프트 창 하나가 추가로 열리고 Metro가 실행됨.  
  <br/>
  <span style="color:#cd853f">Plus.</span> Metro  
  리액트 네이티브를 위한 자바스크립트 번들러로 네이티브가 실행될 때마다 자바스크립트 파일들을 단일 파일로 컴파일함.  
  
  <span style="color:#cd853f">Plus.</span> 번들러 <span style="font-size:0.8em; opacity:0.7">bundler</span>  
  사용자의 코드와 종속성을 하나의 자바스크립트 파일에 통합하는 도구
  <br/><br/>


  <div style="font-size:1.2em">(3) <span style="color:cornflowerblue;">메인 파일 변경</span></div>

  <img src="/assets/images/210810_ch02/main_file_setup.PNG" style="width:640px; object-fit:contain">

  내용이 변경될 App.js 파일을 src 폴더에 만들어 저장하고 처음에 만들어진 App.js에서 연결시킴.


<br/>

---


## 참고  
* Expo eject 명령어  
https://velog.io/@max9106/React-Native-Expo-eject-v8k2akbliq
https://floydkim.netlify.app/development/2019-05-04-React%20Native%20:%20EXPO%EC%99%80%20%EC%9D%B4%EB%B3%84%ED%95%98%EA%B8%B0/
* 자바스크립트 번들러
https://gist.github.com/jeffminsungkim/9cd5c592dfe39a5eaae93ffcc1818cef


<div style="font-size:13px; text-align:right">
<br/><br/>
END.</div>

---