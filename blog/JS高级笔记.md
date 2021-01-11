## JS高级笔记

1.函数和普通对象的区别是可以执行，数组和普通对象的区别是有数值下标

2.判断类型

typeof:返回数据类型的字符串表达，可判断undefined，number，string，boolean，function等类型

不能判断:null和object   object和array

instanceof:判断对象的具体类型

===：可判断null和undefined

3.实例对象

var p = new Person()

person理解为一类对象，p就理解为一类对象中拿出来的一个例子。

4.初始时赋初值null；结束时需要对垃圾对象进行回收时赋值为null。

5.变量类型和数据类型的区别

数据的类型：基本类型/对象类型

变量的类型：基本类型：数据/引用类型：地址值

6.数据，内存，变量

内存：存储数据的临时空间，存储数据和其对应的地址值。内存中的栈用于存储全局变量和局部变量，堆用于存储对象。

每个变量都会占用一小部分内存，变量值是内存中保存的数据。

变量是内存的标识，内存是用来存储数据的空间。

7.赋值和内存

a = xxx   xxx可能是变量，对象和数据值

2个变量指向同一个对象，通过一个变量修改内部数据，另一个变量看到的是修改之后的数据。

2个引用变量指向同一个对象，让其中一个引用变量指向另一个对象，另一引用变量依然指向前者。

```
 var obj1 = {name: 'Tom'}
 var obj2 = obj1
 obj2.age = 12
 console.log(999,obj1,obj2)
 function fn (obj) {
     obj.name = 'A'
 }
 fn(obj1)
 console.log(obj2.name) // A
        
  var a = {age:12}
  var b = a
  a = {name: 'BOB', age:13}
  console.log(b.age,a.age,a.name)
  function fn2 (a) {
      //会成为垃圾对象，这边通过构造函数创建了一个新的对象，只会对函数内部的a进行赋值，而函数体外面的a的并未改变
      a = {age:15}
  }
  fn2(a)
  console.log(a.age) //13
```

以a = {};这种方式赋值只改变了引用变量的内存地址，而不会改变数值

8.数据传递的问题

js调用函数时传递变量参数时，值传递还是引用传递

理解1：都是值传递(真实数值和引用值)

理解2：可能值，可能引用



9.JS引擎如何管理内存

(1).内存生命周期

​    *分配小内存空间，得到它的使用权

​    *存储数据，可以反复进行操作

​    *释放小内存空间

(2).释放内存

​    *局部变量：函数执行完**自动**释放

​    *对象：成为垃圾对象 ==>垃圾回收器回收

9.对象

(1)

*保存多个数据的封装体。

*用来保存多个数据的容器。

*一个对象代表现实中的一个事物。

(2)

使用对象可以统一管理多个数据

(3)

对象由属性和方法构成，方法本身也是一种特别的属性

(4)

访问属性内部数据：属性名：编码简单

​                                ['属性名']：编码复杂

​                                               属性名包含特殊字符：- 空格

​                                                变量名不确定

10.函数

实现特定功能的n条语句的封装体，只有函数是可以执行的，其他类型的数据不能执行。

使用函数是为了提高代码复用，便于阅读交流。

定义函数：函数声明，表达式

调用（执行）函数：test(): 直接调用

​                                Obj.test(): 通过对象调用

​                                new test(): new调用

​                                test.call()/apply(obj):  临时让test成为obj的方法调用

```
 var obj = {}
 function test2 () {
     this.xxx = "hjjhjj"
 }
 test2.call(obj)
 // 可以让一个函数成为指定任意对象的方法
```

**回调函数：

//自定义函数，未调用，它最终执行了

// dom事件回调函数

//定时器回调函数

//ajax请求回调函数

//生命周期回调函数

**IIFE

隐藏实现

不会污染外部（全局）命名空间

可使用该方式写js模块

11.this

（1）

任何函数本质上都是通过某个对象来调用的，没有直接指定就是window；

所有函数都有一个变量都有一个变量this；

它的值是调用函数的当前对象。

(2)

test(): window

p.test()： p

new test(): 新创建的对象

p.call(obj): obj 

12.原型和原型链

原型：

*函数的prototype属性，默认指向一个Object空对象(即称为： 原型对象)

*原型对象中都有一个constructor, 它指向函数对象

显式原型：

每个函数function都有一个prototype,在**定义函数**时自动添加，默认值是空的object对象。

隐式原型：

每个实例对象都有一个__ proto __，**创建对象**时自动添加，默认值为构造函数的prototype属性值。

对象的隐式原型的值为其对应构造函数的显式原型的值



ES6之前不可以直接操作隐式原型。



```
 function fun () {}

        console.log(Date.prototype.constructor === Date)
        console.log(fun.prototype.constructor === fun)
        fun.prototype.test = function (){
     console.log('test()')
 }

 var fn = new fun; //这里内部写入 this.__proto__ === fun.prototype
console.log(fn.__proto__ === fun.prototype) //true
```

原型链：

（1）访问对象属性,在自身属性中查找，找到返回

   ( 2 ) 如果没有，沿着__ proto __这条链向上查找，找到返回

   ( 3 )如果最终没找到，返回undefined(找到了Object原型对象的原型链顶端)

别名：隐式原型链

作用： 查找对象的属性(方法)   (不查找变量，变量查找在作用域链中寻找)

构造函数/原型/实体对象的关系

*var Foo = new Function()

Function = new Function()



*var Fn = new Object()

所有函数的__ proto __ 都是一样的



**函数的显式原型指向的对象默认是空Object实例对象(但Object不满足)

**所有函数都是Function的实例(包含Function)

**Object的原型对象是原型链的尽头



读取对象的属性值时：会自动到原型链中查找

设置对象属性值时：不会查找原型链，如果当前对象中没有此属性，直接添加此属性并设置其值

方法一般定义在原型中，属性一般通过构造函数定义在对象本身上。



13.instanceof

判断原理：

如果B函数的显式原型对象在A对象的原型链上，返回true，否则返回false





