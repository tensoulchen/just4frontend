# JS

javaScript三部分

ECMAScript+DOM+BOM

动态语言，不确定变量类型，在代码执行过程中，可以随意修改

var name="abc";		//string类型

name=123;			//整型



编译性语言：

​	先编译再运行 

​	c／c++／Objective-C／...

解释性语言：

​	边读边运行

​	Python/JavaScript/MATLAB



书写位置

<script></script>写在body子元素的最后一行
html标签里

```
<a href="" onclick="alert('弹窗')">弹窗</a>
```

body里

```
<script>
	alert('弹窗');
</script>
```

外部引入 

```
<script src="./js/test.js"></script>
```





#4_变量、作用域、内存

编写代码存储在硬盘中，

代码在内存中执行，内存有栈空间堆空间，

基本数据类型在栈空间分配，

对象类型在堆空间，栈空间指针指向堆空间



## 变量

变量包括两种不同的类型：原始值和引用值

原始值就是最简单的数据，保存原始值的变量是按值访问的

​	Undefined、Null、Boolean、Number、String、Symbol

引用值：多个值构成的对象，保存在内存中，js不能直接访问内存位置，实际操作的是该对象的引用，保存引用值的变量按照引用访问

​	Array、object



## 执行上下文

JS为每段可执行代码创建其执行上下文

可执行代码：全局代码、函数代码、eval代码

所以就有：全局上下文、函数上下文

函数被调用执行就会创建这个函数的执行上下文，这个函数的执行上下文会被推到执行上下文栈上

上下文在其所有代码都执行完毕后会被销毁，包括定义在它上面的所有变量和函数(全局上下文在应用程 退出前才会被销毁，比如关闭网页或退出浏览器)。



每个执行上下文的都有三个重要属性：

变量对象

作用域链

this

 

### 作用域链

上下文中的代码在执行的时候，会创建变量对象的一个作用域链(scope chain)。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。 。

查找变量会从当前上下文的变量对象去查找，如果没找到就去父级作用域查找，一直查找到全局上下文中的变量对象

这么一个查找过程形成的链条就叫做作用域链。



### 全局上下文

在浏览器中，全局上下文就是我们常说的 window 对象 

### 函数上下文

函数执行时形成的私有上下文EC(FN)，正常情况下，代码执行完会出栈后释放;但是特殊情况下，如果当前私有上下文中的某个东西被上下文以外的事物占用了，则上下文不会出栈释放，从而形成不销毁的上下文，形成了个闭包。



## 作用域

隔离变量，不同作用域下的同名变量不会有冲突

```
var a=1;
function aa() {
	var a=2;
	var b=3;
	console.log(a);
	console.log(b);
  function bb() {
    var a=2;
    var b=3;
    console.log(a);
    console.log(b);
  }
}
console.log(a)			//1
console.log(aa())			//2 3
console.log(bb())		//error：bb not defined aa没调用


```

var只有函数作用域和全局作用域

可以重新声明

```
let user;
let user; //error

var user="pete"
var user="john"		
```



## 静态词法作用域／动态作用域

静态词法：函数作用域在定义时决定

动态作用域：函数作用域在调用时决定







####输出元素类型

```
console(typeof  "string");				//string 

console(typeof  123);				//number

console(typeof ("string"));			//string
```



###数据类型转换

```
var message1="123";		//message为string

var num1=Number(message1);					//num为number



var message2="abc";

var num2=Number(message2);				//NaN:not a number(NaN是number类型)



console.log(Number(true));			//1

console.log(Number(false));			//0



console.log(Number(undefined));			//NaN



consolr.log(Number(null));				//0
```



#### 数值转换

##### Number()

将任何数据类型转为数值

```javascript
console.log(Number(true))			//1
Number(false)			//0
Number(undefined)		//NaN
Number(null)			//0

Number(101)			//101

Number('')		//0
Number('101')		//101
Number('011')			//11,返回不为0的数值部分
Number('01.1')			//1.1

//除上都返回NaN
Number('1.0.1')			//NaN
Number('foo')				//NaN
```



##### parseInt()/parseFloat()

从字符串中“读取”数字，直到无法读取为止。如果发生 error，则返回收集到的数字。函数 `parseInt` 返回一个整数，而 `parseFloat` 返回一个浮点数

```javascript
alert( parseInt('100px') ); // 100
alert( parseFloat('12.5em') ); // 12.5
	
alert( parseInt('12.3') ); // 12，只有整数部分被返回了
alert( parseFloat('12.3.4') ); // 12.3，在第二个点出停止了读取

alert( parseInt('a123') ); // NaN，第一个符号停止了读取

```

`parseInt()` 函数具有可选的第二个参数。它指定了数字系统的基数，因此 `parseInt` 还可以解析十六进制数字、二进制数字等的字符串：

```javascript
alert( parseInt('0xff', 16) ); // 255
alert( parseInt('ff', 16) ); // 255，没有 0x 仍然有效

alert( parseInt('2n9c', 36) ); // 123456
```



##### toFixed()

函数 [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) 将数字舍入到小数点后 `n` 位，并以字符串形式返回结果。

```javascript
let num = 12.34;
alert( num.toFixed(1) ); // "12.3"
```

这会向上或向下舍入到最接近的值，类似于 `Math.round`

```javascript
let num = 12.36;
alert( num.toFixed(1) ); // "12.4"
```

`toFixed` 的结果是一个字符串。如果小数部分比所需要的短，则在结尾添加零：

```javascript
let num = 12.34;
alert( num.toFixed(5) ); // "12.34000"，在结尾添加了 0，以达到小数点后五位
```





#5 基本数据类型

####Number

数值类型,整数浮点数二进制八进制十六进制都是Number类型

NaN

```
console.log(NaN == NaN); // false
```

对NaN,Not a Number一个特殊数值，计算没有返回值时，返回NaN

​	console.log("number")

​	isNaN 判断是否不是一个数字，不是数字为true，是为false



####String

单双引号都可，多用双引号

打印文本，文本中有双引号

```
	console.log('string "string"');
```

打印文本，文本中有双引号双引号,转义字符\

```
	console.log('string \"\'string ');
	\\	转义\
	\"	转义"
	\t 	转义tab
	\n	转义换行
	
```

字符串长度

```
var name="tensoul";

console.log(name.length);
```



####Boolean

 true flase

```
var isLogin=true;

console.log(isLogin);
```



####Null

null 值表示一个空对象指针，这也是给 typeof 传一个 null 会返回"object"的原因:

在定义将来要保存对象值的变量时，建议使用 null 来初始化，不要使用其他值 

undefined 值是由 null 值派生而来的，因此 ECMA-262 将它们定义为表面上相等，用等于操作符(==)比较 null 和 undefined 始终返回 true 

只要变量要保存对象，而当时又没有那个 对象可保存，就要用 null 来填充该变量。这样就可以保持 null 是空对象指针的语义，并进一步将其 与 undefined 区分开来。 

```
console.log(null == undefined);  // true
```

```
var info={name:"name1",age:19};

info=null;		一个对象不再使用可以赋值null
```



####Undefined

```
var flag;

console.log(flag);

一个变量申明了，没赋值，默认为undefined
```



undefined由null衍生

```
console.log(undefined==null);			//true
```



# 6 引用数据类型



## Array

### 数组创建

#### new Array()

基于构造函数创建,可省略new

```
let arr=new Array();
let arr=Array();

let arr=new Array(20);			//arr.length=20
let arr=Array(20);

let arr=new Array('a1')		//arr.length=1
let arr=Array('a1','a2')
```



#### []

**基于字面量创建**

在使用数组字面量表示法创建数组不会调用Array构造函数。 

```
let arr=[]
```



#### Array.from()

**使用Array构造函数的静态方法创建**

Array.from()将类数组结构转换为数组实例



**Array.from(String)**

