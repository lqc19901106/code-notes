# HTML+CSS

## 1. html5的新特性

- 语义化标签
  

header footer summary article content dialog section nav details

- 表单增强
> 新属性：placeholder pattern required autofocus multiple max（min）等
> 新表单类型：date time datetime range number color url email search week 等

- 多媒体标签
    > video\audio
    ```html
    <audio controls>
        <source src="horse.ogg" type="audio/ogg">
        <source src="horse.mp3" type="audio/mpeg">
        您的浏览器不支持 audio 元素。
    </audio>

    <video width="320" height="240" poster="url地址" controls >
        <source src="movie.mp4" type="video/mp4">
        <source src="movie.ogg" type="video/ogg">
        您的浏览器不支持Video标签。
    </video>
    // preload 属性：none不进行预加载，metadata 只预加载媒体的元数据 auto 预加载全部视频或音频
    // poster 当视频不可用时，最好使用  poster 属性，以免展示视频的区域中出现一片空白。
    // loop 循环播放视频或音频
    // controls 属性指定是否为视频或音频添加浏览器自带的播放用的控制条。控制条中具有播放、暂停等按钮。
    // media.play()播放视频  media.play()暂停视频 media.load() 重新载入视频   media.paused 表示状态
    
    <bgsound  src="背景音乐地址"/> //给某个网页设置背景音乐

    ```

- svg

    > SVG是指可伸缩的矢量图形

- Canvas

    ```js
    // html
     <canvas id="ctx" width="200" height="100"></canvas>

    //js
     var canvas = document.getElementById('ctx');
     var ctx = canvas.getContext('2d');
     ctx.font = "30px Arial";
     ctx.fillStyle = "#FF0000";
     ctx.fillRect(0, 0, 100, 100);
    // 路径
     ctx.moveTo(0,0);
     ctx.lineTo(200,100);
     ctx.stroke();
    //绘制文字
     ctx.fillText("Hello World",10,50);

    //渐变颜色处理
    var gdColor = ctx.createLinearGradient(0,0,200,0);
    gdColor.addColorStop(0,"red");
    gdColor.addColorStop(1,"white");
    ctx.fillStyle=grd;
    // 绘制图像
    var img=document.getElementById("img-scream");
    ctx.drawImage(img,10,10);
    ```

- 地理定位API

```js
    //地理定位
    // window.navigator.geolocation {
    //     getCurrentPosition:  fn  用于获取当前的位置数据
    //     watchPosition: fn  监视用户位置的改变
    //     clearWatch: fn  清除定位监视
    // }

    navigator.geolocation.getCurrentPosition(
        function(pos) { 
            //用户定位成功
            console.log('定位时间：',pos.timestamp)
            console.log('经度：',pos.coords.longitude)
            console.log('纬度：',pos.coords.latitude)
            console.log('海拔：',pos.coords.altitude)
            console.log('速度：',pos.coords.speed)
        }，
        function(err){
            //用户定位失败
        }
    );

```

- 拖拽API

```js
dragSource
// 拖放事件的源对象
// dragstart： 开始拖拽
// drag  拖动中
// dragend 拖拽结束

dragTarget
// 拖放事件的目标对象
// dragenter：拖动着进入
// dragover：拖动着悬停
// dragleave：拖动着离开
// drop：释放

dataTransfer
//用于数据传递的“拖拉机”对象；
e.dataTransfer.setData(key, value)
var value = e.dataTransfer.getData(key)

```

- WebWorker

> web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。
```js
    if(typeof Worker !== 'undefined') {
        var nw = new Worker('demo_worker.js');
        nw.onmessage = function(evt) {
            console.log(evt.data);
        }
    }
    function stopWorker(){ 
        w.terminate();
    }

    //demo_worker.js
    postMessage('demo_worker test');
```

- WebStorage

> 客户端存储的对象主要有两个对象：
> - localStorage: 没有时间限制
> - sessionStorage: 针对session的数据存储，用户关闭浏览器后，数据会被删除

