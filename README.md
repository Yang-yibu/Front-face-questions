## 面試題小結（！！）
### 一、javascript
+ 1、实现一个formDate方法，其能将一个Date日期对象，格式化为yyy-MM-dd HH:mm:ss的字符串形式

```javascript

	'use strict';
	// 对Date的扩展，将 Date 转化为指定格式的String
	// 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符，
	// 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字)
	// 例子：
	// (new Date()).Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423
	// (new Date()).Format("yyyy-M-d h:m:s.S")      ==> 2006-7-2 8:9:4.18
	Date.prototype.format = function(format) { //author: meizz
	  let o = {
	    "M+": this.getMonth() + 1, //月份
	    "d+": this.getDate(), //日
	    "H+": this.getHours(), //小时
	    "m+": this.getMinutes(), //分
	    "s+": this.getSeconds(), //秒
	    "q+": Math.floor((this.getMonth() + 3) / 3), //季度
	    "f+": this.getMilliseconds() //毫秒
	  };
	  if (/(y+)/.test(format))
	    format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
	  for (let k in o)
	    if (new RegExp("(" + k + ")").test(format))
	      format = format.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
	  return format;
	};

```
<p><code>
var a=new Date();a.format('yyyy/MM/dd HH:mm:ss');
</code></p>




+ 2、如何将JS中的arguments对象，转换成普通的数组(可以用ES6的语法)
	```javascript
	方法一：
		var argumentsArr=[].slice.call(arguments);//(es5)
	方法二：
		let argumentsArr = Array.form(arguments);//(es6)
	```

+ 3、请使用两种不同的方法实现数组去重(可以使用ES6语法)

	```javascript

	// author:JsLin
	uniqueArray: function(arry) { //数组去重
	            var n = {},
	                r = []; //n为hash表，r为临时数组
	            for (var i = 0; i < arry.length; i++) //遍历当前数组
	            {
	                if (!n[arry[i]]) //如果hash表中没有当前项
	                {
	                    n[arry[i]] = true; //存入hash表
	                    r.push(arry[i]); //把当前数组的当前项push到临时数组里面
	                }
	            }
	            return r;
	        }

	// 一：利用filter
	// 利用 indexOf 总是返回第一个元素的位置，后续重复元素与indexOf 返回的位置不符
    var arr = [1, 2, 3, 1, 4, 3], r;
    r = arr.filter(function (ele, index, self) {
        return self.indexOf(ele) === index;
    })
    console.log(r);

	// 二：ES6
	function unique(arr){
        return Array.from(new Set(arr));
    };
    unique([1,2,3,3,'3', '']); // [1,2,3,'3', '']
	
	// 三：判断在s数组中是否存在，不存在则push到s数组中（javascript）
	var arr = ['abc','abcd','sss','2','d','t','2','ss','f','22','d'];
	//定义一个新的数组
	var s = [];
	//遍历数组
	for(var i = 0;i<arr.length;i++){
	    if(s.indexOf(arr[i]) == -1){  //判断在s数组中是否存在，不存在则push到s数组中
	        s.push(arr[i]);
	    }
	}
	console.log(s);

	```

+ 4、请实现一个‘深拷贝’方法，（如jQuery.extend(true,{},{name:'Bob',schools:['保安中学'，'中山大学']})；）

+ 5、简要描述函数表达式与函数声明的区别，以及函数表达式的作用是什么？

```javascript

	A:函数声明
	function fun(){}
	fun()
	//1、函数声明必须有函数名
	//2、函数声明可以在任何地方调用

	B:函数表达式
	var fun=function(){}
	//1、函数的名字是可以省略的
	//2、想要调用的话必须在函数表达式声明的后面调用，否则报错some is not a function


```
+ 6、请简要解释作用域以及作用域链

```javascript

	A：作用域是函數內部可用，外部無法訪問，起到避免全局污染的作用
	B：作用域鏈是原型鏈中的的概念，是JS實現繼承的基礎。

```

+ 7、请简要解释什么是原型链

