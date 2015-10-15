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