```js
    //api
    localStorage.setItem(key,value);
    localStorage.setItem(key);
    localStorage.removeItem(key);
    localStorage.clear()
```

- WebSocket

> HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议
```js
    if(typeof WebSocket !== 'undefined'){
        var ws = new WebSocket('ws://127.0.0.1/getData');
        ws.open = function(){
            //打开成功
            //用send方法想后端发送数据
            ws.send({});
        }
        ws.onmessage = function(evt){
            //接收后端的数据
            console.log(evt.data);
        }
        ws.onclose = function() {
            //关闭websocket
        }
    }
```
### 相关面试题
> svg和canvas的区别
>
> - canvas是位图，svg是矢量的
>
> - svg只是事件的绑定，canvas不支持
>
> - canvas在绘制后不能修改
>
> - canvas支持的颜色比svg多
>
> - 绘制方式不同，canvas主要靠js绘制，svg靠html绘制
>

## 2. 盒子模型

- margin 外边距
- border 边框
- padding 内边距
- content 内容区域

> 兼容性
> - 标准盒模型：**width 和 height 指内容区域的宽度** ，增加padding和margin的宽度不会影响width和height只会影响总尺寸
> - 怪异盒模型：**width 和 height 指内容区域的宽度 + padding + border** 

> box-sizing 默认为 content-box，显示为标准盒模型； border-box显示为怪异盒模型

## 3. BFC（Block Formating Context）块级格式上下文
> **触发条件**
> - float 不为 none
> - overflow 不是 visible
> - position的值为 absolute fixed
> - display的值为 inline-block、flex、grid 和 table-*

> **渲染规则**
> - 自顶向下，依次排列
> - 垂直方向的margin会发生重叠
> - 每个盒子的做外边缘（margin-left）会碰到容器的左边缘（border-left）
> - 内部元素和外部元素互不影响
> - BFC盒子的高度，浮动元素也会参与计算

> **用途**
>
> - 清除元素内部浮动

## 4. CSS 浮动

浮动的框可以左右移动，直到外边缘遇到包含框或者另一个浮动框

引发问题
1. 父元素的高度无法撑开，影响父元素的兄弟元素的布局
2. 与浮动元素同级的非浮动元素会紧跟后面
3. 如果浮动元素不是父元素下的第一个元素，会影响页面结构

清除浮动的方法
1. 设置父元素为BFC元素
2. 浮动标签的后面添加空标签，设置`clear:both;`
3. 使用伪元素清除浮动
    ```css

    .clearfix:after{
        content: " "; 
        display: block; 
        height: 0; 
        clear: both; 
        visibility: hidden;  
    }
    /* IE6 */
    .clearfix {
        /* 触发 hasLayout */ 
        zoom: 1; 
    }
    ```

## 5. flex原理

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。

主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；

交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

> **属性介绍**
```css
.flexbox{
    display: flex;
    flex-direction: row(default) | column | row-reverse | column-reverse; 
    // flex布局的方向
    flex-wrap: nowrap | wrap(换行，第一行在上) | wrap-reverse(换行，第一行在下);   
    flex-flow: <flex-direction> || <flex-wrap>;//（默认row nowwrap)
    justify-content: flex-start | flex-end | center | space-between | space-around; 
    //(水平方向分布方式)
    align-items: flex-start | flex-end | center | baseline | stretch;
    //(垂直方向的分布方式)
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
    //多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
}
.flex-items{
    order: <integer>; 
    //定义项目的排列顺序。数值越小，排列越靠前，默认为0。
  	flex-grow: <number>; 
    //定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
    flex-shrink: <number>;
    // 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
    flex-basis: <length> | auto; /* default auto */
    //定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间。
   //flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
  	//允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性
}
```
> 如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。
> 如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
> 如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。
> 如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。


> **flex实现垂直水平布局**
```css
.flexbox{
    display: flex;
    justify-content: center;
    align-items: center;
}
```

## 6. CSS单位

