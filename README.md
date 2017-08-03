## 面試題小結
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

+ 3、请使用两种不同的方法实现数组去重(可以使用ES6语法)
	```javascript
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
	```

+ 4、请实现一个‘深拷贝’方法，（如jQuery.extend(true,{},{name:'Bob',schools:['保安中学'，'中山大学']})；）

+ 5、简要描述函数表达式与函数声明的区别，已结函数表达式的作用是什么？

+ 6、请简要解释作用域以及作用域链

+ 7、请简要解释什么是原型链

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

+ 13、有这样一个"URL=http://www.xxx.net/article/46326.htm?abc=10&b=2000&c=999",请写出一段JS程序提取URL中的各个参数(参数名和参数值的个数不确定)，将去按key:value形式返回到一个json结构中，如：{aaa: "10", bbbd: "2000", ffc: "999"}

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




### 二、CSS

+ 1、下面的元素如何水平居中：
	+ a:行内元素(如span)
	+ b:固定宽度的会计元素(宽度为500px的div)
	+ c:不固定宽度的块级元素

+ 2、如何理解position属性？设置不同的position值对文档流布局有和影响

+ 3、较大z-index值的元素一定能'盖住'较小的z-index值的元素吗？如果不是，请举例说明

+ 4、你所知道的清除浮动有哪些方式，各有何利弊？

+ 5、如何避免子元素的margin-top与父容器的前一个相邻元素的margin-bottom发生“合并”？

+ 6、什么是BFC(haslayout)，什么情况下可以使用BFC的特效来布局？

### 三、思维题
+ 1、你认为使用jQuery.each代替传统的for..in迭代有什么好处？

+ 2、使用模块化加载器(如requireJS)有什么好处？

+ 3、Git与SVN有什么区别，你最喜欢他们哪些特性？

+ 4、你选择IDE的标准是什么？

+ 5、阐述一下你对Ajax的理解

+ 6、你做理解的前端模拟包含哪些内容，其对前端开发的意义是什么？

+ 7、请列举出你所了解的跨域的实现方法
