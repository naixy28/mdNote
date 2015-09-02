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
