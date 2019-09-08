## JS笔记
### 判断对象是否为数组

```javascript
var friends = new Array();
friends[0] = "Alice";
friends[1] = "Bob";
// 判断是否支持该方法
if (Array.isArray) {
    if(Array.isArray(friends)) {
        document.write("该对象是一个数组。");
    }
}
// 或者
if (friends instanceof Array) {
    document.write("该对象是一个数组。");
}

```

### 注释符号验证浏览器是否支持 JavaScript 脚本功能

```javascript
<script>
<!--
document.write("您的浏览器支持JavaScript脚本!");
//-->
</script>
```

> 注意：注释行结尾处的两条斜杠 `//` 是 JavaScript 注释符号。这可以避免 JavaScript 执行`-->`标签。

### `let`的使用
> 作用域规则：`let` 声明的变量只在其声明的块或子块中可用，这一点，与`var`相似。二者之间最主要的区别在于`var`声明的变量的作用域是**整个封闭函数**。

```javascript
function varTest() {
    var x = 1;
    if (true) {
        var x = 2;       // 同一个变量
        console.log(x);  // 2
    }
    console.log(x);  // 2
}
function letTest() {
    let x = 1;
    if (true) {
        let x = 2;       // 不同的变量    
        console.log(x);  // 2  
    }
    console.log(x);  // 1
}
```
> 未声明而直接赋值的变量，将作为window的一个属性
```javascript
// num1为全局变量，num2为window的一个属性
var num1 = 1;
num2 = 2;
// delete num1;  无法删除
// delete num2;  可以删除
function model(){
    var num1 = 1; // 本地变量
    num2 = 2;     // window的属性
    // 匿名函数
    (function(){
        var num = 1; // 本地变量
        num1 = 2; // 继承作用域（闭包）
        num3 = 3; // window的属性
    }())
}
```

### 函数定义

> JavaScript 没有重载这个概念，它仅依据**函数名**来区分函数。后定义的同名函数覆盖之前的，与参数无关。

```javascript
function test() {
    console.log("test");
}
test();     //输出 "test arg0 + undefined"，因为下面这个同名函数把上面这个函数覆盖了

function test(arg1) {
    console.log("test arg" + arguments.length + " + " + arg1);
}
test(1,2);  //输出 "test arg2 + 1"
```

> 实参个数如果比形参少，那么剩下的默认赋值为 **undefined**，如果实参传的比形参数量多，那么是全部都会被传进去的，只不过没有对应的形参可以引用（但可以用 arguments 来获取剩下的参数）。

```javascript
function test(arg1) {
    for(var i = 0; i < arguments.length; i++) {
        console.log(arguments[i]);
    }
}
test(1,2); //输出 1 2
```

> **变量与函数重名的时候，变量生效**（变量提升）

这涉及到了变量和函数的预解析：

- 变量声明会被顶置，函数声明也会被顶置且比变量更先声明。
- 变量的声明和赋值语句一起写时，JS引擎在解析时，会将其拆成声明和赋值两部分，声明置顶，赋值保留在原来位置。
- 声明过的变量不会再重复声明。

```javascript
var a = 100;
function a() {
    return "function";
}
console.log(a);     //输出 100
console.log(a());   
/*
报错
Uncaught TypeError: a is not a function
    (anonymous function) @test.html:9
*/
//变量提升，函数优先于变量-----------------
function a() {
    return "function";
}
var a;				//与函数同名
a = 100;
console.log(a);     //输出 100
console.log(a()); 	//报错, Uncaught TypeError: a is not a function
```

> JS 中有两种函数，一种是普通函数，一种是函数对象。下面的这种就是“函数对象”，它实际上是声明一个匿名函数，然后将该函数的 init 方法赋值给该变量。

