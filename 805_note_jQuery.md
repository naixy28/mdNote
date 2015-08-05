## jQuery少许笔记

本文内容基于廖先生的[教程](http://www.liaoxuefeng.com/)

---

[TOC]

[jQuery下载](http://jquery.com/download/)

### 使用jQuery
在`<head>`内引入jQuery文件
```javascript
<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
```

### 关于$符号
`$`等于全局变量`jQuery`，本质上是函数。冲突时可用`jQuery.noConflict();`解除对`$`的绑定

### 选择器
* 重要概念，用于快速定位DOM节点，一般形式为
`$('#dom-id')`
* 返回对象为jquery对象，形如
`[<div id="abc">...</div>]`或空`[]`
* jQuery对象与DOM的转化
```javascript
var div = $('#abc'); // jQuery对象
var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
var another = $(divDom); // 重新把DOM包装为jQuery对象
```
#### 基本格式
```javascript
//按tag查找
var ps = $('p');

//按class查找
var a = $('.red');
//同时包含多个class，不能有空格
var a = $('.red.green');

//按属性查找
var email = $('[name=email]');
var a = $('[items="A B"]');//属性中有特殊字符时用双引号括起
var icons = $('[name^=icon]'); //按前缀查找，以icon开头
var names = $('[name$=with]'); //按后缀查找，以with结尾，类似regexp

//组合查找，即把上面的方法合并
$('input[name=email]')
$('tr.red')

//多项选择，使用逗号分隔
$('p,div'); // 把<p>和<div>都选出来
$('p.red,p.green'); // 把<p class="red">和<p class="green">都选出来

```
小总结一下，在选择器的语法中，`,`代表了**或**，`noSpace`代表了**与**，`space`代表了**前后的从属关系**，即下文中的**层级选择器**

#### 层级选择器
* 基本的层级选择器形如`$('ancestor descendant')`
如对于下面的html
```html
<div class="testing">
    <ul class="lang">
        <li class="lang-javascript">JavaScript</li>
        <li class="lang-python">Python</li>
        <li class="lang-lua">Lua</li>
    </ul>
</div>
```
要选出**JavaScript**，可用层级选择器
```javascript
$('ul.lang li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
$('div.testing li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
```
`空格`是跨代的，即可以隔代寻找匹配
* 子选择器
形如`$('parent>child');`，特点是规定了前后必须是父子关系，不许隔代
* 过滤器
有单独的函数`filter()`供使用，但主要用在选择器中
```javascript
$('ul.lang li'); // 选出JavaScript、Python和Lua 3个节点

$('ul.lang li:first-child'); // 仅选出JavaScript
$('ul.lang li:last-child'); // 仅选出Lua
$('ul.lang li:nth-child(2)'); // 选出第N个元素，N从1开始
$('ul.lang li:nth-child(even)'); // 选出序号为偶数的元素
$('ul.lang li:nth-child(odd)'); // 选出序号为奇数的元素
```
* 表单相关的选择器
	* `:input`:可选择`<input>`，`<textarea>`，`<select>`和`<button>`
	* `:file`:选择`<input type="file">`，等同于`input[type=file]`
	* `:checkbox`:等同于`input[type=checkbox]`
	* `:radio`
	* `:focus`:可以选择当前输入焦点元素`$('input:focus')`
	* `:checked`:选择勾上的单复选框`$('input[type=radio]:checked')`，很有用
	* `:enabled`:未被禁用的`<input>`和`<select>`
	* `:disabled`:类似上面
	* `:visible`
	* `:hidden`

### 查找与过滤
基于定位到的元素，可以进一步查找和过滤
#### 查找
使用`find()`方法，以调用该方法的对象为基准继续定位其子元素
```html
<ul class="lang">
    <li class="js dy">JavaScript</li>
    <li class="dy">Python</li>
    <li id="swift">Swift</li>
    <li class="dy">Scheme</li>
    <li name="haskell">Haskell</li>
</ul>
```
使用`find()`查找
```jsvascript
var ul = $('ul.lang'); // 获得<ul>
var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
var swf = ul.find('#swift'); // 获得Swift
var hsk = ul.find('[name=haskell]'); // 获得Haskell
```
若要从当前节点向上查找，使用`parent()`方法
```javascript
var swf = $('#swift'); // 获得Swift
var parent = swf.parent(); // 获得Swift的上层节点<ul>
var a = swf.parent('div.red'); // 从Swift的父节点开始向上查找，直到找到某个符合条件的节点并返回
```
同层节点可使用`next()`和`prev()`方法
```javascript
var swift = $('#swift');

swift.next(); // Scheme
swift.next('[name=haskell]'); // Haskell，因为Haskell是后续第一个符合选择器条件的节点

swift.prev(); // Python
swift.prev('.js'); // JavaScript，因为JavaScript是往前第一个符合选择器条件的节点
```
#### 过滤
* `filter()`方法过滤节点
```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var a = langs.filter('.dy'); // 拿到JavaScript, Python, Scheme
```
甚至可以传入函数，此处`this`被绑定为DOM对象
```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
langs.filter(function () {
    return this.innerHTML.indexOf('S') === 0; // 返回S开头的节点
}); // 拿到Swift, Scheme
```
* `map()`方法把jQuery对象中的多个DOM节点转化为其他对象,这个get()我是还没看懂..
```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var arr = langs.map(function () {
    return this.innerHTML;
}).get(); // 用get()拿到包含string的Array：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
```
* `first()`,`last()`,`slice()`方法可返回新的jQ对象，不修改原对象
```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var js = langs.first(); // JavaScript，相当于$('ul.lang li:first-child')
var haskell = langs.last(); // Haskell, 相当于$('ul.lang li:last-child')
var sub = langs.slice(2, 4); // Swift, Scheme, 参数和数组的slice()方法一致
```

### 操作DOM
#### 修改text与html
使用`text()`和`html()`方法，若不输入参数则分别获取节点文本和原始html文本，
若输入参数即可直接修改

**一个jQuery对象可以包含0个或任意个DOM对象，它的方法实际上会作用在对应的每个DOM节点上**，所以即使选择器没有返回任何DOM节点，调用jQuery对象的方法仍然**不会报错**
#### 修改CSS
使用`css('name','value')`方法即可

jQuery对象的所有方法都返回一个jQuery对象（可能是新的也可能是自身），这样我们可以进行链式调用，非常方便
```javascript
$('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
```

神奇的api可以灵活使用
```javascript
var div = $('#test-div');
div.css('color'); // '#000033', 获取CSS属性
div.css('color', '#336699'); // 设置CSS属性
div.css('color', ''); // 清除CSS属性
```
为了和JavaScript保持一致，CSS属性可以用`'background-color'`和`'backgroundColor'`两种格式。

修改`class`的方法
```javascript
var div = $('#test-div');
div.hasClass('highlight'); // false， class是否包含highlight
div.addClass('highlight'); // 添加highlight这个class
div.removeClass('highlight'); // 删除highlight这个class
```

#### 显示和隐藏DOM
显示和隐藏DOM元素使用非常普遍，jQuery直接提供show()和hide()方法，我们不用关心它是如何修改display属性的，总之它能正常工作
```javascript
var a = $('a[target=_blank]');
a.hide(); // 隐藏
a.show(); // 显示
```
注意，隐藏DOM节点并未改变DOM树的结构，它只影响DOM节点的显示。这和删除DOM节点是不同的。
#### 获取DOM信息
```javascript
// 浏览器可视窗口大小:
$(window).width(); // 800
$(window).height(); // 600

// HTML文档大小:
$(document).width(); // 800
$(document).height(); // 3500

// 某个div的大小:
var div = $('#test-div');
div.width(); // 600
div.height(); // 300
div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
```
`attr()`和`removeAttr()`方法用于操作DOM节点的属性
```javascript
// <div id="test-div" name="Test" start="1">...</div>
var div = $('#test-div');
div.hasAttr('name'); // true
div.attr('name'); // 'Test'
div.attr('name', 'Hello'); // div的name属性变为'Hello'
div.removeAttr('name'); // 删除name属性
div.attr('name'); // undefined
```
`prop()`方法和`attr()`类似，但是HTML5规定有一种属性在DOM节点中可以没有值，只有出现与不出现两种，如checked
```javascript
var radio = $('#test-radio');
radio.attr('checked'); // 'checked'
radio.prop('checked'); // true
```
`prop()`返回值更合理一些。不过，用`is()`方法判断更好
```javascript
var radio = $('#test-radio');
radio.is(':checked'); // true
```
类似的属性还有selected

#### 操作表单
jQuery对象统一提供`val()`方法获取和设置对应的`value`属性

### 修改DOM结构
#### 添加DOM
使用`append()`方法
```javascript
ul.append('<li><span>Haskell</span></li>');
```
还可以传入原始的DOM对象，jQ对象和函数对象
```javascript
// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);

// 添加jQuery对象:
ul.append($('#scheme'));

// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});
```
传入函数时，要求返回一个字符串、DOM对象或者jQuery对象。因为jQuery的`append()`可能作用于一组DOM节点，只有传入函数才能针对每个DOM生成不同的子节点

`append()`把DOM添加到最后，`prepend()`则把DOM添加到最前

另外注意，如果要添加的DOM节点已经存在于HTML文档中，它会首先从文档移除，然后再添加，也就是说，用`append()`，你可以移动一个DOM节点

同级节点可以用after()或者before()方法
```javascript
var js = $('#test-div>ul>li:first-child');
js.after('<li><span>Lua</span></li>');
```

#### 删除节点
要删除DOM节点，拿到jQuery对象后直接调用remove()方法就可以了。如果jQuery对象包含若干DOM节点，实际上可以一次删除多个DOM节点
```javascript
var li = $('#test-div>ul>li');
li.remove(); // 所有<li>全被删除
```

#### 练习
除了列出的3种语言外，请再添加Pascal、Lua和Ruby，然后按字母顺序排序节点
```html
<!-- HTML结构 -->
<div id="test-div">
    <ul>
        <li><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>
```
代码如下
```javascript
var ul = $("#test-div ul");
//用map和sort，从而省略了dumb的循环
['Pascal','Lua','Ruby'].map(function(x){ul.append("<li><span>"+x+"</spam></li>");});
var li = ul.find('li');
//可以注意到，传入的是DOM对象
li.sort(function(x,y){
if($(x).text()>$(y).text()){return 1;}
else{return -1;}
})
ul.append(li);
```
---
