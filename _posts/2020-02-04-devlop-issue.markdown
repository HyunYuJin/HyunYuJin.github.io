---
layout: post
title: WebView에서 Map이 안보이던 이유
date: 2020-02-04 15:09:00
description: 🤫 NativeScript-Vue로 개발하면서 겪었던 이슈
---

### WebView에서 Map이 안보이던 이유

<br/>

#### 🧹내가 일주일동안 삽질한 이유?
* Nativescript vue에서 문제를 해결하려고 했다.
* 하지만 다시 잘 생각해보면 나는 엄연히 Android를 개발하고 있었던 것.
* 그러니 Android 개발의 구조를 잘 알아야 할 필요성을 느꼈다.

#### 💡해결 방법
* AndroidManifest에 android:usesCleartextTraffic="true" 추가해주어야 했었다.

<br/>

**그렇다면...🤔**
* Manifest란 무엇일까?
* Android Studio는 무슨 용도로 이용되는 것일까?
* 어떻게 Nativescript-vue와 Android AVD Manager와 연결되는 것일까?

<br/>
#### 🙇Mobile에서 Request하고, Web에서 Response하는 방법🙎
Vue.js에 필요에 따른 REST API 생성하기
* 출발지와 목적지 선택하는 것
* 선택한 경로 표시해주기

<br/>
#### Nativescript vue에서 httpModules 사용해서 통신하기🌐
	httpModule.request({
    	url: "http://115.00.000.0:0000/api/daegu",
    	method: "GET"
  	}).then((response) => {
    	console.log(response);
  	});

<br/>
#### 차량시간 예측
##### 로직
* 사용자가 출발지 A, 도착지 B를 선택한다.
* 걸리는 시간을 보여준다.
* A부터 B까지 이동을 하며 시간이 줄어드는 것을 보여준다.
* 이때 시간을 어떻게 구하느냐가 문제!
* 거속시를 이용해야 한다.
* '시간 = 거리 / 속력' 이기 때문에, 거리를 구하고 현재 차량의 속력과 나누어주어 시간을 구한다.

##### 문제점
* 시간을 잴 때 음수 발생 가능성 있음. -> 미리 측정한 시간 - 현재 시간 이기 때문. (자율주행 차는 속도가 완벽해서 안나올 수 있다 치지만... 만일을 대비)
* 우리는 남은시간을 구하는게 가장 큰 목표이기 때문에
	* '(전체거리 - 현재 거리): *남은 거리* / 속도 = 시간' 이걸로 예측을 해야한다.
	* leaflet은 남은 거리를 보여줄 수 있는 것으로 보임.
		1. 현유진이 해야할 일: totalTime이 무엇인가?
		2. 이게 나오면, 내 속도 값을 입력할 수 있는지?
		3. 남은거리 알아오기⭐️
		4. Routing machine 에서 waypoint를 bus를 넣어두고 waypoint와 도착치의 거리 구할수 있는지? ( 경로내부에 버스 위치 )
			* 자율 주행의 차의 경우 속도가 정해져있기 때문에 leaflet에서 구한 time의 시간과 차이가 있을 수 있다.

##### 구현 순서
1. 셔틀과 출발지간의 거리를 구하기
2. 속력과 시간 구하기
3. L.Routing.control을 하나 더 만들고 waypoints 지정해주고 totalDistance를 뽑기! ( 경로외부에 버스 위치 ) => 이게 남은 거리
4. 셔틀이 사용자가 원하는 출발지로 이동 중일때는 이동중 아이콘 표시
5. 버스와 출발지 사이의 거리를 구하기

<br/>
#### APK 파일
##### APK 파일이란?
* Android Package의 줄임말
* 안드로이드 플랫폼을 가지고 있는 스마트폰에 어플리케이션을 설치할 수 있도록하는 포맷형태
* 일반 유저의 경우는 앱 스토어에서 설치가 가능하지만 개발자의 경우는 파일로 받거나 인터넷에서 다운을 받아 사용할 경우, 이 파일 다운 받는다.
* zip 형식으로 압축이 되어있다.
* 이 파일이 있으면 앱 스토어에서 다운받지 않아도 어플을 설치 할 수 있다.

##### Nativescript에서 APK 파일 추출하는 방법
<a href="https://nativescript.org/blog/steps-to-publish-your-nativescript-app-to-the-app-stores/">[참고사이트]</a>
1. 애플리케이션의 실행이 가능한 Android 파일을 생성 (.apk - Nativescript CLI를 사용해서 생성)
2. tns run을 이용해서 .apk파일을 Android Emulator에 해당 파일을 설치한다.
3. Release 버전을 만드려면 두가지 작업이 필요 - keystore 또는 .jks(Java 키 저장소) 파일 작성
	1. Android 문서에는 .keystore 파일을 작성하는 방법에 대한 몇가지 옵션이 있다.
	2. keytool 명령: Java JDK Nativescript에 포함되어 있으므로 개발 시스템의 명령 줄에서 사용이 가능
4. 빌드하는 동안 이 파일을 사용하여 앱에 서명하기

<br>

#### 🚎셔틀 호출하기🤙
호출하기 버튼을 누르면 셔틀 아이콘 위에 남은 시간 표시

* 전체 경로를 보여주는 페이지와 셔틀 호출 페이지 따로 지정해주기
* 이렇게 되면 경로도 따로 지정해줄 수 있다.
<code>tns build android --release --key-store-path C:\Users\gusdb\task\on-demand\tasio --key-store-password spring --key-store-alias tasio --key-store-alias-password spring
keytool -genkey -v -keystore tasio.jks -keyalg RSA -keysize 2048 -validity 10000 -alias tasio</code>