```javascript
var a = 100;
var a = function() {
    return "function";
}
console.log(a);
/* 
输出
function() {
    return "function";
}
*/
console.log(a());   //输出 "function"
//变量提升------------------------------
var a;
a = 100;
a = function() {	//a被重新赋值了
    return "function";
}
console.log(a);
console.log(a());
```

> **函数与内部变量重名**
>
> 定义普通函数，即在 window 变量下，定义一个 key，它的名字为该函数名，值为该函数的地址。函数内部的 this 指向 window 对象。

```javascript
function a() {
    console.log(this);  //输出 window{...}
    this.a = 1;         //即 window.a = 1，此时window下的function a已经被该变量覆盖了。
    var a = 5;          //下面的这几个变量都是局部变量，仅在花括号范围内有效。  
    a = 10;
    var v = "value"
    return "function";
}
console.log(a);         //输出 function a {...}
console.log(a());       //输出 "function"
console.log(a);         //输出1，上一行调用了a()方法，方法内a已经被改成1了
console.log(v);
/*
输出
Uncaught ReferenceError: v is not defined
    (anonymous function) @ mycolor.html:15
*/
```

### 箭头函数（ES6）

```javascript
// 传统定义函数方式
function test () {
  //some code
}
const test = function () {
  //some code
}
// 使用箭头函数定义函数时可以省略 function 关键字
const test = (...params) => {
  //
}
// 该函数只有一个参数时可以简写成：
const test = param => {
  return param;
}
// 该函无参数时可以简写成：
const test = () => {
  return param;
}
console.log(test('hello'));   // hello
```

### == 与 === 的区别

1. 对于 string、number 等基础类型，== 和 === 是有区别的

- 不同类型间比较，== 比较的是**转化成同一类型后的值，看值是否相等**，===中 如果类型不同，其结果就是不等。
-  同类型比较，直接进行 "值" 比较，两者结果一样。

2. 对于 Array，Object 等高级类型，== 和 === 是没有区别的

- 都是进行**指针地址**比较

3. 基础类型与高级类型，== 和 === 是有区别的

- 对于 ==，将高级转化为基础类型，进行 "值" 比较
- 因为类型不同，=== 结果为 false

4. != 为 == 的非运算，!== 为 === 的非运算

### + 运算符

```javascript
var result1 = 5 + 5 + "abc"; //结果将是"10abc"
var result2 = "" + 5 + 5 + "abc"; //结果将是"55abc"
```

1. 字符串和数字相加，数字转成字符串.
```javascript 
var car = "hello" + 1;    // 结果为hello1
```
2. 数字和布尔值相加，布尔值 false 转成 0，true 转成 1
```javascript 
var car = 1 + true;    // 结果为2
var bus = 1 + false;    // 结果为1
```
3. 字符串与布尔值相加，布尔值转化成字符串。
```javascript 
var car = "hello" + true;    // 结果为hellotrue
```
4. 数字与 null(空值) 相加，null 转化为数字 0
```javascript 
var car = null + 3 + 4;    // 结果为7
```
5. 字符串与 null(空值) 相加，null 转化为字符串
```javascript 
var car = null + "ok";    // 结果为nullok
```

### % 运算符

取模运算的结果符号只与**左边值**的符号有关：

-  如果 % 左边的操作数是正数，则模除的结果为正数或零；
-  如果 % 左边的操作数是负数，则模除的结果为负数或零。

```javascript
var x = 7 % 3; // 结果为 1
var y = 7 % (-3); // 结果为 1
var z = (-7) % 3; // 结果为 -1
```

### 其他类型转`boolean`

**null、undefined、0、NaN、空字符串**转换为`false`，其他转化为` true`，在做`if`判断时需要了解。

### switch使用

```javascript
switch(n)
{
    case 1:
        //执行代码块 1
        break;
    case 2:
        //执行代码块 2
        break;
    default:
        //与 case 1 和 case 2 不同时执行的代码
}
```

