#javascript高级语言程序设计笔记

## 第10章 
---
在HTML中，一个文档只有一个文档元素`<html>`，每一段标记都可以通过树中的一个节点来表示，共有12种节点类型，都继承自一个基类
### `Node`类型
* DOM1级定义了`node`接口，所有节点类型都继承自`node`类型
* 每个节点都有`nodeType`属性，十二种属性对应1-12，不是所有浏览器都支持属性的名称但都支持数字
```javascript
if(someNode.nodeType == Node.ELEMENT_NODE){
	//是element node
}

if(someNode.nodeType == 1){
	//与上等价
}
```
* `nodeName`和`nodeValue`属性，对于`element node`，`nodeName`记录了标签名，`nodeValue`为`null`

#### 节点关系
* 众所周知的parent与children的关系，`childNodes`属性返回一个`NodeList`对象，并不是Array的实例，其内容是根据DOM结构变化而动态变化的
```javascript
//一些基本的操作，前两者功能相同
var friendChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var count = someNode.childNodes.length;

//将NodeList转为Array的方法，即将类数组对象转Array的通用方法
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
```
* 其他的属性如：`parentNode firstChild lastChild nextSibling previousSibling`
* 一些方法和属性：`hasChildNodes()`用于判断是否有子节点，`ownerDocument`是所有节点的最后一个属性，直接指向文档节点

#### 操作节点
* 列举一些方法，都懂的
```javascript
//appendChild()，在尾部添加节点
var returnedNode = someNode.appendChild(newNode);
alert(returnedNode == newNode); //true
alert(someNode.lastChild == newNode); //true
//还可以用于移动节点位置
someNode.appendChild(someNode.firstChild); //将第一个子节点移动到最后

//insertBefore()
returnedNode = someNode.insertBefore(newNode,null); //插入到最后
returnedNode = someNode.insertBefore(newNode,someNode.firstChild); //插入到最前面

//替换节点replaceChild()
var returnedNode = someNode.repalceChild(newNode,someNode.firstChild);

//移除节点removeChild()
var formerFirstChild = someNode.removeChild(someNode.fitstChild);

//复制节点cloneNode()
var deepList = myList.cloneNode(true); //深复制，复制整个子树
var shallowList = myList.cloneNode(false); //浅复制，只复制自己

//normalize() 合并子树中的空文本节点

```

### `Document`类型
* document对象是`HTMLDocument`的一个实例（继承自Document类型），表示整个html页面，是window对象的唯一属性，nodeType为9，nodeName为`#document`
```javascript
//一些属性方法
document.documentElement; //<html>的引用
document.body;
document.doctype;

document.title; //标题

//网页请求相关
document.URL; //返回url
document.domain; //返回域名，能稍作设置
document.referrer; //返回可能存在的之前的页面地址

//查找元素的方法
document.getElementById();
document.getElementsByTagName();
document.getElementsByName();

//特殊集合
document.anchors; //<a>的集合
document.applets; //<applet>的集合
document.forms; //<form>的集合
document.images; //<img>的集合
document.links; //<a href='...'>的集合

//一致性检测，很多特性
document.implenmentation.hasFeature('XML','1.0');

//文档写入
document.write(); //文档加载时使用没问题，加载完使用会使文档重写
document.writeln();
document.open(); //用于在文档加载完打开关闭网页输出流
document.close();

```

### `Element`类型
* 最常用的类型
	* nodeType为1
	* nodeName为标签名
	* nodeValue为null
	* parentNode可能是Document或Element
	* 子节点可能是各种
* 使用`tagNeme`测试的时候注意都是大写，使用toLowerCase()
* HTML元素，由`HTMLElemrnt`类型或其子类型表示，有标准属性：`id title lang dir className`
* `HTMLElemrnt`类型或其子类型表示，有标准属性：子类型，也就是正常用的标签
```javascript
//操作特性的方法
div.getAttribute('id'); //对任何属性可用
div.id; //非自定义的属性可以这样调用

div.setAttribute('id','something');
div.id = 'something';

div.removeAttribute('class');

//创建元素
document.createElement('div');
```

* 元素的子节点
```javascript
//对于下面代码
<ul id ='myList'>
	<li>item</li>
	<li>item</li>
	<li>item</li>
</ul>

//对ie来说，childNodes有三个，对其他浏览器，有7个元素，其中有4个空白的文本节点表示文件之间的空白符

//判断节点类型，不操作文本节点
for(var i =0 , len = element.childNodes.length ; i< len; i++){
	if(element.childNodes[i].nodeType == 1){
		//执行某操作
	}
}

//还可以
ul.getElementsByTagName('li'); //返回的可能不只是直接子元素
```

### `Text`类型
