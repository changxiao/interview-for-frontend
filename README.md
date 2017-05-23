# interview-for-frontend

## CSS相关

### bfc原理 触发方式
> * BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

> * 根元素
> * float不为none
> * position为absolute or fixed
> * diaplay为inline-block or table-cell or table-caption or flex or inline-flex
> * overflow不为visible

### margin重叠原理 解决
> * Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
> * 解决办法就是触发BFC

### 不定大小元素多方法居中
> * [伪元素方法](https://jsfiddle.net/lunhui/xt5871ox/)
> * 绝对定位-50%，css3 transform 位移-50%
> * [table-cell方法](https://jsfiddle.net/lunhui/86663Ljx/)
> * line-height方法
> * 兼容办法，负元素绝对定位50%，子元素相对定位-50% 

### 五个平级元素flex布局, 前三个元素占满一行并宽度相等，第二行开始第四个元素为定宽元素，最后一个元素为自适应占满剩余空间。
``` html
<div class="a"></div>
<div class="b"></div>
<div class="c"></div>
<div class="d"></div>
<div class="e"></div>
```
``` css
div{border: 1px solid #000; box-sizing:border-box;}
body{display:flex; justify-content:center; align-items:stretch; min-height: 100px; flex-wrap:wrap; align-content:stretch;}
.a,.b,.c{ flex: 1 1 32%;}
.d,.e{ flex:1 1 auto;}
.d{ flex: 0 0 50px;}
```

### 让三个平级元素实现品字形自适应布局

todo

### zindex默认排序 交叉重叠
> 顺序：正z-index
> 1. 正z-index
> 2. z-index：auto； or z-index：0； 不依赖z-index的层叠上下文
> 3. inline/inline-block水平盒子
> 4. float浮动盒子
> 5. block块级盒子
> 6. 负z-index
> 7. 层叠上下文 background/border
> * [面试题目](https://jsfiddle.net/lunhui/z6x948cv/)
> * [原理详解](http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)

### html5新标签语义化
> w3c网站查
> [一些例子](http://www.html5jscss.com/html5-semantics-section.html)

### div和section语意区别
> [没有太明确答案，知乎答案](https://www.zhihu.com/question/20227599)

### 语义化意义
> 1. 根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。
> 2. css没有时候也能看
> 3. SEO
> 4. 用户体验 
> 5. 后续人员维护方便 
> 6. 其他设备正确解析 

### 判断盒模型
> content-box和border-box

### boxsizing可选值和意思

> 同上

### flex相关属性和值

> [flex](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) 


## javascript相关

### undefined

### 写一个函数，实现获取url后面参数，并转换为对象

### 实现一个函数pipe，使pipe(a,b,c,d)的运行结果和a(b(c(d())))相同

``` js
还需完善
var pipeline = function (...funcs) {
  return function (val){
    return funcs.reduce(function(a,b){
      return b(a)
    }, val)
  }
}

var plus1 = function (a){
  return a + 1
}
var mult2 = function (a){
  return a * 2
}

var plus2 = function (a){
  return a + 2
}
var plus3 = function (a){
  return a + 3
}
var addThenMult = pipeline(plus1, mult2, plus2, plus3)
console.log(addThenMult(5))
```

### promise实现原理

TODO
状态机

### foreach map区别

forEach和map都支持2个参数：一个是回调函数（item,index,list）和上下文；

不管是forEach还是map 都支持第二个参数值，第二个参数的意思是把匿名回调函数中的this进行修改。

forEach没有返回值，不会影响原数组

map有返回值，返回新数组，不会影响原数组

### 数组套数组parse
> 解析字符串"[1,3,2,[2,3,4,[2,3,4],2],231,[23,42]]" (不使用原生方法)
> 编译原理的知识点，用到堆栈。
[答案](https://jsfiddle.net/v8n70fk2/)

``` javascript
var str = "[1,3,2,[2,3,4,[2,3,4],2],231,[23,42],[12,3,[23,2]]]";
function popStack(stack){
  var temp = [];
  var pop = stack.pop();
  while(pop !== '[' && stack.length>0){
    temp.unshift(pop)
    pop = stack.pop();
  }
  stack.push(temp);
}

function parseArr(str){
  var tarr = str.split(',');
  var temp = []
  for(var i = 0; i < tarr.length; i++){
    var temp = temp.concat(tarr[i].split('[').map(function(item,idx, array) {
      if (item.length === 0){
        item = '['
      }
      return item;
    }));
  }
  
  tarr = []
  for(var i = 0; i < temp.length; i++){
    var tarr = tarr.concat(temp[i].split(']').map(function(item,idx, array) {
      if (item.length === 0){
        item = ']'
      }
      return item;
    }));
  }
  
  var rusult = [];
  var stack = [];
  var stackCount = 0;//实际没有用到
  tarr.forEach(function(item, idx, arr){
    if(item === '['){
      stackCount++;
      stack.push(item)
      return;
    } else if( item === ']'){
      stackCount--;
      popStack(stack)
      return;
    } else if(stackCount > 0 ){
      stack.push(parseInt(item))
    }
  })
  
  return stack[0]  
}

console.log('生成结果：')
console.log(parseArr(str))


console.log('正确结果：')
console.log(JSON.parse(str))
```

### 用settimeout实现setinterval 和cleartimeout接口

[答案1](https://jsfiddle.net/nsy3kqhm/2/)

``` javascript
function myInterval(fn,t) {
  var timer = null;
  (function interval(fn,t){
    timer = setTimeout(function(){
      fn();
      interval(fn,t)
    },t)
  })(fn,t);
  return function(){
  	return timer;
   }
}

var myInter = myInterval(function(){
	console.log(1)
},300)

setTimeout(function(){
	clearTimeout(myInter())
},3000)
```

[答案2](https://jsfiddle.net/nsy3kqhm/3/)

``` javascript
function myInterval(fn,t) {
  var timer = {
  	ti :null,
  	cleartime: function(){
    	clearTimeout(this.ti);
      console.log('success clear')
    }
  };
  (function interval(fn,t){
    timer.ti = setTimeout(function(){
      fn();
      interval(fn,t)
    },t)
  })(fn,t);
  return timer;
}

var myInter = myInterval(function(){
	console.log(1)
},300)

setTimeout(function(){
	console.log(myInter.ti)
	clearTimeout(myInter.cleartime())
},3000)
```

### array方法列表

1. concat
2. join
3. pop
4. push
5. reverse
6. shift
7. slice
8. sort
9. splice
10. toString
11. toLocalString
12. unshift
13. forEach
14. map
15. filter
16. some
17. every
18. indexOf
19. lastIndexOf
20. reduce
21. reduceRight

### http头缓存相关

Expires/Cache-Control

缓存日期时间和最大时间

### localstorage接口

1. setItem存储value

用法：.setItem( key, value)

2. getItem获取value

用法：.getItem(key)

3. removeItem删除key

用法：.removeItem(key)

4. clear清除所有的key/value

用法：.clear()

5. 其他操作方法：点操作和[]

### cookie字段

1. name字段为一个cookie的名称。

2. value字段为一个cookie的值。

3. domain字段为可以访问此cookie的域名。

4. path字段为可以访问此cookie的页面路径。 比如domain是abc.com,path是/test，那么只有/test路径下的页面可以读取此cookie。

5. expires/Max-Age 字段为此cookie超时时间。若设置其值为一个时间，那么当到达此时间后，此cookie失效。不设置的话默认值是Session，意思是cookie会和session一起失效。当浏览器关闭(不是浏览器标签页，而是整个浏览器) 后，此cookie失效。

6. Size字段 此cookie大小。

7. http字段  cookie的httponly属性。若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie。

8. secure 字段 设置是否只能通过https来传递此条cookie



### ls和cookie区别

> 共同点：都是保存在浏览器端，且同源的。localStorage和cookie 在所有同源窗口中都是共享的。
> 1. cookie数据始终在同源的http请求中携带（即使不需要）,cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。
> 2. 存储大小限制也不同，cookie数据不能超过4k; localStorage可以额达到5M
> 3. 数据有效期不同，localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。

### 后台存放防止前端获取的数据

> cookie 设置 httponly字段

### web安全

> XSS
> TODO

### vue双向绑定原理
[vue官方文档](https://cn.vuejs.org/v2/guide/reactivity.html)

### 前端灾备方案 比如后端接口挂了

TODO

### 继承

``` javascript
function Parent(){
	this.pname = 'parent';
}
Parent.prototype.say = function(){
	console.log('im' + this.pname);
}
function Child(){
	Parent.call(this);
	this.cname = 'cname'
}
(function(){
	var F = function(){};
	F.prototype = Parent.prototype;
	F.constructor = Child;
	Child.prototype = new F();
}())
```

### 多继承

TODO

### 判断字符串是否回文

``` javascript
var s = 'abcdcba';
function plalindrome(str) {
	str = str.split('').reverse().join('');
  return str;
}
console.log(plalindrome(s) == s)
```


### 数组去重

[基本类型前提下，for in 方法](https://jsfiddle.net/ancwqw9u/)

``` javascript
var arr = [123,12,31,23,12,312,312,1,21,21,23,12,31,1,1,1,23,4,14,15,55213];

function removal(arr) {
	var tempobj = {};
  var arrlength = arr.length;
  var result = [];
	for(var i = 0; i < arrlength; i++) {
  	var temp = arr[i]
  	if(typeof tempobj[temp] === 'undefined') {
    	tempobj[temp] = true;
    } 
  }
  for( var key in tempobj) {
  	result.push(key);
  }
  
  return result
}

console.log( removal(arr))
```

``` js
var arr = [123,12,31,23,12,312,312,1,21,21,23,12,31,1,1,1,23,4,14,15,55213];
var newarr = new Set(arr)
var newarr = Array.from(newarr)
console.log(newarr)
```

### 基本类型

1. String
2. Number
3. Boolean
4. null
5. undefined

复杂的数据类型————Object

Object、Array和Function则属于引用类型

### 数组去重延伸到复杂类型

TODO

### 深度clone

TODO

### cmd规范

[作者github](https://github.com/seajs/seajs/issues/242)

### requestAnimationFrame

其用法跟setTimeout差不多，与setTimeout相比，最大的优势是由浏览器来决定函数的执行时机

requestAnimationFrame更加智能，它并非加快执行速度，而是适当时候降帧，防止并解决丢帧问题。

### 元素宽高取值方法

> offsetWidth
> offsetHeight
> [参考](http://blog.csdn.net/charlene0824/article/details/50535449)


### js事件模型

> 先捕获，后冒泡

### js向后台发起请求方式有多少种

> TODO

### push数组，并打印日志

### $.extend 的实现

### gulp、webpack如何异步

### zepto的常见bug

### 类型转换原理([] == false, {} == false, [1] == [1], [1] == false)

### 模块化开发中过程中，循环调用问题怎么解决

[原理](http://www.ruanyifeng.com/blog/2015/11/circular-dependency.html)

## 算法相关

### 冒泡排序

[答案](https://jsfiddle.net/ancwqw9u/1/)

``` javascript
var arr = [1,3,2,4,1,12,2,5,213,14,1,24,124,21,312,321,31,41,51,25,12,512,5,12,512,521,51,25,12,51];
function bubbleSort(arr) {
	var arrlength = arr.length;
	for( var i = 0; i < arrlength - 1; i++){
  	for( var j = 0; j < arrlength - i -1; j++  ){
    	var temp = null;
      if( arr[j] > arr[j+1] ){
      	temp = arr[j];
        arr[j] = arr[j+1]
        arr[j+1] = temp;
      } 
    }
    console.log(arr)
  }
 	return arr
}
console.log(bubbleSort(arr))
```


### 快速排序

``` javascript
var arr = [1,3,2,4,1,12,2,5,213,14,1,24,124,21,312,321,31,41,51,25,12,512,5,12,512,521,51,25,12,51];
function quickSort(arr) {
	if(arr.length < 2) {
  	return arr;
   }
   var feature = arr.splice(Math.floor(arr.length/2),1),
        left = [],
        right = [],
        arrlength = arr.length;
   for( var i = 0; i < arrlength; i++){
   		if( arr[i] < feature ) {
      	left.push(arr[i])
      } else {
      	right.push(arr[i])
      }
   }
   return quickSort(left).concat(feature, quickSort(right));
}
console.log( quickSort(arr))
```

### 随机数组

``` javascript
var arr=[1,2,3,4,5,6,7,8,9,10,22,33,55,77,88,99];  
arr.sort(function(){
	return Math.random()>0.5?-1:1;
});  
console.log(arr)
```

### 函数curry化和使用情景

> TODO 

### 函数curry化和偏函数区别

> TODO 

### js实现动画的几种方法

> TODO 

### es6箭头函数和普通函数的区别

> TODO 

### 连等赋值的可能问题

``` javacript
var a = {n:1}
b = a
a.x = a = {n:2}
console.log(a.x) 
console.log(b.x)
//运行一下就知道了，属性访问表达式优先级高于赋值运算符。 
```

### Function, Object, Array的方法名称和例子。

### 过滤下面对象二级内容为isfilter：true的name值
``` javascrtipt
[
  {
    name: 'index',
    child: [
      {
        name: 'Jhon',
        isfilter: true
      },
      {
        name: 'Mary',
        isfilter: true
      },
      {
        name: 'Bill',
        isfilter: false
      }
    ]
  },
  {
    name: 'about',
    child: [
      {
        name: 'Jhon',
        isfilter: true
      }
    ]
  },
  {
    name: 'contact',
    child: [
      {
        name: 'Peter',
        isfilter: false
      },
      {
        name: 'Gate',
        isfilter: false
      }
    ]
  }
]
```
> 提示reduce，filter

### 实现add函数

> 提示valueOf

``` javasrcipt
add(1)  //return 1
add(1)(2)(3)  //return 6
add(1)(4)(5)(7)  //return 17
```

``` javascript
function add () {
  var args = Array.prototype.slice.call(arguments,0)
  var fn = function(){
    return add.apply(null, args.concat(Array.prototype.slice.call(arguments, 0)))
  }
  fn.valueOf = function(){
    var s = args.reduce(function(prev,next){
      return prev+next;
    })
    return s;
  }
  return fn;
}
console.log(
add(1)(2)(4)
)
```

### 二维数组转一维数组（笛卡尔乘积）

> var arr = [[1,2],[2,3],[4,6]]

``` js
var arr = [[1,2],[2,3],[4,6]]

var a = arr.reduce(function(oldv,newv){
  console.log(arguments)
  var temp = []
  oldv.forEach(function(item,idx){
    newv.forEach(function(item2, idx2){
      temp.push(item + '' +item2)
    })
  })
  return temp
})
console.log(a)
```

### 设计算法，无序不重复的数组，取N个数字，总和为M。写出空间复杂度和时间复杂度

``` js
var arr = [1,3,2,4,1,12,2,5,213,14,1,24,124,21,312,321,31,41,51,25,12,512,5,12,512,521,51,25,12,51];
arr = new Set(arr);
arr = Array.from(arr)
// arr去重
const N = 5
const M = 50

```


## 概念相关

### MVC和MVVM区别

> TODO

### 面向对象概念

> TODO

### CMD,AMD,UMD,Commonjs规范

> TODO


## 未整理

### 用reduce实现filter

### 获取url中参数，转成对象