```javascript

	
	其實是js中原型链和作用域链根本不存在这样的链结构，而是通过查找引用，递归遍历，形式上是“链”结构。
	我們經常這樣
	function Person () {
        this.name = 'John';
    }
    var person = new Person();
    Person.prototype.say = function() {
        console.log('Hello,' + this.name);
    };
    person.say();//Hello,John
	上述代码非常简单，Person原型对象定义了公共的say方法，虽然此举在构造实例之后出现，但因为原型方法在调用之前已经声明，因此之后的每个实例将都拥有该方法。从这个简单的例子里，我们可以得出：
	原型对象的用途是为每个实例对象存储共享的方法和属性，它仅仅是一个普通对象而已。并且所有的实例是共享同一个原型对象，因此有别于实例方法或属性，原型对象仅有一份。所有就会有如下等式成立
	也許會這樣寫：
	function Person () {
        this.name = 'John';
    }
    var person = new Person();
    Person.prototype = {
        say: function() {
            console.log('Hello,' + this.name);
        }
    };
    person.say();//person.say is not a function
	很不幸，person.say方法没有找到，所以报错了。其实这样写的初衷是好的：因为如果想在原型对象上添加更多的属性和方法，我们不得不每次都要写一行Person.prototype,还不如提炼成一个Object来的直接。但是此例子巧就巧在构造实例对象操作是在添加原型方法之前，这样就会造成一个问题：
	当var person = new Person()时，Person.prototype为：Person {}(当然了，内部还有constructor属性),即Person.prototype指向一个空的对象{}。而对于实例person而言，其内部有一个原型链指针proto,该指针指向了Person.prototype指向的对象，即{}。接下来重置了Person的原型对象，使其指向了另外一个对象,即
	Object {say: function}，
	这时person.proto的指向还是没有变，它指向的{}对象里面是没有say方法的，因为报错。
	从这个现象我们可以得出：
	在js中，对象在调用一个方法时会首先在自身里寻找是否有该方法，若没有，则去原型链上去寻找，依次层层递进，这里的原型链就是实例对象的__proto__属性。
	若想让上述例子成功运行，最简单有效的方法就是交换构造对象和重置原型对象的顺序，即：
	function Person () {
	        this.name = 'John';
	    }
	    Person.prototype = {
	        say: function() {
	            console.log('Hello,' + this.name);
	        }
	    };
	    var person = new Person();
	    person.say();//person.say is not a function
	
	其实，只需要明白原型对象的结构即可：
	Function.prototype = {
        constructor : Function,
        __proto__ : parent prototype,
        some prototype properties: ...
    };
	总结：函数的原型对象constructor默认指向函数本身，原型对象除了有原型属性外，为了实现继承，还有一个原型链指针__proto__，该指针指向上一层的原型对象，而上一层的原型对象的结构依然类似，这样利用__proto__一直指向Object的原型对象上，而Object的原型对象用Object.prototype.__proto__ = null表示原型链的最顶端，如此变形成了javascript的原型链继承，同时也解释了为什么所有的javascript对象都具有Object的基本方法。
	

```

+ 8、阐述一下你对Ajax的理解（可以从单一、底层、对前端交互的影响等方面来说）

+ 9、JS中，如何单一类？如何实现类与类之间的继承？

+ 10、JS中，new运算符背后做了哪些事情？(或者说对一个function而言，使用new运算符调用与直接调用有何不同？)

+ 11、JS中，this关键字有几种指向？分别举例说明

+ 12、有一个字符串如：var str="abc345efgabcab",请写出3条JS语句分别实现如下3个功能
	+ 1) 去掉字符串中的a、b、c字符，形成结果为："345efg"
	+ 2) 将字符串中的数字用括号括起来，形成结果："abc34[5]efgabcab"
	+ 3) 将字符串中的每个数字的值分别乘以2，形成结果为："abc6810efgabcab"

