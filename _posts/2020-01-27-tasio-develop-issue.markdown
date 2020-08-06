---
layout: post
title: 📱 타시오 온디멘드 개발
date: 2020-01-27 23:45:13
description: 신생아 유진이가 신나게 개발에게 혼났던 내용들
---

#### 사용 기술

<br/>

##### Front-end
<ul>
	<li>Vue.js</li>
	<li>Vuetify</li>
</ul>

##### Back-end
<ul>
	<li>Django - station, vehicle API 사용</li>
	<li>Google Firestore - 회원 관리(휴대폰 인증)</li>
	<li>아임포트 - 결제 [KG 모빌리언스]</li>
</ul>

##### Native
<ul>
	<li>Android Webview</li>
</ul>

<br/>

#### Map 띄워주기
##### /utils/map.js

	import "leaflet/dist/leaflet.css"
	import $L from "leaflet"

	const createMap = (divId, options) => {
			let map = $L.map(divId, options);
			return map;
	};

	const createTileLayer = async (map, url, options) => {
			let tileLayer = await $L.tileLayer(url, options);
			tileLayer.addTo(map);
			return tileLayer;
	}



	this.map = this.$utils.map.createMap('map-container', {
			zoomControl: false,
			attributionControl: false
	});

	this.$utils.map.createTileLayer(this.map, this.OSMUrl, {});
	this.map.setView([35.812484, 126.4101], 15);

* div에 'map-container'이라는 id를 부여해준다.
* DOM이 생성되면 id를 선택하여 map 객체를 인스턴스화 한다.
* 나는 3개의 제어 옵션을 지정해주었다.
	* zoomControl: 확대/축소를 컨트롤 할 수 있는 UI를 표시할 것인가
	* attributionControl: 속성 제어를 할 수 있는 UI를 표시할 것인가
* 지도에 tileLayer를 로드하고 표시하기
* 원하는 OSRM URL를 선택하고 필요에 따라 옵션을 지정하면 tileLayer 객체를 인스턴스화 한다.
* setView([latitude, longitude], zoom)로 지도의 center를 지정해준다.

<br />

#### 정류장 표시 - '/api/station'
##### /utils/map.js
Back-end에서 만들어준 API를 이용해서 군산 지역의 Station을 표시해준다.

	const createIcon = options => {
			return $L.icon(options);
	}
	const createMakerByXY = (map, coordinate, options = {}) => {
			let marker = $L.marker($L.latLng(coordinate[0], coordinate[1]), options);
			marker.addTo(map);
			return marker;
	}



	let gifIcon = this.$utils.map.createIcon({
			iconUrl: require("../../assets/station_icon.svg"),
			iconSize: [12, 12]
	});

	for (let i = 0; i < this.waypoints.length; i++) {
			this.$utils.map.createMakerByXY(this.map, [this.waypoints[i].lat, this.waypoints[i].lng], {
					icon: gifIcon
			});
	}

* 나는 axios를 이용해서 site가 1인(군산) 지역에 해당하는 데이터만 뽑아 waypoints에 저장했다.
* marker: 지도에서 아이콘을 표시하는데 사용되는 확장 Layer로, 위도/경도와 필요에 따른 옵션을 지정하면 marker 개체를 인스턴스화 한다.
* 그래서 waypoints에서 latitude와 longitude를 뽑아내 icon 옵션에 원하는 아이콘의 이미지를 넣어주었다.

<br />

#### 셔틀 표시 - '/api/vehicles'
##### /utils/map.js
Back-end에서 만들어준 API를 이용해서 군산 지역의 Station을 표시해준다.

	const createIcon = options => {
			return $L.icon(options);
	}
	const createMakerByXY = (map, coordinate, options = {}) => {
			let marker = $L.marker($L.latLng(coordinate[0], coordinate[1]), options);
			marker.addTo(map);
			return marker;
	}



	var vehicleIcon = this.$utils.map.createIcon({
			iconUrl: require("../../assets/vehicle1.svg"),
			iconSize: [32, 32]
	});

	this.vehicle[i] = await this.$utils.map.createMakerByXY(this.map, [vehicle_data[i].lat, vehicle_data[i].lon], {
			draggable: false,
			icon: vehicleIcon
	});



	setInterval(async function () {
			axios.get('/api/vehicles/')
					.then(response => {
							var vehicle_data = response.data.sort(function (a, b) {
									return a.id < b.id ? -1 : 1;
							})
							var vehicleCount = Object.keys(vehicle_data).length;
							for (let i = 0; i < vehicleCount; i++) {
									if (vehicle_data[i].site == 1) {
											if (vehicle_data[i].lat != null || vehicle_data[i].lon != null || vehicle_data[i].lat != undefined || vehicle_data[i].lon != undefined) {
													this.vehicle[i].setLatLng([vehicle_data[i].lat, vehicle_data[i].lon]);
											}
									}
							}
					}).catch(error => {
							console.log(error);
					})
	}.bind(this), 1000);

* 마찬가지로 axios를 이용해서 site가 1인(군산) 지역에 해당하는 셔틀 데이터만 뽑아 vehicle_data에 저장했다.
* vehilce에 셔틀의 marker를 저장하고 이를 marker의 위치가 변경되면 새 좌표로 이동시킬 수 있는 setLatLng을 이용해 실시간 라우터로 받아오는 셔틀의 latitude와 longitude 값을 1초마다 update 한다.

<br />

#### 경로 그려주기
##### /utils/map.js

	const createRouting = (map, options = {}) => {
			let control = $L.Routing.control(options)
			control.addTo(map);
			return control;
	}

	addRouting(waypoints) {
			control = this.$utils.map.createRouting(this.map, {
					waypoints: waypoints,
					serviceUrl: 'https://osrm.xxxxxxxxxxx.com/route/v1',
					addWaypoints: false,
					draggableWaypoints: false,
					showAlternatives: false,
					routeWhileDragging: false,
					lineOptions: {
							draggable: false,
							styles: [{
									color: '#E51973',
									weight: 5
							}]
					},
					autoRoute: true,
					createMarker: function () {
							return null;
					}
			});
	},

Leaflet-Routing-Machine을 이용해서 경로를 그려주었다.

앞서 latitude와 longitude를 넣어준 waypoints가 원하는 경로이고 이를 표시해 줄 것이다.

* 표시해줄 map과 옵션을 지정하면 routing control 개체를 인스턴스화 한다.
* 출발지와 도착지 사이의 경로를 표시해줄 waypoints에 앞서 저장해주었던 waypoints를 값으로 지정한다.
* serviceUrl을 지정해준다.
* lineOptions의 styles에 선의 색상과 굵기를 지정한다.
* 그 외의 옵션
	* addWaypoints: 사용자가 새로운 waypoints를 추가
	* draggableWaypoints: map에서 waypoints를 드래그
	* showAlternatives: polyLine이 기본 polyLine과 동시에 사용 가능한 경우 map에 표시
	* routeWhileDragging: marker를 드래그하는 동안 경로가 계속 다시 계산
	* autoRoute: true인 경우 경유지가 변경 될 때마다 경로가 자동으로 계산
	* createMarker: waypoints에 사용할 marker 만들기
