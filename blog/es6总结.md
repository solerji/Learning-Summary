![es6-1](/pictures/blog/Es6.png)

今天对es6语法进行梳理。也对在公司业务中遇到的相关问题做一个基础回归，脑图是根据阮一峰的《ES6入门教程》写的。

一、ECMAScript 与 JavaScript的关系

ECMAScript相当于一个准则，来约束JavaScript；JavaScript是用来实现它的。随着ECMAScript版本上升，它越来越倾向于扩展和规范化JavaScript的规则，让它逐渐向面向对象的思路上走的更远。

二、为什么扩展，扩展了什么

1.因为函数作用域导致变量提升的问题，扩展了let和const

新增了块级作用域，解决了变量提升问题。规定了暂时性死区。

2.因为多变量赋值表达麻烦，规定了解构赋值（遵循模板语法）

以前赋值个对象或者数组得先new，再声明变量，再把参数塞进去。现在按键值组合按模板语法写进去就行。主要是所见即所得。

3.因为字符串遍历不方便，扩展了for...of...;因为Unicode表示看着不方便，就加了大括号。因为模板写法繁琐，所以用了模板字符串。

4.因为常有判断数值类型的需求，所以数值上扩展了isInteger,isNaN,isFinite等等……

5.函数上为了书写方便加了（...(rest参数和扩展运算符)， 箭头函数表达（省略function和{}））优化了尾调用，尾递归，name属性。

6.对象，同样规定了对象的name属性，一些简写规则， 一些遍历对象的方法

例如：Object.is()，比较两个值是否严格相等;Object.assign()，对象的合并（浅拷贝）。

Object.keys(), Object.values()，Object.entries()（返回遍历器对象）

ES2017将扩展运算符引入对象

三、新增的数据类型Symbol——为避免变量重复而生

表示独一无二的值。

四、新增的数据结构Set 和 Map（为了方便数组的遍历和去重操作，为了扩展对象的用途新增）

Set:类似数组，成员唯一，可直接用于去重。 Array.from方法可以将 Set 结构转为数组。 keys()，values()，entries()， foreach（）用于遍历。

WeakSet（里面只能放对象）

Map: 它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。（Map可以和对象，数组，JSON之间来回转换）

WeakMap（ 只接受对象作为键名（null除外），不接受其他类型的值作为键名； 键名所指向的对象，不计入垃圾回收机制）

五、新增的API ——proxy,reflect

Proxy 用于修改某些操作的默认行为，它大多用于拦截操作。 Vue3.0中要使用Proxy替代Object.defineProperty 来实现数据响应式。所以在这里我着重用一个例子来使用该API。

```
let example = {
  name: 'demo1',
  chinese: '例子1'
}


let handler = {
  get: (example, name) => {
    const exampleName = example.name + ' get '
    return exampleName
  },
  set: (example, name, value) => {
    console.log(example.name + 'set to' + value)
    example[name] = value
    return
  }
}


let exampleProxy = new Proxy(example, handler)
let newObj = Object.create(exampleProxy)


exampleProxy.name = 'demo2'
newObj.name = 'newObj'


console.log(exampleProxy)
console.log(newObj)
console.log(newObj.name)
```

打印结果：

![1580976916318](C:\Users\24744\AppData\Roaming\Typora\typora-user-images\1580976916318.png)

example是我们需要拦截的对象。

在这段代码中我使用了Proxy函数的get 和set 方法，通过set的打印值我们可以看出原始example值中的demo1在通过Proxy创建和原型对象创建后的赋值过程均成功被赋值，拦截生效，最后打印结果是第二次拦截后的原型对象newObj。虽然newObj本身没有name这个属性，但是根据原型链，会在exampleProxy上读取到name属性，之后会执行相对应的拦截操作。

Proxy如何替代Object.defineProperty实现数据绑定呢？

其实不是替代，只是对它的一个升级，Object.defineProperty 在使用过程中的一些弊端（比如不能数组变化），在Proxy中都通过拦截方法接口解决了。基本实现原理还是观察者模式。

Reflect：

```
let reflect = {
  name: '1'
}

let handler = {
  get(target, name, receiver) {
    if (name === "name") {
      console.log("success");
    } else {
      console.log("failure");
    }
    return Reflect.get(target, name, receiver)
  }
}

let reflectProxy = new Proxy(reflect, handler)
console.log(reflectProxy.name)
```

