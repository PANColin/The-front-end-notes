## DOM基础

### 获取节点对象的方式

有 6 种主要的方法，可以在 DOM 中搜素节点：

| Method                   | Searches by... |
| ------------------------ | -------------- |
| `querySelector`          | CSS-selector   |
| `querySelectorAll`       | CSS-selector   |
| `getElementById`         | `id`           |
| `getElementsByName`      | `name`         |
| `getElementsByTagName`   | tag or `'*'`   |
| `getElementsByClassName` | class          |

目前为止，最常用的是 `querySelector` 和 `querySelectorAll`，但是 `getElementBy*` 可能会偶尔有用。

**`JS`是一门以事件驱动为核心的语言。**

## 事件

**事件** 是某事发生的信号。所有的 DOM 节点都生成这样的信号（但事件不仅限于 DOM）。
**常见事件类型**: [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/Events)
**鼠标事件：**

- `onclick` —— 当鼠标点击一个元素时。
- `onmouseover` / `onmouseout` —— 当鼠标指针移入/离开一个元素时。
- `onmousedown` / `onmouseup` —— 当在元素上按下/释放鼠标按钮时。
- `onmousemove` —— 当鼠标移动时。

**表单（form）元素事件**：

- `onchange` —— 当访问者改变域的内容时。
- `onfocus` —— 当访问者聚焦于一个元素时，例如聚焦于一个 ``。

**键盘事件**：

- `onkeydown` 和 `onkeyup` —— 当访问者按下然后松开按键时。

**Document 事件**：

- `onload` —— 当 HTML 页面加载完成时。

还有很多其他事件。我们将在下一章中详细介绍具体事件。

## 事件处理程器

为了对事件作出响应，我们可以分配一个 **处理程序（handler）**—— **一个在事件发生时运行的函数。**

处理程序是在发生用户行为（action）时运行 JavaScript 代码的一种方式。

**有几种绑定事件处理程序的方式：**

### 行内绑定

```html
<p onclick="alert('hi')">
    hello
</p>
<p onclick="foo()">
    hello
</p>
<script>
    // 标签中不适合编写大量代码，因此我们最好创建一个 JavaScript 函数，然后在事件中调用这个函数。
    function foo(){
        console.log('hi')
    }
</script>
```

在鼠标点击时，`onclick` 中的代码就会运行。

### 属性绑定

**`ele.onclick`：**

```html
<p>
    click me!
</p>
<script>
    var pEl = document.getElementsByTagName('p')[0];
    pEl.onclick = function() {
        alert('Thank you');
    };
</script>
```

**因为每一个`dom`只有一个 `onclick` 属性，所以我们无法分配更多事件处理程序。也就是无法同时绑定多个事件**

**要移除一个处理程序 —— 赋值 `elem.onclick = null`。**

## 类名操作

`elem.classList`可以实现类名的基本操作

`elem.classList` 是一个特殊的对象，它具有 `add/remove/toggle` 单个类的方法。

例如：

```html
<body class="main page">
  <script>
    // 添加一个 class
    document.body.classList.add('article');

  </script>
</body>
```

`classList` 的方法：

- `elem.classList.add/remove(class)` — 添加/移除类。
- `elem.classList.toggle(class)` — 如果类不存在就添加类，存在就移除它。
- `elem.classList.contains(class)` — 检查给定类，返回 `true/false`。

## 元素样式

`elem.style` 属性是一个对象，它对应于 `"style"` 特性（attribute）中所写的内容。

`elem.style.width="100px"` 的效果等价于我们在 `style` 特性中有一个 `width:100px` 字符串。

**对于多词属性，使用驼峰式 ：**

```js
background-color  => elem.style.backgroundColor
z-index           => elem.style.zIndex
border-left-width => elem.style.borderLeftWidth
```

**注意单位**

例如，我们不应该将 `elem.style.top` 设置为 `10`，而应将其设置为 `10px`。

**`elem.style.属性名` 只能获取行内样式**

````html
<body>
  <script>
    // 无效！
    document.body.style.margin = 20;
    alert(document.body.style.margin); // ''（空字符串，赋值被忽略了）

    // 现在添加了 CSS 单位（px）— 生效了
    document.body.style.margin = '20px';
    alert(document.body.style.margin); // 20px


  </script>
