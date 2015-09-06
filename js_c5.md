#javascript高级语言程序设计笔记\_(:з」∠)\_

## 第5章 
---
### `Object`类型
创建`Object`实例的两种方式
* `new`操作符后跟`Object`构造函数
```javascript
var person = new Object();
person.name = 'aaa';
person.age = 29;
```
* 对象字面量表示法  **不会调用构造函数**
```javscript
var person = {
  name:'aaa',
  age:'20',
  5:true
};
//person.name  person['name']
//person.age  person['age']
//person[5]
```

### `Array`类型
* 通过length属性增加项 **length**属性是动态变化的
```javascript
var colors = [ 'red' , 'blue' , 'yellow' ];
colors[colors.length] = 'black' ; //末尾增加了颜色
```
* `array`的长度由插入过的最大`index`决定，空出来的默认`undefined`
* 检测数组不要使用instanceof操作符，假如存在多个全局执行环境，则可能存在多个版本的Array构造函数
```javascript
if( Array.isArray(value) ){
  //使用这个方法
}
```
* `toLocaleString()`,`toString()`   
调用每个元素的相应的方法，然后用逗号连接形成字符串
* `valueOf()`
返回的还是数组
* 拼接`join()`   
```javascript
colors.join(',') //用','连接各个数组中的值toString后的结果
```
* 栈方法
```javascript
colors.push('a','b') //接受任意数量参数，逐个添加到尾部
colors.pop() //移除最后一项
//另有shift(),unshift()方法，对头部进行相同的操作，可实现队列
```
* 排序方法
```javascript
var values = [0 , 1 , 5 , 10 , 15];
values.sort(); // 0,1,10,15,5

//sort()方法比较的是toString后的字符串，因此通常要接受一个函数修改sort方法
//如果第一个参数应位于第二个之前则发挥负数，反之正数，相等返回0
function compare(x,y){
  if(x<y) return -1;
  if(x>y) return 1;
  return 0;
}
values.sort(compare); // 0,1,5,10,15
```
* `concat()`连接数组，**不修改原数组**
```javascript
arr = arr1.concat(arr2,arr3,item1,item2); // 可传入多个数组非数组，全都提取出元素追加到原数组的副本后
```
* `slice()`方法，基于当前数组创建新数组，可接受一到两个参数，分别为起始位置和结束位置的后一个位置
```javascript
var colors = ['red','green','blue','yellow','purple'];
var colors2 = colors.slice(1); //green,blue,yellow,purple
var colors3 = colors.slice(1,4); //green,blue,yellow
//slice()方法支持负数
```
* `splice()`方法，三部分参数：第一个参数：要删除的项的起始位置；第二个参数：要删除的项数；第三个参数开始：要在起始位置插入的项。
```javascript
var colors = ['red','green','blue'];
var removed = colors.splice(0,1); //删除第0项
alert(removed); //red
alert(colors); //green,blue

removed = colors.splice(1,0,'yellow','orange'); //从位置1开始插入项，不删除
alert(colors); //green,yellow,orange,blue
alert(removed); //空数组

removed = colors.splice(1,1,'red','purple'); //从位置1删除一项，再插入两项
alert(colors); //green,red,purple,orange,blue
alert(removed); //yellow
```
* 位置方法`indexOf()`与`lastIndexOf()`，接受两个参数，第一个为查找的项，第二个（可选）为起始的位置，两个函数方向相反，找不到则返回`-1`
```javascript
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.indexOf(4); //3;
numbers.indexOf(4,4); //5
numbers.lastIndexOf(4); //5
numbers.lastIndexOf(4,4); //3
numbers.indexOf(6); //-1
```
* 迭代方法
  1. `every()`，对每一项运行给定函数，都为`true`则结果为`true`，类似与运算
  2. `some()`，类似或运算
  3. `filter()`，返回结果为`true`组成的数组
  4. `forEach()`，无返回值，遍历
  5. `map()`，对每项执行给定函数，返回结果组成的数组

**回调函数的参数**
```javascript
function(item,index,array){}
```
* 缩小方法
  1. `reduce()`，迭代前两项，返回一个结果，取其与下一个项继续迭代,缩小规模到1为止
  2. `reduceRight()`，反向