打印结果：

![1580977260760](C:\Users\24744\AppData\Roaming\Typora\typora-user-images\1580977260760.png)

Reflect对象的操作和Proxy对象的操作一一对应，在Proxy的拦截操作中，可以直接利用Reflect对象直接获取Proxy的默认值。

六、异步编程解决方案

一般的异步编程使用回调函数，事件监听，发布订阅解决。

1.Promise：一个保存未来才会结束的事件的容器

三种状态：Pending,Resolved,Rejected（异步操作的结果）

两种状态变化：

Pending=>Resolved,

Pending=>Rejected

then，catch方法是定义在原型对象Promise.prototype上的,里面返回的分别是成功和失败的回调。

在工作过程中遇到了要同步获取几个axios请求的需求，promise.all就可以把多个promise返回的结果一起获取，不过必须是请求结果都返回时才会获取。

2.Generator

(1)基础

遍历器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

具有 Symbol.iterator属性就可以生成遍历器对象，遍历器对象就可使用for...of遍历方式，其原理就是Symbol.iterator。解构，扩展运算符， yield*等等都可以调用了遍历器接口。

Generator 函数是一个状态机，还是一个遍历器对象生成函数。返回的遍历器对象（指向内部状态的一个指针），可以依次遍历 Generator 函数内部的每一个状态。 Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行。但没有执行器它是不会自动运行的。

```
function* Generator() {
  yield 'hello'; // 这个是已定义的内部状态,可以有多个内部状态
  return 'ending'
}
var hw = Generator()
// 返回的遍历器对象
hw.next()
// { value: 'hello', done: false }
hw.next()
// { value: 'ending', done: true }
```

(2)async

是Generator 函数的语法糖，内置了执行器（co模块）。将*替换为async,将yield替换为await。解决了Generator 函数需要执行器（co模块）才能运行的问题，它的返回值是Promise对象而非遍历器对象，较好操作。

只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。

七、类和model

1.类：类的出现是为了替代js用构造函数定义和生成对象而生的。它是js向面向对象思想前进的一步。

```
function example (x, y) {
  this.x = x
  this.y = y
}
example.prototype.toString = function () {
  return ‘(‘ + this.x + ‘,’ + this.y + ‘)'
}

var p = new example(1,2)

// 类的写法

class example (x, y) {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
  toString() {
    return ‘(‘ + this.x + ‘,’ + this.y + ‘)'
  }
} 

// 可以一次向类中添加多个方法
Object.assign(Point.prototype, {
  example1() {}，
  example2() {}
  …
})
```

继承：ES5通过修改原型链实现继承，每个函数在创建之初都会拥有同一个原型对象，通过对原型链的修改，可以达到继承的效果。ES6通过extends实现继承。

```
// es6 的继承
class Point {

constructor(x, y) {

this.x = x

this.y = y

}


toString() {
return '(' + this.x + ', ' + this.y + ')'

}
}
var point = new Point(2, 3)


class ColorPoint extends Point {

constructor(x, y, color) {

super(x, y) // 调用父类的constructor(x, y)

this.color = color

}

toString() {
return this.color + ' ' + super.toString() // 调用父类的toString()
  }
}
let cp = new ColorPoint(4, 5, '3')

console.log(cp)
```

super关键字：由于对象总是继承其他对象的，所以可以在任意一个对象中，使用super关键字。它只能用于子类的构造函数继承父类属性。

prototype、__proto__:每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Object.prototype只是一个普通对象(普通对象没有prototype属性，所以值是undefined)，Object.prototype是js原型链的最顶端，在js中如果A对象是由B函数构造的，那么A.proto === B.prototype。

```
class C extends Object {}
console.log(C.__proto__ === Object) // true
console.log(C.prototype.__proto__ === Object.prototype) // true
```

##### js中每一个对象或函数都有__proto__属性，但是只有函数对象才有prototype属性。内置的Object是其实也是一个函数对象，它是由Function创建的。Function是一个函数对象,因为Function.prototype__proto__指向Object.prototype。

私有属性：#表示其他面向对象语言中的private

2.Model

(1)默认使用严格模式

（2）export ,export default 

(3)import 

(4) async,defer:defer要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行；async一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。一句话，defer是“渲染完再执行”，async是“下载完就执行”。
