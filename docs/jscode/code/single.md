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
##### 天猫导航特效(缓动动画)
```css
#nav{
    width: 900px;
    height: 63px;
    background:url("images/doubleOne.png") no-repeat right
    center #fff;
    border-radius: 5px;
    position: relative;
    margin: 100px auto;
}

#nav ul{
    position: relative;
}

#nav ul li{
    float: left;
    width: 88px;
    height: 63px;
    text-align: center;
    line-height: 70px;
    cursor: pointer;
}

#t_mall{
    width: 88px;
    height: 63px;
    background: url("images/tMall.png") no-repeat;
    position: absolute;
}
```
```html
<nav id="nav">
    <span id="t_mall"></span>
    <ul>
        <li>双11狂欢</li>
        <li>服装会场</li>
        <li>数码家电</li>
        <li>家具建材</li>
        <li>母婴童装</li>
        <li>手机会场</li>
        <li>美妆会场</li>
        <li>进口会场</li>
        <li>飞猪旅行</li>
    </ul>
</nav>
```
```js
var nav = $('nav'),
    t_mall = nav.children[0], //拿到动的那个方块
    ul = nav.children[1],   
    lis = ul.children,  
    startX = 0;
for(var i=0;i<lis.length;i++){
    var li = lis[i];
     //鼠标进入状态，滑块滑动到鼠标所指的li的位置
    li.onmouseover = function(){
        end = this.offsetLeft;   
        /*鼠标进入获取当前鼠标放在li上的偏移量
        把该值赋给end，就可以知道缓冲动画的终点
        假设此时的终点是400的位置
        */
    }
    //鼠标点击状态，设置滑块的起始位置
    li.onmousedown = function(){
        startX = this.offsetLeft;
        //鼠标点击该li，设置滑块的起始位置
    }
    //鼠标离开状态，滑块滑回到起始位置
    li.onmouseout = function(){
        end = startX;           
        /*鼠标离开该li，就把起始位置的值赋给缓冲动画的终点
        让滑块(位置在400)回到起始位置归位*/        
    }
}
    // 缓冲动画
var start=0,end=0;
setInterval(() => {
    start = start + (end-start)*0.1;    //滑动的距离
    t_mall.style.left = start + 'px';
    //获得偏移量以后，就可以使用滑块滑动到该li上，让他滑动
}, 10);
function $(id){
    return typeof id === 'string' ? document.getElementById(id) : null;
}
```
##### 简单的缓动动画
```css
.box{
    width: 200px;
    height: 200px;
    background: yellow;
    text-align: center;
    position: relative;
    top: 0px;
    left: 0;
     
}
button{
    padding: 0 20px;
    height: 40px;
    margin-top: 40px;
}
```
```html
<div class="box">动画越来越来越慢，叫缓动动画</div>
<button>点击有动画效果</button> 
```
```js
var odiv = document.querySelector('.box'),
obtn = document.querySelector('button'),
timer = null,
leader = 0, //开始位置
target = 400;//结束位置

obtn = onclick = function(){
    timer = setInterval(() => {
        leader = leader + (target - leader) /10; //滑动的距离
        //起点 = 起点 + （终点 - 起点） / 10  //下一步的长度该步数的1/10
        odiv.style.left = leader + 'px'; 
    }, 10); //10表示 0.01秒走一步
}
/*动画分析
    1.  leader=0,target=400
        40 = 0 + (400-0) /10    走了40px  //下一步的长度该步数的1/10
    2.  leader=40,target=400
        76 = 40 + (400-40) /10  走了36
    3.  leader=76,target=400
        108.4 = 76 + (400-76) /10 走了 32.4
    ···
      leader=400,target=400
        400 = 400+ (400-400) /10  走了0 ==>到达终点

*/
```
##### 简单的缓动动画1
```js
var timer = null, target = 600;
$("btn").onclick = function () {
    // 1. 清除定时器
    clearInterval(timer);

    // 2. 设置定时器
    timer = setInterval(function () {
        // 移动距离 = box.offsetLeft + 步长
        box.style.left = box.offsetLeft + 10 + "px";

        if(box.offsetLeft >= target){
            clearInterval(timer);
        }
    }, 20);
}
```
##### 商品放大镜效果
```js
var box = $('box'),
    small_box = box.children[0],
    small_img = small_box.children[0],
    mask = small_box.children[1],
    big_box = box.children[1],
    big_img = big_box.children[0],
    list = $('list'),
    imgs = list.children;

//1. 监听进入小盒子，遮罩显示，大盒子显示
small_box.onmouseover = function(){
    mask.style.display = 'block';
    big_box.style.display = 'block';
}
//2. 监听小盒子内鼠标的移动事件
small_box.onmousemove = function(e){
    //2.1 计算鼠标的坐标
    var e = e || window.event;
    var x = e.pageX - small_box.offsetParent.offsetLeft - mask.offsetWidth/2;
    /*鼠标的x坐标 = 鼠标在文档的x坐标 - 小盒子的父亲距离文档左边的距离 - 遮罩宽度的一半(鼠标指针放在遮罩的正中间)*/
   var y = e.pageY - small_box.offsetParent.offsetTop - mask.offsetHeight/2;
    /*鼠标的y坐标 = 鼠标在文档的y坐标 - 小盒子的父亲距离文档顶部的距离 - 遮罩宽度的一半(鼠标指针放在遮罩的正中间)*/
    //2.2 设置边界，遮罩不能超过盒子的
    if(x<0){
        x = 0;
    } else if(x >= small_box.offsetWidth - mask.offsetWidth)(
        x= small_box.offsetWidth - mask.offsetWidth
    )
    if(y<0){
        y = 0;
    } else if(y >= small_box.offsetHeight - mask.offsetHeight)(
        y= small_box.offsetHeight - mask.offsetHeight
    )
    //2.2 mask的坐标
    console.log(mask.style.left = x + 'px');
    console.log(mask.style.top = y + 'px');
    //2.3 放大事件
    //x / bigx = small_img / big_img
    big_img.style.left = -x / (small_img.offsetWidth / big_img.offsetWidth) + 'px';
    big_img.style.top = -y / (small_img.offsetHeight / big_img.offsetHeight) + 'px';
}
//3. 监听退出小盒子，遮罩隐藏，大盒子隐藏
small_box.onmouseout = function(){
    mask.style.display = 'none';
    big_box.style.display = 'none';
}
//4. 监听进入小图

for(var i=0;i<imgs.length;i++){
    (function(index){
        var img = imgs[index];
        img.onmouseover = function(){
            small_img.src = 'images/pic00' + (index + 1) + '.jpg'
            big_img.src = 'images/pic0' + (index + 1) + '.jpg'
        }
    })(i)
}

function $(id){
    return typeof id === 'string' ? document.getElementById(id) : null;
}
```
##### 滚动条特效
```css
        #progress{
            width: 600px;
            height: 35px;
            line-height: 35px;
            /*background-color: #e8e8e8;*/
            margin: 100px auto;

            position: relative;
        }

        #progress_bar{
            width: 500px;
            height: 100%;
            background-color: #ccc;
            border-radius: 8px;

            position: relative;
        }

        #progress_value{
            position: absolute;
            right: 30px;
            top: 0;
        }

        #progress_bar_fg{
            width: 0;
            height: 100%;
            background-color: orangered;
            border-top-left-radius: 8px;
            border-bottom-left-radius: 8px;
        }

        span{
            width: 25px;
            height: 50px;
            background-color: orangered;

            position: absolute;
            left: 0;
            top: -7px;
            border-radius: 5px;
            cursor: pointer;
        }
```
```html
<div id="progress">
    <div id="progress_bar">
        <div id="progress_bar_fg"></div>
        <span></span>
    </div>
    <div id="progress_value">0%</div>
</div>
```
```js
//获取元素
var progress = $('progress'),
    progress_bar = progress.children[0],
    progress_bar_fg = progress_bar.children[0],
    mask = progress_bar.children[1],
    progress_value = progress.children[1];
//2. 监听滑块
mask.onmousedown = function(event){
    //获取滑块的初始位置
    var offsetLeft = event.clientX - mask.offsetLeft;
    //监听鼠标的移动事件，鼠标在整个文档里移动时都可以移动
    document.onmousemove = function(event){
        //鼠标移动的距离 = 鼠标距离文档左边的距离 - 滑块的初始距离
        var x = event.clientX - offsetLeft;
        //设置边界
        if(x<0){
            x=0;
            //如果移动距离大于滚动条的长度-滑块本身的距离，那么移动距离就等于该距离
        }else if(x>= progress_bar.offsetWidth - mask.offsetWidth){
            x= progress_bar.offsetWidth - mask.offsetWidth
        }
        //移动滑块
        mask.style.left = x + 'px';
        //滑块经过的宽度
        progress_bar_fg.style.width = x + 'px';
        //经过的宽度所占整条滚动条的比例
        progress_value.innerHTML = parseInt(x / (progress_bar.offsetWidth - mask.offsetWidth) * 100) + '%';
        return false;
    }
    //监听鼠标的抬起事件
    document.onmouseup = function(){
        document.onmousemove = null;
    }
}
function $(id){
    return typeof id === 'string' ? document.getElementById(id) : null;
}  
```
##### 水平滚动特效
```css
        #box{
            width: 800px;
            height: 200px;
            border: 1px solid #ddd;

            position: relative;
            margin: 100px auto;

            overflow: hidden;
        }

        #box ul{
            width: 2600px;
            position: absolute;
            left: 0;
            top: 20px;
        }

        #box ul li{
            float: left;
        }

        #box_bottom{
            position: absolute;
            left: 0;
            bottom: 0;
            background-color: #e8e8e8;

            width: 100%;
            height: 25px;
        }

        .mask{
            position: absolute;
            left: 0;
            top:0;
            height: 25px;
            background-color: orangered;
            border-radius: 12px;
            cursor: pointer;
        }
```
```html
    <div id="box">
        <ul id="box_top">
            <li><img src="images/img1.jpg" alt=""></li>
            <li><img src="images/img2.jpg" alt=""></li>
            <li><img src="images/img3.jpg" alt=""></li>
            <li><img src="images/img4.jpg" alt=""></li>
            <li><img src="images/img5.jpg" alt=""></li>
            <li><img src="images/img6.jpg" alt=""></li>
            <li><img src="images/img7.jpg" alt=""></li>
            <li><img src="images/img8.jpg" alt=""></li>
            <li><img src="images/img1.jpg" alt=""></li>
            <li><img src="images/img2.jpg" alt=""></li>
            <li><img src="images/img1.jpg" alt=""></li>
            <li><img src="images/img2.jpg" alt=""></li>
            <li><img src="images/img3.jpg" alt=""></li>
            <li><img src="images/img4.jpg" alt=""></li>
            <li><img src="images/img5.jpg" alt=""></li>
            <li><img src="images/img6.jpg" alt=""></li>
            <li><img src="images/img7.jpg" alt=""></li>
            <li><img src="images/img8.jpg" alt=""></li>
            <li><img src="images/img1.jpg" alt=""></li>
            <li><img src="images/img2.jpg" alt=""></li>
        </ul>
        <div id="box_bottom">
            <span class="mask"></span>
        </div>
    </div>
```