</body>
```

## 计算样式：getComputedStyle

**要读取所有 `CSS` 应用在元素上的最终值: **

```js
getComputedStyle(element, [pseudo])
```

`element`: 需要被读取样式值的元素。

`pseudo`: 伪元素（如果需要），例如 `::before`。空字符串或无参数则意味着元素本身。

例如：

```html
<head>
  <style> body { color: red; margin: 5px } </style>
</head>
<body>

  <script>
    var body = document.body;
    console.log(body.style.color); //获取不到 ,因为没有行内样式

    var computedStyle = getComputedStyle(document.body);
    // 现在我们可以读取它的 margin 和 color 了
    alert( computedStyle.marginTop ); // 5px
    alert( computedStyle.color ); // rgb(255, 0, 0)
  </script>

</body>
```

# DOM关系

**DOM 让我们可以对元素和它们中的内容做任何事，但是首先我们需要获取到对应的 DOM 对象。**

对 DOM 的所有操作都是以 `document` 对象开始。它是 DOM 的主“入口点”。从它我们可以访问任何节点。

![null](https://zh.javascript.info/article/dom-navigation/dom-links.svg)

## documentElement 和 body

最顶层的树节点可以直接作为 `document` 的属性来使用：

- `` = `document.documentElement`: 最顶层的 document 节点是 `document.documentElement`。这是对应 `` 标签。
- `` = `document.body`: 另一个被广泛使用的 DOM 节点是 `` 元素 — `document.body`。
- `` = `document.head`: `` 标签可以通过 `document.head` 访问。

## 父子兄关系

![null](http://doc.bufanui.com/uploads/js/images/m_fb19e6c01bd1dd1e12e77a44b9996ea0_r.png)

**节点的访问关系，是以**属性**的方式存在的。**

#### 获取兄弟节点

**1、下一个节点 | 下一个元素节点**：

> Sibling 的中文是**兄弟**。

（1）`nextSibling`：

- 火狐、谷歌、IE9+版本：都指的是下一个节点（包括标签、空文档和换行节点）。
- IE678 版本：指下一个元素节点（标签）。

（2）`nextElementSibling`：

- 火狐、谷歌、IE9+版本：都指的是下一个元素节点（标签）。

**总结**：为了获取下一个**元素节点**，我们可以这样做：在 IE678 中用 `nextSibling`，在火狐谷歌 IE9+以后用 `nextElementSibling`，于是，综合这两个属性，可以这样写：

```javascript
下一个兄弟节点 = 节点.nextElementSibling || 节点.nextSibling;
```

#### nodeType 属性

- `nodeType` == 1 表示的是元素节点（标签） 。记住：元素就是标签。
- `nodeType` == 2 表示是属性节点(该类型已弃用)。
- `nodeType` == 3 是文本节点。
- `nodeType` == 8 注释节点

## 属性操作

```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div class="box">
        <ul class="nav">
            <li><img src="" alt="">1</li>
            <li><img src="" alt="">2</li>
            <li><img src="" alt="">3</li>
            <li><img src="" alt="">4</li>
            <li><img src="" alt="">5</li>
        </ul>
        <button onclick="appendCe()">向ul里追加标签</button>
        <button onclick="insertCe()">向ul里插入标签</button>
        <button onclick="deleteCe()">把ul里的img标签删除</button>
        <button onclick="deleteSelf()">把ul标签删除</button>
        <button onclick="cloneEl()">克隆ul标签</button>
    </div>
    <script>
        // 封装一个创建节点对象的方法
        function createEl(el) {
            return document.createElement(el);
        }
        // 使用创建的方法创建对象
        console.log(createEl("li"));

        // 插入节点对象
        // 封装一个获取节点对象的方法和
        function getElObj(cssStyle) {
            return document.querySelector(cssStyle);
        }

        function getElObjs(cssStyle) {
            return document.querySelectorAll(cssStyle);
        }
        //通过方法获取ul节点对象
        var ulEl = getElObj(".nav");
        console.log(ulEl);
        // 向ul里追加一个p标签
        function appendCe() {
            ulEl.appendChild(createEl("li"));
        }
        // 向ul里插入一个p标签
        // 获取参考节点对象
        var refPEl = getElObj(".nav p");

        function insertCe() {
            ulEl.insertBefore(createEl("input"), refPEl);
        }
        // 删除所有ul的img标签
        // 获取Ul所有的img节点对象
        var imgObjs = getElObjs(".nav li img");
        console.log(ulEl.children);

        function deleteCe() {
            // 遍历
            for (var i = 0; i < imgObjs.length; i++) {
                // ulEl.removeChild(imgObjs[i]);
                // ulEl.firstElementChild.removeChild(imgObjs[i]);
                ulEl.children[i].removeChild(imgObjs[i]);
            }
        }
        console.log(ulEl.parentNode);
        // 删除ul标签
        function deleteSelf() {
            ulEl.parentNode.removeChild(ulEl);
        }
        // 复制节点对象
        function cloneEl() {
            // 可以传递参数false浅克隆 （默认值） true 深克隆 此时还没有向浏览器渲染，需要使用插入方法
            ulEl.cloneNode();
            // 向div标签里在追加复制的节点对象
            // 获取div节点 对象
            var divEl = getElObj(".box");
            // 然后追加复制的节点对象
            divEl.appendChild(ulEl.cloneNode(true));
        }
    </script>
