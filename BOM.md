## BOM

BOM （Browser Object Model）即浏览器对象模型，它提供了独立于内容而与浏览器进行交互的对象，其核心对象是 Window，表示浏览器的一个实例。在浏览器中， `window`对象有双重角色，它既是通过 *JS* 访问浏览器窗口的一个接口，又是 `ECMAScript` 规定的 `Global` 对象。所有全局作用域中声明的变量、函数都会变成 `window` 对象的属性和方法。

## URL简介

### 网址解析

我们在浏览器内输入的网址是有一定格式的，认识这种格式有助于我们认识浏览器加载页面过程，了解应该如何去使用或者修改网址信息。

一般而言，网址统称为 URL，是统一资源定位符（*Uniform Resource Locator*）的简称，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该如何处理它。

URL 的一般语法格式为：

```http
protocol://host[:port]/path/[?query]#fragment
```

| 组成     | 说明                                                        |
| :------- | :---------------------------------------------------------- |
| protocol | 通讯协议，常见的 *http*，*https*                            |
| host     | 域名或网络IP地址                                            |
| port     | 端口号，*http*协议对应 80端口，*https*协议对应443端口       |
| path     | 资源路径。以 / 分隔的路径地址，表示主机上的文件存放地址     |
| query    | 查询参数。以 ? +键值对的形式表示，多个键值对通过 & 符号分隔 |
| fragment | 片段 # 后面的内容，常见于链接，锚点                         |

