# 리액트 네이티브 복습

RN은 React-dom의 대안이다
네이티브 기기 API를 키긴 해야되지만 그래도 자바스크립트에서 쓰도록 할 수있음.

## 리액트 네이티브 내부 살펴보기 

- View
웹브라우저 / 네이티브 컴포넌트 ios , android , react native jsx 다음과 같이 컴파일

![image](https://github.com/MAGHC/TIL-2024-/assets/89845540/6777b0ef-0454-4896-b285-953ccbc03257)

- Logic

Ui와 요소와 다르게 논리는 컴파일 되지 않는다. 
자바스크립트 코드는 리액트 네이티브가 호스트한 대로 실행하게 됨.
네이티브앱 안에 리액트 네이티브를 이용해서 돌아가게 만듦

## Expo Cli vs React Native Cli 

두 가지 툴 모두 리액트 네이티브 프로젝트를 만들고 테스팅 기기 및 시뮬레이터에 React Native앱을 실행할 뿐만 아니라 
앱을 구축하고 앱스토어 배포하게 해줌.

실질적으로 앱을 구축하고 배포 가능한 패키ㄱ지로 만들어. 업로드하기 위해 꼭 필요한 툴. 

Expo에서 언제든 네이티브로 바꿀수있음. 

Expo Cli 보다 Rn이 더 불편하다.  단지 있는건 Expo이전에 존재해서. 

순수 Rn 워크플로우인 Rn cli의 장점은 자바나 Ojective-C , Swift , kotlin 과 같은 네티이브 소스 코드와 통합하기가 비교적 쉽다. 

__JS코드와 네이티브 기기 소스 코드를 합쳐야 한다면 Rn cli 를 쓰는게 좋다.__ 


## 프로젝트 생성 / 실행

npx create-expo-app

npm start 

expo 앱 => scan 


## 로컬 개발 환경 설정 

Android studio => create device => more action => virtual device => play store가 에뮬레이터에 포함되어있어야 expo 앱 다운로드해서 미리보기 가능 

ios => appstore = > xcode => preferences 메뉴 => location => cli tools에 버전선택 

윈도우는 아이폰 테스트불가. 

a를누르면 안드 i를누르면 ios 시뮬레이터 실행 



# 네이티브 기초 

RN의 본질은 내장된 핵심 컴포넌트를 다루는 것 
전체적인 앱 ui와 Ui를 구성하는 커스텀 컴포넌트는 핵심 컴포넌트를 합쳐서 만드는 것. 

RN에 의해 핵심 컴포넌트가 네이티브 Ui요소로 변환됨.

핵심 컴포넌트를 합쳐서 모든 컴포넌트를 구성하는 것임. 

리액트 네이티브에는 CSS가 없음. 존재도안한다.  대신 스타일을 사용.

인라인 스타일을 사용하거나 StyleSheet 객체를사용함. 

자바스크립트에서 스타일 작성. 추가 언어는 없다. 

모든 UI와 요소는 APP 컴포넌트 산하에 이루어져야함.


<Text>없이 View로만 하로 바로 에러 

div랑 거의 비슷한 View에는 텍스트를 넣을수없음.

div는 일반적으로는 콘텐츠를 담거나 컨테이너 구축에 사용됨. 
텍스트 상자/ 버튼 /이미지 같은것들도 넣을 수 있음 

View는 컨테이너다. View안에 네스팅도 가능. 

웹이랑 달리 네이티브는 요소를 임포트해야됨. 

오 버튼 까서 title 넣으니까 각각 스타일이달라; 