```
Array.from(String)		
Array.from("str")			//["s"."t","r"]
```



**Array.from(Map)**

```
const m=new Map().set(1,2).set(3,4)s
Array.from(m)			//[[1,2],[3,4]]
```



**Array.from(Set)**

```
const s=new Set().add(1).add(2)
Array.from(s)			//[1,2]
```



**Array.from(arr)**

浅复制arr

```
const a1=[1,2,3]
const a2=Array.from(a1)				//[1,2,3],a2!==a1,没有指向同一块内存地址
```



**Array.from(arguments)**

```
function getArgsArray() {
	return Array.from(arguments);
}

console.log(getArgsArray(1,2))		//[1,2]
```



Array.from(可迭代对象，func ,func内this指向值)

```js
const a1=[1,2,3]
const a2=Array.from(a1,x=>x**2)				//[1,4,9]
const a3=Array.from(a1,function(x) {return x**this.exponent},{exponent:2})		//[1,4,9]
```



Array.of()将一组参数转换为数组实例



### 数组基本

数组空位

数组索引

数组长度



检测数组类型



### 数组迭代



### 数组复制填充



### 数组转换



### 数组操作

#### 元素操作方法

- `arr.push(...items)` —— 从尾端添加元素，
- `arr.pop()` —— 从尾端提取元素，
- `arr.shift()` —— 从首端提取元素，
- `arr.unshift(...items)` —— 从首端添加元素。



#### 搜索方法：查找某值

##### arr.indexOf()

`arr.indexOf(item, from)` 

从索引 `from` 开始搜索 `item`，如果找到则返回索引，否则返回 `-1`。

##### arr.lastIndexOf()

`arr.lastIndexOf(item, from)` ：从数组末from 位置开始查找item，返回其index

—— 和上面相同，只是从右向左搜索。

##### arr.include()

`arr.includes(item, from)` ：判断数组内是否有item

—— 从索引 `from` 开始搜索 `item`，如果找到则返回 `true`（译注：如果没找到，则返回 `false`）。



`includes` 的一个非常小的差别是它能正确处理`NaN`，而不像 `indexOf/lastIndexOf`：

```js
const arr = [NaN];
alert( arr.indexOf(NaN) ); // -1（应该为 0，但是严格相等 === equality 对 NaN 无效）
alert( arr.includes(NaN) );// true（这个结果是对的）

```



#### arr.find()

语法如下：

```javascript
let result = arr.find(function(item, index, array) {
  // 如果返回 true，则返回 item 并停止迭代
  // 对于假值（falsy）的情况，则返回 undefined
});

```

依次对数组中的每个元素调用该函数：

- `item` 是元素。
- `index` 是它的索引。
- `array` 是数组本身。

如果它返回 `true`，则搜索停止，并返回 `item`。如果没有搜索到，则返回 `undefined`。

例如，我们有一个存储用户的数组，每个用户都有 `id` 和 `name` 字段。让我们找到 `id == 1` 的那个用户：

```javascript
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

let user = users.find(item => item.id == 1);

alert(user.name); // John

```



#### 操作方法

##### arr.map()

它对数组的每个元素都调用函数，并返回结果数组。

```javascript
let result = arr.map(function(item, index, array) {
  // 返回新值而不是当前元素
})

```



```javascript
//将每个元素转换为它的字符串长度
let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
alert(lengths); // 5,7,6

```



##### arr.slice

它会返回一个新数组，将所有从索引 `start` 到 `end`（不包括 `end`）的数组项复制到一个新的数组。`start` 和 `end` 都可以是负数，在这种情况下，从末尾计算索引。

```
arr.slice([start], [end])

let arr = ["t", "e", "s", "t"];

alert( arr.slice(1, 3) ); // e,s（复制从位置 1 到位置 3 的元素）

alert( arr.slice(-2) ); // s,t（复制从位置 -2 到尾端的元素）

```



##### arr.filter

`filter` 返回的是所有匹配元素组成的数组

```javascript
let results = arr.filter(function(item, index, array) {
  // 如果 true item 被 push 到 results，迭代继续
  // 如果什么都没找到，则返回空数组
});


```

```javascript
let users = [
  {id: 1, name: "John"},
  {id: 2, name: "Pete"},
  {id: 3, name: "Mary"}
];

// 返回前两个用户的数组
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2


```



##### arr.reduce

上一个函数调用的结果将作为第一个参数传递给下一个函数。

```javascript
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);


```

- `accumulator` —— 是上一个函数调用的结果，第一次等于 `initial`（如果提供了 `initial` 的话）。
- `item` —— 当前的数组元素。
- `index` —— 当前索引。
- `arr` —— 数组本身。

因此，第一个参数本质上是累加器，用于存储所有先前执行的组合结果。最后，它成为 `reduce` 的结果。

```javascript
//一行代码得到一个数组的总和
let arr = [1, 2, 3, 4, 5];
//accumulator:(sum, current) => sum + current，initial：0
let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15



```

如果没有初始值，那么 `reduce` 会将数组的第一个元素作为初始值，并从第二个元素开始迭代。。如果数组为空，那么在没有初始值的情况下调用 `reduce` 会导致错误。建议始终指定初始值。



##### arr.reduceRight()

遍历为从右到左



array转成字符串

```javascript
arr1=[1,2,3]
arr2=arr1.toString()


```



##### arr.sort()

[arr.sort](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) 方法对数组进行 **原位（in-place）** 排序，更改元素的顺序。(译注：原位是指在此数组内，而非生成一个新数组。)

它还返回排序后的数组，但是返回值通常会被忽略，因为修改了 `arr` 本身。



`Array.sort(compareFunction)`了，`compareFunction`接受两个值，假设为`a`和`b`，`compareFunction(a, b)`的返回值有三种情况：

- `compareFunction(a, b)` < 0，`a`将会被排到`b`的前面。
- `compareFunction(a, b)` = 0，二者相对位置不发生改变。
- `compareFunction(a, b)` > 0，`a`将会被排到`b`的后面。



```javascript
let arr = [ 1, 2, 15 ];

//直接调用，不指定排序func，会按照字符串形式排序
arr.sort()			//1,15,2   按照字符串顺序排序了


//指定排序func，sort会遍历arr，将arr中的值作为func的参数，
//func返回值:为正,a排在b后面，升序；为负,a排在b前面,降序

//升序
//retun a-b>0   a>b   b a(升序)
//return a-b<0   a<b		a b(降序)
arr.sort(function(a, b) { return a - b; });		
alert(arr);  // 1, 2, 15

//降序
//return b-a>0   a<b   b a(降序)
//return b-a<0   a>b		a b(降序)
arr.sort(function(a, b) { return b - a; });			
alert(arr);  // 15, 2, 1


```











**乱序**

```
//使用 Math.random()：

var values = [1, 2, 3, 4, 5];

values.sort(function(){
    return Math.random() - 0.5;
});

console.log(values)
// Math.random() - 0.5 随机得到一个正数、负数或是 0，如果是正数则降序排列，如果是负数则升序排列，如果是 0 就不变，然后不断的升序或者降序，最终得到一个乱序的数组。

```



#### 字符串数组方法

字符串分割、结构

```javascript
location="120.046869,31.633804"
item.location.split(',')     [120.046869,31.633804]		字符串分割
let [x,y]=item.location.split(',')    //数组解构


```



```
//技巧
//删除最后一个字符
const str='str1'
console.log


```



##### 逆序

使用场景

![屏幕快照 2022-03-01 下午5.55.44](../H5C3/图片/屏幕快照 2022-03-01 下午5.55.44.png)

使用reverse()

```
function reverseString(str) {
	return str.split('').reverse().join('');
}
let str='abcdefg'
console.log(reverseString(str));


```



栈方法

