# js知识点
##### offset家族
###### `offsetWidth`和`offsetHeight`

```css
#box{
    width: 200px;
    height: 150px;
    background-color: red;
    padding: 10px;
    border: 5px solid #ddd;
    margin: 10px;
}
```
```html
<div id="box" style="width: 100px;height: 100px;"></div>
```
```js
var box = document.getElementById("box");
// offsetHeight  = 内容 + 内边距 + 边框
console.log( 'box.offsetWidth==>', box.offsetWidth);
//box.offsetWidth==> 130
console.log( 'box.offsetHeight==>', box.offsetHeight);
//box.offsetHeight==> 130
console.log( 'box.style.width==>', box.style.width);
//box.style.width==> 100px
console.log( 'box.style.height==>', box.style.height);
//box.style.height==> 100px
// div.style.width()只能获取行内的宽度
box.style.width = 500 + 'px';
box.offsetWidth = 200 + 'px';
```
###### `offsetLeft`和`offsetTop`

```css
body{
    margin: 11px;
    border: 12px solid yellow;
    padding: 13px;
}

#father{
    width: 400px;
    height: 400px;
    background-color: red;
    margin: 50px;
    padding: 12px;
    border: 30px solid pink

   /* position: relative;*/
}

#box{
    width: 200px;
    height: 150px;
    background-color: blue;
    padding: 15px;
    border: 5px solid #000;
    margin-left: 20px;
    margin-top: 40px;
    left: 3px;
    top: 4px;    
}
```
```html
<div class="grand">
    <div id="father">
        <div id="box" style="left: 1px;top: 2px;"></div>
    </div>
</div>
```
```js
var box = document.getElementById("box");
var father = document.getElementById("father");
var grand = document.getElementById("grand");
console.log('box.offsetLeft==>',box.offsetLeft); 
//box.offsetLeft==> 148
//左外边距+父盒子的左内边距+父盒子边框+父盒子外边距+body做内边距+body做边框+body左外边距
//20 + (12 + 30 + 50) + (13 +12 +11)
console.log('box.offsetTop==>',box.offsetTop);
//box.offsetTop==> 168
//上边距+父盒子的上内边距+父盒子边框+父盒子外边距+body做内边距+body做边框+body左外边距
//40 + (12 + 30 + 50) + (13 + 12 + 11)
father.style.position = 'relative'
console.log('父盒子加了定为relative以后box.offsetLeft==>',box.offsetLeft); 
//父盒子加了定为relative以后box.offsetLeft==> 32
//左外边距20+父盒子左内边距12
console.log('父盒子加了定为relative以后box.offsetTop==>',box.offsetTop);
//父盒子加了定为relative以后box.offsetTop==> 52
//上边距40+父盒子上内边距12
father.style.position = 'absolute'
console.log('父盒子加了定为absolute以后box.offsetLeft==>',box.offsetLeft); 
//父盒子加了定为absolute以后box.offsetLeft==> 32
//左外边距20+父盒子左内边距12
console.log('父盒子加了定为absolute以后box.offsetTop==>',box.offsetTop);
//父盒子加了定为absolute以后box.offsetTop==> 52
//上边距40+父盒子上内边距12
```
###### `div.style.left`和`div.style.top`
- 只能获取行内的`left`和`top`，没有行内值则返回空

```js
console.log('box.style.left==>',box.style.left);
//box.style.left==> 1px
console.log('box.style.top==>',box.style.top);
//box.style.top==> 1px

```
###### `offsetParent`
- 找到最近的带有定位的父级

```js
console.log('box.parentNode==>',box.parentNode);
/*
box.parentNode==>
<div id="father" style="position: absolute;">
    <div id="box"></div>
</div>
*/
console.log('box.offsetParent==>',box.offsetParent);
/*
box.parentNode==>
<div id="father" style="position: absolute;">
    <div id="box"></div>
</div>
*/
```

