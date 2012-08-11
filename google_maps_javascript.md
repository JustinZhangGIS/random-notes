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

## Marker

	var marker = new google.maps.Marker({ 
	    position: new google.maps.LatLng(31.196784, 121.586530), 
	    map: map,
	    title: "my first marker",
	    icon: "/static/img/pin.png"
	});

其中`position`指marker的坐标位置，`map`指之前创建的地图，`title`指鼠标停留在marker上是显示的标记，`icon`可以指定自定义的marker图标。

## 信息窗口(infowindow)

infowindow指单击marker后弹出的信息窗口，可以呈现该marker详细的描述信息，该infowindow可以看做一个独立的html页面。创建infowindow的代码如下：

	google.maps.event.addListener(marker, 'click', function() {
	 	 infoWindow = new google.maps.InfoWindow();
	 	 infoWindow.setContent('<p>my first infowindow</p>');
	 	 infoWindow.open(map, marker);
	});

其中，`google.maps.event.addListener`给marker增加了一个`click`事件的响应函数。

#### 注意事项

假如有一个marker数组mk，其中每一个marker都要创建信息窗口，我们采用下面的代码（**注意是错误的**）：

	for(var i = 0; i < mk.length; i++) {
		google.maps.event.addListener(marker, 'click', function() {
			infoWindow = new google.maps.InfoWindow();
		 	infoWindow.setContent('<p>infowindow No. ' + i.toString() + '</p>');
		 	infoWindow.open(map, mk[i]);
		});
	}

代码本身没有问题，用循环为每个marker创建了各自的infowindow，但是代码运行后，单击每个marker后，发现所有的信息窗口显示的内容是一样的。其原因是javascript的closure问题，不做探讨，正确的作法参考下面一段代码：

	var theInfowindow = null;
	for(var i = 0; i < mk.length; i++) {
        (function() {
            google.maps.event.addListener(mk[i], 'click', function() {
                if (!theInfowindow) {
                    theInfowindow = new google.maps.InfoWindow();
                }
                infoWindow.setContent('<p>infowindow No. ' + i.toString() + '</p>');
                infoWindow.open(map, mk[i]);
            });
        })();
    }

其中，创建一个全局变量theInfowindow来存放唯一的google.maps.InfoWindow()对象。通过使用匿名函数来解决closure问题：

	(function() {
		//code
    })();

## infobox库

(待续)