**回调函数的参数**
```javascript
function(prev,cur,index,array){}
```
### `Date`类型
```javascript
var now = new Date(); //获取现在时间
```
* 创建指定的时间,`Date.parse()`
```javascript
var someDate = new Date(Date.parse('May 25,2004')); 
//或
var someDate = new Date('May 25,2004');
```
`Date.parse()`接受的日期格式
  * 6/13/2004
  * May 12,2005
  * Tue May 25 2004 00:00:00 GMT-0700
  * 2004-05-25T00:00:00
* 创建指定的时间,`Date.UTC()`，**月份从0开始**，参数中年月必须
```javascript
var y2k = new Date(Date.UTC(2000,0)); //GMT时间2000年1月0时
var allFives = new Date(Date.UTC(2005 , 4 ,5 ,17 ,55 ,55)); //GMT时间2005年5月5日下午5点55分55秒
```
### `RegExp`类型
* 三种模式
  1. g：全局模式，不会在匹配到第一次之后停止匹配
  2. i：不区分大小写
  3. m：多行模式，到一行末尾后继续查找
* 定义正则表达式的两种方法
  1. 字面量形式
  ```javascript
  var pattern  = /[bc]at/i
  ```
  2. 构造函数
  ```javscript
  var pattern = new RegExp('[bc]at','i')
  ```
  使用构造函数时有时会需要**双重转义**，将字符串转义成字面量形式  
  字面量形式**始终共享同一个RegExp实例**，构造函数形式每次`new`都会新建实例
* 实例属性
  <table>
  <tr><th>属性名</th><th>含义</th></tr>
  <tr><td>global</td><td>布尔类型，是否设置了g标志</td></tr>
<tr><td>ignoreCase</td><td>同上，i标志</td></tr>
<tr><td>multiline</td><td>同上，m标志</td></tr>
<tr><td>lasrIndex</td><td>开始搜索下一个匹配项的起始位置</td></tr>
<tr><td>source</td><td>按照字面量形式输出正则的字符串</td></tr>
  </table>
* `exec()`方法  
  * 专门为捕获组设计，返回**包含第一个匹配项的数组或null**，包含额外的index和input属性，分别表示匹配项的位置和被匹配的字符串  
  * 数组的第一项是与整个模式匹配的字符串，其他项是与捕获组匹配的项
  * 设置了`g`模式后，**==多次调用exec()会**继续查找新匹配项**
  ```javascript
  var text = "cat,bat,sat,fat";
  var pattern = /.at/g;
  var matches = pattern.exec(text);
  alert(matches.index); //0
  alert(matches[0]); //cat
  ```
* `test()`方法
  返回**布尔类型**，判断是否存在匹配
* 构造函数属性
  直接用`RegExp.property`调用，每个属性都有长属性明与对应的短属性名

### `Function`类型
* 函数名本质上是指向函数对象的指针，不会与函数绑定
```javascript
  //函数声明方式定义函数
  function sum(num1.num2){
    return num1+num2;
  }
  //函数表达式方式定义函数
  var sum = function(num1,num2){
    return num1+num2;
  }
  //以上两种方式几乎无差别
```
* **没有重载**，函数也是一种类型，先后声明同名函数只会**覆盖**原函数
* 函数声明会在代码开始执行前被**提升**，并添加到执行环境中
```javascript
  alert(sum(10,10)); //可以运行
  function sum(num1,num2){
    return num1+num2;
  }
```
* 当函数位于一个初始化语句中，而不是一个函数声明中时，只会提升变量的初始化，函数未被声明
```javascript
  alert(sum(10,10)); //可以运行
  var sum = function(num1,num2){
    return num1+num2;
  }
```
* 函数可以作为参数来传递和返回，也可以嵌套
* 函数内部的属性`callee,caller,this`（前两种在严格模式下不可用）  
  `callee`指向拥有`arguments`的函数，是**arguments的属性**
```javascript
    function factorial(num){
      if (num<=1){
       return 1;
      }else{
        return num * arguments.callee(num-1); //消除了阶乘函数对函数名的依赖
      }
    }
```
  `caller`指向调用当前函数的函数，是**函数的属性**
```javascript
function outer(){
  inner();
}
function inner(){
  alert(inner.caller);
}

outer(); //显示outer的源代码
```
  `this`指向函数运行时的环境对象，浏览器内全局时则指向`window`
* 函数的`length`与`prototype`
  `length`指希望接受的命名参数个数，`prototype`保存了实例方法的真正所在