```javascript
//数组模拟栈
function Stack() {			//构造函数首字母大写
	this.data=[];
	this.top=0;			//top指向下一个被push的位置，栈顶的后一个
}


Stack.prototype={			//在函数的原型链上添加方法
	push:function push(elem) {
		this.data[this.top++]=elem;
	}
	pop:function pop() {
		return this.data[--this.top];
	}
	length:function() {
		return this.top
	}
}


function reverseString(str) {
  let s=new Stack()
  let sarr=str.split('');
  let res=''
  for(let i=0;i<sarr.length;i++) {
    s.push(sarr[i])
  }
  for(let i=0;i<sarr.length;i++) {
    res+=s.pop(i)
  }
  return res  
}

let str='abcdefg';
console.log(reverseString(str))


```



### Map 

由键名：键值组成的集合，键名、键值可以是任何类型

map像object，但是map可以存储任何类型的键名key，map可以使用对象键，object不可，因为object会将所有键都转化为字符串键

#### 属性和方法

- `new Map()` —— 创建 map。

- `map.set(key, value)` —— 根据键存储值，可以链式调用。

  ```
  map.set('1', 'str1')
    .set(1, 'num1')
    .set(true, 'bool1');
  ```

- `map.get(key)` —— 根据键来返回值，如果 `map` 中不存在对应的 `key`，则返回 `undefined`。

- `map.has(key)` —— 如果 `key` 存在则返回 `true`，否则返回 `false`。

- `map.delete(key)` —— 删除指定键的值。

- `map.clear()` —— 清空 map。

- `map.size` —— 返回当前元素个数。



#### Map 迭代

如果要在 `map` 里使用循环，可以使用以下三个方法：

- `mapName.keys()` —— 遍历并返回所有的键（returns an iterable for keys），

- `mapName.values()` —— 遍历并返回所有的值（returns an iterable for values），

- `mapName.entries()` —— 遍历并返回所有的实体（returns an iterable for entries）`[key, value]`，`for..of` 在默认情况下使用的就是这个。

- mapName.forEach()

  ```
  // 对每个键值对 (key, value) 运行 forEach 函数
  recipeMap.forEach( (value, key, map) => {
    alert(`${key}: ${value}`); // cucumber: 500 etc
  });
  
  ```

  

### set

只有键值的集合，值不重复，可以是任何类型，如对象类型

```
let set=new set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

set.add(john);
set.delete(john);
set.has(john)
set.clear()
set.size

set.keys() —— 遍历并返回所有的值（returns an iterable object for values），
set.values() —— 与 set.keys() 作用相同，这是为了兼容 Map，
set.entries() —— 遍历并返回所有的实体（returns an iterable object for entries）[value, value]，它的存在也是为了兼容 Map。
```





# 数据类型判断

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures

```
						  typeof						instanceof			Object.prototype.toString.call()
原始值
undefined     undefined													[object undefined]
Boolean				boolean														[object Boolean]
Number				number														[object Number]
String				string														[object String]
Bigint				bigint														[object Bigint]
Symbol				symbol														[object Symbol]
Null					object														[object Null]


引用值
object				Object						true/false			[object Object]
 Array 		    object						true/false			[object Array]
 Date 				object						true/false			[object Date]
 Function			Function					true/false			[object Function]
```

Array、Date   function string





# 8 object对象

对象：一组没有特定顺序的值，由键名key和键值value组成

对象的属性或方法由一个名称标识，值可以是数据或者函数

## 基本创建方法

**new**

```
let person=new Object();
person.name='pname'
person.sayName=()=>{
	console.log(this.name)
}
```



**对象字面量**

```
let person= {
	name:'pname',
	sayName() {
		console.log(this.name)
	}
}
```





## 对象属性

**属性访问方法**

.操作符

```
objectName.propertyName

```

[]操作符

```
objectName["propertyName"]

```



区别

[] 操作符是动态的，可以传变量、数字、字符串

.操作符是静态的，必须是有效变量名形式

```
let name="person name"

person[name]		
person.name    //error

person["person name"]
person.person name 		//error

person[1]
person.1			//error

```



## 对象属性方法Obect.defineProperty()

用来配置对象的每个属性

每个对象属性的数据属性，描述这个属性是否可删、可改、可迭代、值配置

每个对象属性的访问器属性，包含一个获取(getter)函数和一个设置(setter)函数 



### 数据属性

**4个特性**

数据属性有4个特性描述它们的行为。

前三个默认为true

[[Configurable]]

表示属性是否可以通过delete删除并重新定义,是否可以修改它的特性，以及是否可以把它改为访问器属性。

默认为true，通过defineProperty改为false之后，非严格模式下无法删除对象属性，严格模式下error，

设置为false后，不可再修改了



[[Enumerable]]

表示属性是否可以通过for-in循环返回。



[[writable]]

表示属性的值是否可以被修改

默认为true，通过defineProperty改为false之后，非严格模式下无法删除对象属性，严格模式下error



[[Value]]

包含属性实际的值。这就是前面提到的那个读取和写入属性值的位置。这个特性的默认值为
undefined。



**修改特性**

```
Object.defineProperty(修改对象名,对象属性名,配置项)
```

```javascript
let person = {
	name:'pname'
}

Object.defineProperty(person,'name',{
	value:"pname1",		//修改值
	writable:false				//不能再修改了
})

person.name="pname2"		//非严格忽略，严格error
console.log(person.name)			//pname1

```



### 访问器属性

访问器属性不包含数据值,包含一个获取(getter) 函数和一个设置(setter) 函数，

在读取访问器属性时，会调用获取函数,这个函数的责任就是返回一个有效的值。

在写入访问器属性时,会调用设置函数并传入新值,这个函数必须决定对数据做出什么修改。

访问器属性有4个特性描述它们的行为。

[[Configurable]]:表示属性是否可以通过delete删除并重新定义，是否可以修改它的特性，以及是否可以把
它改为数据属性。默认情况下，所有直接定义在对象.上的属性的这个特性都是true。

[[Enumerable]]：表示属性是否可以通过for-in循环返回。默认情况下，所有直接定义在对象上的属性的这个

特性都是true。

[[Get]]:获取函数,在读取属性时调用。默认值为undefined.

[[Set]]: 设置函数,在写入属性时调用。默认值为undefined。



访问器属性是不能直接定义的，必须使用Object.defineProperty()。

```
let book = {
	year_:2017,			//属性名_  表示私有成员，不希望该属性在对象方法的外部使用
	edition:1				//普通成员
}
//define对象的year属性，无论year在不在book中都可以define,创建普通属性来操作私有属性
Object.defineProperty(book,"year",{
	get:function () {
		return this.year_
	},
	set:function (newValue) {
		if(newValue>2017) {
			this.year_=newValue;
			this.edition+=newValue-2017
		}
	}
})
book.year=2019		//调用了set方法,year_=year=2019
console.log(book.edition)   //2

```



## 对象方法

### Object.keys(obj)   

返回属性名数组

### Object.values(obj)		

返回属性值数组

### Object.entries(obj)

`Object.entries()`返回一个数组，其元素是与直接在`object`上找到的可枚举属性键值对相对应的数组。属性的顺序与通过手动循环对象的属性值所给出的顺序相同。

```javascript
//Object.entries(obj)
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj)); // [ ['foo', 'bar'], ['baz', 42] ]

// array like object
const obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(obj)); // [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ]

// 对象转map
var obj = { foo: "bar", baz: 42 };
var map = new Map(Object.entries(obj));
console.log(map); // Map { foo: "bar", baz: 42 }

```



## 对象遍历

```
const obj={
	name:'name',
	age:18
}
for(let key in obj) {
	console.log(key)				//name age
}
```







## 设计模式创建对象

### **工厂模式**

 抽象出创建对象和属性赋值的过程，根据传入的属性值批量创建对象，减少冗余重复的属性编写

