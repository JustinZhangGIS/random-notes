# Google Maps #

## 引用google maps javascript库

在html中包含：

	<script type="text/javascript" src="http://maps.google.com/maps/api/js?libraries=places&sensor=false"></script>

即可完成对google maps javascript库的引用，其中`libraries=places`表示使用库中的google places相关接口。`sensor=false`表示不使用GPS定位模块，在具有定位功能的移动设备上可以开启此功能。

## 创建并显示地图

首先在html中建立放置地图的div元素：

	<div id="map" style="width:600px;height:400px"></div>

javascript代码如下(默认使用了jquery)：

    var options = { 
        center: new google.maps.LatLng(31.196784, 121.586530), 
        zoom: 11, 
        mapTypeId: google.maps.MapTypeId.ROADMAP
    }; 
    var map = new google.maps.Map($("#map")[0], options);

简单的两句代码，地图就显示在页面中了。其中，`center`为地图中心位置点，采用经纬度坐标创建。`zoom`为地图的缩放级别，值变大，则地图随之放大，最大值为15。`mapTypeId`为地图模式，`ROADMAP`方式为典型的道路模式，亦可设置为卫星地图模式。