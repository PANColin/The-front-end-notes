# HTML5新增

# JS操作媒体元素

之前已经提到过 *HTML5* 新增的 audio 标签 和 video 标签的用法，忘记的同学[请看这里](http://doc.bufanui.com/docs/html-css/html-css-1clqbc0o8liuo)，接下来我将给你们演示常见的媒体元素JS操作。

### 媒体元素属性

**下面介绍的属性 video 标签和 audio 标签皆适用。**

#### 属性

- `currentTime` 用于获取或者设置音视频播放的当前进度。单位：秒，*number*类型，可读写。
- `duration` 用于获取音视频的总时长。单位：秒，*number*类型，只读。
- `paused` 用于获取音视频当前是否是暂停状态，*boolean* 类型，只读
- `volume` 用于获取或者设置音视频的声音大小，*number* 类型，值范围：[0, 1]，可读写
- `playbackRate` 用于设置媒体文件播放时的速率，当设置播放速率低于或高于浏览器内核规定的可用范围（比如，Gecko约定范围是0.25～5.0）时，播放过程将静音。

#### 方法

- `load()` 重新加载音频/视频元素
- `play()` 播放音视频
- `pause()` 暂停音视频

#### 事件

- `canplay`：当媒体加载完成，可以播放时触发
- `timeupdate`：当 `currentTime` 更新时会触发此事件
- `ratechange`：播放速率更改时触发此事件
- `volumechange`：音量发生更改时触发此事件
- `ended`：播放完时触发

更多内容可以自行查阅相关API。

#### 实现播放/暂停功能

```html
<audio src="https://mp3.9ku.com/hot/2004/10-13/61171.mp3" class="ad"></audio>

<button onclick="toggle()">点击播放/暂停音频</button>

<script>
    var ad = document.querySelector(".ad");
    function toggle(){
        // 判断当前音频是否是暂停状态
        if(ad.paused){
            // 是，播放
            ad.play();
        }else {
            // 不是，暂停
            ad.pause();
        }
    }
</script>
```



#### 实现音量拖拽进度条控制

```html
<audio src="https://mp3.9ku.com/hot/2004/10-13/61171.mp3" class="ad"></audio>
<button onclick="toggle()">点击播放/暂停音频</button>
<br>
<input class="ipt" type="range" min="0" max="100" value="0">

<script>
    var ad = document.querySelector(".ad"),
        ipt = document.querySelector(".ipt");

    ipt.oninput = function (){
        ad.volume = this.value / 100;
        console.log("设置的音量大小为：", ad.volume);
    }

    function toggle(){
        // 判断当前音频是否是暂停状态
        if(ad.paused){
            // 是，播放
            ad.play();
        }else {
            // 不是，暂停
            ad.pause();
        }
    }
</script>
```



#### 实现播放进度监听

```html
<audio src="https://mp3.9ku.com/hot/2004/10-13/61171.mp3" class="ad" controls></audio>
<p class="time">
    <span>0:00</span> / <span>0:00</span>
</p>

<script>
    var ad = document.querySelector(".ad"),
        time = document.querySelector(".time");
    ad.load();

    console.log(ad.duration); // 可能为NaN 无法获取，因为音频加载需要时间

    ad.oncanplay = function(){
        // 音频加载完成，可以是用 ad.duration
        console.log("加载完成", ad.duration);
        // 获取到时长，赋值时间
        time.children[1].innerText = dealTime(ad.duration);
    }

    // 监听播放进度事件
    ad.ontimeupdate = function (){
        // 将当前进度的 秒 -> 分:秒的结构赋值
        time.children[0].innerText = dealTime(this.currentTime);
    }

    // 将 秒值 ->  分:秒
    function dealTime(num){
        var minute = Math.floor(num / 60);
        var second = Math.floor(num % 60);
        return minute + ":" + (second < 10 ? "0" + second : second);
    }
</script>
```



#### 实现播放进度条拖拽控制

```html
<audio src="https://mp3.9ku.com/hot/2004/10-13/61171.mp3" class="ad"></audio>
<button onclick="toggle()">点击播放/暂停音频</button>
<br>
<input class="ipt" type="range" min="0" max="100" value="0">

<script>
    var ad = document.querySelector(".ad"),
        ipt = document.querySelector(".ipt");

    ipt.oninput = function (){
        ad.currentTime = this.value / 100 * ad.duration;
    }


    // 播放过程中让进度条跟着动
    ad.ontimeupdate = function (){
        // input的值为 当前播放进度/总时长 的百分比值
        ipt.value = Math.round(ad.currentTime/ad.duration * 100);
    }

    function toggle(){
        // 判断当前音频是否是暂停状态
        if(ad.paused){
            // 是，播放
            ad.play();
        }else {
            // 不是，暂停
            ad.pause();
        }
    }
</script>
```



实现播放速度控制：

```html
<audio src="https://mp3.9ku.com/hot/2004/10-13/61171.mp3" class="ad"></audio>
<button onclick="toggle()">点击播放/暂停音频</button>
<br>
<select class="speed">
    <option value="0.5">0.5</option>
    <option value="0.75">0.75</option>
    <option value="1" selected>1.0</option>
    <option value="1.25">1.25</option>
    <option value="1.5">1.5</option>
    <option value="2">2.0</option>
</select>

<script>
    var ad = document.querySelector(".ad"),
        speed = document.querySelector(".speed");

    // 更改播放速度
    speed.onchange = function (){
        console.log(this.value);
        ad.playbackRate = this.value;
    }
    function toggle(){
        // 判断当前音频是否是暂停状态
        if(ad.paused){
            // 是，播放
            ad.play();
        }else {
            // 不是，暂停
            ad.pause();
        }
    }
</script>
```

# 新尺寸边界属性

`Element.getBoundingClientRect()` 方法返回元素的大小及其相对于视口的位置。该方法返回的对象属性包括以下属性：

- **top、left、right、bottom** 是指元素四周与页面的上左边距。
- **width、height** 用于获取元素占据页面的尺寸，相当于元素的 *offsetWidth* 和 *offsetHeight* 属性。
- **x、y** 是 *left* 和 *top* 属性的简写，存在浏览器兼容问题，目前不推荐使用这两个属性。

```html
<style>
    .box{
        position: absolute;
        top: 50px;
        left: 50px;
        width: 300px;
        height: 200px;
        padding: 50px;
        border: 20px solid #000;
        background-color: #f00;
    }

    .inner {
        position: relative;
        width: 200px;
        height: 100px;
        margin: 20px;
        padding: 30px;
        background-color: #0f0;
        border: 10px solid #9000ff;
    }

    .insert {
        width: 100px;
        height: 50px;
        margin: 50px;
        background-color: #00f;
    }
</style>
<body>
    <div class="box">
        <div class="inner">
            <div class="insert"></div>
        </div>
    </div>
    <script>
        console.log("box", getDistance($(".box")));
        console.log("inner", getDistance($(".inner")));
        console.log("insert", getDistance($(".insert")));


        function getDistance(ele){
            var obj = ele.getBoundingClientRect();
            console.log(obj);
            return {
                top: obj.top,
                left: obj.left
            }
        }

        function $(select){
            return document.querySelector(select);
        }
    </script>
</body>
```

# 元素可编辑模式

我们通过给元素设置 **contentEditable** 属性可以开启元素的编辑模式，让用户更改元素的文本内容。

```html
<p contenteditable="true">竹外桃花三两只，春江水暖鸭先知</p>
```

如果想让页面内所有元素的文本内容都可以被编辑，可以直接使用文档编辑模式：

```js
document.designMode = "on";
```

该属性控制整个文档是否可编辑。有效值为 “on”、“off”。根据规范，该属性默认为 “off” 。Firefox 遵循此标准。早期版本的 Chrome 和 IE默认为 “inherit”。从 Chrome 43 开始，默认值为 “off” ，并且不再支持 “inherit”。在 IE6 到 IE10 中，该值为大写。

# 新的拖拽属性

HTML5新增拖放接口使应用程序能够在浏览器中使用拖放功能。例如，用户可使用鼠标选择可拖拽元素，将元素拖拽到可放置位置，并释放鼠标按钮以放置这些元素。拖拽操作期间，会有一个可拖拽元素的半透明快照跟随着鼠标指针。

通过此接口我们可以自定义什么元素是可拖拽的、可拖拽元素产生的反馈类型，以及可放置的元素。

我们可以通过给元素设置 `draggable="true"` 属性来让元素变成可拖拽元素，拖拽元素拥有以下常用监听事件：

| 事件名    | 说明                                             |
| :-------- | :----------------------------------------------- |
| dragstart | 用户开始拖拽时触发，鼠标按下并开始移动           |
| drag      | 整个拖拽过程都会调用，鼠标按下到抬起期间持续触发 |
| dragleave | 当鼠标离开目标元素时触发                         |
| dragend   | 当拖拽结束时触发，此时鼠标抬起                   |

任何与拖拽元素交互的元素都可以是目标元素（包括拖拽元素自身）。目标元素可以通过以下事件监听与拖拽元素的交互：

| 事件名    | 说明                                       |
| :-------- | :----------------------------------------- |
| dragenter | 鼠标携带拖拽元素拖拽元素进入当前元素时触发 |
| dragover  | 鼠标携带拖拽元素在当前元素上停留时触发     |
| dragleave | 鼠标携带拖拽元素离开当前元素时触发         |
| drop      | 在当前元素上松开鼠标，释放拖拽元素时触发   |

细心的你可以发现 *dragleave* 事件出现了两次，这个事件在鼠标携带拖拽元素离开任意元素边界时都会触发。

**Tips：**因为浏览器是默认禁止元素堆叠的，因此你可以在实验拖拽属性时发现鼠标在拖拽过程中始终是一个黑色的圈，当鼠标在某个元素上是一个黑色的圈时，停留元素是无法触发 *drop* 事件的。如果想要触发该事件，我们需要在停留元素的 *dragover* 事件中阻止默认事件。

示例：

```html
<style>
    .box{
        width: 150px;
        height: 150px;
        background-color: #f00;
    }

    .content {
        width: 400px;
        height: 230px;
        border: 3px solid #f00;
    }
</style>
<body>
    <div class="content"></div>

    <div class="box" draggable="true"></div>
    <script>
        var box = document.querySelector(".box"),
            content = document.querySelector(".content");

        box.ondragstart = function (){
            console.log("拖拽开始");
        }

        // box.ondrag = function (){
        //     console.log("拖拽进行中");
        // }

        box.ondragleave = function (){
            console.log("鼠标离开拖拽元素");
        }

        box.ondragend = function (){
            console.log("结束拖拽");
        }


        // 目标元素 -> 与拖拽元素有交互的元素
        content.ondragenter = function (){
            console.log("鼠标进入目标元素");
            content.style.borderColor = "#00f";

        }

        content.ondragover = function (){
            console.log("鼠标在目标元素上停留");
            // 浏览器是默认禁止元素堆叠的，阻止默认既可让drop事件执行
            event.preventDefault();
        }

        content.ondrop = function (){
            content.style.borderColor = "";
            console.log("鼠标在目标元素上松开，释放拖拽元素");
        }

        content.ondragleave = function (){
            content.style.borderColor = "";
            console.log("鼠标离开目标元素");
        }
    </script>
</body>
```

### 拖拽传值

我们可以通过 **dataTransfer** 拖拽内容对象来实现在拖放操作过程中的数据交换。此对象是事件对象的一个属性，用于从被拖动元素向放置目标传递字符串格式的数据，使用方法：

- `setData(type, value)`：向 *dataTransfer* 对象中存入数据。接收两个参数，第一个表示要存入数据种类的字符串，第二个参数为要存入的数据。
- `getData(type)`：从dataTransfer对象中读取数据。参数为在setData中指定的数据种类。
- `clearData()`：清除 *dataTransfer* 对象中存放的数据。参数可选，为数据种类。若参数为空，则清空所有种类的数据。

```html
<style>
    .select {
        width: 50px;
        height: 50px;
        color: #fff;
        background-color: #00f;
    }

    .box {
        width: 200px;
        height: 100px;
        border: 2px solid #f00;
    }
</style>
<body>

    <div class="select" draggable="true">天王盖地虎</div>
    <div class="box">将蓝块拖拽到这个框后释放</div>

    <script>
        var select = document.querySelector(".select"),
            box = document.querySelector(".box");


        select.ondragstart = function (){
            event.dataTransfer.setData("text", "天王盖地虎");
        }
        box.ondragover = function (){
            return false;   
        }
        box.ondrop = function (){
            box.innerText = event.dataTransfer.getData("text");
        }
    </script>
</body>
```

*dataTaransfer* 还可以用于获取拖拽的文件，这个留在上传文件再提。

# 全屏属性

### 开启全屏

```js
if (E.requestFullscreen) {
    E.requestFullscreen();
}else if (E.webkitRequestFullscreen) {
    E.webkitRequestFullscreen();
}else if (E.mozRequestFullscreen) {
    E.mozRequestFullscreen();
}else{
    alert("sorry,无法全屏");
}
```

E 为要开启全屏的元素。

### 退出全屏必须使用 document 的API

```js
if (document.exitFullscreen) {
    document.exitFullscreen();
}else if (document.webkitCancelFullScreen) {
    document.webkitCancelFullScreen();
}else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen();
}
```

### 判断是否是全屏

```js
function ifFullscreen(){
    return document.fullscreen || document.webkitIsFullScreen || document.mozFullScreen || false;
}
```

**Tips：全屏状态下的元素不能更改宽高位置样式。**

给全屏状态下的元素的子类添加样式可用 *:fullscreen” 伪类：

```css
/* :fullscreen 实验中的功能,使用请加浏览器前缀 慎用 */
.全屏状态下的元素:fullscreen 子类{
    /* CSS code */
}
```

示例：

```html
<style>
    .box{
        position: absolute;
        top: 50px;
        left: 50px;
        width: 150px;
        height: 150px;
        background-color: #f00;
    }

    /* 全屏状态下宽高、位置属性不可更改 */
    .box:fullscreen {
        background-color: #ff9000;
    }
    .box:-webkit-full-screen .inner{
        width: 100px;
        height: 100px;
        background-color: #00f;
    }
</style>
<body>
    <div class="box">
        <div class="inner">我是inner</div>
    </div>
    <script type="text/javascript">
        var box = document.querySelector(".box");

        box.onclick = function (){
            if(ifFullscreen()){
                // 关闭全屏
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }else if (document.webkitCancelFullScreen) {
                    document.webkitCancelFullScreen();
                }else if (document.mozCancelFullScreen) {
                    document.mozCancelFullScreen();
                }
            }else {
                // 开启全屏
                if (box.requestFullscreen) {
                    box.requestFullscreen();
                }else if (box.webkitRequestFullscreen) {
                    box.webkitRequestFullscreen();
                }else if (box.mozRequestFullscreen) {
                    box.mozRequestFullscreen();
                }else{
                    alert("sorry,无法全屏");
                }
            }
        }

        // 返回当前是否是全屏的 boolean
        function ifFullscreen(){
            return document.fullscreen || document.webkitIsFullScreen || document.mozFullScreen || false;
        }
    </script>
</body>
```