```javascript

	var str="abc345efgabcab";
	//1
	str=str.replace(/[a]*[b]*[c]*/gi,'');
	console.log(str)//=>"345efg"

	//2
	str=str.replace(/[5]/gi,function(r){
		return '['+r+']';
	});
	console.log(str)//=>"abc34[5]efgabcab"

	//3
	var str="abc345efgabcab";
	var reg=/[0-9]/gi;
	str=str.replace(reg,function(s){
		return s*2;
	});
	console.log(str);

```

+ 13、有这样一个"URL=xxxx.aaa.bbb/index.php?abc=10&b=2000&c=999",请写出一段JS程序提取URL中的各个参数(参数名和参数值的个数不确定)，将去按key:value形式返回到一个json结构中，如：{aaa: "10", bbbd: "2000", ffc: "999"}

```javascript

	var ss="http://www.xxxx.net/article/46326.htm?abc=10&b=2000&c=999";
	var index=ss.indexOf("?")+1;
	var s1=ss.slice(index);
	var s2=s1.split("&");
	//console.log(s2);
	var object={};
	for(var i=0;i<s2.length;i++){
		var now=s2[i];
		var nowIndex=now.indexOf("=");
		// 	-------
		var k=now.slice(0,nowIndex);
		var value=now.slice(nowIndex+1).toString();
		object[k]=value;
		//console.log(k);
		//console.log(value);
		//console.log(object);
	}
	console.log(object);//=>{abc: "10", b: "2000", c: "999"},undefined未判断

```
```javascript

	var ss="http://www.xxxx.net/article/46326.htm?abc=10&b=2000&c=999";
	//获取到链接后的值,以对象的形式展示
	function getRequest(){
		var url=location.search;//获取url中"?"符后的字符串,
		var Request=new Object();
		if(url.indexOf("?")!=-1){
			var str=url.substr(1);//获取到"?"后的字符串
			var strs=str.split("&");
			for(var i=0;i<strs.length;i++){
				Request[strs[i].split("=")[0]]=strs[i].split("=")[1];
			}
			return Request
		}
	}

	//使用方法
	var ruquest=new getRequest();
	console.log(request.abc);
	console.log(request.b);
	console.log(request.c);
```

+ 14、几种基本数据类型?复杂数据类型?值类型和引用数据类型?堆栈数据结构?

> 基本数据类型：Undefined、Null、Boolean、Number、String

> 值类型：数值、布尔值、null、undefined。

> 引用类型：对象、数组、函数。

> 堆栈数据结构：是一种支持后进先出(LIFO)的集合,即后被插入的数据,先被取出!

> js数组中提供了以下几个方法可以让我们很方便实现堆栈：

> shift:从数组中把第一个元素删除，并返回这个元素的值。

> unshift: 在数组的开头添加一个或更多元素，并返回新的长度

> push:在数组的中末尾添加元素，并返回新的长度

> pop:从数组中把最后一个元素删除，并返回这个元素的值。

+ 15、声明函数作用提升?声明变量和声明函数的提升有什么区别?

> (1) 变量声明提升：变量申明在进入执行上下文就完成了。
只要变量在代码中进行了声明，无论它在哪个位置上进行声明， js引擎都会将它的声明放在范围作用域的顶部；

> (2) 函数声明提升：执行代码之前会先读取函数声明，意味着可以把函数申明放在调用它的语句后面。
只要函数在代码中进行了声明，无论它在哪个位置上进行声明， js引擎都会将它的声明放在范围作用域的顶部；

> (3) 变量or函数声明：函数声明会覆盖变量声明，但不会覆盖变量赋值。
同一个名称标识a，即有变量声明var a，又有函数声明function a() {}，不管二者声明的顺序，函数声明会覆盖变量声明，也就是说，此时a的值是声明的函数function a() {}。注意：如果在变量声明的同时初始化a，或是之后对a进行赋值，此时a的值变量的值。eg: var a; var c = 1; a = 1; function a() { return true; } console.log(a);

+ 16、判断数据类型?

> typeof返回的类型都是字符串形式，可以判断function的类型；在判断除Object类型的对象时比较方便。
> 判断已知对象类型的方法： instanceof，后面一定要是对象类型，并且大小写不能错，该方法适合一些条件选择或分支。

