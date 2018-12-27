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
<!-- 맵css -->
#mapid1{
    height:800px;
    margin:0;
}

<!-- 행정구역 정보div css -->
.info {
    padding: 6px 8px;
    font: 14px/16px Arial, Helvetica, sans-serif;
    background: white;
    background: rgba(255,255,255,0.8);
    box-shadow: 0 0 15px rgba(0,0,0,0.2);
    border-radius: 5px;
}
.info h4 {
    margin: 0 0 5px;
    color: #777;
}
</style>
</head>
<body>
    <div id="mapid1"></div>
</body>
<script src="jquery-2.1.4.js"></script>
<script>
</script>
</html>
```


- 기본 맵생성
```
<script>
var osm = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{ // 타일레이어 url
	id : 'base',
    attribution:'&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
});//.addTo(map1);
// 좌표는 [세로,가로] [↑,→]로 갈수록 커진다

var osm2 = L.tileLayer('https://{s}.tile.thunderforest.com/cycle/{z}/{x}/{y}.png',{ // 타일레이어 url
    id:'cycle',
	attribution:'&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
}); /*.addTo(map1);*/

var osm3 = L.tileLayer('https://{s}.tile.thunderforest.com/transport/{z}/{x}/{y}.png',{ // 타일레이어 url
    id:'transport',
	attribution:'&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
});

var osm4 = L.tileLayer('https://{s}.tile.thunderforest.com/landscape/{z}/{x}/{y}.png',{ // 타일레이어 url
    id:'landscape',
	attribution:'&copy; <a href="https://openstreetmap.org">OpenStreetMap</a> Contributors' , // 우측 하단 출처
	maxzoom:20, // 줌최대치
	minzoom:10, // 줌최소치
});

var map1=L.map('mapid1',{
	center: [37.5464700, 126.9769630], // 센터
	zoom: 17, // 줌설정
	layers: [osm] // 최초 레이어
});
// 좌표는 [세로,가로] [↑,→]로 갈수록 커진다
</script>
```


- 마커
>마커생성
```
var marker1 = L.marker([37.5664700, 126.9779630]);
var marker2 = L.marker([37.5674700, 126.9779630]);
```
>마커 생성과 바로 표현
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
marker1.bindPopup("'손에 손잡고'가 울려 퍼지는 이곳..");
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
>아이콘 설정
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

>아이콘 마커 생성
```
L.marker([37.565569, 126.974788], {icon : PIcon}).addTo(map1)
	.bindPopup("페페페펭귄")
	.openPopup();
```

>다중 아이콘 선언
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

>다중아이콘 마커 생성
```
L.marker([37.567321, 126.972417], {icon: Icon1}).addTo(map1).bindPopup("교회아이콘");
L.marker([37.564965, 126.97188], {icon: Icon2}).addTo(map1).bindPopup("추카포카").openPopup(); // 팝업을 처음부터 오픈
L.marker([37.566241, 126.971301], {icon: Icon3}).addTo(map1).bindPopup("김연아쨩");
```

***
20181219

- GeoJson
>feature 포인트(점) 선언
```
var geojsonFeature1 = {
    "type": "Feature",
    "properties": {
        "name": "광화문",
        "amenity": "광화문근처 정류장",
        "popupContent": "팝업 컨텐츠"
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.975474, 37.570059] // [가로,세로]
    }
};
var geojsonFeature2 = {
    "type": "Feature",
    "properties": {
        "name": "광화문2",
        "amenity": "광화문근처 정류장2",
        "popupContent": "팝업 컨텐츠2"
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.978457, 37.570246] // [가로,세로]
    }
};
var geojsonFeature3 = {
    "type": "Feature",
    "properties": {
        "name": "광화문우체국",
        "amenity": "광화문근처 우체국 포인트",
        "popupContent": "우체국 팝업 컨텐츠"
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.978017, 37.569795] // [가로,세로]
    }
};
```

>feature 라인 선언
```
var testLine1 = [{
    "type": "LineString",
    "coordinates": [ [126.978086937297, 37.5703003037838], [126.97767, 37.57031], [126.9776, 37.57031],[126.970592311981, 37.569436980203] ]
}, {
    "type": "LineString",
    "coordinates": [ [126.96689, 37.56614], [126.96537, 37.56428], [126.96467, 37.56331], [126.96436, 37.56283], [126.962639867182, 37.5600616334415] ]
}];
// 스타일 선언
var testStyle = {
	"color" : "#0000ff",
	"weight" : 5,
	"opacity" : 0.6
};
```

>폴리곤 선언
```
var poly1 = [{
    "type": "Feature",
    "properties": {"col": "type1"},
    "geometry": {
        "type": "Polygon",
        "coordinates": [[
            [126.978629, 37.57309],
            [126.978714, 37.572572],
            [126.979197, 37.57247],
            [126.979122, 37.573073],
            [126.978629, 37.57309]
        ]]
    }
}, {
    "type": "Feature",
    "properties": {"col": "type2"},
    "geometry": {
        "type": "Polygon",
        "coordinates": [[
            [126.980978, 37.574264],
            [126.980485, 37.573771],
            [126.980903, 37.573703],
            [126.981107, 37.573363],
			[126.98144, 37.574119],
            [126.980978, 37.574264]
        ]]
    }
}];
```


>레이어에 추가
```
// 포인트 추가
//L.geoJSON(geojsonFeature1).addTo(map1);