1. switch 中 case的判断是`===`的判断，即数据类型和值的双重判断，这点要注意。
2. switch的判断条件可以是`String 、Number、Boolean、char、枚举、null、undefined`。

### for...in使用

```javascript
// 循环遍历对象的属性
var person = {fname:"John", lname:"Doe",age:25}; 
for (x in person) {  // x为属性名，全局变量
    txt = txt + person[x];
}
// 遍历数组
var nums = [1, 3, 5];
for (let x in nums) { // let修饰，x为局部变量
    document.write(nums[x]);  // x 为数组索引
}
```

### for...of使用（ES6）

> for...of 的语法看起来跟 for-in 很相似，但它的功能却丰富的多，它能循环很多东西。

```javascript
//循环一个数组Array
let iterable = [10, 20, 30];
for (let value of iterable) {
  console.log(value);	//隔行输出：10, 20, 30
}
//循环一个字符串String
let iterable = "boo";
for (let value of iterable) {
  console.log(value);	//隔行输出：b, o, o
}
//循环一个Map
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);
for (let [key, value] of iterable) {
  console.log(key, value);	//隔行输出：a 1, b 2, c 3
}
for (let entry of iterable) {
  console.log(entry);	//隔行输出：[a, 1], [b, 2], [c, 3]，注意这是数组哦
}
//循环一个Set
let iterable = new Set([1, 1, 2, 2, 3, 3]);
for (let value of iterable) {
  console.log(value);	//隔行输出：1, 2, 3
}
```

### for VS for...in

```javascript
function myFunction(){
    var array = new Array();
    var x;
    var txt = "";
    array[0] = 1;	//下标不连续
    array[3] = 2;
    array[4] = 3;
    array[10] = 4;
    for( x in array ){
        alert(array[x]);     // 依次显示出 1 2 3 4
    } 
    alert(array.length);    // 结果是11
    for( var i = 0; i < 4; i++){
        alert(array[i]);     // 依次显示出 1 undefined undefined 2 
    }
}
//for中定义变量的作用域
for (var i = 0; i < 10; i++) {
    // some code
}
console.log(i);	//10, i是全局的，包括循环体内与循环体外
for (let j = 0; j < 10; j++) {
    // some code
}
console.log(j);	//undefined，j是局部变量
```

### typeof, null, 和 undefined

> 使用 `typeof `操作符来检测变量的数据类型

```javascript
typeof "John"                // 返回 string 
typeof 3.14                  // 返回 number
typeof NaN                   // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
typeof new Date()            // 返回 object
typeof function () {}        // 返回 function
typeof null					 // 返回 object
typeof undefined			 // 返回 undefined
var a; //未赋值的变量a
typeof a					 // 返回 undefined
//null 和 undefined 的值相等，但类型不等
null === undefined           // false
null == undefined            // true
```

-  undefined：是所有没有赋值变量的默认值，自动赋值。
-  null：主动释放一个变量引用的对象，表示一个变量不再指向任何对象地址。

### NaN

1. NaN 是一个特殊的数值，NaN 即非数值（Not a Number），这个数值用于**本来要返回数值的操作数未返回数值**的情况,

2. NaN 与任何值都不相等，包括 NaN 本身。

```javascript
console.log(NaN == NaN); 	//false
console.log(NaN === NaN); 	//false
```

3. 可以通过 `isNaN() `方法来判断某个数值是否是NaN这个特殊的数，使用 `isNaN() `方法会将传入的数值如果是非数值的会将其自动转换成数值类型，若能转换成数值类型，那么这个函数返回 false，若不能转换成数值类型，则这个数就是 NaN，即返回 true。

### try...catch

```javascript
var txt = ""; 
function message() 
{ 
    try { 
        adddlert("Welcome guest!"); 
    } catch(err) { 
        txt = "本页有一个错误。\n\n"; 
        txt += "错误描述：" + err.message + "\n\n"; 
        txt += "点击确定继续。\n\n"; 
        alert(txt); 
    } finally {
        alert('finally');
    }
}
```

