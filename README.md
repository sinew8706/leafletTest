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

var osm = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{ // 타일레이어 url
    attribution:'&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
}).addTo(map1);
// 좌표는 [세로,가로] [↑,→]로 갈수록 커진다
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
    .addTo(map1);
    // 마커에는 openOn 도 사용가능하지만 팝업이 열리면 기존 팝업이 닫히기 때문에 addTo를 사용. 한번만 표현
```


>이벤트 팝업
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


***
20181205

- 아이콘 및 그림자
> 아이콘 설정
```
var PIcon = L.icon({
	iconUrl : 'images/img3.png', // 이미지 url
	shadowUrl : 'images/img6.png', // 그림자 url
	
	iconSize : [80, 80],
	// [가로,세로]아이콘 사이즈 - 기준점 기준 오른쪽,아래로 이미지가 그려진다(이미지의 왼쪽상단이 시작)
	shadowSize : [80, 80],
	// 그림자사이즈 - 기준점 기준 오른쪽,아래로 이미지가 그려진다(이미지의 왼쪽상단이 시작)
	iconAnchor : [40, 80],
	// [가로,세로] 아이콘의 위치조정 - 기존점 기준 왼쪽,위로 이동(아이콘사이즈 기준 [1/2,1] 정도)
	shadowAnchor : [40, 50],
	// [가로,세로] 그림자아이콘의 위치조정 - 기준점 기준 왼쪽,위로 이동
	popupAnchor : [0, -70]
	// [가로,세로]팝업이 열리는 위치 조정 - 기준점 기준 오른쪽, 아래로 이동
});
```

> 아이콘 마커 생성
```
L.marker([37.565569, 126.974788], {icon : PIcon}).addTo(map1)
	.bindPopup("페페페펭귄")
	.openPopup();
```

> 다중 아이콘 선언
```
var xmasIcon = L.Icon.extend({
    options: {
        shadowUrl: 'images/img6.png',
        iconSize:     [80, 80],
        shadowSize:   [80, 80],
        iconAnchor:   [40, 80],
        shadowAnchor: [40, 50],
        popupAnchor:  [0, -70]
    }
});

// 아이콘 객체 선언
var Icon1 = new xmasIcon({iconUrl: 'images/img5.png'}),
	Icon2 = new xmasIcon({iconUrl: 'images/img7.png'}),
	Icon3 = new xmasIcon({iconUrl: 'images/img8.png'});
```

> 다중아이콘 마커 생성
```
L.marker([37.567321, 126.972417], {icon: Icon1}).addTo(map1).bindPopup("교회아이콘");
L.marker([37.564965, 126.97188], {icon: Icon2}).addTo(map1).bindPopup("추카포카");
L.marker([37.566241, 126.971301], {icon: Icon3}).addTo(map1).bindPopup("김연아쨩");
```

