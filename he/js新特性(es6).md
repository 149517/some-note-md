> ECMAS6,2015年，版本变动最多，加入了新的语法特性
>
> 模块化，Promise，箭头函数，let，const，数组解构赋值，面向对象等



## ES6



#### 变量



* let

  * 不能重复声明，
  * 能够同时声明多个变量
  * **没有变量提升**
  * 块级作用域

  ```js
  {
  	let aa = 10;
      var bb = 110;
  }
  console.log(aa);	//报错
  console.log(bb);	//正常输出
  ```



> 使用 var 和 let 在循环中

```js
for (var i = 0;i<items.length;i++){
    items[i].onlick = function(){
        // 修改元素颜色
        // items[i].style.background = 'pink'; 无效
        this.style.background = 'pink';
    }
}
将for循环开头的 var改成let就能用，
使用 var声明的变量变成全局变量，在访问items[i]的时候，列表越界
```



* const
  * 声明时候一定要赋初始值
  * **常量的值不能够修改**
  * 块级作用域
  * **对数组和对象的元素修改，不算做对常量的修改，所以能够改变**
  * 在赋值数组和对象的时候，使用const能够避免误操作

```js
const list= ['aaa','bbb','ccc','ddd'];
list.push('fff');	//能够运行
list = 1900;	//报错
```





### 解构赋值

> 按照一定的模式从数组和对象中提取值，对变量进行赋值



##### 数组的解构

```js
const F4 = ['小沈阳','刘能','赵四','宋小宝'];
let [xiao,liu,zhao,song] = F4;
console.log(xiao,liu,zhao,song);	//小沈阳 刘能 赵四 宋小宝

```



##### 对象的解构

```js
const zhao = {
    name:'zhaobenshang',
    age:'34',
    xiaopin:function(){
        console.log('henduo');
    }
}
// 使用相同的结构
// 名字要和内部的键相同
let {name,age,xiaopin} = zhao;
console.log(name);		// zhaobenshang
console.log(age);		// 34
console.log(xiaopin);	//[Function: xiaopin]

// 函数调用
xiaopin();		// henduo
// 不必在  zhao.xiaopin()
```



### 模板字符串

* 使用  ``
* 可以在内容中直接出现换行符

```js
let str = `你好
            现在是晚上
            恍惚`;
console.log(str);

// 输出
你好
            现在是晚上
            恍惚

```

* 字符串拼接
  * 使用 ${} 的方式

```js
let a = '123';
let b = `${a}将其拼接`;
console.log(b);	//123将其拼接

```



### 简化对象的写法

```js
let name = 'xx';
let change = function(){
    console.log('学习es6');
}

// 书写对象
const my = {
    name,
    change,
    improve(){
        console.log('这是一个函数');
    }
}
console.log(my);	//{ name: 'xx', change: [Function: change],improve: [Function: improve]
// }

// 原始写法
const my = {
    name = name,
    change = change,
    improve = function(){
		console.log('这是一个函数');
    }
}
```



### 箭头函数

> ​          =>

* 声明一个函数

```js
let fn = function(a,b){
    return a+b
}
// 使用 =>

let fn = (a,b) =>{
    return a+b
}
```

* **this是静态的**
* this始终指向**函数声明时所在作用域下的 this 值**

```js
function getName(){
	console.log(this.name);
}
function getName2(){
	console.log(this.name);
}
window.name = 'xuxu';
const na{
    name:'buu';
}

// 直接调用
getName();		//xuxu
getName2();		//xuxu

// call 方法调用
getName.call(na);		//buu
getName2.call(na);		//xuxu  指向的是getName所在作用域(window)下的name
```

* 不能作为构造函数传参

```js
let Person = (name) => {
    this.name = name;
}
// 只能使用这种方法
let Person = function(name){
    this.name = name;
}
```

* 不能使用 arguments

```js
let fn = function(){
    console.log(arguments);
}
fn(1,2,3,4);

let fn = () =>{
    console.log(arguments);	// 没有定义
}
```

* 箭头函数的简写

  * 省略小括号 ---**当形参只有一个时候**
  * 省略花括号 ---**当只有一条语句时候**
    * 而且此时return也必须省略，语句的执行结果就是函数的返回值

  ```js
  let add = n => {
      console.log(n*2)
  }
  add(3);	//6
  ```

  ```js
  let fn = (n) => n*2;
  console.log(fn(3));	//6
  ```



#### 箭头函数的实践

##### 点击修改颜色