以百度的地址为例：*[https://www.baidu.com](https://www.baidu.com/)*

- *https*：HTTP超文本传输协议，也就是网页在网上传输的协议
- *www*：服务器名，表示网站的功能。一般使用 *www* 服务器表示网站首页
- *baidu.com*：域名，是用来定位网站的独一无二的名字
- *[www.baidu.com](http://www.baidu.com/)*：网站名，由服务器名+域名组成

使用百度搜索内容：https://www.baidu.com/s?wd=孙悟空图片

- */s* 是请求路径
- *?wd=孙悟空图片* 是查询参数

**查询参数的作用是给服务器信息提示，帮助服务器返回给我们更精确的结果。**比如你去书店找书，那么你需要给店员提供书名或者作者名或者其它的信息，帮助他快速找到这本书，你提供的这些信息就是查询参数，找到的书就是服务器返回给你的内容，如果你不提供这些信息，那么你将什么也得不到。就像你不在百度搜索框中输入搜索内容，直接点击搜索，结果只会导致页面刷新而已，因为全知的度娘也不知道你想要什么。

如果我们将 *a* 标签作为锚点使用，那么你会在浏览器地址栏发现网址有 *#xxx* 的后缀，这个就是所谓的片段。

### 域名

由于IP地址具有不方便记忆并且不能显示地址组织的名称和性质等缺点，人们设计出了域名，并通过网域名称系统（[DNS](https://baike.baidu.com/item/DNS)，Domain Name System）来将域名和IP地址相互映射，使人更方便地访问互联网，而不用去记住能够被机器直接读取的IP地址数串。

例如百度知道的IP地址：http://110.242.68.4/，很明显 *baidu.com* 要比这一串数字跟容易记忆，且标识性更强。

可以简单地将域名理解为 IP 地址的美化，使相应的地址更易读易记易辨识。

### 服务器名

因为一个域名可能包含不同的功能，比如 *163.com* 这个域名，网易旗下有新闻服务、音乐服务、网络课堂服务、邮箱服务等等，为了区别这些页面，使用不同的服务器名作为域名的前缀。

例如我们点击 [www.163.com](https://www.163.com/) 访问网易新闻，点击 [mail.163.com](https://mail.163.com/) 访问网易邮箱，点击 [study.163.com](http://study.163.com/) 访问网易云课堂，点击 [music.163.com](http://music.163.com/) 访问网易云邮箱。

### 同源策略

同源策略是一个安全策略。同源意味着两个网址的【协议、域名地址或者IP地址、端口号】三者完全一致，有一个不一致即为不同源。常见形式分析如下表：

| URL                                                    | 说明                   | 是否同源 |
| :----------------------------------------------------- | :--------------------- | :------- |
| http://www.a.com/a.js http://www.a.com/b.js            | 同一域名下             | 同源     |
| http://www.a.com/lab/a.js http://www.a.com/script/b.js | 同一域名下不同文件夹   | 同源     |
| http://www.a.com:8000/a.js http://www.a.com/b.js       | 同一域名，不同端口     | 不同源   |
| http://www.a.com/a.js https://www.a.com/b.js           | 同一域名，不同协议     | 不同源   |
| http://www.a.com/a.js http://70.32.92.74/b.js          | 域名和域名对应ip       | 不同源   |
| http://www.a.com/a.js http://script.a.com/b.js         | 主域相同，服务器名不同 | 不同源   |
| http://www.cnblogs.com/a.js http://www.a.com/b.js      | 不同域名               | 不同源   |

# window.open

使用 `window.open()` 方法可以在新的浏览器标签页或者新的浏览器窗口打开一个特定的网址。

注：不同浏览器使用该方法表现形式不尽相同。本文示例浏览器环境为：Google Chrome 版本 87.0.4280.88（64 位）



语法：

```js
var windowObjectReference = window.open(strUrl, strWindowName, [strWindowFeatures]);
```

### 参数

- *strUrl*：要加载的URL
- *strWindowName*：窗口目标名
- *strWindowFeatures*：一个描述新窗口特征的字符串



传入一个参数：

```html
<button onclick="window.open('http://bufanui.com')">点击打开新的标签页</button>
```



传入两个参数，第二个参数可以是任意名/a标签target属性值/iframe的name属性：

传入三个参数，第三个参数是一个逗号分隔的设置字符串，表示在新窗口中都显示哪些特性。下表列出了可以出现在这个字符串中的设置选项。

| 设置       | 值      | 说明                                                         |
| :--------- | :------ | :----------------------------------------------------------- |
| fullscreen | yes或no | 表示浏览器窗口是否最大化。仅限IE                             |
| height     | 数值    | 表示新窗口的高度。不能小于100                                |
| left       | 数值    | 表示新窗口的左坐标。不能是负值                               |
| location   | yes或no | 表示是否在浏览器窗口中显示地址栏。不同浏览器的默认值不同。如果设置为no，地址栏可能会隐藏，也可能会被禁用（取决于浏览器） |
| menubar    | yes或no | 表示是否在浏览器窗口中显示菜单栏。默认值为no                 |
| resizable  | yes或no | 表示是否可以通过拖动浏览器窗口的边框改变其大小。默认值为no   |
| scrollbars | yes或no | 表示如果内容在视口中显示不下，是否允许滚动。默认值为no       |
| status     | yes或no | 表示是否在浏览器窗口中显示状态栏。默认值为no                 |
| toolbar    | yes或no | 表示是否在浏览器窗口中显示工具栏。默认值为no                 |
| top        | 数值    | 表示新窗口的上坐标。不能是负值                               |
| width      | 数值    | 表示新窗口的宽度。不能小于100                                |

如果第二个参数设置后效果是**打开新的标签页**，再加上此参数，会打开新的浏览器窗口；

如果第二个参数设置后效果是**不打开新标签页**，方法会忽略第三个参数的作用：

#### 返回值

**WindowObjectReference**：打开的新窗口对象的引用。如果调用失败，返回值会是 `null`。如果父子窗口满足“**同源策略**”，可以通过这个引用访问新窗口的属性或方法。如果不同源，那么只能使用 *window.close()* 方法关闭打开的标签页或者窗口。

**同源策略**：两个网址的协议、域名地址/ip地址、端口号三者完全一致，即为符合同源。

返回的对象还有一个 `opener` 属性，其中保存着打开它的原始窗口对象。这个属性只在弹出窗口中的最外层 `window` 对象（top）中有定义，而且指向调用 `window.open()` 的窗口或框架。

# history对象

保存从窗口打开后，成功访问过的url的历史记录栈。出于安全方面的考虑，开发人员无法得知用户浏览过的 URL。不过，借由用户访问过的页面列表，同样可以在不知道实际 URL 的情况下实现后退和前进。

### 属性

`length` ：保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，`history.length` 等于1。

### 方法

语法：`history.go(n)`

参数：n 表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转（类似于单击浏览器的“后退”按钮），正数表示向前跳转（类似于单击浏览器的“前进”按钮）。

```js
history.go(2)    // 前进两页
history.go(-1)   // 后退一页
history.go(0)     // 刷新
```

**Tips：**如果输入的 *n* 的值超出了 *length* 属性或者小于 0，那么不会进行跳转。

名词方法：

- `history.back()` 后退一页
- `history.forward()` 前进一页

# location对象

保存当前窗口正在打开的URL的对象，它既是 `window` 对象的属性，也是 `document` 对象的属性（`window.location` === `document.location`）

#### 方法

1. 在当前窗口打开，可后退
   - `location.assign(url)`
   - `location.href = url`
   - `location = url`
2. 在当前窗口打开，不会生成历史记录，不可后退即替换当前页面的地址
   - `location.replace(url)`
3. 重新加载页面
   - 普通刷新：优先从浏览器本地缓冲获取资源
     - F5
     - `history.go(0)`
     - `location.reload(/*false*/)`
   - 强制刷新：无论本地是否有缓存，总是强制从服务器获取资源
     - Ctrl + F5
     - `location.reload(true)`

## navigator对象

```html
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
    <meta name="referrer" content="never">
    <title>navigator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        
        ul {
            list-style: none;
        }
    </style>
</head>

<body>
    <!-- navigator 浏览器信息 userAgent 分析出当前浏览器环境 结合正则 onLine 当前设备是否是联网工作 -->
    <script type="text/javascript">
        // 正则是由 /xx/，用于匹配字符串中是否含有对应内容的规则书写
        // /xx/.test(str)  判断字符串中是否含有xx片段，有为true
        // console.log(navigator.userAgent);
        var browserName = navigator.userAgent.toLowerCase();
        var isChrome = /chrome/.test(browserName),
            isFirfox = /firefox/.test(browserName),
            //区分手机端还是PC端
            isMobile = (/android|webos|iphone|ipad|ipod|blackberry|iemobile|opera mini/i.test(browserName));

        console.log(isChrome);
        console.log(isFirfox);
        console.log("移动端", isMobile);

        console.log("联网", navigator.onLine);

        window.ononline = function() {
            console.log("上线了");
        }

        window.onoffline = function() {
            console.log("下线了");
        }
    </script>
</body>

</html>
```

# window的其它方法

- **`onload`： 页面所有内容加载完毕后触发改事件**
- **`onunload`： 用户退出页面触发该事件**
- **`widnow.onresize`：当页面窗口大小发生变化的时候会触发该事件**

- **`resizeBy(w, h)`：根据指定的像素来调整窗口的大小。**

  该方法定义指定窗口的右下角移动的像素，左上角将不会被移动(它停留在其原来的坐标)。有两个参数，第一个参数是必需的，指定窗口宽度增加的像素数。第二个参数可选，指定窗口高度增加的像素数。两个参数可为正数，也可为负数。

- **`resizeTo(w, h)`：用于把窗口大小调整为指定的宽度和高度。**

　　该方法两个参数都是必需的，用来指定设置窗口的宽度和高度，以像素计。

- **`moveBy(xnum, ynum)`：可相对窗口的当前坐标把它移动指定的像素。**

　　该方法有两个参数，第一个参数指定要把窗口右移的像素数，第二个参数指定要把窗口下移的像素数。

- **`moveTo(x, y)`：可把窗口的左上角移动到一个指定的坐标。**

　　该方法有两个参数，第一个参数指定窗口新位置的 x 坐标，第二个参数指定窗口新位置的 y 坐标。

- **`scrollBy(xnum, ynum)`：可把内容滚动指定的像素数。**

　　该方法有两个必需参数，第一个参数指定把文档向右滚动的像素数。第二个参数指定把文档向下滚动的像素数。

- **`scrollTo(x, y)`：可把内容滚动到指定的坐标。**

　　该方法有两个必需参数，第一个指定要在窗口文档显示区左上角显示的文档的 x 坐标。第二个参数指定要在窗口文档显示区左上角显示的文档的 y 坐标。

# 请求动画帧

动画关键是帧率。一般浏览器的刷新频率是 *60HZ*，也就是每秒钟重绘60次。大多数浏览器都会对重绘操作加以限制，不超过显示器的重绘频率，因为即使超过那个频率用户体验也不会提升。因此，最佳的动画频率就是浏览器的刷新频率。
`RequestAnimationFrame` 就是跟随浏览器刷新设置回调的函数。此API会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成。通过递归的方式实现跟随浏览器刷新频率，保持最佳绘制效率，能达到最佳的动画效果。

### requestAnimationFrame

MDN的上的解释：

> `window.requestAnimationFrame()` 方法告诉浏览器您希望执行动画并请求浏览器在下一次重绘之前调用指定的函数来更新动画。该方法使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。

### 语法

```
var requestId = requestAnimationFrame(callback);
```

这会让 `callback` 函数在浏览器每次重绘的最近时间运行。

多个 `requestAnimationFrame` 回调只会有一次几何重新计算和重绘，而不是多次。

### 返回值

一个整数，请求 ID ，是回调列表中唯一的标识。是个非零值，没别的意义。可以传这个值给 `window.cancelAnimationFrame()` 以取消回调函数。

```javascript
// 取消回调的周期执行
window.cancelAnimationFrame(requestId);
```

### 回调函数参数

`callback` 默认得到一个参数 —— 从页面加载开始经过的毫秒数。这个时间戳也可通过调用 `performance.now()` 得到。

> 注：`performance.now()` 返回一个表示从性能测量时刻开始经过的毫秒数

### 例子

```html
<style>
    #box {
        position: absolute;
        top: 50px;
        left: 0px;
        width: 150px;
        height: 150px;
        background-color: #f00;
    }
</style>

<div id="box"></div>

<script type="text/javascript">
    box.onclick = function(){
        // 声明起始值
        var n = 0;
        move();
        function move(){
            // 匀速运动
            n += 5;
            box.style.left = n + "px";
            // 如果未达到终点，重复执行
            if(n < 600){
                requestAnimationFrame(move);
            }
        }
    }
</script>
```

### 时间型动画

之前我们所写动画的运动效果是通过步长控制的，比如我们实现一个运动动画，那么我们就将步长设置为一个定值，如果我们想实现一个缓动动画，那么我们将步长设置为一个逐渐减小的值。如果我们想要实现一些更加复杂的动画效果，比如：先快后慢、先慢后快、先快后慢再快等等，步长的设置计算量会变大，计算过程将变得非常复杂。因此我们效仿 CSS 控制动画的方式，使用时间来控制动画：

```txt
    已运动的路程         动画已过时间
------------------ = -----------------
    运动的总路程         动画的总时长
```

如上所示：欲求已运动路程，只需只要运动的总路程、动画已过时间和动画的总时长即可，总路程和总时长是人为设置，而运动已过时间则可以通过 *当前动画执行的时间戳* - *开始执行动画的时间戳* 来求出。

将上一个例子改为总时长 2s 的动画：

```html
<style>
    #box {
        position: absolute;
        top: 50px;
        left: 0px;
        width: 150px;
        height: 150px;
        background-color: #f00;
    }
</style>

<div id="box"></div>

<script type="text/javascript">
    box.onclick = function(){
        // 声明起始时间戳
        var startTime = performance.now();
        // 声明总时长
        var duration = 2000;
        // 通过 raf 执行函数，函数默认接受一个参数为 当前性能测量时间戳
        requestAnimationFrame(function move(now){
            // 求出已过时间
            var passTime = now - startTime;
            // 求出已过时间的比例
            var bite = passTime / duration;
            // 因为用时间戳减出来的，一不定就严格等于，所以修正一下
            if(bite >= 1){
                bite = 1;
            }
            // 总路程 * 时间比率
            box.style.left = 600 * bite + "px";
            // 如果未到达终点，重复执行
            if(bite < 1){
                requestAnimationFrame(move);
            }
        })
    }
```

#### 通用的时间型动画函数封装

将上述动画过程封装成一个函数，使其不限于使用元素和改变的样式，甚至更加抽象一点，将得到的比率传给用户，让用户根据比率自定义更改哪个元素的什么样式，这里就需要使用一个回调函数。

```js
/**
 * 封装动画函数
 * @param { Number } duration  动画时长
 * @param { Function } callback 每帧的回调函数，带有一个参数：已过时间与总时长的比率
 **/
function animate(duration, callback){
    var startTime = performance.now(); // 设置开始的时间戳
    // 创建每帧之前要执行的动画
    function loop(now){
        var passTime = now - startTime; // 获取当前时间和开始的时间差
        var bite = passTime / duration; // 计算当前已过百分比
        if( bite >= 1 ){  // 消除误差
            bite = 1;
        }
        callback(bite); // 调用回调函数,把比值传递进去
        if(bite < 1){
            requestAnimationFrame(loop) //下一阵调用每帧之前要执行的函数
        }
    }
    requestAnimationFrame(loop) // 下一阵调用每帧之前要执行的函数
}
```

由此函数更改上例的 JS 内容：

```js
box.onclick = function(){
    animate(2000, function(bite){
        box.style.left = 600 * bite + "px";
    })
}
```



#### 更改动画效果

现在我们已经根据动画时长实现了动画，相信各位已经发现这是一个匀速动画，如果想实现一个缓动动画，我们的代码应该如何更改呢？

在我们的动画函数中，已走路程 = 总路程 * 时间比率，总路程是固定的，因此时间比率就是控制动画运动效果的关键属性。时间比率从 0 -> 1，元素样式从起点运动到终点。如果时间比率是匀速增长的，那么元素的运动效果也是匀速的，就如上面的例子所显示那样。

聪明的你或许已经想到了，如果我们通过数学公式更改时间的增长速率，那么元素运动会呈现相同的趋势。

#### 时序函数

现在我们知道通过不同的数学公式去更改时间的增长速度，CSS 中将这种扭曲时间的函数称作——*timing-function*（时序函数），更改原来的动画封装代码，专门写一个函数用来更改动画进程：

```js
function animate(duration, timingFunction, callback){
    var start = performance.now();
    requestAnimationFrame(function animate(now){
        var bite = (now - start) / duration;
        if(bite >= 1){
            bite = 1;
        }
        // 扭曲时间增长比例
        var process = timingFunction(bite);
        // 将扭曲后的时间比例传入回调函数
        callback(process);

        if(bite < 1){
            requestAnimationFrame(animate);
        }
    })
}
```

这里面的 `timingFunction` 就是时序函数，它接受原本的时间进度比，然后返回修改后的时间进度比，如果想要实现匀速运动，函数为：

```js
function linear (bite){
    return bite;
}
```

想要实现先慢后快的函数，我们使用抛物线函数：

```js
// n 为几次方
function quad(bite, n){
    return Math.pow(bite, n);
}
```