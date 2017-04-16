# interview-for-frontend

## CSS相关
### bfc原理 触发方式
> * BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。


> * 根元素
> * float不为none
> * position为absolute or fixed
> * diaplay为inline-block or table-cell or table-caption or flex or inline-flex
> * overflow不为visible

margin重叠原理 解决
> * Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
> * 解决办法就是触发BFC

### 不定大小元素多方法居中
> * [伪元素方法](https://jsfiddle.net/lunhui/xt5871ox/)
> * 绝对定位-50%，css3 transform 位移-50%
> * [table-cell方法](https://jsfiddle.net/lunhui/86663Ljx/)
> * line-height方法

### zindex默认排序 交叉重叠
> * [面试题目](https://jsfiddle.net/lunhui/z6x948cv/)
> * [原理详解](http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)

### html5新标签语义化

### div和section语意区别

### 语义化意义判断盒模型

### boxsizing可选值和意思

## javascript相关
### promise实现原理

### foreach map区别

### 数组套数组parse

### 用settimeout实现setinterval 和cleartimeout接口

### sort传参

### array方法列表

### http头缓存相关

### localstore接口

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

### 元素宽高取值方法

## 算法相关
### 冒泡排序

### 快速排序

### 随机数组