```
function createPerson(name,age,address) {
	let o=new Object();
	o.name=name;
	o.age=age;
	o.getName=function() {
		return this.name;
	}
	return o;
}

let person1=createPerson('person1Name',18,{
	name:'address name1',
	code:'100000'
})
console.log(person1)
let person2=createPerson('person2Name',18,{
	name:'address name2',
	code:'100000'
})
console.log(person2)

```

缺点：创建的实例都是绑定在O上的，不能更好的区分



#### 基于构造函数创建对象

任何函数只要用new调用就是构造函数



```
function Person(name,age) {
	this.name=name;
	this.age=age;
	this.getName=function() {
		return this.name
	}
	return o;
}

let person1=new Person('person1Name',18,{
	name:'address1',
	code:'code1'
})
console.log(person1)		

let person2=new Person('person2Name',18,{
	name:'address2',
	code:'code2'
})
console.log(person2)	

每个实例的getName function都是单独的
console.log(person1.getName==person2.getName)			//false
```

缺点：每个实例的函数都会占据一定的空间，资源浪费

解决：把函数的定义移到构造函数的外部



### 基于构造函数和原型混合创建对象

构造函数与 通函数唯一的区别就是调用方式不同。除此之外，构造函数也是函数。并没有把某个 函数定义为构造函数的特殊语法。任何函数只要使用 new 操作符调用就是构造函数，而不使用 new 操 作符调用的函数就是 通函数。 

使用构造函数定义实例对象属性，使用原型对象定义实例对象的共享属性和函数方法，使得属性都有自己的属性值，同时共享函数引用，节省内存空间

```
function Person(name,age) {
	this.name=name;
	this.age=age;
	this.address=address;
}
Person.prototype.getName=function() {
	return this.name;
}

let person1=new Person('person1',18)
let person2=new Person('person2',18)

console.log(person1.getName())		//person1
console.log(person2.getName())		//person2
console.log(person1.getName===person2.getName)		//true，共享了getName()
person2.name='person22'			//改变实例属性，不影响其他实例的属性值
console.log(person1.getName())		//person1				
console.log(person2.getName())		//person22
```

缺点：每次在创建实例的时候，都会进行原型设置



### 基于动态原型创建对象

每个函数都会创建一个 prototype 属性，这个属性是一个对象，包含应该由特定引用类型的实例 共享的属性和方法。 

使用原型对象的好处 是，在它上面定义的属性和方法可以被对象实例共享。原来在构造函数中直接赋给对象实例的值，可以 直接赋值给它们的原型，





## 原型、原型链

构造函数都有一个原型对象prototype，这个原型对象又有constructor属性指回构造函数，构造函数创建的实例有一个指针`_proto_`指向原型对象

如果原型是另一个类型的实例，那么这个原型本身有一个内部指针指向另一个原型，相应地另一个原型也有一个指针指向另一个构造函 数。这样就在实例和原型之间构造了一条原型链。这就是原型链的基本构想。 

<img src="https://camo.githubusercontent.com/9a69b0f03116884e80cf566f8542cf014a4dd043fce6ce030d615040461f4e5a/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6d717971696e6766656e672f426c6f672f496d616765732f70726f746f74797065352e706e67" alt="原型链示意图" style="zoom:50%;" />



## 对象继承

es6之前主要使用原型链实现对象继承、es6之后有class但本质上还是原型链和构造函数

### 原型链继承

：重写子类的prototype，将其指向父类实例

```javascript
function Parent() {
	this.name='Pname'
}

Parent.prototype.getName=function() {
	console.log(this.name)
}

function Child() {

}

Child.prototype=new Parent();

let child1=new Child();

console.log(child1.getName())		//Pname
```



问题1：

子类实例共享父类属性，子类某个实例修改父类的引用值(比如数组)，所有实例继承的该值都会被影响

```
function SuperType() {
      this.colors = ["red", "blue", "green"];
、}
、function SubType() {}
// 继承SuperType
SubType.prototype = new SuperType();

let instance1 = new SubType(); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black"

let instance2 = new SubType(); 
console.log(instance2.colors); // "red,blue,green,black"

```





问题2:

子类实例化时不能给父类构造函数传参



问题3:

不能多继承，子类的实例只能指向一个父类



问题4:

为子类的原型添加属性和函数，必须要在new Parent()继承之后在实例对象上添加属性和函数，如果在继承前添加，继承后，子类原型本身的属性和函数会被覆盖

```
let instance1 = new SubType(); 
```



### 盗用构造继承

：在子类构造函数中调用父类构造函数，通过call() apply()改变this指向，让子类的实例化的对象拥有各自的属性和函数

```
function SuperType() {
	this.colors=['red','blue','greed'];
}

function SubType() {
	SuperType.call(this);
}

let instance1 = new SubType(); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black"

let instance2 = new SubType();
console.log(instance2.colors); // "red,blue,green"
```



优点：

1、解决原型链继承方法的子类实例共享影响父类属性问题

2、可以给父类构造函数传参

```
function SuperType(name) {
	this.name=name
}

function SubType() {
	SuperType.call(this,'name1')
	this.age=29
}

let instance = new SubType();
console.log(instance.name); // "Nicholas";
console.log(instance.age);  // 29
```

3、可以多继承

在子类构造函数中，多次call()来继承多个父类，所有继承的都绑定到该子类的this上



缺点

子类的实例仅是子类的而不是父类的

```
function Animal() {
	
}

function Cat(name) {
	Animal.call(this)
}
```



## 对象克隆

对象属性是基本数据类型，深/浅克隆后，改变值不会相互影响

对象属性引用数据类型，浅克隆之后实际指向了同一块内存地址，修改后该内存地址的值变了，指向同一块内存的地址的变量的值就变了，所以对引用数据类型要深克隆

```javascript
//name和age都是基本数据类型，深浅克隆都不会影响，而color是array是引用类型，所以克隆后指向了同一块内存地址，改变值后相会影响，那么这就是浅克隆，改变值之后不相互影响的是深克隆
let user={
    name:'John',					
    age:18,
    color:['red','black']
}

let clone={}

for (let key in user) {
    clone[key]=user[key];
}

clone.name="Pete"
clone.color[1]='pink'
console.log(user)
console.log(clone)

//{ name: 'John', age: 18, color: [ 'red', 'pink' ] }
//{ name: 'Pete', age: 18, color: [ 'red', 'pink' ] }
```

JSON序列化实现深克隆

```javascript
// 对象深克隆
// 如果一个对象的所有属性都是可以序列化的，
//JSON.parse(JSON.stringfy())
// 先序列再反序列化得到的对象就是深克隆的对象
/*
* 问题：不能克隆函数、正则；对象的constructor
*/
let deepObj=JSON.parse(JSON.stringify(origin))
deepObj.b[2]=5
console.log(deepObj)
console.log(origin)
```

```js
function Test(name) {
    this.name=name
}
let test=new Test('test1')
let origin1={
    a:function() {
        return 'a'
    },
    b:new RegExp('\d','g'),
    c:test
}
// console.log(origin1.c)
let deepObj=JSON.parse(JSON.stringify(origin1))
console.log(deepObj)
//{ b: {}, c: { name: 'test1' } }			function、正则没克隆、实例原型的constructor被抛弃不再指向实例

```



利用递归来实现每一层都重新创建对象并赋值

```
function deepclone(source) {
	const targetObj=source.constructor===Array?[]:{}
	for(let keys in source) {
		if(source.hasOwnProperty(keys)) {
		if(source)
		}
	}
}
```



## 类



### 类定义

类名的首字母要大写 

```
class Person {}				类声明式

const Animal = class {};			类表达式
```

类可以包含构造函数方法、实例方法、获取函数、设置函数和静态类方法



### 类实例

```
let p=new Person();

```



立即实例类表达式

```
let p=new class Foo {
	constructor(x) {
		
	}
}('bar')

console.log(p);  // Foo {}


```