+ 17、异步编程？

> 方法1：回调函数，优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合（Coupling），流程会很混乱，而且每个任务只能指定一个回调函数。

> 方法2：时间监听，可以绑定多个事件，每个事件可以指定多个回调函数，而且可以“去耦合”（Decoupling），有利于实现模块化。缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。

> 方法3：发布/订阅，性质与“事件监听”类似，但是明显优于后者。

> 方法4：Promises对象，是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。
> 简单说，它的思想是，每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。

+ 18、事件流?事件捕获？事件冒泡？

> 事件流：从页面中接收事件的顺序。也就是说当一个事件产生时，这个事件的传播过程，就是事件流。

> 事件捕获是不太具体的元素应该更早接受到事件，而最具体的节点应该最后接收到事件。他们的用意是在事件到达目标之前就捕获它；也就是跟冒泡的过程正好相反，以html的click事件为例，document对象（DOM级规范要求从document开始传播，但是现在的浏览器是从window对象开始的）最先接收到click事件的然后事件沿着DOM树依次向下传播，一直传播到事件的实际目标；

> IE中的事件流叫事件冒泡；事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的节点（文档）。对于html来说，就是当一个元素产生了一个事件，它会把这个事件传递给它的父元素，父元素接收到了之后，还要继续传递给它的上一级元素，就这样一直传播到document对象（亲测现在的浏览器到window对象，只有IE8及下不这样

+ 19、如何清除一个定时器?
+ 20、如何添加一个dom对象到body中?innerHTML和innerText区别?
+ 21、数据持久化技术(ajax)?简述ajax流程
+ 22、回调函数?
+ 23、什么是闭包?* 堆栈溢出有什么区别？ 内存泄漏? 那些操作会造成内存泄漏？怎么样防止内存泄漏？
+ 24、平时工作中怎么样进行数据交互?如果后台没有提供数据怎么样进行开发?mock数据与后台返回的格式不同意怎么办?
+ 25、简述ajax执行流程



### 二、CSS

+ 1、下面的元素如何水平居中：
	+ a:行内元素(如span)
	+ b:固定宽度的会计元素(宽度为500px的div)
	+ c:不固定宽度的块级元素

```javascriipt

	a:設置display:block,給個固定寬度，再：margin:0 auto
	b:直接margin:0 auto
	c:使用css3,定位

```

+ 2、如何理解position属性？设置不同的position值对文档流布局有和影响

+ 3、较大z-index值的元素一定能'盖住'较小的z-index值的元素吗？如果不是，请举例说明

+ 4、你所知道的清除浮动有哪些方式，各有何利弊？

+ 5、如何避免子元素的margin-top与父容器的前一个相邻元素的margin-bottom发生“合并”？

+ 6、什么是BFC(haslayout)，什么情况下可以使用BFC的特效来布局？

### 三、思维题
+ 1、你认为使用jQuery.each代替传统的for..in迭代有什么好处？

+ 2、使用模块化加载器(如requireJS)有什么好处？

+ 3、Git与SVN有什么区别，你最喜欢他们哪些特性？

```javascript

	1、SVN是Subversion的简称，是一个开放源代码的版本控制系统,支持大多数常见的操作系统。作为一个开源的版本控制系统,Subversion管理着随时间改变的数据。这些数据放置在一个中央资料档案库(repository)中。这个档案库很像一个普通的文件服务器,不过它会记住每一次文件的变动。这样你就可以把档案恢复到旧的版本,或是浏览文件的变动历史。Subversion是一个通用的系统,可用来管理任何类型的文件,其中包括了程序源码。

	2、Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目
	Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
	分布式相比于集中式的最大区别在于开发者可以提交到本地，每个开发者通过克隆（git clone），在本地机器上拷贝一个完整的Git仓库.

```

+ 4、你选择IDE的标准是什么？

+ 5、你做理解的前端模拟包含哪些内容，其对前端开发的意义是什么？

+ 6、请列举出你所了解的跨域的实现方法
