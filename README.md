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


## javascript相关
### promise实现原理

### foreach map区别

forEach和map都支持2个参数：一个是回调函数（item,index,list）和上下文；

不管是forEach还是map 都支持第二个参数值，第二个参数的意思是把匿名回调函数中的this进行修改。

forEach没有返回值，不会影响原数组

map有返回值，返回新数组，不会影响原数组

### 数组套数组parse
> 解析字符串"[1,3,2,[2,3,4,[2,3,4],2],231,[23,42]]"(不使用原生方法)
> //TODO

### 用settimeout实现setinterval 和cleartimeout接口

#### 正确解法

还不会写。。。。下面为错误答案
`var myInterval = (function() {
	var t = null;
	return function interval(fn,time){
		t = setTimeout(function() {
			fn();
			interval(fn,time);
		},time);
		return t;
	}
})()`


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

setItem存储value

用法：.setItem( key, value)

getItem获取value

用法：.getItem(key)

removeItem删除key

用法：.removeItem(key)

clear清除所有的key/value

用法：.clear()

### cookie字段

### ls和cookie区别

### 后台存放防止前端获取的数据

### web安全

### vue双向绑定原理

### 前端灾备方案 比如后端接口挂了

### 继承

### 多继承

### 判断字符串是否回文

### 数组去重基本类型

### 数组去重延伸到复杂类型

### 深度clone

### cmd标准 

### requestAnimationFrame

### 元素宽高取值方法

## 算法相关
### 冒泡排序

### 快速排序

### 随机数组