### 类构造函数

constructor构造函数，不是必须，可为空

当实例被new时，会调用构造函数

构造函数内的thi指向新对象

```
class Parent {
	constructor() {
		console.log(this)
	}
}
let child=new Parent()			//返回Parent {} ，新创建的对象，parent是这个新对象的地址名

```

使用 new 调用类的构造函数会执行如下操作。
 (1) 在内存中创建一个新对象。
 (2) 这个新对象内部的[[Prototype]]指针被赋值为构造函数的 prototype 属性。

 (3) 构造函数内部的 this 被赋值为这个新对象(即 this 指向新对象)。
 (4) 执行构造函数内部的代码(给新对象添加属性)。
 (5) 如果构造函数返回非空对象，则返回该对象;否则，返回刚创建的新对象。 





### 类实例属性

每个实例都有各自的成员对象,所有成员都不会在原型上共享

```
class Person {
	constructor() {
		this.name=new String('Jack')
		this.sayName=()=>console.log(this.name);
    this.nicknames = ['Jake', 'J-Dog']
	}
}

let p1=new Person();
let p2=new Person();

 p1.sayName(); // Jack
 p2.sayName(); // Jack
 console.log(p1.name === p2.name);
 console.log(p1.sayName === p2.sayName);
 console.log(p1.nicknames === p2.nicknames);  // false
 p1.name = p1.nicknames[0];
 p2.name = p2.nicknames[1];
 p1.sayName();  // Jake
 p2.sayName();  // J-Dog
```



### 类原型方法

在类块中定义的方法，都会被定义在类原型上，用于在实例间共享方法

```
class Person {
	constructoe() {
	//添加到 this 的所有内容都会存在于不同的实例上
		this.locate=()=>{
		 console.log('instance.locate')
		}
	}
	// 在类块中定义的所有内容都会定义在类的原型上
	locate() {
  	console.log('prototype.locate');
	}
}

let p=new Person()
p.locate()				// instance
Person.prototype.locate()			// prototype
```

类方法名可以是字符串、符号、计算值...

```
class Person {
	stringKey(){}
	[symbolKey](){}
	['computed' + 'Key'](){}
}


p.stringKey()
p.[symbolKey]()
p.[computedKey]()s
```



### 类set get方法

```
class Person {
	set name(newName) {
		this.name_=name
	}
	get name() {
		return this.name_
	}
}

let p=new Person()
p.name='Jake'			//调用set
console.log(p.name)			//调用get
```



### 类静态方法

静态类成员在类定义中使用 static 关键字作为前缀。在静态成员中，this 引用类自身。 

```
class Person {
  constructor() {
  // 添加到 this 的所有内容都会存在于不同的实例上
  	this.locate = () => console.log('instance', this);
  }
  // 定义在类的原型对象上 
  locate() {
  	console.log('prototype', this);
  }
	// 定义在类本身上 
	static locate() {
		console.log('class', this);
	}
}
let p = new Person();
p.locate();                 // instance, Person {}
Person.prototype.locate();  // prototype, {constructor: ... }
Person.locate();            // class, class Person {}
```



可以把方法定义在类构造函数中或者类块中，但不能在类块中给原型添加原始值或对象作为成员数据 ,可以在类外加

```
class Person {
  name: 'Jake'
}
// Uncaught SyntaxError: Unexpected token
```



### ⚠️

类是特殊函数

```
class Person {}
console.log(Person)				//class Person={}
console.log(typeof Person)			//function
```



也有原型链

```
console.log(Person===Person.prototype.constructor)
```



可以使用instance of 检测一个类实例是否属于某个类

```
class Person {}

let p=new Person();

console.log(p instanceof Person) //true
```



### 类继承

#### extend

可以继承类、继承普通构造函数

```
class Parent1 {}
function Parent2 () {}

class Child1 extends Parent1 {}
class Child12 extends Parent2 {}
```



#### super()

派生类可以使用`super`调用父类原型。这个关键字只能在派生类中使用，而且仅限于类构造函数、实例方法和静态方法内部 。

1、派生类没有定义构造函数，其实例化时会自动调用super()

```
class Parent {
	constructor(name) {
		this.name=name;
	}
}

class Child extends Parent {

}

console.log(new child('child'));			//Child {name:'child'}

```

2 、在派生类构造函数中，不能在调用 super()之前引用 this 

3、super()显式调用父类并传参数

```
class Parent {
	constructor(name) {
		this.name=name;
	}
}

class Child extends Parent {
	constructor(name) {
		super(name)
	}
}
//上下同
class Child extends Parent {

}


console.log(new child('child'));			//Child {name:'child'}

```



### 抽象基类

#### new.target

保存通过new调用的函数／类，通过判断new.target===className，

父类作为抽象基类，阻止父类被实例

```
class Parent {
	constructor() {
		console.log(new.target)
		if(new.target===Parent) {
			throw new Error('Vehicle cannot be directly instantiated')
		}
	}
}

class Child extends Parent {}

new Child() //	class Child()
new Parent()   //class Parent() 'Vehicle cannot be directly instantiated'
```



在抽象基类构造函数中进行检查，可以要求派生类必须定义某个方法 

```
class Parent {
	constructor () {
		if(new.target===Parent) {
			throw new Error('Parent cant be instantiated')
		}
		if(!this.foo) {
			throw new Error('foo() must be defined')
		}
	}
}

// 派生类
class Bus extends Vehicle {
	foo() {} 
}
// 派生类
class Van extends Vehicle {}

new Bus(); // success!
new Van(); // Error: Inheriting class must define foo()
```



# 10 函数

在 JavaScript 中，函数是对象类型。可被调用的“行为对象（action object）”。我们不仅可以调用它们，还能把它们当作对象来处理：增/删属性，按引用传递等。



函数名是保存指针的变量

console.log(func)



```
console.log(func)			//打印函数
func()			//执行函数
```

## 创建函数

```
函数声明法
function func() {

}
函数表达式
let func=function () {

}
普通构造函数,函数名大写
function Func() {

}
function func() {
	...
	return function () {
	
	}
}
```

函数属性

一个内建属性 “length”，它返回函数入参的个数

```
functionName.length
```







## 10.10 函数属性和方法

### call()  apply()

通过call(),apply可以改变函数执行主体，使得执行主体(对象)可以直接调用该函数

```
function sum(num1,num2) {
	return num1+num2
}

var Person={}
console.log(sum.call(Person,1,2))
console.log(sum.call(Person,[1,2]))

```



#### 改变指向 call() apply() bind()

```
var value=10;
var obj={
	value=20;
}

var method=function() {
	console.log(this.value)
}

method();			//10

```







## 闭包

引用了另一个函数作用域的变量的函数，通常在嵌套函数中实现

函数有函数作用域，外部函数无法访问内部函数的变量，内部函数可以访问外部。

那么将内部函数return出去，就可以实现在别的作用域中访问

**闭包形成的条件**：

1. 函数的嵌套
2. 内部函数引用外部函数的局部变量，延长外部函数的变量生命周期

**闭包的特性**：

- 1、内部函数可以访问定义他们外部函数的参数和变量。(作用域链的向上查找，把外围的作用域中的变量值存储在内存中而不是在函数调用完毕后销毁)设计私有的方法和变量，避免全局变量的污染。

  1.1.闭包是密闭的容器，，类似于set、map容器，存储数据的

  1.2.闭包是一个对象，存放数据的格式为 key-value 形式

- 2、函数嵌套函数

- 3、本质是将函数内部和外部连接起来。优点是可以读取函数内部的变量，让这些变量的值始终保存在内存中，不会在函数被调用之后自动清除



**闭包的用途**：

1. 模仿块级作用域
2. 保护外部函数的变量 能够访问函数定义时所在的词法作用域(阻止其被回收)
3. 封装私有化变量
4. 创建模块



