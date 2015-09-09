#javascript高级语言程序设计笔记\_(:з」∠)\_

## 第6章 
---
###属性类型(不多解释)
分为数据属性与访问器属性
#### 数据属性
* configurable 
* enumerable
* writable
* value

#### 访问器属性
* configurable
* enumerable
* get
* set
```javascript
var book = { //字面量方式创建对象
  _year: 2004; //使用下划线表示只能通过对象方法访问的属性
  edition: 1;
};
//设置单个属性，传入对象名、属性名、修改属性用的对象
Object.defineProperty(book, 'year', { 
// getter与setter若只定义了其中一个，则缺少另一个操作
  get: function(){
    return this.year;
  }
  set: function(newValue){
    if (newValue > 2004){
      this._year = newValue;
      this.edition += newValue - 2004; //同步更新其他属性
    }
  }
});

book.year = 2005;
alert(book.edition); //2
```

### 创建对象的模式
js支持面向对象编程，但不使用类或接口，对象在执行过程中可以被增强，具有动态性而非严格定义的实体，可采用下列模式创建对象

#### 工厂模式  
为了**减少**使用字面量创建对象而产生的**大量重复代码**而诞生，用简单的函数创建对象并返回，被构造函数模式取代
```javascript
function createPerson(name,age,job){
  var o = new Object(); //建立新对象
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function(){
    alert(this.name);
  };
  return o; //返回对象
}
```

#### 构造函数模式  
**工厂模式不能识别创建的对象类型**，由此诞生构造函数模式，使用`new`操作符，缺点在于每个成员如函数**无法复用**
* 普通的类型定义  
  值得注意的
  * 没有显式的创建对象
  * 赋值于`this`对象，与`new`操作符配合
  * 没有`return`
  * 大写字母开头的类名，已经成为惯例，便于区分函数与构造函数
  * 使用`new`操作符，其步骤是
    1. 创建新对象
    2. 将构造函数的作用域赋给新对象（与this对应）
    3. 执行构造函数
    4. 返回新对象（对应没有`return`）
```javascript
function Person(name,age,job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function(){
    alert(this.name);
  };
}
var person1 = new Person('haha',20,'student');
```
缺点在于，每个实例都是全新创建的，例中的`sayName()`方法被**重复创建**

#### 原型模式  
使用`prototype`属性指定需要共享的属性和方法，常常组合使用原型模式定义共享的属性和方法，用构造函数定义实例属性
* 理解原型对象  
  只要创建了新函数，就会根据特定的规则创建一个`prototype`属性，指向原型对象。而原型对象默认会获得一个`constructor`属性，指向`prototype`属性所在的函数的指针。即`Person.prototype.constructor<-->Person`。新建的实例内部包含不可见的指针`[[Prototype]]`（部分浏览器支持`__proto__`属性），指向原型对象
![盗图](http://www.liaoxuefeng.com/files/attachments/001435299854512faf32868f60348be878982909b5a5d04000/l)
* 一些方法
  可以使用`Person.prototype.isPrototypeOf(person1)`和`Object.getPrototypeOf(person1)`方法判断和获得原型对象
* 链式的搜索过程  
  读取某个对象的属性时，会从实例本身开始搜索该属性，若不存在则向上搜索其原型对象直到找不到，因此当实例中找到了某个属性时，即使原型对象中存在同名属性，这个属性已经被实例中的属性**屏蔽**
```javascript
function Person(){};

Person.prototype.name = 'Nick';
Person,prototype.age = 20; 

var person1 = new Person();
var person2 = new Person();

person1.name = 'Greg';
alert(person1.hasOwnProperty('name'); //true 判断实例是否拥有自己的属性
alert('name' in person1); //true 不论在原型中还是在实例中
alert(person1.name); //'Greg'
alert(person2.name); //'Nick'

delete person1.name; //使用delete删除实例中的属性
alert(person1.name); //'Nick'
```
* for-in循环  
  只有**可枚举**的属性会被返回，包括了原型中的属性
```javascript
for (var prop in obj){
  //do something...
}

Object.keys(Person.prototype); //返回可枚举的属性名数组
Object.getOwnPropertyNames(Person.prototype); //返回所有实例属性名
```
* 更简单的原型语法  
  使用字面量直接重写原型对象，相当于**用新对象代替**了原来的原型对象，会导致原先原型结构被破坏。新的原型对象需要**手动定义**`constructor`属性
```javascript
function Person(){};

var friend = new Person(); //先于重定义原型创建了实例

Person.prototype = {
  constructor : Person,
  name : 'Nick',
  age : 20,
  sayName : function(){
    alert(this.name);
  }
}

friend.sayName() //出错，其__proto__指向最初的原型对象
```
* 原型对象的问题  
  不同于基本类型属性会被实例中同名属性屏蔽的特性，**原型对象中的引用类型属性会被共享**

### 继承
使用原型链继承，即将超类的实例赋值给子类的构造函数的原型，缺点在于子类实例共享继承的属性和方法

#### 组合继承
在用原型链继承的同时，借用超类的构造函数构造子类实例，使每个实例都有自己的属性，同时保证只使用构造函数模式定义类型

#### 原型式继承
在不必定义构造函数的情况下继承，本质是执行给定对象的浅复制，对副本再进行增强改造

#### 寄生式继承
类似原型式继承，常与组合继承一起使用，减少调用超类的构造函数次数

#### 寄生组合式继承
最有效的继承方式
