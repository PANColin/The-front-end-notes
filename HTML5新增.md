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