```js
// 1. 获取需要的标签
var box = $("box"); //盒子
var box_top = $("box_top"); //需滚动内容
var box_bottom = $("box_bottom"); //滚动轴区域
var mask = box_bottom.children[0];  //滚动条
// 2. 获取滚动条的区域的起始值
//滚动条的宽度 / 滚动轴区域 = 盒子宽度 / 需滚动内容宽度
//滚动条的宽度
var scrollLength = (box.offsetWidth / box_top.offsetWidth) * box_bottom.offsetWidth;
mask.style.width = scrollLength + 'px';
//滚动
mask.onmousedown = function(e){
    var e = event || window.event,
        beginX = e.clientX - mask.offsetLeft;
    document.onmousemove = function(e){
        var e = event || window.event,
            //计算滚动条移动的距离
            endX = e.clientX - beginX;

        //设置边界
        if(endX<0){
            endX = 0;
        }else if(endX >= box.offsetWidth - mask.offsetWidth){
            endX = box.offsetWidth - mask.offsetWidth
        }
        //移动起来
        mask.style.left = endX + 'px';
        //内容滚动起来
        //内容移动的距离 / 滚动条移动的距离= （需滚动内容的宽度 - 盒子宽度） / （滚动轴的宽度- 滚动条的宽度）
        var content_len = (box_top.offsetWidth - box.offsetWidth) / (box_bottom.offsetWidth - mask.offsetWidth) * endX;
        box_top.style.left = - content_len + 'px';
        return false;
    }
    document.onmouseup = function(){
        document.onmousemove = null;
    }
}
function $(id){
    return typeof id === 'string' ? document.getElementById(id) : null;
}
```
##### 瀑布流布局

