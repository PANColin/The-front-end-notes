# JS进阶

## offsetWidth 和 offsetHeight

`element.offsetWidth/Height` 返回元素占据页面的实际大小。

返回值是只读属性且始终是整数。

一般而言对于块状元素：

```js
element.offsetWidth/Height = 元素自身的宽/高(width/height) + padding + border
```

对于行内元素和行内块元素

```js
element.offsetWidth/Height = 元素内容width、height
```

## offsetParent

`element.offsetParent` 返回距离该元素最近的带有定位的父级元素

如果当前元素的父级元素没有进行 CSS 定位（`absolute`、`relative`） 此元素的 `offsetParent` 为 `body`

如果当前元素是 `fixed` 定位，那么返回值为 *null*

## offsetLeft 和 offsetTop

`element.offsetTop/Left` 返回元素与它的**offsetParent**边框的上左边距，如果元素的**offsetParent**是**body**，那么**body**的边框宽度也会计算在内。

返回值是只读属性且始终是整数。

## 获取事件对象

#### 方式一

DOM事件处理函数中可以使用 `event` 来获取事件对象，例如：

```js
document.body.onclick = function(){
    console.log(event); // 鼠标事件对象
}
<span onclick="console.log(event)">hello world</span>
```

上述两种方式都可以在控制台输出事件对象，里面包含了表示事件相关内容的键值对。

#### 方式二

在事件处理函数中使用浏览器赋予的形参

```js
document.body.onclick = function(e){
    // 如果需要兼容IE低版本
    e = e || window.event;
    console.log(e); // 鼠标事件对象
}
```

需要注意的是，行内绑定的事件处理函数并没有这个形参，因为行内绑定的写法是函数调用，JS中声明函数接受的值为调用传入的值，而非浏览器传递的值。

```html
<span onclick="clickHandle()">这里是函数调用，相当于自己传值</span>
<span onclick="clickHandle(123)">传入123</span>
<script>
    function clickHandle(e){
        console.log(e); // undefined 123
    }
</script>
```

## 常见的事件对象属性

### event.type

事件类型，返回触发的事件名称。例如：

```js
document.body.onclick = function(){
    console.log(event.type); // click
}
```



### 事件相关元素

### event.target

触发事件的元素，在老版本的IE浏览器中请使用 `event.srcElement` 来替代它。

### event.currentTarget

绑定事件的元素，IE >=9 支持。

**tips：**触发事件的元素不一定就是绑定事件的元素，因为存在事件冒泡，点击某个元素的子元素，那么父元素绑定的事件中，触发事件的元素为子元素，绑定事件的元素为父元素。

例如：

```html
<p>
    <span>hello</span>
</p>
<script>
    var pEle = document.querySelector("p");
    pEle.onclick = function(){
        // 点击文字，输出如下内容
        console.log(event.target); // span
        console.log(event.currentTarget); // p
    }
</script>
```

*小知识：*在非行内绑定的事件中，`event.currentTarget` 与事件处理函数中的 `this` 相同，均为绑定事件的元素。

`contextmenu` 事件是浏览器右键菜单的事件，我们可以通过这个事件来控制右键菜单的显示与否或者重写右键菜单内容。

例如：

```js
// 禁止浏览器右键
document.oncontextmenu = function() {
    event.preventDefault(); // 阻止默认事件
    event.returnValue = false;
}
```

## 鼠标的位置信息

鼠标事件几乎都有

| 属性名          | 说明                                                       |
| :-------------- | :--------------------------------------------------------- |
| pageX/pageY     | 获取事件触发时鼠标相对于页面的位置                         |
| clientX/clientY | 获取事件触发时鼠标相对于可视区域的位置                     |
| offsetX/offsetY | 获取事件触发时鼠标相对于**触发**事件的元素**内容区**的位置 |
| screenX/screenY | 获取事件触发时鼠标相对于屏幕的位置，不常用                 |
| x/y             | clientX/clientY的别名                                      |

## 键盘信息

相关事件：`event.keydown`、`event.keyup`

| 属性名   | 说明                          |
| :------- | :---------------------------- |
| key      | 按键表示的值内容              |
| altKey   | 触发事件时 alt 键是否被按下   |
| ctrlKey  | 触发事件时 ctrl 键是否被按下  |
| shiftKey | 触发事件时 shift 键是否被按下 |

## 阻止事件冒泡

使用 `event.stopPropagation()` 可以阻止事件冒泡。旧版本 IE 浏览器使用：`event.cancelBubble = true` 方法阻止事件冒泡。

兼容写法：

```
event.stopPropagation ? event.stopPropagation() : event.cancelBubble = true;
```



## 阻止默认事件

使用 `event.preventDefault()` 可以阻止默认事件。旧版本 IE 浏览器使用：`event.returnValue = false` 阻止默认事件。

兼容写法：

```
event.preventDefault ? event.preventDefault() : event.returnValue = false;
```

对于一般的默认事件，也可以通过设置事件处理函数 `return false;` 来达到阻止默认事件的目的。

#### Element.tagName

##### 概述

返回当前元素的标签名

##### 语法

```
elementName = element.tagName
```

- `elementName` 是一个字符串,包含了element元素的标签名.

##### 备注

在XML (或者其他基于XML的语言,比如XHTML,xul)文档中,`tagName的值会`保留原始的大小写. 在HTML文档中, `tagName`会返回其大写形式. 对于元素节点来说,`tagName属性`的值和[nodeName](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeName)属性的值是相同的.

## 事件委托

> 又名事件代理，利用事件的冒泡机制，以及事件对象中可以准确获知触发事件元素的属性，将多个使用相同事件进行类似操作的子元素的事件处理函数委托给父元素绑定的现象。

### 

**意义：**

1. 减少重复性绑定的事件处理函数，减少内存占用
2. 新添加的子元素也能拥有事件

## Element.closest()

`**Element.closest()**` 方法用来获取：匹配特定选择器且离当前元素最近的祖先元素（也可以是当前元素本身）。如果匹配不到，则返回 `null`。

#### 语法

```
var closestElement = targetElement.closest(selectors);
```

#### 参数

- *selectors* 是指定的选择器，比如 `"p:hover, .toto + q"`。

#### 返回值

- *elt* 是查询到的祖先元素，也可能是 `null`。

#### 异常

- 如果传入的选择器不合法，则抛出 [`SyntaxError`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMException#exception-syntaxerror) 异常。

