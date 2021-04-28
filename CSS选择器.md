# CSS选择器

- 标签，类，ID选择器
- 属性选择器
- 伪类和伪元素
- 关系选择器

| 选择器                                                       | 示例                | 学习CSS的教程                                                |
| :----------------------------------------------------------- | :------------------ | :----------------------------------------------------------- |
| [类型选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Type_selectors) | `h1 { }`            | [类型选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Type_selectors) |
| [通配选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Universal_selectors) | `* { }`             | [通配选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#The_universal_selector) |
| [类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Class_selectors) | `.box { }`          | [类选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Class_selectors) |
| [ID选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/ID_selectors) | `#unique { }`       | [ID选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#ID_Selectors) |
| [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors) | `a[title] { }`      | [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Attribute_selectors) |
| [伪类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes) | `p:first-child { }` | [伪类](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-class) |
| [伪元素选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements) | `p::first-line { }` | [伪元素](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-element) |
| [后代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator) | `article p`         | [后代运算符](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Descendant_Selector) |
| [子代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator) | `article > p`       | [子代选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Child_combinator) |
| [相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator) | `h1 + p`            | [相邻兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Adjacent_sibling) |
| [通用兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator) | `h1 ~ p`            | [通用兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#General_sibling) |

## 选择器优先级

**内联>id>类>标签>通配符>继承>浏览器默认**

#### 优先级提升

**！important（仅可直接选中）：使得优先级最大**

- p{color：red ！important;}
  1、只能直接选中，不能用于间接选中
  2、通配符选中标签也是直接选中的
  3、仅提升选中的样式，而其他样式不会提升
  4、必须写分号前面

### 选择器权重计算

内联 style（1000） > id多看id（100）>类名多看类名（10）>标签多看标签（1）（都一样时 谁写后面听谁的）通配符权重为0 不计入
注：一般只有选择器直接选中才会用到权重计算

##### 继承性

1、以color/font-/text-/line开头属性可继承
2、只要是后代就可继承
3、a标签文字颜色和下划线无法继承
4、h标签的文字大小无法继承

### 选择器列表(并集选择器)

CSS **选择器列表**（`,`），常被称为并集选择器或并集组合器，选择所有能被列表中的任意一个选择器选中的节点。

```
/* 选择所有 <span> 和 <div> 元素 */
span, div {
  border: red 2px solid;
}
```

为了使样式表更简洁，可以使用逗号分隔的列表来对选择器进行分组。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list#语法)

```
element, element, element { style properties }
```

## [例子](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list#例子)

### [单行分组](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list#单行分组)

在一行之中使用逗号为选择器分组。

```
h1, h2, h3, h4, h5, h6 { font-family: helvetica; }
```

### [多行分组](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list#多行分组)

使用逗号对多行选择器进行分组。

```
#main,
.content,
article {
  font-size: 1.1em;
}
```

### [选择器列表无效化](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list#选择器列表无效化)

使用选择器列表的一个缺点是，以下两段 CSS 代码并不不等价：

```
h1 { font-family: sans-serif }
h2:maybe-unsupported { font-family: sans-serif }
h3 { font-family: sans-serif }
h1, h2:maybe-unsupported, h3 { font-family: sans-serif }
```

这是因为，在选择器列表中如果有一个选择器不被支持，那么整条规则都会失效。

解决这个问题的一个方法是使用 [`:is()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is) 选择器，它会忽视它的参数列表中失效的选择器，但是由于 [`:is()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is) 会影响优先级的计算方式，这么做的代价是，其中的所有选择器都会拥有相同的优先级。

```css
h1 { font-family: sans-serif }
h2:maybe-unsupported { font-family: sans-serif }
h3 { font-family: sans-serif }
:is(h1, h2:maybe-unsupported, h3) { font-family: sans-serif }
```

### 交集选择器

条件：交集选择器由两个选择器构成，找到的标签必须满足：既有标签一的特点，也有标签二的特点。
代码：

```html
<head>
	<title>交集选择器</title>
	<style >
		/*需求：让p标签变成红色*/
		/*交集选择器：既是 p标签 ，又是 .red类选择器*/
		p.red{
			color: red;
		}
	</style>
</head>

<body>
	<p class="red">红色</p>
	<p class="red">红色</p>
	<p class="red">红色</p>
	<div class="red">红色</div>
	<p>蓝色</p>
	<p>蓝色</p>
	<p>蓝色</p>
</body>

```

结果：只有 p标签中存在 class类的标签变成了红色。

格式： p.red 是一个名字，中间不能有空格。交集选择器并不常用