```html
<head>
    <meta charset="UTF-8">
    <title>点击改变颜色</title>
    <style>
        .box{
            width: 200px;
            height: 200px;
            background-color: #6273f5;
        }
    </style>
    <script>
        window.onload = function(){
            let box = document.querySelector('.box');

            box.addEventListener('click',function(){

                setInterval(function(){
                    // 这里不能修改，因为这里的 this 指向的是setInterval所在得对象，window，而不是box
                    this.style.background = 'pink';
                },2000);
            })
        }
    </script>
</head>
<body>
    <div class="box">

    </div>
</body>
```

* 修改方法

> 在setInterval 为开始之前把this赋值给其他变量
> 或者直接使用box

```js
            box.addEventListener('click',function(){
                let _this = this;
                setInterval(function(){
                    _this.style.background = 'pink';
                },2000);
            })
```

> 使用 箭头函数 =>
>
> 指向的是声明时候所在的作用域下的this的值
>
> 这里的this指向的是事件源box

```js
            box.addEventListener('click',function(){
                setInterval(() => {
                    this.style.background = 'pink';
                },2000);
            })
```



##### 从数组中返回偶数

```js
const arr = [1, 3, 4, 6, 7, 8, 2];
const fn = arr.filter(function(n) {
        if (n % 2 === 0) {
            return true
        }else{
            return false
        }
})
console.log(fn);	//[ 4, 6, 8, 2 ]


// 使用箭头函数方法
const re = arr.filter(item => item % 2 === 0);
console.log(re);	//[ 4, 6, 8, 2 ]

```



* 箭头函数**适用与于** this 无关的回调，定时器，数组的方法回调

* **不适合于**this 无关的回调，事件回调，对象的方法





#### call

> 它可以用来调用所有者对象作为参数的方法。
>
> 通过 `call()`，能够使用属于另一个对象的方法。

* 在这个对象下调用另一个对象的方法

```js
const my = {
    name,
    change,
    improve() {
        console.log('这是一个函数');
    }
}
console.log(my);
const Prin = {
    print : function () {
        console.log(name + " " + change);
    }
}
Prin.print.call(my);
// 输出
//xx function () {
//    console.log('学习es6');
//}

```



### 参数的初始值

> 允许给参数添加初始值

* 具有参数的形参位置一般靠后



* 与解构赋值结合

```js
// 原本写法
function connect(options){
    let host = options.host;
    let usename = options.username;
}
// 结合形参赋值
funciton connect({host='127.0.0.1',username,password,port}){
    console.log(host);	// host具有参数，使用的是 localhost
}
connect({
    host:'localhost',
    username:'root',
    password:'root',
    port:'3306'
})
```



### rest 参数

> 获取函数的实参，用来代替arguments



* arguments 返回的是一个对象

```js
function date(){
    console.log(arguments); 
    //[Arguments] { '0': '这个', '1': '是', '2': '什么' }

}
date('这个','是','什么');
```



* rest参数
  * 形参的位置添加 ```...args```
  * 必须放到参数的最后
  * 返回的是一个数组

```js
function date(...args){
    console.log(args);  
    //[ '这个', '是', '什么' ]

}
date('这个','是','什么');
```





### 扩展运算符

>  `...` 拓展运算符

* 能够将 数组 转化为逗号分割符分割的参数序列

```js
let arr = ['liu','ju','zhan'];
function start(){
    console.log(arguments);
}
start(arr);	//[Arguments] { '0': [ 'liu', 'ju', 'zhan' ] }
// 这里的数组只是作为一个参数


function start2(){
    console.log(arguments);
}
start2(...arr);		//[Arguments] { '0': 'liu', '1': 'ju', '2': 'zhan' }
// 使用 ... 变成了三个参数
```



* 数组合并

```js
// 使用以前做法
const arr = ['12','19'];
const arr2 = ['99','77'];
const hebing = arr.concat(arr2);
console.log(hebing);	//[ '12', '19', '99', '77' ]


// 使用 扩展运算符 

const bebing2 = [...arr,...arr2];
console.log(hebing2);	//[ '12', '19', '99', '77' ]

```



* 数组的克隆

```js
const aa = ['a','b','c'];
const bb = [...aa];
console.log(bb);
// 当数据有复合类型的时候，只是浅拷贝
```

> 浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。



* 伪数组转化为真正的数组
* 也能够将 arguments 转化为数组