// 빈객체를 만들어 데이터를 추가하는 방식
var ftLayer = L.geoJSON().addTo(map1);
ftLayer.addData(geojsonFeature2);

// 라인과 스타일로 레이어에 추가
L.geoJSON(testLine1, {
    style: testStyle
}).addTo(map1);

// 레이어에 추가 및 스타일 개별 설정
L.geoJSON(poly1, {
    style: function(feature) {
        switch (feature.properties.col) {
            case 'type1': return {color: "#ff0000"};
            case 'type2': return {color: "#0000ff"};
        }
    }
}).addTo(map1);
```

>포인트 마커 옵션(pointToLayer)
```
var geojsonMKOption = {
	radius : 10,
	color : "#00ff00",
	weight : 3,
	opacity : 1,
	fillOpacity : 0.8
};

// 포인트 마커 옵션을 레이어에 추가
L.geoJSON(geojsonFeature3, {
	pointToLayer : function (feature, latlng) {
		return L.circleMarker(latlng, geojsonMKOption);
	}
}).addTo(map1);
```
>객체들의 팝업설정 함수
```
function onEachFeature1(feature, layer){
	if(feature.properties && feature.properties.popupContent){
		layer.bindPopup(feature.properties.popupContent);
	}
};
function onEachFeature2(feature, layer){
	if(feature.properties && feature.properties.popupContent){
		layer.bindTooltip(feature.properties.popupContent);
	}
};

// 함수를 사용해서 포인트에 팝업추가
L.geoJSON(geojsonFeature1,{
	onEachFeature : onEachFeature1
}).addTo(map1);
```

>filter로 표현 제어
```
var filterFeatures = [{
    "type": "Feature",
    "properties": {
        "name": "필터포인트1",
        "popupContent": "산책코스1",
		"showOn" : true
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.977051, 37.57318] // [가로,세로]
	}
}, {
    "type": "Feature",
    "properties": {
        "name": "필터포인트2",
        "popupContent": "산책코스2",
		"showOn" : false
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.977073, 37.573503] // [가로,세로]
	}
}, {
    "type": "Feature",
    "properties": {
        "name": "필터포인트3",
        "popupContent": "산책코스3",
		"showOn" : true
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.977041, 37.573843] // [가로,세로]
	}
}, {
    "type": "Feature",
    "properties": {
        "name": "필터포인트4",
        "popupContent": "산책코스4",
		"showOn" : true
    },
    "geometry": {
        "type": "Point",
        "coordinates": [126.977019, 37.574183] // [가로,세로]
	}
}];

