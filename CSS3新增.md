## CSS3新增

#### 计算长度函数

## calc

`calc(expression)` 函数用于动态计算长度值

- `expression` 必须，一个数学表达式，结果将采用运算后的返回值。
- 需要注意的是，+ - 运算符前后都需要保留一个空格(* / 例外了)，例如：`width: calc(100% - 10px)`
- 任何长度值都可以使用`calc()`函数进行计算
- `calc()`函数支持 “+”, “-“, “*”, “/“ 运算
- `calc()`函数使用标准的数学运算优先级规则

#### CSS变量

复杂的网站都会有大量的CSS代码，通常会有很多重复的值。例如网站统一使用的字体样式，颜色等，如果开发者希望更换一种样式，就需要全局搜索并且一个个的替换，操作繁琐无趣。

CSS预处理语言通过设定变量的方式简化了这种操作，并且收到开发者的一致好评。依据社区推动标准的规律，CSS官方也推出自定义属性写法。

官方的标准出现很晚，前端框架最喜欢的 $ 已经被 Sass 变量用掉了， @ 也被 Less 用掉了。无奈只能使用 `--` 这种前缀写法，哪怕看起来有点丑。。。

##### 使用方式

声明一个CSS变量，属性名需要以两个减号（`--`）开始，属性值可以是任何有效的CSS值，变量声明在元素选择器中。例如：

```css
.box {
    --bg-color: #f00;
}
```

变量的使用范围即书写位置所在的元素及其子集，如果想在HTML文档的任何地方访问到它，我们可以将它声明在根伪类（即 html）中：

```css
:root {
    --bg-color: #f00;
}
```

当我们定义好一个CSS变量后，使用 `var()` 函数包裹变量名即可使用：

```css
element {
    background-color: var(--bg-color);
}
```

### Tips

1、自定义属性是大小写敏感的，`--my-color` 和 `--My-color` 会被认为是两个不同的CSS变量

```css
:root {
    --my-color: #f00;
    --My-color: #00f;
}
element {
    color: var(--my-color);
    background-color: var(--My-color);
}
```

2、同一个CSS变量，可以在多个选择器内声明，依照CSS优先级使用

```css
:root {
    --width: 300px;
}

element {
    --width: 200px;
    width: var(--width); /* 200px */
}
```

3、变量不赋值代表空值，并不会报错。`initial` 是变量的无效值，使用了空值或无效值的样式会自动转换为该样式的默认初始值；

```css
:root {
    --width: ;
    --color: initial;
}

element {
    width: var(--width);
    color: var(--color);
}
```

4、当浏览器遇到无效的 `var()` 时，会使用继承值或初始值代替。

```css
:root {
    --color: 16px;
}

element {
    color: var(--color);    
}
```

##### 变量默认值

`var()` 函数还可以定义默认值，只需在变量后面跟上逗号，加上默认值即可：

```css
element {
    width: var(--width, 200px);
}
```

也可以函数嵌套：

```css
:root {
    --color: #f00;
}

element {
    background-color: var(--bg-color, var(--color, #00f));
}
```

不推荐使用函数嵌套的方式，这会造成浏览器浪费更多时间在处理变量上。

### 通过JS修改CSS变量

```js
// 获取元素行内样式设置的的 CSS 变量
element.style.getPropertyValue("--my-var");

// 获取元素内联/外联样式设置的的 CSS 变量
getComputedStyle(element).getPropertyValue("--my-var");

// 修改一个 DOM 节点上的 CSS 变量
element.style.setProperty("--my-var", value);
```

### 瀑布流