```js
let sp = document.querySelectorAll('span');
console.log(sp);	//NodeList(4)
nst spp = [...sp];
console.log(spp);	//Array(4)
```

> **NodeList 对象代表一个有顺序的节点列表。**



### Symbol

> 新的原始的数据类型，表示独一无二的值
>
> 第七种数据类型
>
> 类似于字符串的数据类型



* 值唯一，解决命名冲突的问题
* 不能与其他数据进行运算



```js
// 创建Symbol
let s = Symbol()
console.log(s,typeof s)     //Symbol() symbol

// 在内部实现了唯一性
let s2 = Symbol('张三');
let s3 = Symbol('张三');
console.log(s2 === s3);     //false

// 第三种创建方法
let s4 = Symbol.for('zhangsan');
let s5 = Symbol.for('zhangsan');
console.log(s4,typeof s4);    //Symbol(zhangsan) symbol

console.log(s4 === s5);     // true
```



#### Symbol 创建对象属性

* 给**对象添加属性和方法**

>  在外部添加方法的时候，需要确定原来的对象中是否有该方法
>  而使用Symbol(),给对象添加的方法是唯一的，

```js
let game = {
    name:'wee',
    up:function(){
        console.log('up');
    }

}

// 声明一个对象
let methods = {
    up:Symbol(),
    down:Symbol()
};

// 给game添加新的方法
game[methods.up] = function(){
    console.log('向上');
}
game[methods.down] = function(){
    console.log('向下');
}

console.log(game);
```

> 添加Symbol类型的方法
>
>    // Symbol():aa
>    // 不能够直接这样添加，因为Symbol 是一个标识符，不能作为一个变量

```js
let youxi = {
    name:'liangrenx',
    [Symbol('say')]:function(){
        console.log('I can');
    },
    [Symbol('not')]:function(){
        console.log("I can't");
    }
   // Symbol():aa
   // 不能够直接这样添加，因为Symbol 是一个标识符，不能作为一个变量
}
```



#### Symbol 内置值

> Symbol的属性，将作为对象的属性存在
>
> 共11种

##### Symbol.hasInstance

* **自己控制类型检测**

```js
class Person{
    static [Symbol.hasInstance](param){
        console.log(param);
        console.log('被用来检测类型了');
        return ture;	//返回false就是false
    }
}

let o = {};

console.log(o instanceof Person);

// {}
// 被用来检测类型了
// true

```



##### Symbol.isConcatSpreadable

* **控制对象是否展开**
* 值为布尔值

```js
let arr = [1, 2, 34];
let arr2 = [5, 6, 78];
console.log(arr.concat(arr2));	// [ 1, 2, 34, 5, 6, 78 ]

arr2[Symbol.isConcatSpreadable] = false;	// 不可展开

console.log(arr.concat(arr2));
// 第四个值是 [5,6,78]
//[ 1, 2, 34, [ 5, 6, 78, [Symbol(Symbol.isConcatSpreadable)]: false ] ]
```



##### ...



### 迭代器

> 迭代器是一种接口，为不同的数据提供统一的访问机制
>
> 任何数据类型只要部署了iterator 就口，就能够完成遍历操作



> 在对象内部具有  "[Symbol.iterator] ()方法"
>
> [Symbol.iterator] ()使用next() 方法，不断的执行数据结构成员



#### for ... of

Iterator接口主要供 for ... of 消费

具备iterator 接口的数据（可用 for .. of 遍历）

> Array  arguments  Set  Map String  TypedArray  NodeList



```js
let arr = ['tan','sun','zhu','sha'];
for (let v of arr){
    console.log(v);
}
//tan
//sun
//zhu
//sha

// for in
for (let v in arr){
    console.log(v);
}
//0
//1
//2
//3
```



#### 自定义遍历数据

```js
const banj = {
    name: 'ban',
    staus: [
        'ming',
        'hong',
        'tian',
        'gnag'
    ],
    [Symbol.iterator]() {
        // 索引
        let index = 0;

        let _this = this;
        return {
            next: function () {
                if (index < _this.staus.length) {
                    const result = {value: _this.staus[index], done: false};
                    //
                    index++;
                    return result;
                } else {
                    return {value: undefined, done: true};
                }
            }
        };
    }
}
// 遍历这个对象
for(let v of banj){
    console.log(v);
}
//ming
//hong
//tian
//gnag

```





### 生成器

> 是一个特殊的函数
>
> 用于实现异步编程



#### 创建

> 创建的时候 函数名称前面需要加 * 

