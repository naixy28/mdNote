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

var friend2 = new Person(); //后于重定义原型创建了实例

friend.sayName() //出错，其__proto__指向最初的原型对象
friend2.sayName() //Nick
```
* 原型对象的问题  
  不同于基本类型属性会被实例中同名属性屏蔽的特性，**原型对象中的引用类型属性会被共享**，解决方式是组合使用构造函数模式和原型模式
```javascript
function Person(){}

Person.prototype={
  constructor: Person,
  name: 'Nick',
  friends: ['Shelby','Court'],
  sayName: function(){
    alert(this.name);
  }
};

var person1 = new Person();
var person2 = new Person();

person1.friends.push('Van'); //并没有创建实例属性，本质上在修改原型属性

alert(person1.friends); //'Shelby','Court','Van'
alert(person2.friends); //'Shelby','Court','Van' person2与person1共享引用类型的原型属性

```
#### 组合使用构造函数模式和原型模式
此为创建自定义类型的**最常见方式**，前面讲的都是为了这个做铺垫。特点是，使用构造函数模式定义实例属性，而使用原型模式用于定义方法和共享的属性。结果每个实例都会有自己的一份实例属性的副本，同时共享对方法的引用，节省了内存。另外，这种模式还支持向构造函数传递参数。
```javascript
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.friedns = ['shelby','court'];
}

Person.prototype = {
  constructor: Person,
  sayName: function(){
    alert(this.name);
  }
};

var person1 = new Person('Nick',29,'sf');
var person2 = new Person('Greg',20,'doc');

person1.friends.push('Van');
alert(person1.friends); //'Shelby','Court','Van'
alert(person2.friends); //'Shelby','Court'

```
#### 其他的构造模式
* 动态原型模式  
  组合模式把创建自定义类型分成了两部分，而动态原型模式通过使原型内的属性在构造函数内初始化（如果必要）来解决这个‘问题’
```javascript
function Person(name,age,job){
  this.name = name;
  //...
  
  //方法
  if( typeof this.sayName != 'function'){  //每次构造时判断这是不是第一个本类的实例，由此决定是否要初始化原型
    Person.prototype.sayName = function(){ // 用此方法只需判断任意一个原型属性是否存在
      alert(this.name);
    };
  }
}
```
* 寄生构造函数模式  
  本质上是使用了`new`的工厂模式，作用在于临时增强某个已有类型（主要是原生类型如Array）
* 稳妥构造函数模式  
  稳妥对象即没有公共属性，其方法也不引用this对象，用于安全的环境 
```javascript
function Person(name,age,job){
  var o =new Object();
  o.sayName=function(){
    alert(name);
  };
  return o;
}

var friend = Person('Nick' , '20, 'tea');
friend.sayName(); //除了用sayName方法，无法访问到name属性
```

### 继承
全靠原型链，图片书上有
#### 原型链继承
使用原型链继承，即将超类的实例赋值给子类的构造函数的原型，缺点在于子类实例共享继承的属性和方法
```javascript
function SuperType(){
  this.property = true;
}

SuperType.prototype.getSuperValue = function(){
  return this.property;
};

function SubType()[
  this.subproperty = false;
}

//继承了supertype
SubType.prototype = new SuperType();

Subtype.prototype.getSubValue = function(){
  return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue()); //true
```
上例中，继承是通过创建`SuperType`实例，并将其赋给`SubType.prototype`实现的。另外，此时`instance.constructor`指向的是`SuperType`

上例中搜索原型链的步骤如下  
1. 搜索实例
2. 搜索`SybType.prototype`
3. 搜索`SuperType.prototype`
4. 搜索默认的原型，即`Object.prototype`，此乃所有引用类型都继承`Object`的本质

**原型链的问题**  
* 与构造函数类似，由于继承自超类的实例，原本**超类实例中的实例属性就会变成子类的原型属性**了，对于引用对象来说这就意味着原型属性**被所有的子类实例共享使用**
* 单独使用原型链时，不能像超类的构造函数中**传递参数**

#### 借用构造函数
为了解决单独使用原型链的问题，诞生了这种方法，又叫做伪造对象或经典继承。基本思想是在子类构造函数中调用超类构造函数
```javascript
function SuperType(name){
  this.colors = ['red','blue','green'];
  this.name = name; //演示可以传递参数
}
  
function SubType(){
  //使拥有实例属性
  SuperType.call(this,'Nick');
}

```
显然，借用构造函数的方法由于不能复用函数也不单独使用

#### 组合继承
在用原型链继承的同时，借用超类的构造函数构造子类实例，使每个实例都有自己的属性，同时保证只使用构造函数模式定义类型
```javascript
function SuperType(name){
  this.name= name;
  this.colors = ['red','blue','green'];
}

SuperType.prototype.syaName = function(){
  alert(this.name);
};

function SubType(name,age){
  //继承属性
  SuperType.call(this.name);
  this.age = age;
}
//继承方法
SubType.prototype = new SuperType();

SubType.prototype.sayAge = function(){
  alert(this.age);
};


```
#### 原型式继承
在不必定义构造函数的情况下继承，即基于已有对象实现继承，本质是执行给定对象的浅复制，对副本再进行增强改造
```javascript
function object(o){
  function F(){}
  F.prototype = o;
  return new F(); //返回的对象与被浅复制的对象共享一个原型对象
}
```
* ECMAScript5新增了`Object.create()`方法规范了原型式继承（和上例作用一样），可以额外增加第二个参数是**为新对象定义额外属性的对象**
* 在没必要创建构造函数，只是想让一个对象与另一个对象保持相似的情况下可以使用原型式继承


#### 寄生式继承
类似原型式继承，常与组合继承一起使用，减少调用超类的构造函数次数
```javascript
function createAnother(original){
  var clone = object(original); //此处object函数不是必须的，任何能返回新对象的函数都适用
  clone.sayHi = function(){
    alert('hi');
  };
  return clone;
}
```

#### 寄生组合式继承
最有效的继承方式，利用寄生式继承，避免了为子类指定原型而调用超类的构造函数，转而用超类原型的一个副本作为子类的原型对象
```javascript
//用来代替 subType.prototype = new superType()的函数
function inheritPrototype(subType,superType){ 
  var prototype = object(superType.prototype); //复制了超类的原型对象
  prototype.constructor = subType; //将新的原型对象与子类相关联
  subType.prototype = prototype;
}

function SuperType(name){
  this.name = name;
  this.colors = ['red','blue'];
}
SuperType.prototype.sayName = function(){
  //say something
};
function SubType(name,age){
  SuperType.call(this,name);
  this.age = age;
}
inheritPrototype(SubType,SuperType); //就是他！
SubType.prototype.sayAge= function(){
  alert(this.age);
};


```
![test](http://ww4.sinaimg.cn/mw690/86444fb9gw1evxh5vav1rj20it0aa758.jpg)
试着画了个图表示寄生组合式继承，可以看出`SubType`的原型对象因为不是调用`SuperType`的构造函数得到所以没有多余属性（`name`和`colors`）
![test](http://ww2.sinaimg.cn/mw690/86444fb9gw1evxhapyhstj20by04g74c.jpg)
上图为书中的组合继承示意图