```html
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
    <meta name="referrer" content="never">
    <title>瀑布流</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        
        ul {
            list-style: none;
        }
        
        body {
            width: 100%;
            height: 100%;
            /* overflow: hidden; */
        }
        /* 清除浮动的影响 */
        
        .container::after {
            content: "";
            display: block;
            clear: both;
        }
        
        .container {
            width: fit-content;
            height: fit-content;
            margin: 50px auto;
            /* color: hsl(360, 100%, 100%); */
        }
        
        ul {
            float: left;
            width: 200px;
            /* height: 100%; */
            height: fit-content;
            margin: 0 50px;
            font-size: 50px;
            color: #fff;
            text-align: center;
            /* overflow: hidden; */
        }
        /* ul:last-child {
            margin-right: 0;
        } */
    </style>
</head>

<body>
    <div class="container">
        <ul></ul>
        <ul></ul>
        <ul></ul>
        <ul></ul>
    </div>

    <script type="text/javascript">
        var uls = document.querySelectorAll("ul");
        var n = 0; // 序号

        // uls[0].style.cssText = "height: 300px;background-color: #f00;"

        // 进入页面，生成滚动条，让阴影部分距离底部 > 10
        while (document.body.scrollHeight - window.innerHeight < 10) {
            // console.log("============");
            createLi();
            // console.log(arr);
        }

        window.onscroll = function() {
            var sct = window.pageYOffset;
            if (document.body.scrollHeight - window.innerHeight - sct < 100) {
                createLi();
            }
        }

        function createLi() {
            // 声明数组，记录出现过的高度
            var arr = [];

            for (var j = 0; j < 4; j++) {

                // 生成一行li，高度在 [230,260]，一行li高度最小为 5，颜色随机，优先添加到最短的 ul 列中
                var h = rd(230, 260, 5);
                if (arr.indexOf(h) !== -1) {
                    j--;
                    continue;
                }

                arr.push(h);

                n++;
                var liEle = document.createElement("li");
                liEle.style.cssText = `height:${h}px;line-height:${h}px;background-color: hsl(${rd(0,360,1)},${"50%"},${"50%"})`;
                // liEle.style.cssText = `height:${h}px;line-height:${h}px;background-color:rgb(${rd(0,255,1)},${rd(0,255,1)},${rd(0,255,1)})`;

                liEle.innerText = n;
                // liEle.style.height = "";
                // liEle.style.lineHeight = "";
                // liEle.style.backgroundColor = "";
                // liEle.style.cssText = "height: " + h + "px;line-height: " + h + "px;background-color: rgb(" + rd(0, 255, 1) + "," + rd(0, 255, 1) + "," + rd(0, 255, 1) + ")";

                // 添加到最短的ul列中
                var minIndex = 0;
                for (var i = 1; i < uls.length; i++) {
                    // if (uls[minIndex].offsetHeight >= uls[i].offsetHeight) {
                    if (uls[minIndex].scrollHeight > uls[i].scrollHeight) {
                        console.log("scrollHeight", uls[i].scrollHeight, "offsetHeight", uls[i].offsetHeight, "clientHeight", uls[i].clientHeight);
                        minIndex = i;
                    }
                }

                uls[minIndex].append(liEle);
            }
        }

        function rd(min, max, cha) {
            // [0,50]/5 ===> [0,10]*5 
            return Math.floor(Math.random() * (max - min + 1) / cha) * cha + min;
            // return Math.round(Math.random() * (max-min)) + min;
        }
    </script>
</body>

</html>
```

