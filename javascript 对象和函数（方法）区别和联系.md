# javascript 对象和函数（方法）区别和联系

### 1.对象

“JavaScript” 对象是变量的容器，但是通常我们认为对象是键值对的容器，键值对的通常写法是 name:value（键与值由冒号分割）。
键值对在 javaScript 对象中通常称为对象属性。
例子：

```js
var person = {
    firstName:"zhang",
    lastName:"erga",
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
```

#### 2.函数( *对象*  )

函数（方法Function）是对js操作过程的封装，以后操作同样的过程，只要调用相应的函数（方法）即可。对象同样是对js代码封装，不过对象可以封装函数（方法）。比如把某一类的函数（方法）都封装到某个对象中。这样可以系统的管理调用函数（方法）。
例子：

```js
function sum(num1, num2) {
    return num1 + num2; 
}
```



#### 3.联系

对象里面可以封装函数。
例子：

```js
var person = {
    firstName:"zhang",
    lastName:"erga",
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
```

#### 4.区别

对象里面包含的是而且只能是键值对（键值以“：”分割，值内容包括普通的变量和函数）。
函数里面可以封装操作过程，但是对象里面无法封装操作过程。

#### 5.对象的创建

Object:
JS里有一句话叫万物皆对象（除了基本数据类型）。但是new String(’’)或者new Number()这种不属于基本数据类型，属于引用类型。

对象就是属性的无序集合。数组是有序集合。

创建对象的方法：

- **直接量（也叫字面量）**
  属性（对象的静态体征）
  方法（对象的动态行为）
一个属性属性名结束后用，逗号隔开。

	```js
	var stu = {
	//name与age属于静态体征
	name: '李四',
	age: 13,
	//study与eat属于动态行为
	study: function(course) {
		console.log('学习课程:' + course)
	},
	eat: function(pig) {
	console.log('吃：' + pig)
  }
  }
  ```
  
  
  

- **通过构造函数创建**
  方法：var person = new 函数名();
  通过该方法创建对象时，会自动执行该构造函数。
  例如：

  ```js
  //构造函数的函数名首字母大写，区分与普通函数的区别，不是强制规定的，你也可以小写。
  function Person(name, sex) {
  	this.name = name;
  	this.sex = sex,
  	this.job = function() {
  		alert(this.name)
  	}
  }
  var child = new Person('Jack', '男');
  Person.job();
  //此代码一共会两次跳出对话框，因为创建对象时会自动执行构造函数一次。this指的是调用函数的对象。
  ```

  


- **通过new Object()创建**
  方法:通过object构造器new一个对象，在丰富对象信息。

  ```js
  var person = new Object();
  person.name = 'wuxiaodi';
  person.sex = 'boy';
  ```

  


- **工厂方式**

  ```js
  function createStudent(name, age) {
  	let stu = new Object();
  	stu.name = name;
  	stu.age = age;
  	stu.study = function() {
  		console.log(this.name + " 学习...");
  	}
  	return stu;
  }
  ```

  


注意：不能区分对象的具体类型

构造函数 + prototype
prototype: 原型对象
对象共享资源
_proto_:原型属性
prototype：默认每个函数对象都有prototype属性，显式属性
_proto_：默认每个对象都有 _proto_ 属性，该属性默认指向创建对应对象的构造函数中的 prototype，隐式属性
JS中的对象都是基于原型的对象。

#### 6.对象属性调用：

对象名.属性名
对象名.方法名([参数列表])
或：
对象名[‘属性名’]
对象名[‘方法名’]()

先在自身空间中查找，如果有则直接使用，如果没有，则到原型中查找；如果原型中存在，则调用使用，如果原型中不存在，则继续到原型的原型(原型链)中查找，依此方式继续查找使用，如果一直不存在，会继续向原型链中查找，直到 Object.prototype（原型链终点）中。如果原型链中不存在待调用的属性，则返回 undefined

Object.create()
class 语法糖
…