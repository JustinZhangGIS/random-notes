
#GPS定位坐标校正

国内的GPS定位结果直接显示在地图上时，可实际位置有偏差，可以使用baidu地图提供的api进行校正。

## api

	http://api.map.baidu.com/ag/coord/convert?from=0&to=4&x=116.308992&y=40.059225

其中

1. `x`表示经度，`y`表示纬度；
1. `from`和`to`对应的值分别是：0--真实坐标；2--google坐标；4--baidu坐标。

## 例子

假设GPS定位结果为：纬度31.196784，经度121.586530。我们需要把该定位结果显示在google maps上，那么：

	http://api.map.baidu.com/ag/coord/convert?from=0&to=2&x=121.586530&y=31.196784

返回结果是：
	{"error":0,"x":"MTIxLjU5MDczOTIwMzU2","y":"MzEuMTk0NTc0NjUyNzc4"}。

该结果经过base64加密，转换后结果为(121.59073920356, 31.194574652778)。