**闭包应用场景**：回调函数

一个事件绑定的回调方法;

发送ajax请求成功|失败的回调;

setTimeout的延时回调;

或者一个函数内部返回另一个匿名函数



**闭包的优点**：延长局部变量的生命周期

**闭包缺点**：会导致函数的变量一直保存在内存中，过多的闭包可能会导致内存泄漏



## 回调函数callback

某个操作执行结束后，自动被调用的函数

例子

```js
//callback：隐藏后回调函数，显示弹窗
$("button").click(function(){
  $("p").hide('slow',function() {
    alert('...hided...')
  })
})

//不写成callback，先弹窗，再隐藏
$("button").click(function() {
  $("p").hide(1000);
  alert('...hided...')
})
```





#### 闭包

函数嵌套：在一个函数内部定义函数

闭包：内部函数可以访问其所在外部函数的变量和参数，即使外部函数return了

```javascript
//闭包实现sum(a)(b)=a+b
//sum(1)(2)=3
function sum(a) {
  //写法1
  return function(b) {
    return a+b;
  }
  //写法2
  return b=>a+b;
}

sum(1)(2)

```



##### setTimeOut()

将函数推迟到一段时间间隔之后再执行

```javascript
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)


```

参数说明：

- `func|code`

  想要执行的函数或代码字符串。 一般传入的都是函数。由于某些历史原因，支持传入代码字符串，但是不建议这样做。

- `delay`

  执行前的延时，以毫秒为单位（1000 毫秒 = 1 秒），默认值是 0；

- `arg1`，`arg2`…

  要传入被执行函数（或代码字符串）的参数列表（IE9 以下不支持）



##### clearTimeOut()

取消调度



#### (function(){})();立即调用函数表达式

在之前，JavaScript 中只有 `var` 这一种声明变量的方式，并且这种方式声明的变量没有块级作用域，程序员们就发明了一种模仿块级作用域的方法。这种方法被称为“立即调用函数表达式”（immediately-invoked function expressions，IIFE）。

需要使用圆括号把该函数表达式包起来，以告诉 JavaScript，这个函数是在另一个表达式的上下文中创建的，因此它是一个函数表达式：它不需要函数名，可以立即调用。





# 14 DOM



### 插入标签

示例html代码：

```xml
<div id="test">
   <span style="color:red">test1</span> test2
</div>


```

获得id为test的DOM对象，下面就不一一获取了。

```dart
var test = document.getElementById('test');


```

#### innerHTML

描述：也就是从对象的起始位置到终止位置的全部内容,包括Html标签。

上例中的test.innerHTML的值也就是`“test1 test2 ”。`

#### innerText

描述：从起始位置到终止位置的内容, 但它去除Html标签 。

上例中的test.innerText的值也就是`“test1 test2”`, 其中`span`标签去除了。

#### outerHTML

描述：除了包含innerHTML的全部内容外, 还包含对象标签本身。

上例中的test.outerHTML的值也就是`test1 test2`

完整示例：

```xml
<div id="test">
   <span style="color:red">test1</span> test2
</div>

<a href="javascript:alert(test.innerHTML)">innerHTML内容</a>
<a href="javascript:alert(test.innerText)">inerText内容</a>
<a href="javascript:alert(test.outerHTML)">outerHTML内容</a>


```

结果：

```awk
//test.innerHTML结果：<span style="color:red">test1</span> test2
//test.innerText结果:test1 test2
//test.outerHTML结果：<div id="test"><span style="color:red">test1</span> test2</div>


```

#### textContent

描述：textContent 属性设置或返回指定节点的文本内容，以及它的所有后代。

提示：有时，此属性可用于取代 nodeValue 属性，但是请记住此属性同时会返回所有子节点的文本。

得到的结果跟innerText的结果是一样的。

> 注释：Internet Explorer 8 以及更早的版本不支持此属性。

#### 兼容性

`innerHTML`所有浏览器兼容；`innerText`与`outerHTML`虽然主流浏览器，如谷歌，火狐，IE7-IE11，QQ等都已支持（这里提到的谷歌火狐等都是最新浏览器的版本），但是`W3C`的标准属性就是`innerHTML`,因此，尽可能地去使用`innerHTML`，而少用`innerText`与`outerHTML`。



#### insertAdjacentHTML(位置,HTML)

#### insertAdjacentText(位置,Text)

  "beforebegin" :  插入到当前元素前面，作为前一个同胞节点

afterbegin:插入到当前元素内部，作为新的子节点或放在第一个子节点前面

beforeend:插入到当前元素内部，作为新的子节点或放在最后一个子节点前面

afterend:插入到当前元素后面，作为下一个同胞节点



## 事件

### 事件对象

#### DOM事件对象

事件处理程序的唯一参数event

event对象的属性／方法

| stopPropagation() | 取消后续事件捕获或冒泡，当bubbles为true时可以调用 |
| ----------------- | ------------------------------------------------- |
|                   |                                                   |
|                   |                                                   |
|                   |                                                   |



### 事件流

