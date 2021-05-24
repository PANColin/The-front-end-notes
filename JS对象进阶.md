# JS对象

什么是对象：

ECMA-262 把对象定义为：“无序属性的集合，其属性可以包含原始值、对象或者函数。”

可以这样理解对象：

1. 对象是单个事物的抽象。比如一支笔，一本书，一辆车都可以是一个对象。
2. 对象是一个容器，封装了属性和方法。比如：一辆车。它的颜色，大小，重量等是它的属性，而启动，加速，减速，刹车等是它的方法。

属性组成：

- 属性名 : 字符串(标识)
- 属性值 : 任意类型

属性的分类：

- 一般 : 属性值不是 function 描述对象的状态
- 方法 : 属性值为 function 描述对象的行为

特别的对象：

- 数组: 属性名是0,1,2,3之类的索引
- 函数
- JSON: 对象的字符串表示，用来传输数据

## ES6简化的对象操作

1. 当对象的属性名与代表属性值的变量名一致的时候可以只写属性名

```js
var school = "不凡学院";
var course = "UI & H5";
var obj = {
    // school: school
    // course: course
    school,
    course
}
console.log(obj); // { school: "不凡学院", course: "UI & H5"}
```

1. 对象属性值为函数的时候可以简写

```js
var obj = {
    sayName: function(){
        console.log("obj");
    }
    sayHello(){
        console.log("hello");
    }
}
obj.sayName(); // obj
obj.sayHello(); // hello
```

# 对象克隆

声明一个对象

```js
// 声明一个对象
var user = {
    name: "李白",
    age: 29,
    hobbies: ["唱歌","吟诗","旅游"],
    sayName: function (){
        alert(this.name);
    }
}
```

简单克隆（浅克隆 ）

```js
// 创建一个新的对象,简单类型值不会改变,引用对象值会跟着改变
function clone(obj){
       var newObj = {};
    for(var i in obj){
        newObj[i] = obj[i];
    }
    return newObj;
}
let obj1 = user;  // 指向同一个地址,一个改变,另一个跟着改变

/* 使用Object.assign 来拷贝对象
Object.assign(dest[, src1, src2, src3...])
    参数 dest 和 src1, ..., srcN（可以有很多个）是对象。
    这个方法复制了 src1, ..., srcN 的所有对象到 dest。换句话说，从第二个参数开始，所有对象的属性都复制给了第一个参数对象，然后返回 dest
    如果接收的对象已经有了同样属性名的属性，前面的会被覆盖

    改变合并后的对象的某个引用类型属性，最后合并此引用类型属性的对象的这个属性也会改变
*/
let obj1 = Object.assign({},user); // 创建一个新的对象,简单类型值不会改变,引用对象值会跟着改变
```

## **深度克隆**

```js
// 为了解决上面的的问题，我们在复制的时候应该检查 `user[key]` 的每一个值，如果是一个对象，我们再复制一遍这个对象，可以利用 递归思想
let obj1 = JSON.parse(JOSN.stringigy(user)) // 无法拷贝函数
function deepClone(obj){
    // 如果是null，直接返回
    if(obj === null){
        return null;
    }
    // 判断类型初始化克隆变量的类型
    var newObj = obj instanceof Array ? [] : {};

    for(var i in obj){
        // array object
        newObj[i] = typeof obj[i] === "object" ? deepClone(obj[i]) : newObj[i] = obj[i];
    }

    return newObj;
}
let user1 = deepClone(user);
user.hobbies.push("drink");
console.log(user1.hobbies);
```

```js
  // 递归函数深克隆   ⭐
        function deepClone(obj){
            if(obj === null){
                return null;
            }else if(Array.isArray(obj)){
                var newObj = [];
            }else {
                var newObj = {};
            }

            for(var i in obj){
                // 判断属性值是否是引用类型
                if(typeof obj[i] === "object"){
                    // null array object
                    newObj[i] = deepClone(obj[i]);
                }else {
                    newObj[i] = obj[i];
                }
            }

            return newObj;
        }
```

## 对象属性描述符

对象属性并不是一个简单的“键-值”对，它实际上是更复杂可变的东西。我们使用**对象描述符**来描述对象属性的特性，分为两种：**数据属性和访问器属性**

### 数据属性

数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有 4 个描述其行为的特性

- `[[Configurable]]` ：是否可配置标志，表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。

  有时它会预设在内置对象和属性中。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true

  当它的值为false的时候，一个不可配置的属性不能被 `defineProperty` 删除或修改。

- `[[Enumerable]]` ：表示能否通过 for-in 循环返回属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true 枚举/穷举/遍历

- `[[Writable]]` ：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true

- `[[Value]]` ：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined

像上面的两种方法那样直接在对象上定义属性和方法，它们的 `[[Configurable]] 、 [[Enumerable]] 和 [[Writable]]` 特性都被设置为 true ，而 `[[Value]]` 特性被设置为指定的值。可以通过 `Object.defineProperty()` 来定义。

### 访问器属性

访问器属性不包含数据值；它包含一对儿 *getter* 和 *setter* 函数（不过，这两个函数都不是必需的）。在读取访问器属性时，会调用 *getter* 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用 *setter* 函数并传入新值，这个函数负责决定如何处理数据。

- [[Get]] ：在读取属性时调用的函数。默认值为 undefined
- [[Set]] ：在写入属性时调用的函数。默认值为 undefined

在对象字面量中，它们用 `get` 和 `set` 表示：

```js
// 从外表看，访问器属性看起来像一个普通的属性。这是访问器属性的设计思想。我们不以函数的方式调用 user.fullName，我们通常读取它：getter 在幕后运行。
// 这样就创建了一个“虚拟”属性。它是可读写的，但实际上并不存在。

let obj = {
  get propName() {
    // getter, the code executed on getting obj.propName
  },

  set propName(value) {
    // setter, the code executed on setting obj.propName = value
  }
};
```

##### 访问器属性的描述符

访问器属性的描述符与数据属性相比是不同的。

对于访问器属性，没有 `value` 和 `writable`，但是有 `get` 和 `set` 函数。

所以访问器描述符有：

- **get** —— 一个没有参数的函数，在读取属性时工作，
- **set** —— 带有一个参数的函数，当属性被设置时调用，
- **enumerable** —— 与数据属性相同，
- **configurable** —— 与数据属性相同。

例如，要使用 `defineProperty` 创建 `fullName` 的访问器，我们可以使用 `get` 和 `set` 来传递描述符：

```js
let user = {
  name: "John",
  surname: "Smith"
};

Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  }
});

alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```

**受保护的属性通常以下划线 _ 作为前缀，这不是在语言层面强制实施的，但是有一个在程序员之间人尽皆知的惯例是不应该从外部访问这些属性和方法。**

**注意: 属性可以是“数据属性”或“访问器属性”，但不能同时属于两者。**

```js
// 同时存在 get函数和 value,会报错
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },
  value: 2
});
```

# 对象的方法

## 限制访问整个对象的方法

- [Object.preventExtensions(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)

  禁止向对象添加属性。

- [Object.seal(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)

  禁止添加/删除属性，为所有现有的属性设置 `configurable: false`。

- [Object.freeze(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

  禁止添加/删除/更改属性，为所有现有属性设置 `configurable: false, writable: false`。

还有对他们的测试：

- [Object.isExtensible(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)

  如果添加属性被禁止，则返回 `false`，否则返回 `true`。

- [Object.isSealed(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)

  如果禁止添加/删除属性，则返回 `true`，并且所有现有属性都具有 `configurable: false`。

- [Object.isFrozen(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)

  如果禁止添加/删除/更改属性，并且所有当前属性都是 `configurable: false, writable: false`，则返回 `true`。

## `Object.defineProperty()`

此方法用来定义单个属性的属性描述符。

语法：

```js
Object.defineProperty(obj, propertyName, descriptor)
// obj，propertyName   要处理的对象和属性。
// descriptor   要应用的属性描述符。
```

**注意:**

1. 使属性不可配置是一条单行道。我们不能把它改回去，因为 `defineProperty` 不适用于不可配置的属性。
2. 设置不可用的属性时，只在使用严格模式时才会出现错误。在非严格模式下，写入只读属性等时不会发生错误。但操作仍然不会成功。非严格模式下违反标志的行为只是默默地被忽略。

## `Object.defineProperties()`

此方法用来一次定义多个属性，这个方法接受两个对象参数： 添加或修改其属性的对象，与第一个对象中要添加或修改的属性一一对应的对象

语法:

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

## `Object.getOwnPropertyDescriptor()`

此方法用来获取对象的属性描述符

```js
var obj = {
    name: "obj"
}
Object.defineProperty(obj, "contain", { value: "苦海无边" });
console.log(Object.getOwnPropertyDescriptor(obj, "name"));
console.log(Object.getOwnPropertyDescriptor(obj, "contain"));
```

## `Object.getOwnPropertyNames()`

此方法可以得到对象所有的属性，无论它是否可枚举。

```js
var obj = {
    name: "李白",
    age: 24
}
Object.defineProperty(obj, "tel", {
    value: "123456"
})

Object.getOwnPropertyNames(obj) //  ["name", "age", "tel"]
```

## `Object.keys()`

此方法可以获取对象所有可枚举的属性。

```js
var obj = {
    name: "李白",
    age: 24
}
Object.defineProperty(obj, "tel", {
    value: "123456"
})
console.log(Object.keys(obj)); // ["name", "age"]
```