* `call()`与`apply()`
  用于设置函数体内的`this`对象的值，区别在于`call`要逐个列举参数，`apply`接收参数组成的数组或`arguments`实例
```javascript
function sum(num1,num2){
  return num1+num2;
}
function callSum(num1,num2){
  return sum.apply(this,arguments); 
  //或
  return sum.apply(this, [num1,num2]);
  //或
  return sum.call(this,num1,num2);
}
```
* `bind()`方法
  建立一个函数实例，其`this`值被绑定到传给`bind()`函数的值
```javascript
window.color = 'red';
var o = {color:'blue'};

function sayColor(){
  alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor(); //blue
```
### 基本包装类型
#### 三个基本包装类型，`Boolean`，`Number`，`String`
存在的意义是弥补基本类型没有方法，可用基本类型直接调用，其后台有一系列自动化的处理
```javascript
var s1 = 'some';
var s2 = s1.substring(2);
```
在第二行代码访问`s1`时，访问过程处于一种读取模式，此时后台的处理如下
1. 创建`String`类型的一个实例
2. 在实例上调用指定方法
3. 销毁实例  

可见包装类型生存周期与一般的引用类型的差别，包装类型**只存在于代码执行的瞬间**，因此不能为其添加属性

* `Boolean`类型
  不建议使用
* `Number`类型
<table>
<tr><th>格式化方法名</th><th>作用</th></tr>
<tr><td>toString()</td><td>转换成string，可设置进制</td></tr>
<tr><td>toFixed()</td><td>保留指定小数位数，自动舍入</td></tr>
<tr><td>toExponential()</td><td>科学计数法，可设置保留小数点位数</td></tr>
<tr><td>toPrecision()</td><td>设置所有数字的位数，自动选择转换方式</td></tr>
</table>
* `String`类型
<table>
<tr><th>方法名</th><th>作用</th></tr>
<tr><td>charAt()</td><td>返回特定位置的单字符</td></tr>
<tr><td>charCodeAt()</td><td>返回特定位置的编码</td></tr>
<tr><td>slice()</td><td>起始位置，结尾后一位置，支持负数</td></tr>
<tr><td>substring()</td><td>起始位置，结尾后一位置，负数置0</td></tr>
<tr><td>substr()</td><td>起始位置，截取长度，长度不支持负数，位置支持负数</td></tr>
<tr><td>indexOf()</td><td>返回字符串索引位置，第二个参数设置起始位置</td></tr>
<tr><td>lastIndexOf()</td><td>反向返回字符串索引位置，第二个参数设置起始位置</td></tr>
<tr><td>trim()</td><td>去掉字符串前后空格，另有trimLeft(),trimRight()</td></tr>
<tr><td>toLowerCase()</td><td>不解释系列</td></tr>
<tr><td>exec()</td><td>模式匹配，string.exec(pattern)，本质上与RegExp的方法相同</td></tr>
<tr><td>search()</td><td>模式匹配，找位置</td></tr>
<tr><td>replace()</td><td>玩法多样，支持正则，由第二个参数代替第一个参数</td></tr>
<tr><td>split()</td><td>分割字符串为array，支持正则</td></tr>
<tr><td>localeCompare()</td><td>比较字符串，返回-1,0,1</td></tr>
<tr><td>fromCharCode()</td><td>与charCodeAt相反</td></tr>
</table>

### `Global`对象
一切对象的源头，全局对象的归属，如`isNaN(),inFinite(),parseInt()`  
* URI编码方法`encodeURI`与`encodeURIComponent()`，发送给浏览器以其可以理解的编码
  前者用于将整个URI进行编码，后者用于编码部分URI，后者将原本属于URI的特殊字符一并处理，更常用
* 对应存在`decodeURI()`与`decodeURIComponent()`
* `eval()`方法，执行内部字符串，且带入其运行环境，严格模式下不支持这一点
* `Global`对象是`window`对象的一部分

### `Math`对象
* `min()`和`max()`
```javascript
var max = Math.max(3,4,5); //5

//特殊技巧，用max对array进行操作，利用apply可接受参数数组的特性
var values = [1,2,3];
var max = Math.max.apply(Math,values);
```
* 舍入方法ceil(),floor(),round()
* random()方法及通用格式
```javascript
Math.floor(Math.random()*choices+minValue);
```
其他方法不再列举
