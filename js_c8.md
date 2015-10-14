#javascript高级语言程序设计笔记

## 第8章 
---
###`window`对象
* BOM的核心对象，是浏览器的实例。既是js访问浏览器的接口，又是`Global`对象
* `window`对象拥有全局作用域
```javascript
var age = 39;
alert(window.age); // 39
```
* 直接定义在`window`上的属性与定义全局变量的差别，在于全局变量不能通过`delete`删除

#### 框架
* 访问框架的方法
```javascript
window.frames[0];
window.frames['frameName'];
top.frames[0];
top.frames['frameName'];
frames[0];
frames['frameName'];

// top 对象始终指向最高最外层的框架，也就是浏览器窗口
```
#### 导航和打开窗口
* `window.open()`接受可选的四个参数，url、窗口目标、特性字符串、新页面是否取代历史记录中的当前加载页面的boolean值
```javascript
//第二个参数
window.open('http://www.xxx.com','topFrame'); //以topFrame作为打开新窗口的对象，除了窗口或框架的名称外，还可以使用下列特殊字符串_self,_blank,_parent,_top

//第三个参数
//p200 参照表
window.open('http://www.xxx.com','_blank','height=500,width=500,top=10,location=false');

```

* 关闭窗口，使用`windowName.close()`，只适用于通过window.open()打开的窗口
* `opener`属性，保存着打开它的原始窗口对象，在多进程浏览器中，`opener`属性的解绑可以使两个窗口不再运行在一个进程中
```javascript
var wroWin = window.open('http://www.xxx.com','wroxWindow','height=400,weight=400');
worWin.opener = null; //解除绑定

```

#### 间歇调用与超时调用
* 通过设置间歇值或超时值调度代码在特定时间运行
```javascript
setTimeout('alert()',1000); //1000ms后调用alert，但不建议用字符串，影响效率

setTimeout(function(){
	alert('Hello');
},1000); //建议传入函数

```
* 需要注意，经过该时间后不一定立刻执行。原因在于js是单线程序的解释器，设置超时调用只是在特定时间后将这段代码加入**任务队列**，需要排队等待
```javascript
//取消超时调用
var timeoutId = setTimeout(function(){},1000);
clearTimeout(timeoutId);
```
* 间歇调用
```javascript
//不推荐传入字符串
setInterval('alert()',1000); // 每秒alert一次
//推荐的方式
setInterval(function(){
	alert();
},1000);

//必须要记得取消间歇调用
var num = 0;
var max = 10;
var intervalId = null;

function incrementNumber(){
	num++;
	if(num == max){
	clearInterval(intervalId);
	alert('done');
	}
}

intervalId = setInterval(incrementNumber,500);

```
* 建议使用超时调度模仿间歇调度
```javascript
var num = 0;
var max = 10;

function incrementNumber(){
	num++;

	if(num<max){
		setTimeout(incrementNumber,500);
	}else{
		alert('done');
	}
}

setTimeout(incrementNumber,500); //一般认为用超时模拟间歇是最佳模式

```

#### 系统对话框
* 也就是alert(),confirm(),prompt()
```javascript
//comfirm返回的是布尔值
if(comfirm()){
	...
}else{
	...
}

//prompt返回的是字符串或null
var result = prompt('what is your name?',''); //第二个参数是默认值
if(result !== null ){
	alert(result);
}

```

* 查找和打印，异步不影响操作
```javascript
window.print();
window.find();
```

### `location`对象
