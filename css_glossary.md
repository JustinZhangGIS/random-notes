#CSS

### 引用

	<link rel="stylesheet" href="style.css" />

### 选择器

#### class

	<div class='code'>...</div>
	
	.code {
	  font-family: monospace;
	}

#### div

	<div id='box'>...</div>
	
	#box {
	  width: 100px;
	}

#### 选择多个元素

	selector, selector {declaration}

#### 后代

子后代、孙后代...

	parent descendant: {...}

#### 直接后代

仅限子后代

	parent > child: {...}

#### 通配符*

选中DOM中的所有元素

	* { color: green; }

#### 伪元素

下面代码将父元素的第一个p段落颜色设置为green

	p:first-child {
		color: green;
	}

最后一个p段落

	p:last-child {
		color: green;
	}

p段落的hover

	p:hover {
		color: yellow;
	}

### 注释

	// This is a single line CSS comment.
	/* This is a multi-line
	   CSS comment. */

### 


### border

	border: [style] [width] [color];

### padding

	padding: [top] [right] [bottom] [left];