```js
            function * gen(){
                console.log('hello');
            }

            let iter = gen()
            console.log(iter);
// 输出的是迭代器对象，
// 调用需要需要调用next()
            iter.next()


```

* yield
  * 函数代码的分割符

```js
function * gen(){
    console.log(111);
    yield '这是第一个分割';
    console.log(222);
    yield '第二份';
    console.log(333);
    yield '第三份';
    console.log(444);
}
let iter = gen();
iter.next();	//111
iter.next();	//222
iter.next();	//333
iter.next();	//444

for (let v of gen()){
    console.log(v);
    // 每次调用的返回的结果是 yield 后面的 内容
}
//这是第一个分割
//222
//第二份
//333
//第三份
//444

```



#### 生成器函数参数

> 在调用 next() 时候，next( )内可以传递参数，作为前一个 yield的返回值

```js
function * gen2(){
    let f_1 = yield 123;
    console.log(f_1);
    let f_2 = yield 456;
    console.log(f_2);
    let f_3 = yield 789;
    console.log(f_3);
}
let iter = gen2();
console.log(iter.next())
console.log(iter.next('aaa'))
console.log(iter.next('bbb'))
/* 
{ value: 123, done: false }
aaa
{ value: 456, done: false }
bbb
{ value: 789, done: false }
*/
```

* 使用定时器实现异步

```js
function one() {
    setTimeout(function () {
        console.log('aaaa');
        iter2.next()
    }, 1000)
}
function two() {
    setTimeout(function () {
        console.log('bbbb');
        iter2.next()
    }, 2000)
}

function three() {
    setTimeout(function () {
        console.log('cccc');
    }, 3000)
}
//
function* exert() {
    yield one();
    yield two();
    yield three();
}
let iter2 = exert()
iter2.next()
```

* 回调地狱

```js
setTimeout(()=>{
    console.log(111);
    setTimeout(()=>{
        console.log(222);
        setTimeout(()=>{
            console.log(333);
            setTimeout(()=>{
                console.log(4444);
            },4000);
        },3000);
    },2000);
},1000);
```

* 实例

```js
function getUsers(){
    setTimeout(()=>{
        let data = '用户数据';
        // 调用next,方法并且将数据传入
        iterator.next(data);
    },1000);
}
function getorders(){
    setTimeout(()=>{
        let data = '订单数据';
        iterator.next(data);
    },1000);
}
function getGoods(){
    setTimeout(()=>{
        let data = '商品数据';
        iterator.next(data);
    },1000)
}

function *gen3(){
    let users = yield getUsers();
    console.log(users);		// 用户数据
    let orders = yield getorders();
    console.log(orders);	// 订单数据
    let goods = yield getGoods();
    console.log(goods);		// 商品数据
}
let iterator = gen3()
iterator.next();
```





### Promise

> 异步编程的新的解决方案
>
> Promise是一个构造函数
>
> 用来封装异步操作并获取失败或者成功的结果



* 实例化
  * 调用resolve,和reject方法，改变 p 的状态

```js
const p = new Promise(function(resolve,reject){
    setTimeout(function(){
        // let data = 'shuju';
       	// 读取成功
        // resolve(data);

        let data = 'error';
        reject(data);

    },1000);
});
p.then(function (value){
    // 第一个参数，是成功的形参。读取成功则调用第一个回调
    console.log(value);
},function(reason){
    // 第二个参数，失败的返回调用的参数
    console.error(reason);
})
```



* Promise 读取文件
  * 在异步操作较多的时候

> 使用node.js中的文件读取

```js
fs.readFile('../file/123.txt','utf-8',(err,data)=>{
    // 读取失败则抛出错误
    if(err) throw err;
    // 成功则输出
    console.log(data.toString())
})
```

> 使用 Promise 封装

```js
const p = new Promise(function(resolve,reject){
    fs.readFile('../file/123.txt','utf-8',(err,data)=>{
        if(err) reject(err);

        // 如果成功
        resolve(data);
    });
});

p.then(function (value){
    // 成功的
    console.log(value.toString());
},function(reason){
    console.log('读取失败');
})
```



#### Promise-then

> 如果前面的 promise 对象读取成功则执行第一个回调函数，否则执行第二个
>
> then返回的对象是 Promise 对象，对象状态由回调函数的执行结果决定





#### Promise-catch

> 根据 Pronmise 对象 执行错误的操作
>
> 相当于Promise-then 的失败回调





### Set

>  集合
>
> 类似于数组，成员唯一