</body>

</html>
```

# 表单操作

**表单元素的值由`value`属性控制**

**选框的状态由`checked`属性控制**

**全选反选:**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .wrap {
            width: 800px;
            height: 400px;
            margin: 0 auto;
        }

        table {
            margin: 20px auto;
        }
    </style>
</head>

<body>
    <div class="wrap">
        <table cellspacing="20">
            <tr>
                <th><input type="checkbox" value="全选" id="all"></th>
                <th>产品</th>
                <th>价钱</th>

            </tr>
            <tr>
                <td><input type="checkbox" class="check"></td>
                <td>iphone</td>
                <td>5000</td>

            </tr>
            <tr>
                <td><input type="checkbox" class="check"></td>
                <td>iwatch</td>
                <td>2000</td>

            </tr>
            <tr>
                <td><input type="checkbox" class="check"></td>
                <td>ipad</td>
                <td>3000</td>

            </tr>
        </table>
        <input type="button" class="inverse" value="反选">
    </div>

    <script>
        var btnAll = document.querySelector('#all');
        var checkArr = document.querySelectorAll('.check');
        var inverse = document.querySelector('.inverse');
        // 全选逻辑
        btnAll.onclick = function () {
            // 获取全选框的 状态
            var status = this.checked;
            // 商品全部选中
            for (var i = 0; i < checkArr.length; i++) {
                checkArr[i].checked = status;
            }
        }

        // 反选逻辑
        inverse.onclick = function () {
            // 如何反选
            // 其实就是原来的状态  取反 !
            for (var i = 0; i < checkArr.length; i++) {
                checkArr[i].checked = !checkArr[i].checked;
            }
            isAll();
        }

        // 全选按钮 如何 联动
        // 核心问题 ,就是判断 是否所有商品 都被选中
        // 下一个问题  什么时候调用 isAll
        function isAll() {
            var status = true;
            for (var i = 0; i < checkArr.length; i++) {
                // 如果遇到一个 checked  是 false  就把 status 改成 false
                if (!checkArr[i].checked) {
                    status = false;
                    break;
                }
            }
            // 修改状态
            btnAll.checked = status;

        }

        // 每个选框 和 按钮 点击时 都要从执行 isAll 
        for (var i = 0; i < checkArr.length; i++) {
            checkArr[i].onclick = function () {
                isAll();
            }
        }




    </script>
</body>

</html>
```

## 节点属性操作

我们可以获取节点的属性值、设置节点的属性值、删除节点的属性。(其实就是对象属性的操作)

我们就统一拿下面这个标签来举例：

```html
<img src="images/1.jpg" class="image-box" title="美女图片" alt="地铁一瞥" id="a1" />
```

下面分别介绍。

**1、获取节点的属性值**

方式 1：

```javascript
元素节点.属性;
元素节点[属性];
```

方式 2：

```javascript
元素节点.getAttribute("属性名称");
```

举例：

```javascript
console.log(myNode.getAttribute("src"));
console.log(myNode.getAttribute("class")); //注意是class，不是className
console.log(myNode.getAttribute("title"));
```

**2、设置修改节点的属性值**

方式 1 举例：（设置节点的属性值）

```javascript
myNode.src = "images/2.jpg"; //修改src的属性值
myNode.className = "image2-box"; //修改class的name
```

方式 2：

```javascript
元素节点.setAttribute(属性名, 新的属性值);
```

方式 2 举例：（设置节点的属性值）

```javascript
myNode.setAttribute("src", "images/3.jpg");
myNode.setAttribute("class", "image3-box");
myNode.setAttribute("id", "你好");
```

**3、删除节点的属性**

格式：

