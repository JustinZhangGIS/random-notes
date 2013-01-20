# JavaScript语法要点

参考[http://www.codecademy.com/zh/glossary/javascript](http://www.codecademy.com/zh/glossary/javascript)。

## 变量

变量在编程语言中起到存储值的作用。变量可以具有全局或者局部的作用域。变量名是大写敏感的，首字母必须是字母、下划线(_)或者美元符($)。

	var name = value;

其中，`var`为定义变量的关键字，`name`为变量名，`value`为任意JavaScript值，或者其他变量。

#### 例子
	var x = 1;
	var myName = "Bob";
	var hisName = myName;

## 数字运算

1. 加减乘除*, /, -, +
1. 取模%
1. 自增++
1. 自减--

#### 例子

	14 % 9 // returns 5
	variable1++
	++variable2
	variable1--
	--variable2

## 字符串

	"string of text"
	'string of text'

#### 例子

	"some" + "text"; // returns "sometext"
	var first = "my";
	var second = "string";
	var union = first + second; // union variable has the string "mystring"
	"original string".replace("original", "replaced"); // returns "replaced string"
	"my name".toUpperCase(); // Returns "MY NAME"
	"MY NAME".toLowerCase(); // Returns "my name"
	"adventures".substring(2,9); // Returns "venture"

## 布尔值

	true
	false

#### 布尔运算

	expression1 && expression2
	expression3 || expression4
	!expression5

#### 关系运算

	=== (Equal)
	!== (Not equal)
	> (Greater than)
	>= (Greater than or equal)
	< (Less than)
	<= (Less than or equal)

#### 例子

	var x = 10;
	var y = 10;
	var z = 12;
	(x > 10) || (z < 10); // false
	x === y; // true
	x !== z; // true


## 注释

	// This is a single line comment.
	/*Comment1
	  Comment2
	  .
	  .
	  Comment3 */

## 函数

	var name = function (parameter1, parameter2, ..., parameterN) {
	  statement1;
	  statement2;
	  .
	  .
	  .
	  statementN;
	};

其中，`name`为函数名，`function`为函数定义关键字，`parameter`向函数传递参数的变量名，`statement`为Javascript语句。

#### 例子

	var greeting = function (name) {
	  console.log("Hello " + name);
	};

	greeting('Hello');

## if语句

#### if

	if (condition) {
	  statement1;
	  statement2;
	  .
	  .
	  .
	  statementN;
	}

#### 例子
	if (answer === 42) {
	  console.log('Told you so!');
	}

#### if ... else

	if (condition) {
	  statement1;
	} else {
	  statement2;
	}

#### 例子

	if (name.length > 0) {
	  console.log("Please enter your name.");
	} else {
	  console.log("Hello " + name);
	}

#### else if

	if (condition1) {
	  statement1;
	} else if (condition2) {
	  statement2;
	} else {
	  statement3;
	}

#### 例子

	if (someNumber > 10) {
	  console.log("Numbers larger than 10 are not allowed.");
	} else if (someNumber < 0) {
	  console.log("Negative numbers are not allowed.");
	} else {
	  console.log("Nice number!");
	}



	