* 能够使用扩展运算符号
* 能使用 for...of.. 遍历



```js
let s = new Set();
let s2 = new Set(['1','2','3','4']);

//元素个数
s2.size

// 新增元素
s2.add('6');

// 删除元素
s2.delete('1')

// 检测
s2.has('5');		// false

// 清空
s2.clear();

// 遍历
for(let i of s2){
    console.log(r);
}
```



* 数组去重

```js
let arr = [1, 2, 3, 4, 55, 3, 2, 1,];
let result = [...new Set(arr)];
console.log(result);	//[ 1, 2, 3, 4, 55 ]
```



* 交集

```js
let arr2 = [2,3,5,6,7,7];

let res = result.filter(item => new Set(arr2).has(item));
console.log(res);
```



* 并集、

```js
let res2 = [...new Set([...arr,...arr2])];
// 数组合并后转化为集合去重
console.log(res2);
```



* 差集

```js
// 相对于某个数组来说的
// 相当于交集的反运算
let diff = [...new Set(arr)].filter(item=>!(new Set(arr2).has(item)));
console.log(diff);		//[ 1, 4, 55 ]

```



### Map

> 一种数据结构，类似于对象
>
> 键和值的集合，但是键的范围不仅限于字符串，各种类型的值，包括对象都可以作为键
>
> 具有iterator接口，可以使用扩展运算符合 for...of 遍历

* 创建

```js
let m = new Map();
```

* 添加

```js
m.set('name','xuxu');
let key = {
    school:'goog',
}
// 将对象作为键写入
m.set(key,['chenddu']);
```

* size

```js
m.size()
// 个数
```

* 删除

```js
m.delete('键名');
```

* 获取

```js
m.get('键名');
```

* 清空

```js
m.clear();
```

* 遍历

```js
for (let v of m){
    console.log(v);
}
```



### class

> 类

* 使用构造函数
  * 所有的 JavaScript 对象都会从一个 prototype（原型对象）中继承属性和方法

```js
function Phone(brand,price){
    this.brand = brand;
    this.price = price;
}
// 添加方法
Phone.prototype.call = function(){
    console.log('打电话');
}
// 实例化对象
let huawei = new Phone('华为',5432);
huawei.call();
console.log(huawei);
```

* 使用类
  * 方法需要使用 **方法名(){}** 的方式
  *  构造方法，名字不能修改，
  * 也可以不写构造方法

```js
class Phone{
    // 构造方法，名字不能修改，
    
    constructor(brand,price){
        this.brand = brand;
        this.price = price;
    }
    // 方法需要使用这样的语法,不能用es5
    // es5写法,call:function(){}
    call(){
        console.log('dadianhua');
    }
}
let one = new Phone('1+',2999);
console.log(one);
```



#### class静态成员



```js
function shouji(){

}
shouji.name = 'shou';
shouji.change = function(){
    console.log('Change the world');
}
shouji.prototype.size = '5.5inch';

let nokia = new Phone()

console.log(nokia.name);	// undefined
nokia.change();				// 报错
console.log(nokia.size);	//5.5inch
```



> 对于static标注的属性，只属于类，对象不能使用

```js
class Phone{
    //静态属性
    static name = '手机';
    static change(){
        console.log('Change the world')
    }
}
let nakia = new Phone();
console.log(nokia.name);	//name
console.log(Phone.name);	//手机
```



#### 继承

##### 构造函数实现继承

```js
function Phone(brand,price){
    this.brand = brand;
    this.price = price;
}

function SmartPhone(brand,price,color,size){
    // 这里的this，指向的是SmartPhone
    Phone.call(this,brand,price);
    this.color = color;
    this.size = size;
}

// 设置子级构造函数的原型
SmartPhone.prototype = new Phone;
```



##### class的继承

> 需要添加 extends 关键字
>
> 继承的属性使用 super

```js
class Phone{
    //构造方法
    constructor(brand,price){
        this.brand = brand;
        this.price = price;
    }
    call(){
        console.log('call');
    }
}
// 继承
// 需要添加 extends 关键字
class SmartPhone extends Phone{
    
    constructor(brand,price,color,size){
       	// 继承的属性使用 super
        super(brand,price);
        this.color = color;
        this.size = size;
    }

}
```



#### class 子类对父类方法的重写

> 子类不能调用父类的重名方法
>
> 只能进行重写



#### get 和 set