- rem 相对根元素的字体大小
- em  相对父元素的字体大小
- vh  屏幕高度
- vw  屏幕的宽度

## 7. CSS3新特性

- 选择器
```css
    :last-child /* 选择元素最后一个孩子 */
    :first-child /* 选择元素第一个孩子 */
    :nth-child(1) /* 按照第几个孩子给它设置样式 */
    :nth-child(even) /* 按照偶数 */
    :nth-child(odd)  /* 按照奇数 */
    :disabled /* 选择每个禁用的E元素 */
    :checked /* 选择每个被选中的E元素 */
    :not(selector) /* 选择非 selector 元素的每个元素 */
    ::selection /* 选择被用户选取的元素部分 */
```
- 背景和边框
```
background-clip 规定背景的绘制区域
background-origin
background-size

border-image
border-radius
box-shadow
```
- 渐变
```css
background: linear-gradient(direction, color-stop1, color-stop2,...);   
//线性渐变 向下/向上/向左/向右/对角方向
background: radial-gradient(center, shape size, start-color,...,last-color);
//径向渐变  从中心开始
```
- 多媒体查询
```html
// 使用媒体类型
<link rel="stylesheet" type="text/css" href="site.css" media="screen"/>
<link rel="stylesheet" type="text/css" href="print.css" media="print"/>
```

```css
//媒体查询规则
@media (min-width:800px) and (not max-width:1200px) or (orientation:portrait){
  //css
}

```

- CSS3 文本效果

hanging-punctuation	规定标点字符是否位于线框之外。
punctuation-trim	规定是否对标点字符进行修剪。
text-align-last	设置如何对齐最后一行或紧挨着强制换行符之前的行。
text-emphasis	向元素的文本应用重点标记以及重点标记的前景色。
text-justify	规定当 text-align 设置为 "justify" 时所使用的对齐方法。
text-outline	规定文本的轮廓。
text-overflow	规定当文本溢出包含元素时发生的事情。
text-shadow	向文本添加阴影。
text-wrap	规定文本的换行规则。
word-break	规定非中日韩文本的换行规则。
word-wrap	允许对长的不可分割的单词进行分割并换行到下一行。

- 过渡和动画
```css
.transition{
    transition-property: width;
    transition-duration:1s;
    transition-timing-function: linear;
    transition-delay:2s;
    //或者
    transition: width 1s linear 2s;
}
@keyframes animateName{
    0%{background: red;}
    25%{background: yellow;}
    50%{background: blue;}
    100%{background: green;}
}
.animate{
    animation-name: animateName;
    animation-duration:5s;
    animation-timing-function: linear;
    animation-delay:2s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-play-state: running;
}
```
- 转换变形
```css
transform
transform-origin
```

## 8. viewport

viewport就是设备的屏幕上能用来显示我们的网页的那一块区域，
在具体一点，就是浏览器上(也可能是一个app中的webview)用来显示网页的那部分区域

``` html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
```
| 属性 | 说明 |
| ---- | ---- |
|  width   |  设置layout viewport  的宽度，为一个正整数，或字符串"width-device"    |
|  initial-scale    |  设置页面的初始缩放值，为一个数字，可以带小数    |
|  minimum-scale    | 允许用户的最小缩放值，为一个数字，可以带小数     |
|  maximum-scale    | 允许用户的最大缩放值，为一个数字，可以带小数     |
|  height    |   设置layout viewport  的高度，这个属性对我们并不重要，很少使用   |
|  user-scalable     |  是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许    |

## 9. css 优化提高性能的方法有哪些？

**加载性能：**

- （1）css压缩：将写好的css进行打包压缩，可以减少很多的体积。
- （2）css单一样式：当需要下边距和左边距的时候，很多时候选择:margin:top 0 bottom 0;但marginbottom:bot
  tom;margin-left:left;执行的效率更高。

- （3）减少使用@import,而建议使用link，因为后者在页面加载时一起加载，前者是等待页面加载完成之后
再进行加载。

**选择器性能：**

