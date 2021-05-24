# Flex

布局的传统解决方案，基于[盒状模型](https://developer.mozilla.org/en-US/docs/Web/CSS/box_model)，依赖 [`display`](https://developer.mozilla.org/en-US/docs/Web/CSS/display) 属性 + [`position`](https://developer.mozilla.org/en-US/docs/Web/CSS/position)属性 + [`float`](https://developer.mozilla.org/en-US/docs/Web/CSS/float)属性，对于某些特殊布局非常不方便，比如，[垂直居中](https://css-tricks.com/centering-css-complete-guide/)就不容易实现。

2009年，W3C 提出了一种新的方案—-Flex 布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。

##### 使用时应该注意些什么？

- 如果是Webkit内核的浏览器，需要加上 **-webkit** 前缀
- 在父级元素设置为flex布局后，子元素的float、clear、vertical-align属性都将失效，所以在使用flex布局时，不应该先设置完子元素布局后再使用。

### 1. `display: flex`

![img](http://doc.bufanui.com/uploads/beAdvance/images/m_9fb05a427e17a13e02ca67f32070a4d3_r.png)

- 每个弹性容器都有两根轴：主轴（横轴）和交叉轴（侧轴、纵轴），两轴之间成90度关系。注意：水平的不一定就是主轴。
- 每根轴都有起点和终点，这对于元素的对齐非常重要。
- 弹性容器中的所有子元素称为<弹性元素>，弹性元素永远沿主轴排列。
- 弹性元素也可以通过`display:flex`设置为另一个弹性容器，形成嵌套关系。因此一个元素既可以是弹性容器也可以是弹性元素。

### 弹性容器属性

#### `justify-content` 主轴排列

```html
.box{
    justify-content:flex-start | flex-end | center | space-between | space-around | space-evenly(实验属性);
}

flex-start（默认值）：左对齐
flex-end：右对齐
center： 居中
space-between：两端对齐，项目之间的间隔都相等。
space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
space-evenly：均匀排列每个元素，每个元素之间的间隔相等
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_2392ead3bd705eadf8980d6f56d0dd54_r.png)

------

#### `align-items` 交叉轴排列

```css
.box{
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_0db306d85293caf5561dfaf0b428b20f_r.png)

------

#### `flex-wrap` 的作用

```css
.box{
    flex-wrap: nowarp | wrap | wrap-reverse;
}
nowrap（默认值）：不换行
wrap：换行，第一行在上方
wrap-reverse：换行，第一行在下方
```

------

#### `align-content`

`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

```css
align-content: stretch | flex-start | flex-end | center | space-between | space-around | space-evenly
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_37e1daada7dcf3f074c9bc589f693f1c_r.png)

------

#### `flex-direction` 的使用 及 `align-items` 方向问题

```css
.box{
    flex-direction: row | row-reverse | column | column-reverse;
}
row（默认值）：主轴为水平方向，起点在左端。
row-reverse：主轴为水平方向，起点在右端。
column：主轴为垂直方向，起点在上沿。
column-reverse：主轴为垂直方向，起点在下沿。
注意： flex-direction 改为垂直方向时，jusitify控制垂直方向，align控制水平方向
```

------

#### 复合属性: `flex-flow`

语法：`flex-flow = flex-drection + flex-wrap`

例如：`flex-flow: row nowrap;`

### 弹性元素属性

#### `order` 的使用

`order` 属性定义项目的排列顺序。数值越小，排列越靠前，默认为0

```css
.item {
  order: <integer>;
}
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_5e25bd844d765b0661d16f516614f876_r.png)

#### `align-self` 的作用

`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_bcafe63546076f149e0574f7736251f6_r.png)

#### `flex-grow属性`

`flex-grow`属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。无多余宽度时，flex-grow无效。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_bd1eb0d0e0cedf3c482e1ec10ff4a050_r.png)

#### `flex-shrink`属性

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。负值对该属性无效

```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

![null](http://doc.bufanui.com/uploads/beAdvance/images/m_11a3c7f7f5df76bf6fa0d4ff98291e51_r.png)

关于元素的收缩的计算方法：[详情请参见](https://www.cnblogs.com/qcloud1001/p/9848619.html)

#### `flex-basis`属性

`flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

```css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

它可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间

#### `flex`属性

它是`flex-grow, flex-shrink` 和 `flex-basis`的简写，默认值为0 1 auto。后两个属性可选

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值

除了以上两种取值方式外，`flex`后面跟其它值的情况：

- 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 1;}
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）：

```css
.item-1 {flex: 0%;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
.item-2 {flex: 24px;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 24px;
}
```

- 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 2 3;}
.item {
    flex-grow: 2;
    flex-shrink: 3;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1，如下是等同的：

```css
.item {flex: 200 300px;}
.item {
    flex-grow: 200;
    flex-shrink: 1;
    flex-basis: 300px;
}
```