###### `setTimeout`应用
```js
for(var i =0;i<10;i++){
    setTimeout(function timer(){
        /*setTimeout是异步函数，它会将timer函数放入任务队列中，
        而此时会将循环执行完毕才会执行timer函数，因为js是单线程按顺序执行，
        每次执行timer函数时，循环都已经结束i已经变成10，所以输出的是10*/
        console.log(i);  
    },i*1000)
}
```
解决方案是将setTimeout函数放入自执行函数中
```js
for(var i =0;i<10;i++){
    (function(t){
        setTimeout(function(){
            console.log(t);    
        },i*1000)
    })(i) /*i作为参数传入函数内部，这个时候的t的值不会变，
            每次执行setTimeout函数时，才会传入外部变量i
}
```
或者使用es6语法`let`
```js
for(let i =0;i<10;i++){
    /*i虽然声明在全局，*/
    setTimeout(function timer(){
        console.log(i);  
    /*但是当for循环内部执行时，
    会形成一个封闭的作用域，此时i的值不变*/
    },i*1000)
}
```

##### 事件对象
###### 屏幕`screenX`和`screenY`
`screenX`距离屏幕左边的x坐标
`screenY`距离屏幕顶部的y坐标
```js
document.onclick = function (event) {
    var e = event || window.event;
    // screenX\/screenY 是以屏幕为基准进行测量，即：当前元素距离屏幕的尺寸
    //  alert(event.screenX + ',' + event.screenY);

    // pageX 和 pageY 是以当前文档（绝对定位）为基准，不适用于IE6-8;
    // alert(event.pageX + ',' + event.pageY);

    // clientX 和 clientY 是以当前可视区域为基准，类似于固定定位。
    console.log('screenX和screenY' + event.screenX + ',' + event.screenY);
    console.log('pageX和pageY' + event.pageX + ',' + event.pageY);
    console.log('clientX和clientY' + event.clientX + ',' + event.clientY);
}
```
###### 文档`pageX`和`pageY`包括滚动距离
`pageX`距离屏幕左边的x坐标
`pageY`距离屏幕顶部的y坐标
###### 可视区域`clientX`和`clientY`不包括滚动距离
`clientX`距离屏幕左边的x坐标
`clientY`距离屏幕顶部的y坐标
###### 鼠标事件
`onmousemove`当鼠标在当前元素中**移动**时触发，鼠标移动就会执行，**频率高**

`onmouseover`当鼠标**进入**当前元素时触发，只触发**一次**

`onmouseup`当鼠标**弹起**时触发

`onmousedown`当鼠标**按下**时触发

##### 内置对象`document`
>是window对象的一部分，可以通过window.document属性对其进行访问

>document对象使我们可以从脚本中对HTML页面中的所有元素进行访问

`document.head` 获取文档头部节点
`document.body` 获取文档身体节点
`document.title` 获取文档标题
`document.documentElement`获取整个html

##### scroll家族
###### `document.body.scrollWidth`和`document.body.scrollWidth`

`document.body.scrollWidth`网页全宽

`document.body.scrollHeight`网页全高

`document.body.scrollTop`网页被卷进去的高

`document.body.scrollLeft`网页被卷进去的左

![blockchain](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=110942531,1218998494&fm=26&gp=0.jpg "区块链")

`window.onscroll`窗口滚动事件

```js
//  console.log(document.body.scrollTop, document.body.scrollLeft);
//
window.onscroll = function () {
    console.log(document.documentElement.scrollTop, document.documentElement.scrollLeft);
}
```
```js
window.onscroll = function () {
    /*ie9+ 和 最新浏览器*/
    //console.log(window.pageXOffset, window.pageYOffset);
    //火狐或其他浏览器
    //console.log(document.documentElement.scrollTop, document.documentElement.scrollLeft);
    //没有声明<DOCTYPE>
    //console.log(document.body.scrollTop, document.body.scrollLeft);
    var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0;
    var scrollLeft = window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft || 0;
    console.log(scrollTop, scrollLeft);
}
```
```js
//json封装
    /*
     * 获取滚动的头部距离和左边距离
     * scroll().top scroll().left
     * @returns {*}
     */
    function scroll() {
        if(window.pageYOffset !== null){
            return {
                top: window.pageYOffset,
                left: window.pageXOffset
            }
        }else if(document.compatMode === "CSS1Compat"){ // W3C
            return {
                top: document.documentElement.scrollTop,
                left: document.documentElement.scrollLeft
            }
        }

        return {
            top: document.body.scrollTop,
            left: document.body.scrollLeft
        }
    }

    window.onscroll = function () {
        document.getElementById('div').innerHTML = scroll().top;
    }
```