- （1）关键选择器（key selector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部
  分）。CSS选择符是从右到
  左进行匹配的。当使用后代选择器的时候，浏览器会遍历所有子元素来确定是否是指定的元素等等；

- （2）如果规则拥有ID选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统
  就不会浪费时间去匹
  配它们了）。

- （3）避免使用通配规则，如*{}计算次数惊人！只对需要用到的元素进行选择。

- （4）尽量少的去对标签进行选择，而是用class。

- （5）尽量少的去使用后代选择器，降低选择器的权重值。后代选择器的开销是最高的，尽量将选择器的深度降
  到最低，最高不要超过
  三层，更多的使用类来关联每一个标签元素。

- （6）了解哪些属性是可以通过继承而来的，然后避免对这些属性重复指定规则。

**渲染性能：**

- （1）慎重使用高性能属性：浮动、定位。

- （2）尽量减少页面重排、重绘。

- （3）去除空规则：｛｝。空规则的产生原因一般来说是为了预留样式。去除这些空规则无疑能减少css文档体
  积。

- （4）属性值为0时，不加单位。

- （5）属性值为浮动小数0.xx，可以省略小数点之前的0。

- （6）标准化各种浏览器前缀：带浏览器前缀的在前。标准属性在后。

- （7）不使用@import前缀，它会影响css的加载速度。

- （8）选择器优化嵌套，尽量避免层级过深。

- （9）css雪碧图，同一页面相近部分的小图标，方便使用，减少页面的请求次数，但是同时图片本身会变大，
  使用时，优劣考虑清楚，再使用。

- （10）正确使用display的属性，由于display的作用，某些样式组合会无效，徒增样式体积的同时也影响解
  析性能。

- （11）不滥用web字体。对于中文网站来说WebFonts可能很陌生，国外却很流行。web fonts通常体积庞
  大，而且一些浏览器在下载web fonts时会阻塞页面渲染损伤性能。

**可维护性、健壮性：**

- （1）将具有相同属性的样式抽离出来，整合并通过class在页面中进行使用，提高css的可维护性。
- （2）样式与内容分离：将css代码定义到外部css中。

## 10. 浏览器是怎样解析css的？

样式系统从关键选择器开始匹配，然后左移查找规则选择器的祖先元素。只要选择器的子树一直在工作，样式系统就会持续左移，直到和规则匹配，或者是因为不匹配而放弃该规则。
试想一下，如果采用从左至右的方式读取CSS规则，那么大多数规则读到最后（最右）才会发现是不匹配的，这样做会费时耗能，最后有很多都是无用的；而如果采取从右向左的方式，那么只要发现最右边选择器不匹配，就可以直接舍弃了，避免了许多无效匹配。

## 相关面试题
### 1. href 属性和 src属性的区别

> href: 超链接地址
> src: 嵌入的外部资源的地址

### 2. script 的 `async` 属性和 `defer` 属性的区别

defer 脚本延迟到dom解析和显示完成后再显示（最好只有一个）；
async 只适用于外部脚本，告诉浏览器立即下载文件；
不同的是标记为async的脚本并不保证按照指定它们的先后顺序执行。

### 3. em和strong的区别
em表示强调的内容，显示为斜体； strong 强调重要性；显示为粗体

### 4. Doctype作用？标准模式与兼容模式各种什么区别？
- 告诉浏览器以什么样的文档标准来解析html文件
- 标准模式的排版和JS运作模式都是该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式来显示，模拟老式浏览器的行为以防止站点无法工作。

### 5. html5哪些操作可以SEO优化
- title标签和元素的title属性
- meta标签
- 其他的语义化标签： header footer nav article 

### 6. 标签上title与alt属性的区别是什么？

alt属性表示图片不显示时的代表文字
title 为属性提供信息

### 7. 前端页面分层

- 结构层  主要由html负责

- 行为层  主要由css负责

- 表示层  只要由css 负责

### 8.如何设置小于 12px的字体

### 9. 如果需要手动写动画，你认为最小时间间隔是多久，为什么？

>   多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms




