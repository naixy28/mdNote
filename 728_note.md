## 过去一周内的汇总
---
[TOC]
### 计算器
:	使用html和js实现了基本四则运算，除此之外加入了括号的输入、clear、delete功能
    做了简陋的格式优化，如循环小数截断显示，大数以科学计数法表示（有待加强）
#### 1. 数字按钮的实现
普普通通的html部分
```html
		<td><input type="button" value="7" onclick="addnum('7')"></td>
```
平淡无奇的函数
```javascript
		var numMessg;
        function addnum(num){
            numMessg = document.getElementById("cal");
            numMessg.value += num; 
        }
```
#### 2. 运算符按钮的实现
取运算框最后的一个符号，并判断是否属于运算符号之一  
以此来避免重复输入运算符
```javascript
		function addOperator(op){
            numMessg = document.getElementById("cal");
            var temp=numMessg.value[numMessg.value.length-1];
            if ( [' ','+','-','*','/','.'].toString().indexOf(temp) == -1){
                numMessg.value += op;
            }else{
                //alert("Wrong input with " + op);
            }
        }
```
#### 3. 运算的实现
使用eval()函数，直接运行string内的内容
```javascript
		var temp = eval(numMessg.value);
```
#### 4. 截断与科学计数
简陋的不行 并没想再去优化它
```javascript
	/*若小于屏幕显示的最大长度的数字，则截断*/
		if(Math.abs(temp) <= 999999999999999){
                numMessg.value=temp.toString().substring(0,15);
                return;
    /*若大于最大的数字则科学计数，同时丢失精度，并不能继续计算*/
            }else{
                var len;
                var arr;
                len=temp.toString().length-1;
                arr = temp.toString().split('');
                arr[0]+='.';
                numMessg.value=arr.join('').substring(0,6)+'*10^'+len.toString();

            }
```
#### 5. 清屏与删除
* clear
```javascript
		document.getElementById("cal").value='';
```
* delete
本想把split、pop、join一起完成  
后发现String.pop()返回的是被pop出的字符便只能一步步写
	`pop() push()`是对array尾部的操作 对应对头部`shift() unshift()` 
```javascript
		function delLast(){
            var temp = document.getElementById("cal").value;
            temp = temp.split('')
            temp.pop();
            temp = temp.join('');
            numMessg.value = temp;
        }
```


### Frame练习
`<frameset></franmeset>`与`<frame></frame>`为包含关系  
`rows cols`代表横向安排或竖向安排`frame`可用百分比等方法规定大小
** * **为‘余下’
```html
	<frameset rows="20%,*" >
    <frame id="banner">
    <frameset cols="20%,*">
        <frame id="menu">
        <frame src="calculator.html" id="content">
    </frameset>
	</frameset>
```
frame标签中用`src`属性指向被包含的页面	
	<frame src="frame_exe2_p1.html">

### signin form
没什么特别的，使用js预先check  
了解到使用`string.search(正则)`判断是否匹配 `0`为匹配 `-1`反之
```javascript
	if(form1.username.value==''){
        alert("用户名必填！")
        return false;
    }else if(form1.username.value.search(/^([a-zA-Z](\d|[a-zA-Z])*)$/)!=0){
        alert("用户名格式不匹配！")
        return false;
    }
```

### radio
* 同一组单选框使用相同的name进行关联
* label通过`for="id"`属性与单选框进行关联
* label的文字可使用`label.innerText`获得

### checkbox
* 使用`name`属性分组，通过`getElementsByName("name")`获取数组，然后可用循环遍历判断`if(obj[i].checked)`是否被选中

### select&option
#### 1. 基本结构
```html
    <select name="fruit" id="fruit" onchange="">
        <option id="banana" value="banana">香蕉</option>
        <option id="apple" value="apple">苹果</option>
    </select>	
```
#### 2. 增加option
获取key与value
```javascript
	var val = document.getElementById("text_value").value;
    var txt = document.getElementById("text").value;
```
新建Option对象
```javascript
	var op = new Option(txt,val);
```
插入option
```javascript
	select.add(op);
```
#### 3. 通过index删除option
获取索引
```javascript
	var pos = document.getElementById("pos");/*pos为一个input对象*/
```
判断超出range
```javascript
	if( pos.value >= sel.options.length || pos.value < 0)
```
删除option
```javascript
	sel.options.remove(pos.value);
```
#### 字符串练习
* `charAt()`与`charCodeAt()`
前者返回指定位置的字符，后者返回unicode
* `indexOf()`与`lastIndexOf()`
* `concat()`
连接两个字符串
* `replace(a,b)`
将a以b来替代，只替代第一个
* `slice()`与`substring()`
差别在于，前者支持用负数
> slice(),第一个参数代表开始位置,第二个参数代表结束位置的下一个位置,截取出来的字符串的长度为第二个参数与第一个参数之间的差;若参数值为负数,则将该值加上字符串长度后转为正值;若第一个参数等于大于第二个参数,则返回空字符串.
> substring(),第一个参数代表开始位置,第二个参数代表结束位置的下一个位置;若参数值为负数,则将该值转为0;两个参数中,取较小值作为开始位置,截取出来的字符串的长度为较大值与较小值之间的差.
substr(),第一个参数代表开始位置,第二个参数代表截取的长度.
* `substr()`
第一个参数为起始位置，后一个参数为截取长度
* `split(',',num)`
以第一个参数中的符号为分割点，取前num个（可不写）