```js
class Phone{
    get price(){
        return 'hhh';
    }
    set price(newVa){
        console.log('修改')
    }
}

// 实例化对象
let s = new Phone();
console.log(s.price);	// 返回的是 hhh ，通常用于动态的计算
s.price = 'free';		// 赋值修改后掉用 set. 通常用于判断和控制
```





***



### 数值扩展



> Number.EPSILON 

* 是 js 的最小精度
  * 用于浮点数的数值比较

```js
function equal(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
}

console.log(0.1 + 0.2 === 0.3);			//false
console.log(equal(0.1 + 0.2, 0.3));		//true
```



> 二进制和八进制

* `0b`开头表示二进制
* `0o`开头表示八进制
* `0x`是16进制



> Number.isFinite( )

* 检测一个数是否是 **有限数**

```js
console.log(Number.isFinite(100));			//false
console.log(Number.isFinite(100/0));		//false
console.log(Number.isFinite(Infinity));		//false
```



> Number.isNan

* 检测数值是否是 NaN



> Number.parseInt 和 Number.parseFloat

* 字符串转为整数和浮点数
* 数字开头



> Number.isInteger()

* 判断一个数是否为你整数



> Math.trunc

* 将数字的小数部分抹掉



> Math.sign

* 判断一个数是正数，负数，还是0
* 正数返回 1
* 负数返回 -1
* 0 返回 0



### 对象方法扩展



> Object.is( , )

* 判断两个数是否完全相等
  * 和 === 的区别

```js
// 在判断NaN 的时候，除了不等，其他的都是 false
console.log(NaN === NaN);	// false
console.log(Object.is(NaN,NaN));	//true
```



> Object.assign(  模板 ( 被覆盖 )  ,  覆盖项 )

* 对象的合并
  * 同名的将会被后一个对象替换

```js
const config = {
    host: 'localhost',
    port: 3305,
    pass: 'root',
}
const config2 = {
    host: '000',
    port: 33,
    name: 'nn',
}
console.log(Object.assign(config,config2));

// { host: '000', port: 33, pass: 'root', name: 'nn' }

```



> Object.setPrototypeOf

* 设置原型对象

```js
        const school = {
            name:'xuxu'
        }
        const cities = {
            dizhi:'beij'
        }
        Object.setPrototypeOf(school,cities);
        console.log(Object.getPrototypeOf(school));		//{ dizhi: 'beij' }

        console.log(school);		//{ name: 'xuxu' }

```





****



### 模块化



#### 模块化命令

> export

* 用于规定模块化的对外接口

> import

* 输入其他模块提供的功能



mo1.js

```js
export let sc = 'xxx';
export function teach(){
    console.log('js');
}
```

ce_mo.html

```html
<script type='module'>
	import * as mo from '../js/mo1.js';
    console.log(mo);
</script>
```



##### export



* 分别暴露
  * 在 变量和函数 之前添加`export`

* 统一暴露
  * `export{变量名1，变量名2，···}`
* 默认暴露

```js
export default{
    name:'xx',
    call:function(){
       	console.log('aa');
    }
}
```



##### import 

* 通用的导入方式，

`import * as mo from '../js/demo.js'`

* 解构赋值形式

`import {bian1,bian2} from '../js/demo.js'`

> 导入多个模块重名的变量将会出错
>
> 使用别名

`import (bian1 as b1,bian3) from '../js/demo2.js'`

* 默认暴露的解决
  * default 需要取别名

`import {default as mo3} from '../js/demo3.js'`

* 只针对默认暴露

`import mo4 from '../js/demo4.js'`





> 引入多个 js 模块的 js 文件导入 script

app.js

```js
import * as m1 from '../m1.js'
import * as m2 from '../m2.js'
import * as m3 from '../m3.js'

```

001.html

```html
<script src='../js/app.js' type='module'></script>
```







## ES7



> Array.includes('')

* 判断数组中是否含有某个元素
* 返回的是 boolean

`indexOf()`返回的是1和-1



>  **

* 幂运算
* `Math.pow(2,10) === 2 ** 10`



## ES8

### async 和 await

async 和 await 两种语法结合能让异步代码想同步代码一样



### async

> 返回的是 promise 对象
>
> promise对象的结果又 async 函数执行的返回值决定

* async函数

```js
async function fn(){
    
    return 'zifu';
    // 返回的不是 promise 对象则 返回的值就是一个成功的promise 对象
    throw new Error('chucuo');	// 返回的是错误的promise 对象
    
    return new Promise（resolve,reject)=>{
        resolve('成功的数据');
        // reject('shibai')
    }
}
const res = fn()
console.log(res);
```



