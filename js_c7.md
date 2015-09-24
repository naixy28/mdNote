
#javascript高级语言程序设计笔记

## 第7章 
---
### 函数声明与函数表达式
* 函数声明提升，执行代码之前会先读取函数声明，新一代浏览器为`function`后的函数名提供了非标准的`name`属性用于访问，匿名函数的`name`为空串
```javascript
  sayHi();
  function sayHi(){
  } //可行
```
* 函数表达式与其他表达式一样，必须先赋值后调用
```javascript
sayHi(); //不可行
var sayHi = function(){
  alert();
};
```
* 区别
```javascript
if(confition){
  function sayHi(){
    alert(1);
  }
}eles{
  function sayHi(){
    alert(2);
  }
}
// 直接alert2 原因是函数声明提升，而第二次声明后覆盖了第一次的函数，与条件判断无关

var sayHi;
if(condition){
  sayHi = function(){alert(1)};
}eles{
  sayHi = function(){alert(2)};
}

// 由于函数表达式的原因，可以正常运行
```
### 递归
* 之前提过使用`arguments.callee`代替内部引用的函数名，但是在严格模式下不适用，因此使用命名函数表达式达成相同效果
```javascript
var fac = (function f(num){
  if(num <= 1){
    return 1;
  }else{
    return num * f(num-1);
  }
});
```

### 闭包
指有权访问另一个函数作用域中的变量的函数
```javascript
function createComparisonFunction(propertyName){
  return function(obj1,obj2){
    var val1 = obj1[propertyName]; //可以调用外部函数的变量
    var val2 = obj2[propertyName];
    
    if (val1<val2){
      reuturn -1;
    }else if(val1>val2){
      reuturn 1;
    }else{
      return 0;
    }
  };
}
```
* 重要概念，当函数**第一次**被调用时，会创建一个执行环境和相应的作用域链，并把作用域链赋值给一个内部属性`[[SCOPE]]`，然后使用`this`、`arguments`和其他命名参数的值初始化函数的活动对象。即**全局变量对象内存了函数，函数运行建立执行环境，内存作用域链，作用域链新建函数活动对象**
```javascript
function compare(val1,val2){
  ...
}
//对于这个函数，其运行时作用域链如图
```
![p179](http://ww2.sinaimg.cn/mw690/86444fb9gw1ewcgbwgkg2j20ey066aa7.jpg)

* 变量对象，每个执行环境都有一个表示变量的对象，全局环境的变量对象始终存在，函数的局部环境的变量对象只在执行过程中存在。在创建函数时，作用域链被保存在`[[SCOPE]]`中，调用时，先创建函数执行环境，后复制这个属性构建起作用域链。此后新创建的活动对象被推入作用域链顶端。作用域链本质上是指针列表，只有引用。**一般来说**，函数执行完后局部的活动对象会被销毁。
* 闭包，本质上就是阻止了外部函数的活动对象被销毁，下图为`createComparisonFunction`的作用域链关系图
![p180](http://ww2.sinaimg.cn/mw690/86444fb9jw1ewcgs2fi0vj20f60843yz.jpg)
* 闭包的副作用，外部函数的循环变量造成的问题
```javascript
// 想返回函数数组来着
fucntion createFunction(){
  var result = new Array();
  
  for(var i = 0;i<10 ;i++){
    result[i] = function(){
      return i;
    };
  }
  return result;
}
// 事实上 返回的每个函数执行结果都是10 因为i最后的值是10

// 解决方法，多定义一层匿名函数，将循环变量i的值保存在它的命名参数中
function createFunction(){
  var result = [];
  
  for(var i=0 ;i<10 ;i++){
    result[i] = function(num){ // 利用了函数参数按值传递的特性
      return function(){
        return num;
      };
    }
  }
}
```
### `this`对象
* `this`对象在**运行时**基于函数的执行环境绑定的
* 匿名函数的执行环境具有**全局性**
* 下面出现的情况同样适用于`arguments`
```javscript
var name = 'the window';

var object = {
  name : 'my obj',
  
  getNameFunc : function(){
    return function(){
      return this.name;
    };
  };
}

alert(object.getNameFunc()()); // the window 
// 匿名函数的this具有全局性，且根据作用域链只会搜索到本身的活动对象的this

// 改写

var object{
  ...
  getNameFunc : function(){
    var that = this;
    return function(){
      return that.name;
    };
  };
}

// my obj 
// 使用that变量保存this，返回的闭包根据作用域链找到了外部函数活动对象中的that
```
### 内存泄露
ie9根据引用数判断是否需要回收垃圾，闭包若引用了外部函数中的如`html`元素时，由于html元素不被销毁会导致闭包占用的内存不被回收，要注意用`null`来处理

### 模仿块级作用域