```javascript
元素节点.removeAttribute(属性名);
```

举例：（删除节点的属性）

```javascript
myNode.removeAttribute("class");
myNode.removeAttribute("id");
```

**总结：**

获取节点的属性值和设置节点的属性值，都有两种方式，但这两种方式是有些许区别的。(了解)

- `attribute`：是HTML标签上的某个属性，如id、class、value等以及自定义属性，它的值只能是字符串，关于这个属性一共有三个相关的方法，`setAttribute`、`getAttribute`、`removeAttribute`；
- `property`(属性)：是js获取的DOM对象上的属性值，比如a，你可以将它看作为一个基本的js对象。

- `set/get/removeAttribute()`只能用于操作标签本身就有的属性，`对象.xx`可以随意的添加和修改属性。

## 自定义属性

在 HTML5 中我们可以自定义属性，其格式如下 `data-*=""`，例如:

`data-info="我是自定义属性"`，通过`Node.dataset['info']` 我们便可以获取到自定义的属性值。

`Node.dataset`是以类对象形式存在的

当我们如下格式设置时，则需要以小驼峰格式才能正确获取

```js
data-my-name="mm"`，获取`Node.dataset['myName']
```

## 事件冒泡和捕捉

当一个事件发生在具有父元素的元素上时，现代浏览器运行两个不同的阶段 - **捕获阶段**和**冒泡阶段**。
**在捕获阶段：**
浏览器检查元素的最外层祖先，是否在捕获阶段中注册了一个onclick事件处理程序，如果是，则运行它。
然后，它移动到中单击元素的下一个祖先元素，并执行相同的操作，然后是单击元素再下一个祖先元素，依此类推，直到到达实际点击的元素。
**冒泡阶段:**
浏览器检查实际点击的元素是否在冒泡阶段中注册了一个onclick事件处理程序，如果是，则运行它
然后它移动到下一个直接的祖先元素，并做同样的事情，然后是下一个，等等，直到它到达元素。

```html
<style>
    #a {
        width: 300px;
        height: 300px;
        background-color: #eeeeee;
    }
    
    #b {
        width: 200px;
        height: 200px;
        background-color: #666666;
    }
    
    #c {
        width: 100px;
        height: 100px;
        background-color: green;
    }
</style>

<div id="a">
    最外层的元素
    <div id="b">
        中间的元素
        <div id="c">
            最里面的元素
        </div>
    </div>
</div>

<script>
    // 来源CSDN
    const list = ["a", "b", "c"];

    for (let elementId of list) {
        document.getElementById(elementId).addEventListener(
            "click",
            function() {
                console.log("捕获阶段: ", this.firstChild.textContent.trim());
            },
            true
        );

        document.getElementById(elementId).addEventListener(
            "click",
            function() {
                console.log("冒泡阶段", this.firstChild.textContent.trim());
            },
            false
        );
    }
</script>
```



#### 绑定事件的三种方式

- `el.onclick = function(){}`

- 行内绑定

- `el.addEventListener('click',function(){})`

  - ```html
    <!DOCTYPE html>
      <html lang="en">
      
      <head>
          <meta charset="UTF-8">
          <meta http-equiv="X-UA-Compatible" content="IE=edge">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
          <style>
              #box {
                  width: 200px;
                  height: 200px;
                  background-color: #bfa;
              }
              
              #inner-box {
                  width: 100px;
                  height: 100px;
                  background-color: #fba;
              }
          </style>
      </head>
      
      <body>
          <div id="box">
              <div id="inner-box"></div>
          </div>
          <script>
              // 获取外部的盒子
              var boxEl = document.getElementById("box");
              console.log(boxEl);
              // 获取内部的盒子 
              var innerBoxEl = document.getElementById("inner-box");
              console.log(innerBoxEl);
              // 分别对他们进行绑定点击事件
              // boxEl.onclick = function() {
              //     console.log("box>>>>>");
              // };
              innerBoxEl.onclick = function() {
                  console.log("inner-box>>>>>");
              };
      
              function first() {
                  alert("thanks");
              }
              // 添加监听
              var boxListener = boxEl.addEventListener("click", first, true);
              console.log(boxListener);
              var innerBoxElListener = innerBoxEl.addEventListener("click", function() {
                  alert("second");
              });
              // 移除监听
              innerBoxEl.removeEventListener("click", function() {
                  alert("second");
              }); //不起作用，需要将回调函数写在外部声明，个人理解就是局部变量和全局变量的区别，如果写在参数里就找不到要移除的函数了
              boxEl.removeEventListener("click", first) //不起作用
              boxEl.removeEventListener("click", first, true);
          </script>
      </body>
      
      </html>
    ```
  
    