### await

> await 必须 放在 async 中
>
> 返回的是promise成功的值

```js
const p = new Promise((resolve, reject) => {
    // resolve(’成功的值');

    // 失败的值
    reject('shibail');
})

async function ma() {
    // 失败的值需要用try catch进行捕获
    try {
        let res = await p;
        consle.log(res);	// 成功的值
    } catch (e) {
        console.log(e);	// shibail
    }

}

ma();

```



* async 和 awit 读取文件

```js
const fs = require('fs');

function read123() {
    return new Promise((resolve, reject) => {
        fs.readFile('../file/123.txt', 'utf-8', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

function read222() {
    return new Promise((resolve, reject) => {
        fs.readFile('../file/222.txt', 'utf-8', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

function read333() {
    return new Promise((resolve, reject) => {
        fs.readFile('../file/333.txt', 'utf-8', (err, data) => {
            if (err) reject(err);
            resolve(data);
        })
    })
}

async function main() {
    let r123 = await read123();

    let r222 = await read222();

    let r333 = await read333();

    console.log(r123.toString(), r222.toString(), r333.toString());
}

main();
```





## ES8

> Object.values

* 返回一个给定对象的所有的可枚举属性的数组

```js
const sc = {
    name:'shangguigu',
    cities:['1','2','3'],
    xuex:['22','33','44']
};
// 获取对象所有的键
console.log(Object.keys(sc));	//[ 'name', 'cities', 'xuex' ]

//获取对象所有的值
console.log(Object.values(sc));	//[ 'shangguigu', [ '1', '2', '3' ], [ '22', '33', '44' ] ]
```



> Onbject.entries

* 返回一个给定对象自身可遍历的属性 
* [key,value] 的数组
* 可以用于创建  **Map**

```js
console.log(Object.entries(sc));
//[
//  [ 'name', 'shangguigu' ],
//  [ 'cities', [ '1', '2', '3' ] ],
//  [ 'xuex', [ '22', '33', '44' ] ]
//]

```



> Object.getOwnPropertyDescriptors()

* 对象属性的描述对象
* 用于进行深层次的对象克隆





## ES9



### 对象的 rest 参数

> 像数组一样的 rest 参数，

```js
function connect({host,port,...user}){
    console.log(host);
    console.log(port);
    console.log(user);
    //localhost
	//8800
	//{ username: 'xuxu', password: 'root', type: 'master' }

}
connect({
    host:'localhost',
    post:8800,
    username:'xuxu',
    password:'root',
    type:'master'
});
```



### 扩展运算符

> 针对于对象的扩展

```js
const skillOne = {
    Q:'天音波'
}
const skillTwo = {
    W:'金钟罩'
}
const skillThree = {
    E:'天雷破'
}
const skillFour = {
    R:'猛龙摆尾'
}
const mangsen = {...skillOne,...skillTwo,...skillThree,...skillFour};
console.log(mangsen);
// { Q: '天音波', W: '金钟罩', E: '天雷破', R: '猛龙摆尾' }

```



### 正则命名分组

> 在正则提取中有小括号包含的作为一组
>
> 在括号内部 加入 	`?<name>` 作为这一组的名字
>
> 使用命名可以在结构发生改变的时候依旧得到想要的值

```js
let str = '<a href="http://www.atguigu.com">尚硅谷</a>';
const reg = /<a href="(?<url>.*)">(?<text>.*)<\/a>/;

const result = reg.exec(str);
console.log(result);

/*
[
  '<a href="http://www.atguigu.com">尚硅谷</a>',
  'http://www.atguigu.com',
  '尚硅谷',
  index: 0,
  input: '<a href="http://www.atguigu.com">尚硅谷</a>',
  groups: [Object: null prototype] {
    url: 'http://www.atguigu.com',
    text: '尚硅谷'
  }
]

*/
```

* 不使用命名

```js
// 不使用命名
let str2 = '<a href="http://www.atguigu.com">尚硅谷</a>';
const reg2 = /<a href="(.*)">(.*)<\/a>/;
console.log(result);
console.log(result[1]);
console.log(result[2]);
```



### 正则扩展-反向断言

* 正向断言

```js
let str = 'js23323你不知道8979啦啦啦';
// 提取8979
const res = /\d+(?=啦)/;
const reg = res.exec(str);

console.log(reg);
```

* 反向断言