L.geoJSON(filterFeatures, {
	filter : function(feature, layer){ // 필터함수
		return feature.properties.showOn;
	},
	onEachFeature : onEachFeature2 // 툴팁함수
}).addTo(map1);
```

- 레이어로 행정구역 나누기와 표현하기
>최상단 js파일 연결
```
<script type="text/javascript" src="geoJson/geoJsonSample.js"></script>
<script type="text/javascript" src="geoJson/seoul.geojson"></script>
<!-- 참고사이트: https://hiseon.me/2018/07/12/south-korea-shp/ -->
<!-- seoul.geojson 출처 http://www.juso.go.kr/addrlink/addressBuildDevNew.do?menu=bsin -->
```

>미국주 샘플 - geoJsonSample.js
```
var geojson1 = L.geoJson(statesData).addTo(map1);
```

>색과 스타일 설정
```
// 값에 따른 색설정함수
function getColor(d) {
    return d > 2	? '#800026' :
           d > 1 	? '#BD0026' :
           d > 0.5	? '#E31A1C' :
           d > 0.2	? '#FC4E2A' :
           d > 0.15	? '#FD8D3C' :
           d > 0.1	? '#FEB24C' :
           d > 0.05	? '#FED976' :
                      '#FFEDA0';
}

// 지도 레이어에 스타일 추가 함수
function style(feature) {
    return {
        //fillColor: '#FFEDA0', // properties값으로 함수 사용 가능
        fillColor: getColor(feature.properties.BAS_AR),
		weight: 2,
        opacity: 1,
        color: 'white',
        dashArray: '3',
        fillOpacity: 0.4
    };
}
```

>마우스 이벤트
```
// 마우스 오버 설정
function overFeature(e) {
    var layer = e.target;

    layer.setStyle({
        weight: 5,
        color: '#666',
        dashArray: '',
        fillOpacity: 0.6
    });

    if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
        layer.bringToFront();
    }
	
	 info.update(layer.feature.properties);
}

// 마우스 아웃 설정
function outFeature(e) {
    geojson1.resetStyle(e.target);
	info.update();
}

// 마우스 클릭 설정
function clickFeature(e) {
    map1.fitBounds(e.target.getBounds());
}

// 마우스 이벤트 선언
function mouseEvent(feature, layer) {
    layer.on({
        mouseover: overFeature,
        mouseout: outFeature,
        click: clickFeature
    });
}

// 마우스 이벤트 적용
geojson1 = L.geoJson(seoul1, { // seoul1 -> seoul.geojson:서울시 샘플데이터
    style: style,
    onEachFeature: mouseEvent
}).addTo(map1);
```

>해당구역의 정보 보여주기
```
var info = L.control();

info.onAdd = function (map) {
    this._div = L.DomUtil.create('div', 'info'); // div에 "info" 클래스 추가
    this.update();
    return this._div;
};

// 우측상단 div 내용
info.update = function (props) {
    this._div.innerHTML = '<h4>서울시 행정구역</h4>' +  (props ?
        '<b>' + props.SIG_KOR_NM + ' ' + props.BAS_MGT_SN + '</b><br/>' + props.BAS_AR + ' (㎢)'
        : '마우스를 올리세요.');
};
info.addTo(map1);
```

***
20181227

- 레이어와 그룹 컨트롤

>마커를 모아서 관리
```
var markers1 = L.layerGroup(
	[marker1,marker2,circle1,polygon1] // 묶어서 관리
);

// 마커 셀렉트박스
var overMarkers = {
	"마커": markers1,
	"행정구역": geojson1 // 레이어관리 가능
};
```

>기본레이어 컨트롤
```
// 레이어 셀렉트박스 - 상단의 osm설정필요
var baseMaps = {
	"기본": osm,
	"자전거": osm2,
	"수송": osm3,
	"조경": osm4
};

// 레이어와 마커그룹 컨트롤
L.control.layers(baseMaps, overMarkers).addTo(map1);
```