[DOM 事件](http://www.w3.org/TR/DOM-Level-3-Events/)标准描述了事件传播的 3 个阶段：

1. 捕获阶段（Capturing phase）—— 事件（从 Window）向下走近元素。
2. 目标阶段（Target phase）—— 事件到达目标元素。
3. 冒泡阶段（Bubbling phase）—— 事件从元素上开始冒泡。

事件首先通过祖先链向下到达元素（捕获阶段），然后到达目标（目标阶段），最后上升（冒泡阶段），在途中调用处理程序。



页面上table表格，click某个表格单元td

捕获型事件流：table—tbody—tr—td  向内传播

冒泡事件流：td—tr—tbody—table 向外传播



一个事件流包含三个阶段

捕获—目标—冒泡



### 事件处理



### Event对象

事件以event对象存在，每触发一个事件都会产生一个event对象

event对象在不同浏览器实现有差异



**获取event对象**

```
1、作为事件处理程序的参数传入获取
2、window.event获取


```



```
let btn=document.getElementById("button");
btn.addEventListener('click',(event)=>{
	//方法1:作为事件处理程序的参数传入
	console.log(event);
	//方法2:通过windwo.event获取
	let windowEvent=window.event;
	console.log(windowEvent)
	//判断是否一致
	console.log(windowEvent===event)
})


```



**获取event对象的目标元素**

```
e.srcElement
e.target


```



```
//先定义事件获取对象
let EventUtil={
	getEvent:function(event) {
		return event||window.event
	}
}
//再获取目标元素
let btn=document.getElementById("button");
btn.addEventListener("click",function(event) {
	//获取对象，如果传参形式获取到了，那么event===event，否则event===window.event
	let event=EventUtil.getEvent(event);
	//两种方式获取目标元素  event.srcElement 和 event.target
	let NoIETarget=event.target		//chrome safari
	let IETarget=event.srcElement
})


```





不同浏览器支持获取方式不同

```
//适配不同浏览器，定义事件获取对象
let EventUtil={
	getEvent:function(event) {
		return event || window.event
	}
	getTarget:function(event) {
		return e.target||e.srcElement
	}
}


```



### 事件循环

JS是单线程的，

为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步代码压入执行栈中，依次执行，将异步代码推入异步队列。

异步队列又分为宏任务队列和微任务队列，因为宏任务队列的执行时间较长，所以微任务队列要优先于宏任务队列。

微任务队列的代表就是，`Promise.then`，`MutationObserver`，宏任务的话就是`setImmediate setTimeout setInterval





## 拖动事件

默认情况下，图片、链接和文本是可拖动的，这意味着无须额外代码用户便可以拖动它们。文本只
有在被选中后才可以拖动，而图片和链接在任意时候都是可以拖动的。
我们也可以让其他元素变得可以拖动。HTML5在所有HTML元素上规定了-一个draggable属性，
表示元素是否可以拖动。图片和链接的draggable属性自动被设置为true,而其他所有元素此属性的默认值为false。如果想让其他元素可拖动，或者不允许图片和链接被拖动，都可以设置这个属性。



某元素被拖动，会按顺序触发以下事件

dragstart:开始拖动那一刻触发，目标元素this为被拖动元素

drag：拖住不放持续触发

dragenter:拖动到某个有效放置触发，this为有效位置元素

dragover:触发完dragenter后立即触发dragover，并且持续出发直到离开当前有效放置范围，this为有效位置元素

dragleave:触发完dragover后触发dragleave，this为有效位置元素

drop:放到了目标位置触发

dragend：拖动到目标位置释放元素时触发



# 27 工作者线程





# ES6

栈 堆

基本数据类型number、str、boolean存在栈上



### var\let\const

var:没有块级作用域

```javascript
//var会变量提升
console.log(a);				//变量提升，返回undefined
var a=10;

//var可以重复定义
var a;
var a=10;
var a=20;
console.log(a);				//20

//var没有块级作用域，es5只有全局和函数作用域，var在大括号之后还可以访问
{
  var a=10;
}
console.log(a);

//var声明for循环的变量，循环外还可以输出，循环结束输出无意义
for(var i=0;i<10;i++){
  console.log(i);
}
console.log(i);			//9

```



let：更完美的var, 没有直接废弃var是考虑到兼容性问题，考虑到执行效率

```javascript
//let取消预解析，变量声明不会提升，必须在声明后使用
console.log(a);				//报错
let a=10;

//let不可以重复定义
let a=10;
let a=20;
console.log(a);				//error


//let存在块级作用域，es6存在块级作用域
{
	let a=10;
}
console.log(a);			//error

//let声明的for循环变量，for作用域外访问不到，error:not declared
for(let i=0;i<10;i++){
  console.log(i);
}
console.log(i);			//error:i not declared

```



const声明

const声明一个常量，代码中有数据在执行过程中不变化，声明为常量

在声明时必须初始化，

不能重新赋值，

不能重复声明

基本值类型数据不可以修改，引用类型的值可以修改，因为const是对引用地址修改的限制，只要不改变内存地址，可以改该内存地址上的值

```
const age=26
age=36			//error

const arr=[10,10,12]
arr[1]=11			// [10,11,12]，ok
```



### 箭头函数

let c=a=>b

用a执行b，返回给c



a=>b

a：只有一个参数,可以省略括号；没有参数／多个不可省略；

b：一行可省{}，多行不可；都别省！



回调callback函数

在某些行为action完成后调用的函数

异步执行某项功能的函数应该提供一个 `callback` 参数用于在相应事件完成时调用。



### spread语法

展开可迭代对象

```
let arr = [3, 5, 1];
//...arr    将可迭代对象arr展开成3,5,1
alert( Math.max(...arr) ); // 5（spread 语法把数组转换为参数列表）


let arr = [3, 5, 1];
let arr2 = [8, 9, 15];
let merged = [0, ...arr, 2, ...arr2];   // 0,3,5,1,2,8,9,15（0，然后是 arr，然后是 2，然后是 arr2）

//使用 spread 语法将字符串转换为字符数组
let str = "Hello";
alert( [...str] ); // H,e,l,l,o
//使用 Array.from 来实现将字符串转换为字符数组
alert( Array.from(str) ); // H,e,l,l,o
//两种方法区别：Array.from 适用于类数组对象也适用于可迭代对象，Spread 语法只适用于可迭代对象。



```



Map

```
let level1Map=new Map()
temp.forEach((item,index)=>{
level1Map.set(item.deps_id,index)
})
for (let entry of level1Map) { // 与 recipeMap.entries() 相同
console.log(entry); // cucumber,500 (and so on)
}


```





### promise

```javascript
let promise=new Promise(function(resolve,reject) {
	//executor执行函数
})


```

传递给 `new Promise` 的函数被称为 **executor**。

当 `new Promise` 被创建，executor 会自动运行并尝试执行一项工作。

尝试结束后，如果成功则调用 `resolve`，如果出现 error 则调用 `reject`。



由 `new Promise` 构造器返回的 `promise` 对象具有以下内部属性：

 ![屏幕快照 2021-09-05 上午10.17.06](../H5C3/图片/屏幕快照 2021-09-05 上午10.17.06.png)

**executor 只能调用一个 `resolve` 或一个 `reject`。**

任何状态的更改都是最终的。所有其他的再对 `resolve` 和 `reject` 的调用都会被忽略：

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve("done");

  reject(new Error("…")); // 被忽略
  setTimeout(() => resolve("…")); // 被忽略
});


```



**Resolve/reject 可以立即进行**

```
let promise = new Promise(function(resolve, reject) {
  // 不花时间去做这项工作
  resolve(123); // 立即给出结果：123
});


```



#### promise封装

封装settimeout

```
function delay(ms) {
	return new Promise(resolve=>setTimeput(resolve,ms))
}
delay(3000).then(()=>{
	alert('after 3000ms');
	return delay(2000);
	}).then(value=>{
	....
	})
```





**访问state和result**

Promise 对象的 `state` 和 `result` 属性都是内部的。我们无法直接访问它们。使用 `.then`/`.catch`/`.finally` 方法。

then　

```javascript
promise.then(
  function(result) { 
    /* handle a successful result */ 
  },
  function(error) { 
    /* handle an error */ 
  }
);


```

`.then` 的第一个参数是一个函数，该函数将在 promise resolved 后运行并接收结果。

`.then` 的第二个参数也是一个函数，该函数将在 promise rejected 后运行并接收 error。



catch

只对 error 感兴趣，那么我们可以使用 `null` 作为第一个参数：`.then(null, errorHandlingFunction)`。或者我们也可以使用 `.catch(errorHandlingFunction)`，`.catch(f)` 调用是 `.then(null, f)` 的完全的模拟，它只是一个简写形式。

```javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// .catch(f) 与 promise.then(null, f) 一样
promise.catch(alert); // 1 秒后显示 "Error: Whoops!"

```



# 异步编程

## 同步、异步

同步：

上一个任务结束后下一个任务才执行，如果一个上一个任务执行需要很长时间，就会阻塞下一个人

异步：

一个任务分成两段，先执行第一段，然后转而执行下一个任务，等做好了准备，再回过头执行第二段。排在异步任务后面的代码，不用等待异步任务结束会马上运行，也就是说，**异步任务不具有”堵塞“效应**。



高阶函数

函数的一个参数是函数，函数的返回值也是函数



### 柯里化（Currying）???

是一个辅助函数，这个辅助函数实现对函数的转换

将 `f(a,b,c)` 转换为可以被以 `f(a)(b)(c)` 的形式进行调用。

参数可以传一个或多个，柯里化的原函数都可以正常调用，并且如果

参数数量不足，则返回原函数的偏函数／部分函数。

```
function sum(a,b,c,d) {
	return a+b+c+d
}

function curring(fn) {
	let args=[]
	const inner=(arr=[])=>{
		args.push(...arr)
		return args.length
	}
}
```



## 异步编程

### 单线程

所谓"单线程"，就是指一次只能完成一件任务。如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务，以此类推。

这种模式的好处是实现起来比较简单，执行环境相对单纯；坏处是只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。常见的浏览器无响应（假死），往往就是因为某一段Javascript代码长时间运行（比如死循环），导致整个页面卡在这个地方，其他任务无法执行。

### 同步与异步

"同步模式"就是上一段的模式，后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的；

"异步模式"则完全不同，每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。

"异步模式"非常重要。在浏览器端，耗时很长的操作都应该异步执行，避免浏览器失去响应，最好的例子就是Ajax操作。在服务器端，"异步模式"甚至是唯一的模式，因为执行环境是单线程的，如果允许同步执行所有http请求，服务器性能会急剧下降，很快就会失去响应。

### 如何进行异步编程

F1:回调函数

```javascript
//实现点击——隐藏——提示隐藏了——其他事件


//callback：隐藏后回调函数，显示弹窗
$("button").click(function(){					//f1:点击
  $("p").hide('slow',function() {			//f2:隐藏
    alert('...hided...')
  })
})

//不写成callback，先弹窗，再隐藏
$("button").click(function() {
  $("p").hide(1000);
  alert('...hided...')
})
```



F2:事件监听



F3:发布订阅



F4:promise



# 模块化module

模块内

写法1：单独export

```
//export文件
export const name='name1'
export function sayName() {
	return 'name:'+name;
}
```

```
//import文件
<script>
	import {name,sayName} from '相对路径'
</script>
```

写法2：统一export

```
const name='name'
function sayName() {
	return 'name:'+name;
}

export {name,sayName}
```

```
import {name,sayName} from '相对路径'
```



默认export deafult ，import的时候可以自定义名字，export模块内只能写一次

```
const name='name'
function sayName() {
	return 'name:'+name;
}

export {name,sayName}

const obj={
	...
}
export default obj
```

```
import objName,{name,sayName} from '...'
```



全部import,引入export模块内所有

```
import * as f from '...'
```



类模块化

```
class Person {
	constructor() {
	
	}
	sayName() {
		...
	}
}
export default Person;
```

```
import Person from '相对路径'
```



# api

### Math

Math.ceil()向大取整

Math.floor()向小取整

Math.round四舍五入



# BOM

#### 回流／重排 & 重绘

浏览器使用流式布局模型 (Flow Based Layout)。

浏览器会把`HTML`解析成`DOM`，把`CSS`解析成`CSSOM`，`DOM`和`CSSOM`合并就产生了`Render Tree`。

有了`RenderTree`，我们就知道了所有节点的样式，然后计算他们在页面上的大小和位置，最后把节点绘制到页面上。

**回流必将引起重绘，重绘不一定会引起回流**



**重排**

当`Render Tree`中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流／重排。

会导致回流的操作：

- 页面首次渲染
- 浏览器窗口大小发生改变
- 元素尺寸或位置发生改变
- 元素内容变化（文字数量或图片大小等等）
- 元素字体大小变化
- 添加或者删除**可见**的`DOM`元素
- 激活`CSS`伪类（例如：`:hover`）
- 查询某些属性或调用某些方法

一些常用且会导致回流的属性和方法：

- `clientWidth`、`clientHeight`、`clientTop`、`clientLeft`
- `offsetWidth`、`offsetHeight`、`offsetTop`、`offsetLeft`
- `scrollWidth`、`scrollHeight`、`scrollTop`、`scrollLeft`
- `scrollIntoView()`、`scrollIntoViewIfNeeded()`
- `getComputedStyle()`
- `getBoundingClientRect()`
- `scrollTo()`



**重绘**

当页面中元素样式的改变并不影响它在文档流中的位置时（例如：`color`、`background-color`、`visibility`等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。



**如何避免**

CSS

- 避免使用`table`布局。
- 尽可能在`DOM`树的最末端改变`class`。
- 避免设置多层内联样式。
- 将动画效果应用到`position`属性为`absolute`或`fixed`的元素上。
- 避免使用`CSS`表达式（例如：`calc()`）。

JavaScript

- 避免频繁操作样式，最好一次性重写`style`属性，或者将样式列表定义为`class`并一次性更改`class`属性。

  ```
  let ad=document.getElementById("ad");
  //不要单独写
  ad.style.width="100px";
  ad.style.height="100px";
  ad.style.background="100px";
  ...
  
  //直接写一个类
  div .ad {
  	width:100px;
  	height:100px;
  	background:red;
  }
  ```

  

- 避免频繁操作`DOM`，创建一个`documentFragment`，在它上面应用所有`DOM操作`，最后再把它添加到文档中。

- 需要复杂处理的元素，先为元素设置`display: none`，操作结束后再把它显示出来。因为在`display`属性为`none`的元素上进行的`DOM`操作不会引发回流和重绘。

- 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。操作缓存起来的变量，避免频繁读取

- 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。



# 防抖节流







# Ajax

通过XMLHttpRequest对象向服务器发送异步请求，获取服务器返回的数据，操作dom更新页面

### XMLHttpRequest对象

abort():如果请求已经发送，则停止当前请求。

getAllResponseHeaders():获取所有HTTP请求的响应头部，作为键值返回，如果没有收到响应，则返回null

getResponseHeader("key")函数:获取指定key的HTTP响应头，如果没有收到响应或者响应中不存在key对象
的报头，则返回null,

4. open("method","URL",[asyncFlag],"userName".["password"])函数:
建立对服务器的调用。
5中专国器
。method参数表示请求方式，可以为GET, POST或者PUT.
。URL参数表示请求的路径，可以使相对路径，也可以时绝对路径。
。后面3个是可选参数，分别表示是否异步，用户名，密码，其中asyncFlag = true表示异步，asyncFlag =
false表示同步，默认值为true,
5. send(content)函数:向服务器发送请求。
6. setRequestHeader(key,value)函数:设置请求头中属性为key的值为value,在设置请求头之前需要先调用
open(函数，设置header将随着send()函数一起发送。





## this指向

**标准函数中，this指向函数／方法的调用者**

创建构造函数实例时，需要new出这个实例，此时this就指向这个实例

普通函数执行，不new实例，this指向全局window

在对象方法内，对象调用了方法，方法的this指向该对象

匿名函数中的this指向window



**箭头函数中,this指向 定义箭头函数的上下文（箭头函数在哪个上下文里定义的就指向哪里）**



使用对象方法call() apply() bind()可以改变this指向

基于Function.prototype上的 `apply 、 call 和 bind `调用模式，这三个方法都可以显示的指定调用函数的 this 指向。`apply`接收参数的是数组，`call`接受参数列表，`` bind`方法通过传入一个对象，返回一个` this `绑定了传入对象的新函数。这个函数的 `this`指向除了使用`new `时会被改变，其他情况下都不会改变。若为空默认是指向全局对象window。



### 具体例子

创建构造函数实例时，需要new出这个实例，此时this就指向这个实例

```
function Person(name) {			//构造函数函数名大写
	this.name=name;
}
var p=new Person('person_name');		//p为实例
console.log(p.name)			//this指向p

```



普通函数执行，不new实例，this指向全局window

```
function person() {
	this.name=name
}
person('person_name')
console.log(window.name)

```



在对象方法内，对象调用了方法，方法的this指向该对象

```
var value=10;
var obj={
	value:100,
	get:()=>{
		console.log(this.value);
	}
}
obj.get();			//100，obj调用了get()


```



匿名函数中的this指向window

```javascript
function(arg) {	
	console.log(this)			//window
}
```



```javascript
var user={
	sport:'user_sport',
	data:[
		{name:'userName1',age:1},
    {name:'userName2',age:2}
	],
  clickHander:function() {
    console.log(this);			//user对象调用clickHander,this指向user
    var _this=this;			//保存
    this.data.forEach(function(person) {
			console.log(this)			//window
      console.log(person.name+'is playing'+ _this.sport)
    })
  }
}
user.clickHander();


```



**箭头函数中,this指向 定义箭头函数的上下文（箭头函数在哪个上下文里定义的就指向哪里）**

```javascript
function a() {
	this.name='aa'
	setTimeout(()=>{
		console.log(this.name)		//aa，箭头函数内this指向箭头函数的定义上下文
	},1000)
}

function b() {
	this.name='bb'
	setTimeout(function() {
		console.log(this.name)		//undefined,普通函数内this指向调用者，b是window调用的，window对象中没有name所以undefined
	},1000)
}
```