##### 什么是json

- 数据在键值对中
- 数据由逗号分割
- 花括号保存对象
- 方括号保存数组

1.1 数组
```js
var data = ["张三", 18, "男"];
```
1.2 对象
```js
var person = {"name": "张三", "age": 18, "sex": "男"};
```
1.3 组合
```js
var persons = [
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"},
    {"name": "张三", "age": 18, "sex": "男"}
];
```

##### client家族
###### `clientWidth`和`clientHeight`
- `document.body.clientWidth`网页可见区域的宽 不包括网页进度条
- `document.body.clientHeight`网页可见区域的高



##### 三大家族合集
```css
body{
    margin: 4px;
    border: 5px solid blue;
    padding: 3px;

}
#box{
    width: 201px;
    height: 201px;
    background-color: red;
    padding: 23px;
    border: 14px solid #000;
    margin: 16px;
    position: relative;
    left: 32px;
}

p{
    margin-bottom: 20px;
}
```


```html
<div id="box">
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
    <p>我是MT</p>
</div>
```
```js
var box = document.getElementById("box");

//可视区域(都是内容+内边距 off外带边框) cli scr算上卷进去的

// 1. width 和 height
   // border + padding + 内容的宽度和高度
console.log('box.offsetWidth==> ' + box.offsetWidth, ' box.offsetHeight==> ' + box.offsetHeight);
//box.offsetWidth==> 275  box.offsetHeight==> 275
//内容 + 内边距 + 边框
//201 + 46 + 28
// padding + 内容的宽度和高度
console.log('box.clientWidth==> ' + box.clientWidth, ' box.clientHeight==> ' + box.clientHeight);
//box.clientWidth==> 247  box.clientHeight==> 247
//内容 + 内边距
//201 + 46
// 能够滚动的内容的 宽度 和 高度
console.log('box.scrollWidth==> ' + box.scrollWidth, 'box.scrollHeight==> ' + box.scrollHeight);
//box.scrollWidth==> 247 box.scrollHeight==> 849
//内容 + 内边距 + 边框 + 外边距
//201 + 46

// 2. top 和 left
// 当前元素距离有定位的父盒子左边的距离；offsetTop: 当前元素距离有定位的父盒子上边的距离
//子盒子的外边距距离到父盒子的外边距(内边距+边框+外边距)
console.log('box.offsetLeft' + box.offsetLeft, ' box.offsetTop ' + box.offsetTop);
//box.offsetLeft 60 box.offsetTop 28
//16 + (4+5+3)
//左边的边框宽度和上边的边框宽度
console.log('box.clientLeft ' + box.clientLeft, ' box.clientTop ' + box.clientTop);
//box.clientLeft 14 box.clientTop 14    
// scrollLeft: 左边滚动的长度; 【重要】scrollTop: 上边滚动的长度;
console.log('box.scrollLeft'+ box.scrollLeft, 'box.scrollTop' + box.scrollTop);
```
##### 节流
拿`window.onresize`举例，鼠标拖动改变窗口大小的时间间隔很短，所以会重复执行很多次时间
使用`setTimeout`函数来达到**节流**的目的表示每隔多少秒执行一次事件
```js
window.onresize = function () {
    clearTimeout(timer);
    // 节流
    timer = setTimeout(function () {
        console.log(1);
        waterFull('main', 'box');
    }, 200); //每隔200毫秒执行一次函数
}
```
##### 阻止冒泡
```js
if(event && event.stopPropagation){ // w3c标准
    event.stopPropagation();
}else{ // IE系列 IE 678
    event.cancelBubble = true;
}
```
##### 获取用户选中的文字
```js
if(window.getSelection){ // 标准模式 获取选中的文字
    selectedText = window.getSelection().toString();
}else{ // IE 系列
    selectedText = document.selection.createRange().text;
}
```
##### 获取事件的源对象
```js
//兼容写法
var targetId = e.target ? e.target.id : e.srcElement.id;
```
##### 选择内容以后立即取消选中的内容
```js
window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
```
