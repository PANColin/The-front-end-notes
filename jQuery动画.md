# jQuery动画

**jQuery animate() 方法允许我们创建自定义的动画**

### 语法

```
$(selector).animate(stylesObj,[duration],[timing-function],[callback])
```

### 参数

- *stylesObj*：要执行动画的 CSS 属性与它设定的终点值组成的对象
- *duration*：执行动画时长，值为三种预定速度之一的字符串(“slow”、”normal”、”fast”)或表示动画时长的毫秒数值（可选）
- *timing-function*：动画运动速率函数，只有两个值： *swing* 和 *linear*（可选）
- *callback*：动画执行完后立即执行的回调函数（可选）

**Tips：**动画设定样式终点值时也可以不带单位。

### 动画插件 

常用的jQuery插件网站

1. https://www.jq22.com/
2. http://www.htmleaf.com/

jQuery 不能给元素颜色样式添加动画，动画的速率曲线也只有两种，如果我们想要更改元素的颜色，或者添加更多样的运动轨迹效果，需要借助插件实现。

常见的两个插件为：*jquery.color.js* 和 *jquery.easing.js*。前者为 jQ 添加颜色动画，后者扩展了时序函数。

**注意：**插件的使用依赖于 jQuery，因此引入插件要在引入 jQuery 文件之后。

### 动画队列

jQuery 默认提供针对动画的队列功能。即如果我们在一个函数内给元素添加了多个 `animate()` 调用，jQuery 会创建包含这些方法调用的“内部”队列。然后逐一运行这些 *animate*调用。

我们将前面的动画拆分成三个：

```js
$box.click(function (){
    $(this).animate({
        top: "150px",
        left: 150,
        backgroundColor: "#00f"
    }, 2000, "easeInOutBack", function(){
        console.log("第一阶段完结");
    })

    $(this).animate({
        width: 500,
        height: 300
    }, 3000)

    $(this).animate({
        opacity: .2
    }, 3000)
})
```

拆分之前动画过程是所有样式一起运动，直至终点。拆分之后动画过程会变成：元素先改变上左边距以及颜色 -> 然后改变宽高 -> 最后改变透明度

更改三个动画的书写顺序，动画执行顺序也会改变，但都是从上往下依次执行。

### 动画延时

jQuery 使用 `$ele.delay(time)` 来给一段动画添加延时效果。例如：

```js
$box.click(function (){
    $(this).delay(2000).animate({
        opacity: .2
    }, 3000)
})
```

执行代码会发现动画是 2s 后开始执行的。

### 停止动画

#### stop

**描述：**此方法用于在动画完成之前停止动画或效果。

**语法：**`$ele.stop(stopAll, goToEnd)`

**参数：**

- *stopAll*：是否全部停止动画(清除队列中所有动画)，默认 *false*。即仅停止活动的动画，允许任何排入队列的动画向后执行。
- *goToEnd*：是否立即完成当前动画，即将当前动画的样式直接设置为终点值，默认 *false*。

#### finish

此方法会立即停止动画队列中所有动画，并将元素样式设置为所有动画的终点值叠加后的状态，如果多个动画改变了同一个属性，以最后一个动画的结束样式为准。

**语法：** `$ele.finish()`

### 快捷动画

常见动画操作如下：

| 动画名      | 说明                     |
| :---------- | :----------------------- |
| show        | 让元素出现               |
| hide        | 隐藏元素                 |
| toggle      | 切换元素出现与隐藏       |
| slideDown   | 元素滑入，类似放下卷门帘 |
| slideUp     | 元素滑出，类似上卷门帘   |
| slideToggle | 切换元素的滑入滑出       |
| fadeIn      | 元素渐现                 |
| fadeOut     | 元素渐隐                 |
| fadeToggle  | 切换元素的淡入淡出效果   |

上述方法均接受三个**可选**参数：*duration*、*timing-function*、*fn*，三个参数与 *animate* 动画的参数效果一致，不再赘述。

**补充：**还有一个 `$ele.fadeTo(时长, 透明度)` 的方法可以快捷的给元素添加透明度动画。

**Tips：**让元素出现和隐藏的动画默认是直接改变元素的 *display* 属性的，即闪现动画。添加上时间才能看到动画过程。

### 尺寸相关

我们可以使用 `$ele.width()` 和 `$ele.height()` 方法来快速读取和设置元素的宽度和高度，获取的值是不带单位的 *number* 类型。

### 位置相关

#### position

此方法用于获取元素距离最近的具有定位的父元素的内容区的位置。效果等同于获取元素的 `offsetLeft` 和 `offsetTop`。此属性只能获取，不能设置。

#### offset

此方法可用于获取或设置元素相对于文档的位置。更改位置是通过调节元素的 *top* 属性和 *left* 属性达成的，为此元素需要有定位属性。如果元素没有定位属性，那么此操作会为元素添加 *relative* 定位属性。

#### scrollTop/scrollLeft

用于获取匹配元素相对滚动条上左位置的偏移。此方法对可见和隐藏元素均有效。

这两个属性可以添加在 *animate* 动画中，实现动画效果。

### 多库共存

此处多库共存指的是：jQuery 占用了*$* 和 *jQuery* 这两个变量。当在同一个页面中引用了其他插件库中也用到了 *$*。那么，为了保证每个库都能正常使用，需要使用 `$.noConflict()` 方法让 jQuery 释放对 *$* 的控制权，让其他库能够使用 *$* 符号，达到多库共存的目的。

#  JS中的动态类数组和静态类数组

##### 

DOM操作：自己操作自己 是剪切

```html
//HTML结构

<ul id="ul">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>



复制代码
```

#### 一、动态类数组

- document.getElementById()
- document.getElementsByTagName()
- document.getElementsByClassName()
- document.getElementsByName()
- xx.children

```js
//使用动态类数组操作DOM 循环数组时，会造成数组塌陷问题
let lis = document.getElementsByTagName('li');

for (let i = 0; i < lis.length; i++) {
    //会造成数组塌陷  原始数组[1,2,3,4,5]
    // 第一次：把数组第一项加入容器末尾 [2,3,4,5,1]
    // 第二次：把数组第二项加入容器末尾 [2,4,5,1,3]
    // 第三次：把数组第三项加入容器末尾 [2,4,1,3,5]
    // 第四次：把数组第四项加入容器末尾 [2,4,1,5,3]
   ul.appendChild(lis[i]); //24153 
    // ul.appendChild(lis[0]); //12345 

    //此时每次数组循环都把数组第一项（即索引为0）加入到容器末尾，即可按顺序排列，但数组塌陷问题仍然存在
};
```

#### 二、静态类数组

- document.querySelectorAll()
- jQuery获取的是静态节点数组

```js
 //使用静态类数组，数组不会变，所以尽量使用querySelectorAll获取元素

let lis = document.querySelectorAll('li');

for (let i = 0; i < lis.length; i++) {
    ul.appendChild(lis[i]); //12345
};
```

