#javascript高级语言程序设计笔记\_(:з」∠)\_
	  
## 第3章 Part One
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
* 数制转换
有`Number()`,`parseInt()`,`parseFloat()`三个函数，推荐后两种  
  * `parseInt()`与`parseFloat()`都是从第一位开始解析适合转换成数字的串直到不能解析
  * `parseInt()`可以用第二个参数确定字符串的进制，建议使用  
    ```javascript
    var num = parseInt('10',2); //2
    ```
### String