```js
let str = 'js23323你不知道8979啦啦啦';
// 提取8979
const res = /(?<=道)\d+/;
const reg = res.exec(str);

console.log(reg);
```



### dotAll 模式

> dot  `.` 元字符   
>
> 除换行以外的任意字符

**dotAll 能够匹配换行**

后面加上 s   `\  \s`





## ES10

> Object.fromEntries( )

* 创建对象
  * 接收一个二维数组或者是Map
  * 与Onbject.entries相反

```js
const result = Object.fromEntries([
    ['name','xx'],
    ['age','11']
])
console.log(result);	//{ name: 'xx', age: '11' }
```

```js
const m = new Map();
m.set('name','gu');
const res = Object.fromEntries(m);
console.log(res);		//{ name: 'gu' }

```



> trimStart()

* 清除字符串左侧的空白



> trimEnd()

* 清除字符串右侧的空白





>  flat()

* 将多维数组转化为低维数组
* 降一维
* 参数为深度，默认是1

```js
const arr = [1,2,3,[4,5,[6,7,9]]];
console.log(arr.flat());	//[ 1, 2, 3, 4, 5, [ 6, 7, 9 ] ]

// 3维遍一维
arr.flat(2);
```



> flatMap()

* 结合map() 和 flat()
* 将复杂数组转化为简单数组

```js
const arr = [1,2,3,4];
const result = arr.map(item => [item*10]); //[ [ 10 ], [ 20 ], [ 30 ], [ 40 ] ]

const res = arr.flatMap(item => [item*10]);
console.log(res);
// [ 10, 20, 30, 40 ]

```



> Symbol.prototype.description

* 获取Symbol 的字符串描述

```js
let s = Symbol('shang');
console.log(s.description);	//shang
```



## ES11

### 私有属性

* 私有属性只能在 类 内部访问

> #属性名：'value'，

```js
class Person{
    //公有属性
    name;
    //私有属性
    #age;
    #weight
    constructor(name,age,weight){
		this.name = name;
        this.#age = age;
        this.#weight = weight;
    }
    // 类内部的访问
    intro(){
        console.log(#age);
        console.log(#weight);
    }
}

const p = new Person('hong',23,'45');
console.log(p);			//Person { name: 'hong' }
console.log(p.#age);	//报错
```



### 方法

> Promise.allSettled

* 返回的结果始终是成功的
* 而且返回的值是promise的结果都有

```js
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('内容');
    }, 1000);
});
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        // resolce('内容2');
        reject('chucuo');
    }, 1000)
})
const res = Promise.all([p1, p2]);
console.log(res);
```



`PRomise.all([p1,p2])`

* 全部成功时候才返回成功
* 否则返回错误的执行



> String.matchAll( )

* 获取批量匹配的结果

* `exec()`需要进行循环

* **返回的结果是一个可迭代对象**
  * 使用for of 和 扩张运算符展开
  * [..res]





### 可选链操作符

* 用于判断传入变量
* 可选链操作符，不用逐级判断
* ` ?. `

```js
function main(config) {

    // const dbHost = config && config.db && config.db.host;
    // 可选链操作符，不用逐级判断
    const dbHost = config ?. db ?. host;
    console.log(dbHost);	//2333 
    // 即使不传递参数也不会报错
}

main({
    db:{
        host: '2333',
        username: 'root'
    },
})
```



### 动态import

* 按需加载
* 不在开头导入，在需要的时候导入

```js
btn.onclick = function(){
    import('../js/hello.js').then(module =>{
        module.hello();
    })
}
```



### BigInt

* 大整数
* 用于进行更大的数值运算

> 在普通的数字类型后面添加 一个 `n`

```js
let n = 234n;
console.log(n,typeof n);	//234n bigint

```



> BIgInt( )

* 将普通数值转化为 大整形
* 不能转化 **浮点数**



> 将数字转化为大整形进行运算

* `Number.MAX_SAFE_INTEGER`最大安全整数
* BigInt 不能和 其他的数字进行 **相加**



```js
let max = Number.MAX_SAFE_INTEGER;
console.log(max);		//9007199254740991
console.log(max + 1);	//9007199254740992
console.log(max + 2);	//9007199254740992

console.log(BigInt(max) + BigInt(1));	//9007199254740992n
console.log(BigInt(max) + BigInt(2));	//9007199254740993n
console.log(BigInt(max) + BigInt(3));	//9007199254740994n

```



### globalThis

#### 全局对象

> 始终指向全局对象，忽略所在环境
