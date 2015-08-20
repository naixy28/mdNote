#javascript高级语言程序设计笔记\_(:з」∠)\_
	  
## 第3章 Part One
---
### 数据类型
基本数据类型5种
* undefined
* boolean
* null
* number
* string
复杂数据类型
* object
```javascript
//null代表一个空的对象 而undefined是未定义
//建议使用null初始化新的对象，对于变量不必特意初始化为undefined
var a = null;
var b;
typeof b; //'undefined'
typeof a; //'object'
```
### Boolean
`Boolean()`对应转换表
<table >
<tr>
<td>Boolean</td>
<td>true</td>
<td>false</td>
</tr>
<tr>
<td>Stirng</td>
<td>非空字符串</td>
<td>""空串</td>
</tr>
<tr>
<td>Number</td>
<td>非零</td>
<td>0和NaN</td>
</tr>
<tr>
<td>Object</td>
<td>任何对象</td>
<td>null</td>
</tr>
<tr>
<td>undefined</td>
<td>不适用</td>
<td>undefined</td>
</tr>
</table>

### Number
* 进制  
```javascript
var num1 = 5 //十进制
var num2 = 070 //八进制，严格模式下不支持，零开头
var num3 = 0xA //十六进制
```
* `isNaN()`  
  * NaN与任何值不相等
  * 判断isNaN时，先尝试转为数值，Object先后尝试`valueOf()`和`toString()`
* 数值转换
有`Number()`,`parseInt()`,`parseFloat()`三个函数，推荐后两种  
  * `parseInt()`与`parseFloat()`都是从第一位开始解析适合转换成数字的串直到不能解析
  * `parseInt()`可以用第二个参数确定字符串的进制，建议使用  
    ```javascript
    var num = parseInt('10',2); //2
    ```
* `Number()`与`parseInt()`和`parseFloat()`的主要区别
	* 支持将`Boolean`类型转换为`0`或`1`，而不是`NaN`
	* 支持将`null`转换成`0`，而不是`NaN`
	* 支持将**空串**转换成`0`，而不是`NaN`    

所以`parse`系列更类似于只是解析字符串前缀是否包含数字，而`Number`会执行类型转换尽量将变量转换成数字
### String
* `string.length`属性返回的长度包含了转义字符的长度，可能大于目测
* `toString()`方法可带一个参数输出各种进制表示的字符串  
```javascript
var num = 10;
num.toString(16); //"a"
```
* `null`与`undeifined`不支持`toString()`  
因此在不确定的情况下，可以使用`String(value)`函数转换，区别在`toString()`方法基础上增加了对这两种类型的支持  
```javascript
var value = null;
var value2;
String(value); //'null'
String(value2); //'undefined'
```
### Object
每个Object都具有这些方法
* `constructor()`
* `hasOwnProperty(propertyName)` 必须传入字符串形式
* `isPrototypeOf(object)`
* `propertyEnumerable(propertyName)` 必须传入字符串形式
* `toString()`
* `valueOf()` 通常与`toString()`返回的相同，但类型不一定是`string`

---
### 操作符
在将操作符应用于`Object`时，往往会调用对象的`valueOf()`或`toString()`方法
#### 一元操作符
* `+`操作符可用于向`number`类型的转换，等同于`Number()`
* 有符号位移`>>`与无符号位移`>>>`的差别在于对负数的操作结果不同，有符号位移会将负数的二进制补码看作是正数，共通点是所有移入的位补零
* 逻辑与`&&`操作适用于各种数据类型而不只是`boolean`型，其操作为**短路**，不一定返回`boolean`
```javascript
object1 && object2; //object2
true && object; //object
false && sth; //false
null && sth; //null
undefined && sth; //undefined
```
* 逻辑或`||`与逻辑与类似，常用`var result = preferred || backup;`
#### 二元操作符
* 使用`==`或`!=`会执行类型转换，且存在一些特殊情况如`null == undeifined //true`
* 使用全等`===`或全不等`!==`不会转换类型
####条件操作符
* 循环内部定义的变量可以在循环外部访问，因为js中不存在块级作用域
* `for-in`语句用于枚举对象的属性，顺序不可预测
* `label`与`break`和`continue`联用，用于返回到特定的位置
```javascript
var num = 0;
outermost:
for(var i=0 ;i<10 ;i++){
	for(something){
		for(something){
			break outermost; //跳出到最外层
		}
	}
}
```
* `with`语句用于将代码的作用域设置到对象中，不建议使用
* `switch`的迷之用法，以及`switch`中使用的比较都是全等
```javascript
//比较了true与case中的值，
var num = 25;
switch(true){
	case num<0:
	...
	break;
	case num<10:
	...
	break;
}
```
### 函数