## 定时器

- `setTimeout()`; 延迟执行
- `setInterval()`; 循环执行
- `clearTimeout()`; 清除延迟执行的定时器
- `clearInterval()`; 清除循环执行的定时器

### 使用

```javascript
setTimeout(function() {
    console.log("延迟了5s才执行");
}, 5000);

var timer = setInterval(function() {
    console.log("每隔1s执行一次");
}, 1000);

// 清除定时器时 需要给定时器命名(用一个变量接受设置的定时器)
btn.onclick = function() {
    clearInterval(timer);
};
```

  **倒计时案例**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            margin: 0 auto;
            text-align: center;
        }
        
        ul {
            text-align: center;
            display: inline-block;
        }
        
        ul::after {
            content: "";
            clear: both;
            display: block;
        }
        
        ul li {
            list-style: none;
            float: left;
            width: 100px;
            height: 40px;
            margin: 20px 40px;
            line-height: 40px;
            background-color: #bfa;
            color: red;
            font-size: large;
            font-weight: bold;
        }
        
        ul span {
            width: 200px;
            display: block;
            margin: 0 auto;
            color: red;
            font-size: large;
            font-weight: bold;
        }
    </style>
</head>

<body>
    <div>
        <ul>
            <span>距离五一还有</span>
            <li>day</li>
            <li>hour</li>
            <li>minute</li>
            <li>second</li>
        </ul>
    </div>
    <script>
        // 获取li节点对象
        var liEls = document.querySelectorAll("ul li");
        var timer = setInterval(function() {
            // 在这里注意变量被声明提前
            // console.log(day, hour, minute, second);
            // 获取五一的具体事件
            var hopeTime = new Date("2021/5/1 00:00:00").getTime();
            // console.log(hopeTime);
            // 创建开始的事件，利用时间戳
            var startTime = new Date().getTime();
            // console.log(startTime);
            var chaTime = hopeTime - startTime;
            // 毫秒的时间转换
            var SECOND_MS = 1000;
            var MINUTE_MS = 60 * SECOND_MS;
            var HOUR_MS = 60 * MINUTE_MS;
            var DAY_MS = 24 * HOUR_MS;
            // 现在的时间戳到五一的天数
            var day = Math.floor(chaTime / DAY_MS);
            // console.log(day);
            var hour = Math.floor((chaTime % DAY_MS) / HOUR_MS);
            var minute = Math.floor((chaTime % HOUR_MS) / MINUTE_MS);
            var second = Math.floor((chaTime % MINUTE_MS) / SECOND_MS);
            liEls[0].innerText = transTime(day) + "天";
            liEls[1].innerText = transTime(hour) + "小时";
            liEls[2].innerText = transTime(minute) + "分钟";
            liEls[3].innerText = transTime(second) + "秒";
            console.log(day, hour, minute, second);
            if (day <= 0 && hour <= 0 && minute <= 0 && second <= 0) {
                liEls[0].innerText = "00" + "天";
                liEls[1].innerText = "00" + "小时";
                liEls[2].innerText = "00" + "分钟";
                liEls[3].innerText = "00" + "秒";
                clearInterval(timer);
                console.log("test");
            }
            // console.log(day, hour, minute, second);
        }, 1000);

        function transTime(num) {
            return num = num > 9 ? num + "" : "0" + num;
        }
    </script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <div><input type="text"><button>点击发送验证码</button></div>
    <script>
        // 获取button的节点对象
        var buttonEl = document.getElementsByTagName("button");

        // 延迟执行
        buttonEl[0].onclick = function() {
            // 创建倒计时60秒
            var count = 5;
            this.disabled = true;
            this.style = "width:30px;height:20px;line-height:20px,font-size:16px";
            this.innerText = count + "s";
            var timer = setInterval(function() {
                if (count == 1) {
                    // 清除循环执行的定时器
                    clearInterval(timer);
                    buttonEl[0].disabled = false;
                    buttonEl[0].innerText = "点击重新发送验证码"
                } else {
                    count--;
                    buttonEl[0].innerText = count + "s";
                    buttonEl[0].style = null;
                }

                console.log(count);
            }, 1000);
            // console.log(count);
        }
    </script>
</body>

</html>
```

