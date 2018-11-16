# leafletTest 


leaflet 사용법
20181116
***

- 기본 html코드
>스타일,스크립트,div선언
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>leaflet 테스트</title>
<link rel="stylesheet" href="leaflet.css">
<script type="text/javascript" src="leaflet.js"></script>
<style>
#mapid1{
    height:800px;
    margin:0;
}
</style>
</head>
<body>
    <div id="mapid1"></div>
</body>
<script>
</script>
</html>
```


- 기본 맵생성
```
<script>
var map1=L.map('mapid1'//,{
	//center: [37.5464700, 126.9769630],
	//zoom: 15
	//});
	).setView([37.5664700, 126.9779630],17); // 센터를 잡거나 셋뷰로 설정

var osm = L.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{ // 타일레이어 url
    attribution:'&copy; <a href="http://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
}).addTo(map1);
</script>
```


- 마커
>마커만들기
```
var marker1 = L.marker([37.5664700, 126.9779630]).addTo(map1);
var marker2 = L.marker([37.5674700, 126.9779630]).addTo(map1);
```
>마커 바로 표현
```
L.marker([37.5667700, 126.9779330]).addTo(map1)
	.bindPopup("객체로 따로 만들지 않고 생성과 바인딩을 한번에 하는 경우")
	.openPopup();
```


- 도형
>원
```
var circle1 = L.circle([37.5664700, 126.9769630], 35, { // 35 = 원의 크기
    color: 'blue', // 선색상
    fillColor: '#f03', // 채우기 색상
    fillOpacity: 0.5, // 투명도
    radius: 500,
	weight: 1 // 선굵기
	//stroke: false // 도형의 선을 사용하는지여부 기본 true
}).addTo(map1);
```
>다각형
```
var polygon1 = L.polygon([
    [37.5670015, 126.9788278],
    [37.5660831, 126.9778600],
    [37.5660916, 126.9795630]
], {
color: 'gray',
}).addTo(map1);
```


- 팝업, 툴팁
>팝업 - 클릭시 생성되어 유지
```
marker1.bindPopup("'손에 손잡고'가 울려 퍼지는 이곳..").openPopup(); // 팝업을 처음부터 오픈
circle1.bindPopup("원팝업");
polygon1.bindPopup("삼각형팝업");
```
>툴팁 - 마우스오버에 안내되는 형식
```
polygon1.bindTooltip("삼각형");
marker2.bindTooltip("마커+툴팁");
```
>레이어팝업
```
var popup1 = L.popup()
    .setLatLng([37.5675700, 126.9772630])
    .setContent("처음에만 나오는 레이어 팝업")
    .addTo(map1); // openOn도 사용가능 하나가 열리면 다른 팝업이 닫히기 때문에 addTo사용, 처음만표현
```


-이벤트 팝업
```
var popup2 = L.popup();
function onMapClick(e){
	popup2	
		.setLatLng(e.latlng)
		.setContent("클릭한 위치의 좌표는 "+e.latlng+"입니다")
		.openOn(map1);
}
map1.on('click', onMapClick);
```
