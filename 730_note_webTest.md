## Web测试题自作
---
[TOC]
### 隐藏图片
测试了多种方法   

* 在`<img>`内使用`hidden`属性，事实证明只要有`hidden`，后面加不加任何属性都行
```html
    <img src="" hidden="hidden" />

    <img src="" hidden="true" /> <!--什么都行-->

    <img src="" hidden/>
```
* 使用样式`style="display:none"`
```html
	<img src="" style="display:none" />
```
### 简单的CSS字体样式
关于字体大小可使用`font-size`  
另外，使用`font-weight`控制粗细，就这点

### 图片层叠
使用`positon:absolute`，也就是绝对位置，深入下去并不了解
```css
	#pic1{
        position: absolute;
        top:50px;

    }
    #pic2{
        position: absolute;
        left:70px;
	}
```

### 下拉框打开窗口
> 页面上有一个下拉框，用javascript实现选中其中一个选项时，打开一个新窗口，并且打开页面的底色变成选中的颜色  
下拉框长这样
```html
<SELECT id="s1" onchange="s1_onchange(this)">
    <OPTION value="1">红色</OPTION>
    <OPTION value="2">蓝色</OPTION>
    <OPTION value="3">黄色</OPTION>
</SELECT>
```

用了最傻的方法
```javascript
	function s1_onchange(sel){
            var color;
            for(var i = 0;i<sel.length;i++){
                if(sel.options[i].selected){
                    color = sel.options[i].value;
                    break;
                }
            }
            //直接写一个html进去，这考试还是笔试，逗我
            switch (color){
                case "1":
                    window.open().document.write("<!DOCTYPE html> <html lang='en'> <head> <meta charset='UTF-8'> <title></title> </head><body bgcolor='red' ></body></html>");
                    break;
                case "2":
                    window.open().document.write("<!DOCTYPE html> <html lang='en'> <head> <meta charset='UTF-8'> <title></title> </head><body bgcolor='blue' ></body></html>");
                    break;
                case "3":
                    window.open().document.write("<!DOCTYPE html> <html lang='en'> <head> <meta charset='UTF-8'> <title></title> </head><body bgcolor='yellow' ></body></html>");
                    break;

            }
        }
```

### 使用js接受回车
> 请用javascript实现在输入框中输入城市名称后按回车键，在div中显示XXX欢迎你！如输入北京，显示北京欢迎你！，如果没有输入就回车，打开警告框提示“请输入城市名称”

form长这样
```html
<form name="testform" method="post">
    城市名称：<input id="cityid" name="city" type="text" value="请输入" onkeydown="show_city()">
</form>
```

测试下来最需要注意的是禁用enter键的自动提交表单，代码如下
```javascript
function show_city(){
        //var e = window.event || arguments.callee.caller.arguments[0];
        if (event.keyCode == 13 ) {
            event.returnValue=false;//不刷新界面 
            if(testform.cityid.value === "请输入" || testform.cityid.value === ""){
                alert("请输入城市名称");
            }else{
                content.innerText=testform.cityid.value+"欢迎你！";
            }
        }
    }
```
### 找到名称包含特定模式的option
> 用javascript实现找到页面中所有名称包含“student”的text框，将名称合并，并打开警告框提示“学生名单有：XXX，XXX，XX”

form长这样
```html
<form name="testform">
    <input type="text" name="student1" value="杰伦">
    <input type="text" name="student2" value="志玲">
    <input type="text" name="student33" value="霆锋">
</form>
```

笔试时异想天开直接用`getElementsByName`匹配正则看来不行  

所用的正则表达式
```javascript
/^.*student.*$/
```

今天电脑上做的时候才发现应该用`getElementsByTagName`,获取到之后用循环匹配`name`然后输出
```javascript
	var
        list = document.getElementsByTagName("input"),
        names = [];
    for(var i = 0;i<list.length;i++){
        if(list[i].name.search(/^.*student.*$/)===0){
            names.push(list[i].value);
        }
    }
    alert("学生名单有："+names.toString());
```

### 数组逆序排列
用高阶函数修改`sort()`方法
```javascript
var list = [2,3,1,5,6,4];
    alert(list.sort(function(x,y){
        if(x<y){
            return 1;
        }
        if(x>y){
            return -1;
        }
        return 0;
}));
```
---
虽说都能做出来，但是让我这个实习生只用笔做这些题真的有意思么=-=
