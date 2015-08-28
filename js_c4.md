#javascript高级语言程序设计笔记\_(:з」∠)\_
	  
## 第4章 
---
### 基本类型与引用类型
* 基本类型的复制是创建新值
<table>
<tr><th colspan="2">复制后的变量对象</th></tr>
<tr><td>num1</td><td>5(Number)</td></tr>
<tr><td>num2</td><td>5(Number)</td></tr>
</table>
* 引用类型的复制仍指向同一个对象
```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name); //"Nicholas"
```
* 所有函数的参数都是按值传递的
```javascript
function setName(obj){
	obj.name = 'Ni';
	obj = new Object();
	obj.name = 'Gr';
}

var person = new Object();
setName（person);
alert(person.name); //'Ni'  并不是很理解两种传递方式
```
* `instanceof`  
使用`instanceof`判断是什么类型的对象
```javascript
result = variable instanceof constructor;
```

### 执行环境与作用域  
执行环境有**全局环境**与**函数执行**环境两种
* 作用域链
每个环境都可以从链前端向上搜索，以查询变量与函数，形如
```
window
  |
  |--color
  |__changeColor()
        |
        |--anotherColor
        |__swapColors()
              |
              |--tempColor
             
```
* 延长作用域链
以下两种方法都会在作用域链的前端添加一个变量对象
	* `try-catch`的`catch`块
	新对象包含了被抛出的错误对象的声明
	* `with`语句
	可添加指定对象
	```javascript
	funciton buildUrl()}
		var qs = 'xxx';
		
		with(location){
			var url = href + qs; //location.href，而location已经被添加到了作用域链前端
		}
		
		return url;
	}
	```
* 没有块级作用域
`if`和`for`花括号中定义的变量可以被从外部访问，即它们没有独立的作用域

### 垃圾收集
两种策略
* 标记清除
变量进入环境后被标记为**进入环境**，此时不能回收直到被标记为**离开环境**
* 引用计数
计算引用类型被引用的次数，为**0**后才可回收，**不常用，会出现相互引用导致的循环引用问题，类似死锁**
* 性能提升
可以通过手工解除引用的方式，`obj = null`使对象在下一次垃圾回收时被回收  
也可手动执行垃圾回收方法
