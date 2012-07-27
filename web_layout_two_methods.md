# 页面布局的两种方法

## 简介

#### 页面html

	<body>
	  <header id="title">
	    <h1>Layout Example</h1>
	  </header>
	  <div id="content">
	    <div id="listView">
	      <ul class='item'></ul>
	    </div>
	    <div id="mapView"></div>
	  </div>
	</body>

#### header css
	
	#title 
	{
	  overflow: hidden;
	  height: 40px;
	}


## 方法一：动态宽高度设置

#### CSS

	#content{
	  position:absolute;
	  overflow:hidden;
	  width:100%;
	  top: 40px;
	}
	
	#listView {
	  float: left;
	  width: 360px;
	  overflow-y: scroll;
	  overflow-x: hidden;
	}
	
	#mapView { 
	  position: absolute;
	  left: 360px;
	  right: 0;
	  border-left: solid 1px #CCC;
	}

#### javascript

	$(document).ready(function() {
		(function() {
	        window.onresize = arguments.callee;
	        var navbarHeight = 40;
	        h = $(window).height() - navbarHeight;
	        w = $(window).width() - $("listView").outerWidth();
	        $('#mapView').height(h);
	        $('#mapView').width(w);
	        $('#listView').height(h);
	    })();
	});

## 方法二：CSS3弹性盒模型（Flexible Box Model）

本方法参考Javascript Web Apllications一书。

弹性盒模型是CSS3的特性，主要作用是引入GUI设计方法进行页面元素的布局。Flexible Box Model参考：

[http://www.html5rocks.com/en/tutorials/flexbox/quick/](http://www.html5rocks.com/en/tutorials/flexbox/quick/)

[http://www.denisdeng.com/?p=938](http://www.denisdeng.com/?p=938)

#### CSS

	#content{
	  position:absolute;
	  overflow:hidden;
	  left: 0;
	  right: 0;
	  bottom: 0;
	  top: 40px;
	
	  display: -webkit-box;
	  -webkit-box-orient: horizontal;
	  -webkit-box-align: stretch;
	  -webkit-box-pack: left;

	  display: -moz-box;
	  -moz-box-orient: horizontal;
	  -moz-box-align: stretch;
	  -moz-box-pack: left;
	}

	#listView {
	  width: 360px;
	  overflow-y: scroll;
	  overflow-x: hidden;
	
	  -webkit-box-flex: 0;
	  -moz-box-flex: 0;
	  box-flex: 0;
	} 

	#mapView { 
	  border-left: solid 1px #CCC;

	  -webkit-box-flex: 1;
	  -moz-box-flex: 1;
	  box-flex: 1;
	}