### 钟表

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        /*时钟的样式*/
        
        ul {
            list-style-type: none;
        }
        
        .box {
            position: relative;
            width: 200px;
            height: 200px;
            background-color: #fff;
            margin: 100px 100px;
            box-shadow: 1px -3px 7px 1px #bbb;
            border-radius: 50%;
            border: 20px solid lawngreen;
        }
        
        .box::after {
            content: "";
            display: block;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background-color: #bfa;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        
        .box .pointer {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translate(-50%, 0);
            /*background-color: black;*/
            transform-origin: bottom;
            /*box-shadow: 3px 4px 9px 0px #000;*/
            box-shadow: 1px -3px 7px 1px #bbb;
            animation: round 60s steps(60) infinite both;
        }
        
        .box .pointer.second-line {
            width: 5px;
            height: 80px;
            margin-top: 20px;
            background-color: #fba;
            border-radius: 50% 50% 0 0;
        }
        
        .box .pointer.minute-line {
            width: 5px;
            height: 60px;
            margin-top: 40px;
            background-color: #faf;
            border-radius: 50% 50% 0 0;
            animation-duration: 3600s;
        }
        
        .box .pointer.hour-line {
            width: 5px;
            height: 40px;
            margin-top: 60px;
            background-color: #bbf;
            border-radius: 50% 50% 0 0;
            animation-duration: calc(3600s * 12);
        }
        
        .box .wrapper {
            position: absolute;
            top: 95px;
            left: 97.5px;
            z-index: 100;
        }
        
        .box .wrapper li {
            position: absolute;
            width: 5px;
            height: 10px;
            background-color: rgba(105, 105, 105, .7);
            box-shadow: 1px -3px 7px 1px #bbb;
            border-radius: 50%;
        }
        
        .box .wrapper li p {
            line-height: 40px;
            text-indent: -3px;
            color: skyblue;
        }
        /* @keyframes round {
            from {
                transform: rotate(0deg);
            }
            to {
                transform: rotate(360deg);
            }
        } */
    </style>
</head>

<body>
    <div class="box">
        <div class="second-line pointer"></div>
        <div class="minute-line pointer"></div>
        <div class="hour-line pointer"></div>
        <ul class="wrapper">
        </ul>
    </div>
</body>
<script>
    // 获取ul
    var ulEl = document.querySelector(".box .wrapper");

    for (var i = 0; i < 12; i++) {
        // 创建表盘刻度
        var liEl = document.createElement("li");
        liEl.style.transform = `rotate(${i * 30}deg) translateY(-95px) `;
        liEl.innerHTML = "<p style='transform: rotate(" + -i * 30 + "deg)'>" + (i == 0 ? 12 : i) + "</p>";
        ulEl.append(liEl);
        console.log(liEl);
    }
    var secEl = document.querySelector('.second-line');
    var minEl = document.querySelector('.minute-line');
    var hourEl = document.querySelector('.hour-line');
    // 让钟表生成系统当前时间

    function time() {
        var date = new Date();
        var s = transTime(date.getSeconds());
        var m = transTime(date.getMinutes());
        var h = transTime(date.getHours());
        console.log(Number(h), m, s);

        secEl.style.transform = 'rotate(' + Number(s) * 6 + 'deg)';
        minEl.style.transform = 'rotate(' + Number(m) * 6 + 'deg)';
        hourEl.style.transform = 'rotate(' + Number(h) * 30 + 'deg)';
    }
    time();

    function transTime(num) {
        return num = num > 9 ? num + "" : "0" + num;
    }
    setInterval(time, 1000);
</script>

</html>
```

### c3轮播

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="ie=edge,chrome=1">
    <meta name="referrer" content="never">
    <title>C3轮播</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        ul {
            list-style: none;
        }

        .container {
            width: 1200px;
            margin: 50px auto;
        }

        nav {
            width: 200px;
            margin: 0 auto;
            border: 1px solid #f00;
        }

        nav button {
            width: 100px;
            line-height: 44px;
            color: #f00;
            background-color: #fff;
            border: none;
        }

        nav button.act {
            color: #fff;
            background-color: #f00;
        }

        .box {
            overflow: hidden;
        }

        .content {
            position: relative;
            left: 0;
            width: 200%;
            margin-top: 50px;
            overflow: hidden;
            transition: left .4s ease;
        }

        .content ul {
            float: left;
            width: 50%;
        }

        .content li {
            float: left;
            width: 22%;
            height: 200px;
            margin-right: 4%;
            margin-bottom: 30px;
            line-height: 30px;
            box-shadow: 3px 3px 14px 5px #e6e6e6;
        }

        .content li:nth-child(4n){
            margin-right: 0;
        }

        .content li div {
            height: 10px;
        }

        .content li p {
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
    </style>
</head>
<body>
    <div class="container">
        <nav></nav>
        <div class="box">
            <div class="content">
                <!-- <ul>
                    <li>
                        <div></div>
                        <h3></h3>
                        <p></p>
                        <p></p>
                        <p></p>
                        <p></p>
                    </li>
                </ul>
                <ul></ul> -->
            </div>
        </div>
    </div>

    <script src="../lib/data/data.js"></script>
    <script type="text/javascript">
        console.log(data);
        var navEle = document.querySelector("nav"),
            content = document.querySelector(".content");

        var str = "";
        for(var i = 0; i < data.course.length; i ++){
            // console.log(data.course[i].cname);
            navEle.insertAdjacentHTML("beforeEnd", `<button ${ i ? "" : "class='act'" }>${data.course[i].cname}</button>`);
            // navEle.insertAdjacentHTML("beforeEnd", `<button ${ !i && "class='act'" }>${data.course[i].cname}</button>`);
            str += `<ul>`
                for(var j = 0; j < data.course[i].details.length; j ++){
                    var detail = data.course[i].details[j];
                    // console.log(detail.title);
                    // console.log(detail.color);
                    str += `<li>
                        <div style="background-color: ${detail.color}"></div>
                        <h3>${detail.title}</h3>`
                        for(var k = 0; k < detail.pcontent.length; k ++){
                            // console.log(detail.pcontent[k]);
                            str += `<p>${detail.pcontent[k]}</p>`;
                        }
                    str += `</li>`;
                    
                }
            str +=`</ul>`;
            
        }

        content.innerHTML = str;

        navEle.onclick = function (){
            // 1. 校验触发事件的元素是否是委托元素
            var ele = event.target;
            if(ele.tagName == "BUTTON"){
				console.log(this);
                // 没有其它类名，直接用className赋值
                // element.querySelector 从子元素中选择
                this.querySelector(".act").className = "";
                ele.className = "act";
                // 过渡需要起始状态
                content.style.left = ele.innerText.indexOf("H5") == -1 ? "0px" : "-100%";
            }
        }

        // mouseenter/leave mouseover/out
        content.onmouseover = function (){
            // element.closest(selector) 返回距离元素最近的符合选择条件的父元素或者己身，如果没有，返回 null
            var ele = event.target.closest("li"); // li null
            
            if(ele){
                ele.style.color = ele.children[0].style.backgroundColor;
            }
        }

        content.onmouseout = function (){
            var ele = event.target.closest("li"); // li null
            
            if(ele){
                ele.style.color = "";
            }
        }
    </script>
</body>
</html>
```