### ★变量提升

> JavaScript 中，函数及变量的声明都将被提升到函数的最顶部。

> JavaScript 中变量的提升仅仅是声明部分，初始化的部分仍然在原处。且**函数提升优先于变量**，即函数会置于变量前。

```javascript
var x = 5; // 初始化 x
console.log(x);	//输出5
console.log(y);	//输出undefined
var y = 7; // 初始化 y
function show(){};
//------------------------------------
//上述代码其实类似于以下变量提升后的代码
function show(){}; //函数优先提升至顶部
var x; 
var y; //y被提升
x = 5; //x赋值
console.log(x);	//输出5
console.log(y);	//输出undefined
y = 7; // y初始化部分，不会提升
```

> **注意**：使用匿名函数的方式不存在函数提升，因为函数名称使用变量表示的，只存在变量提升

```javascript
var getName = function(){	//匿名函数，只会提升变量getName
  console.log(2);
}
function getName(){
  console.log(1);
}
getName();
//结果为2
```

> 可能会有人觉得最后输出的结果是 1。但是 getName 是一个变量，因此这个变量的声明也将提升到顶部，而变量的**赋值依然保留在原来的位置**。需要注意的是，函数优先，虽然函数声明和变量声明都会被提升，但是函数会首先被提升，然后才是变量，上述之所以输出2，是因为之后的变量提升覆盖了之前的函数提升。
```javascript
function getName(){ //函数优先提升
  console.log(1);
}
var getName; //变量提升
getName = function(){	//匿名函数，只会提升变量getName，覆盖之前的getName()方法
  console.log(2);
}
getName();
//结果为2，因为后面的匿名函数覆盖了先提升的函数
```
----

再来些例子

```javascript
test1();//函数声明提升，在执行代码之前会先读取函数声明，不会报错
function test1(){//函数声明方式创建函数
    alert("测试1");
}

test2();//报错，函数还不存在
console.log(test2)//不会报错，变量提升只是提升变量的声明，并不会把赋值也提升上来，输出undefined
var test2 = function(){
    alert("测试2");
};//使用函数表达式创建一个匿名函数(实际是以变量test3命名的函数)
test2();//不会报错，已经创建函数

var test3 = function(){
    alert("测试3");
}();//加了括号立即执行

var test4 = 12;// 注意看，一旦变量被赋值后，将会输出变量
//函数提升优先级高于变量提升，所以函数先提升，然后变量提升覆盖之前的函数声明
function test4() {
    alert("测试4");               
}
console.log(test4); //12

var test5 = "test5_1";
(function(){
    //js中的变量搜索顺序：找变量时，先找局部变量，如果没有局部变量；再找全局变量。
    alert(test5);//此时的test5为局部变量的提升，undefined
    var test5 = "test5_2";
})();
```

### 严格模式

> 为消除Javascript语法的一些不合理、不严谨之处，减少一些怪异行为

```javascript
"use strict";
x = 3.14;       // 报错 (x 未定义)

myFunction();
function myFunction() {
    y = 3.14;   // 报错 (y 未定义)
}
```

```javascript
//在函数内部声明是局部作用域 (只在函数内使用严格模式)
x = 3.14;       // 不报错 
myFunction();
function myFunction() {
   "use strict";	//函数内启用严格模式
    y = 3.14;   	//报错 (y 未定义)
}
```

### void()

```html
<a href="javascript:void(0)">单击此处什么也不会发生</a>
<-- void()仅仅是代表不返回任何值，但是括号内的表达式还是要运行 -->
<a href="javascript:void(alert('Warning!!!'))">点我后会显示警告信息</a>
```
> href="#"与href="javascript:void(0)"的区别

* **#** 包含了一个位置信息，默认的锚是**#top** 也就是网页的上端。
* javascript:void(0), 仅仅表示一个死链接