```js
// 瀑布流布局
waterFull('main','box');

window.onscroll = function(){
    if(checkPageScrol()){
        var dataArr = [
            {'src':'img01.jpg'},
            {'src':'img03.jpg'},
            {'src':'img05.jpg'},
            {'src':'img07.jpg'},
            {'src':'img09.jpg'},
            {'src':'img11.jpg'},
            {'src':'img13.jpg'},
            {'src':'img15.jpg'},
            {'src':'img17.jpg'},
            {'src':'img19.jpg'},
            {'src':'img21.jpg'},
            {'src':'img23.jpg'},
        ]
        var str='',arr=[];
        for(var k=0;k<dataArr.length;k++){
            str = '<div class="box"><div class="pic"><img src="images/'+ dataArr[k].src + '" alt=""></div></div>';
            arr.push(str);
        }
        //console.log(arr.join(''))                
        $('main').innerHTML += arr.join('')
        waterFull('main','box');
                
}
function waterFull(wai,nei){
//1. 外层盒子居中
    var waiBox = $(wai),
        neiBox = waiBox.getElementsByClassName(nei),
    //1.1 计算外层盒子宽度
    //1.2 计算内层单个盒子宽度（宽度固定）
        neiBoxWidth = neiBox[0].offsetWidth,
        //console.log(neiBoxWidth);
    //1.3 计算可视区域内横向能放几个盒子(单个盒子总数)
    screenX = document.documentElement.clientWidth || document.body.clientWidth            
    cols = parseInt(screenX / neiBoxWidth);
    //console.log(cols);
    //外层盒子的宽度 = 单个盒子总数 * 单个盒子宽度
    waiBox.style.width = parseInt(cols * neiBoxWidth) + 'px';
    waiBox.style.margin = '0 auto';
    //console.log(waiBox.style.width);
    //2. 布局
    //1. 遍历所有子盒子
    var neiBoxArr= [],//存放一行的盒子的高度
        minBoxHeight=0,//存放盒子的最小高度
        minBoxIndex=0; //存放最小高度盒子的索引
    for(var i=0;i<neiBox.length;i++){
        //1. 先获取第一行的单个盒子的高度放入数组
        if(i<cols){
            neiBoxArr.push(neiBox[i].offsetHeight);
        } else{
            //获得数组中盒子的最小高度
            minBoxHeight = _.min(neiBoxArr);
            /*  需要知道哪个数组，以及最小盒子的高度，数组的值跟最小高度对比，
                如果相等，俺么该数组对应的索引就是所求的最小高度的索引*/
            minBoxIndex = getMinBoxIndex(neiBoxArr,minBoxHeight); 
            neiBox[i].style.position = 'absolute';
            //把盒子放在第一行最小的那个盒子（索引）的下面 = 盒子的top值就是最小盒子的高度
            neiBox[i].style.top = minBoxHeight + 'px'; //120
            //neiBox[i].style.top = neiBoxArr[minBoxIndex] + 'px'; //120
            //盒子的left值 = 最小高度子盒子的索引*单个盒子的宽度
            neiBox[i].style.left= minBoxIndex * neiBoxWidth + 'px';
            //【更新最小盒子的索引 变成了该索引+放在该索引盒子下面的那个盒子的高度】
            neiBoxArr[minBoxIndex]+= neiBox[i].offsetHeight                    
        }
    }
    console.log(neiBoxArr,minBoxHeight,minBoxIndex,neiBox[minBoxIndex].offsetHeight);      
                        
        
    //获取最小盒子的索引
    function getMinBoxIndex(arr,val){
        for(var j=0;j<arr.length;j++){
            if(arr[j] === val){
                return j
            }
        }
    }

    //监听页面滚动
    function checkPageScrol(){
        var waiBox = $('main'),
            neiBox = waiBox.getElementsByClassName('box'),
            screenY = document.documentElement.clientHeight || document.body.clientHeight,
            //如果当前页面最后一个子盒子的高度的一半+该子盒子的offsetTop <= 当前页面的高度+ 滚动高度 说明该页面滚动了
            lastBox = neiBox[neiBox.length-1]; //拿到最后一个盒子
            return lastBox.offsetHeight*0.5 + lastBox.offsetTop <= screenY + scroll().top;
    }

    function $(id){
        return typeof id === 'string' ? document.getElementById(id) : null;
    }
    
```
##### 吸顶特效
```js
nav_top = $('nav').offsetTop;
window.onscroll = function(){   
    if(scroll().top >= nav_top){    
        //如果滚动高度超过导航栏自身高度
        $('nav').className = 'nav'
    } else {
        $('nav').className = ''
    }
}
```
##### 侧边广告滚动特效
```js
//1. 获取元素到顶部的距离
var aside = $('aside');
aside_top = aside.offsetTop;
var start=0,end=0,timer;//动画准备
window.onscroll =function(){
    // 清楚定时器
    clearInterval(timer);
    //设置终点
    end = aside_top + scroll().top;
    // 缓动动画
    timer = setInterval(function(){
        start = start + (end -start)*0.1;
        aside.style.top = start + 'px';
        console.log(start,end);
        // 缓动动画终点
        if(Math.ceil(start) === end){
            clearTimeout(timer)
        }
    },10)
}
```
##### 返回顶部
```js
var start = 0,end=0, timer =null;
window.onscroll = function(){
    scroll().top > 0 ? xian($('top')) : yin($('top'));
    start = scroll().top;
}        
$('top').onclick = function(){
    clearInterval(timer);
    timer = setInterval(function(){
        start = start + (end - start)*0.1;
        window.scrollTo(0,start);
        if(Math.abs(start) === end){
            clearTimeout(timer)
        }
    },10);
}
```
##### 登录遮罩
```css
#panel{
    width: 100%;
    height: 100%;
    background-color: #000;
    opacity: 0.4;
    filter: alpha(opacity: 40);

    position: absolute;
    left:0;
    top:0;

    display: none;
}

#login{
    width: 300px;
    height: 300px;
    background-color: skyblue;
    border-radius: 5px;

    position: fixed;
    left: 50%;
    top:50%;
    margin-left: -150px;
    margin-top: -150px;

    display: none;
}
```
```html
   <button id="btn">立即登录</button>
   <div id="panel"></div>
   <div id="login"></div>
```
```js
// 1. 监听按钮的点击
$("btn").onclick = function (event) {
    // 1.0 阻止冒泡
    if(event && event.stopPropagation){
        event.stopPropagation();
    }else {
        window.event.cancelBubble = true;
    }

    // 1.1 显示面板和蒙版
    $("panel").style.display = "block";
    $("login").style.display = "block";

    // 1.2 隐藏滚动条
    document.body.style.overflow = "hidden";
};

// 2.点击文档
document.onclick = function (event) {
    var e = event || window.event;

    // 2.1 获取点击的标签
    var targetId = e.target ? e.target.id : e.srcElement.id;
    console.log(e.target)            
    // 2.2 判断
    if(targetId !== "login"){
        // 1.1 隐藏面板和蒙版
        $("panel").style.display = "none";
        $("login").style.display = "none";

        // 1.2 显示滚动条
        document.body.style.overflow = "auto";
    }else {
        window.location.href = "http://www.baidu.com";
    }

}
```