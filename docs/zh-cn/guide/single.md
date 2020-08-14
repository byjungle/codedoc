# 代码示例
##### 星星闪动特效
```css
body{
    background-color: #000;
    position: relative;
    overflow: hidden;
}
span{
    background-image: url(images/star.png);
    background-size: 100%;
    width: 30px;
    height: 30px;
    display: block;
    position: absolute;
    animation: shan 2s infinite;
}
@keyframes shan {
    0%{
        opacity: 0;
    }
    100%{
        opacity: 1;
    }
}
```
```js

//获取视口的宽高，把星星约束在视口内
var screenW = document.documentElement.clientWidth;
var screenH = document.documentElement.clientHeight;
//放很多星星
var num = parseInt(Math.random()*100);
//alert(num);
for(var i=0;i<num;i++){
    //1. 创建星星
    var span = document.createElement('span');
    document.body.appendChild(span);
    //2. 随机位置星星
    var x = parseInt(screenW * Math.random())
    var y = parseInt(screenH * Math.random())
    span.style.left = x + 'px';
    span.style.top = y + 'px';
    //3. 随机大小星星
    var scale = Math.random()*1.2;
    span.style.transform = 'scale(' + scale + ',' + scale + ')';
    //4. 星星随机闪
    var animation = Math.random()*1.5;
    span.style.animationDelay = animation + 's';
}

```

##### 照片墙特效

用到插件
```html
<script src="https://cdn.bootcdn.net/ajax/libs/underscore.js/1.10.2/underscore-min.min.js"></script>
```

```css
#ps{
    position: relative;
}

#ps li{
    width: 250px;
    height: 360px;
    box-shadow: 0 0 10px #000;

    position: absolute;

    transition: all 1s;
    cursor: pointer;
}

#ps li.current{
    left: 50% !important;
    top: 50% !important;
    transform: rotate(0deg) translate(-50%, -50%) scale(1.5, 1.5) !important;
    z-index: 99;
}
```
```js
// 1. 获取需要的标签
var ps = document.getElementById("ps");

// 2. 动态创建li标签
for(var i=0; i<10; i++){
    // 2.1 创建li标签
    var li = document.createElement("li");
    ps.appendChild(li);

    // 2.2 创建img标签
    var img = document.createElement("img");
    img.src = "images/pic" + (i + 1) + ".jpg";
    li.appendChild(img);
}

// 3. 获取所有的li
var allLis = ps.children;

// 4. 求出屏幕的宽度和高度
var screenW = document.documentElement.clientWidth - 250;
var screenH = document.documentElement.clientHeight - 360;

// 5. 遍历
for(var j=0; j<allLis.length; j++){
    // 5.1 取出单个li标签
    var li = allLis[j];

    // 5.2 随机分布
    li.style.left = _.random(0, screenW) + 'px';
    li.style.top = _.random(0, screenH) + 'px';

    // 5.3 随机角度
    li.style.transform = 'rotate(' + _.random(0, 360) +'deg)';

    // 5.4 监听点击事件
    li.onclick = function () {
        for(var i = 0; i<allLis.length; i++){
            allLis[i].className = '';
        }
        this.className = 'current';
    }
}

```