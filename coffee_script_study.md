# CoffeeScript语法要点

参照[CoffeeScript](http://jashkenas.github.com/coffee-script/)主页。

简写：

	`CoffeeScript -> cs`
	`JavaScript -> js`


## 函数定义

cs:

	square = (x) -> x * x
	cube   = (x) -> square(x) * x
	console.log square 3
	console.log square(3)

js:

	var cube, square;
	
	square = function(x) {
	return x * x;
	};
	
	cube = function(x) {
	return square(x) * x;
	};
	
	console.log(square(3));
	
	console.log(square(3));


## 对象和数组

cs:

	song = ["do", "re", "mi", "fa", "so"]
	
	singers = {Jagger: "Rock", Elvis: "Roll"}
	
	bitlist = [
	  1, 0, 1
	  0, 0, 1
	  1, 1, 0
	]
	
	kids =
	  brother:
	    name: "Max"
	    age:  11
	  sister:
	    name: "Ida"
	    age:  9

	console.log song[0]
	console.log singers.Jagger
	console.log bitlist[3]
	console.log kids.brother.name

js:

	var bitlist, kids, singers, song;
	
	song = ["do", "re", "mi", "fa", "so"];
	
	singers = {
	  Jagger: "Rock",
	  Elvis: "Roll"
	};
	
	bitlist = [1, 0, 1, 0, 0, 1, 1, 1, 0];
	
	kids = {
	brother: {
	  name: "Max",
	  age: 11
	},
	sister: {
	  name: "Ida",
	  age: 9
	}
	};
	
	console.log(song[0]);
	
	console.log(singers.Jagger);
	
	console.log(bitlist[3]);
	
	console.log(kids.brother.name);

## 条件语句

cs: 
