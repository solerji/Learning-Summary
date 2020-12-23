## JavaScript 基础

### 1.基本数据类型(6，5)

##### Number  

*运算问题，浮点数运算不准确

Number.MIN_VALUE, Number.MAX_VALUE

 5e-324 1.7976931348623157e+308

##### Null (typeof 输出object类型，为空)

##### Undefined(为未定义，typeof 输出undefined类型)

##### String

##### Boolean (true false 输入数字均为数字类型)

其他类型：

##### Object



### 强制类型转换：

**->String**

(1).toString()

不会影响原变量，会将转换结果返回

对于null和undefined没有toString()方法

(2).String(b)

变成参数返回该值，null返回"null",undefined返回"undefined"

对于Number和Boolean来说调用的是toString()方法

对于null和undefined不会调用toString()方法

**->Number(数字类型的值在前端为了保持它值的一致，最好在使用前都对它的类型做一个标准转换，否则可能会导致数值不能正确使用，也可能导致浏览器报错)**

(1).Number(a)

纯数值字符串直接转数字

带有字符串转换为NaN形式

字符串为空格会转为0

boolean转换为0/1

(2).字符串转数字

只读到数字结束的位数

parseInt()

parseFloat()

**-> 进制转换**

十六进制：0x

八进制：0

二进制可能会导致浏览器无法识别，IE内核可能会对此有不同处理

**-> Boolean**

（1）Boolean()函数

NaN, null,0 => false

"", undefiend => false

*对象都是true

**-> 普通运算符**

强制类型会先将除String转类型换为Number类型。

任何值与NaN做运算都得NaN。

+:

任何值和字符串相加都会转换为字符串

String使用"+"会直接拼接。

增加空串时可以用作字符串类型转换(隐式类型转换)

例：

Result = 1 + 2 +"3"

Result = "1" + 2 + 3

注意是先做了类型转换还是先做了加减运算，结果可能不一致。

-，*，/ 都会先转换为Number类型  （隐式类型转换2）

*: 

let result = 2 * 2;

// undefined本身 转换为Number类型是NaN

result = 2 * undefined;

// null本身 转换为NaN类型是0

result = 2 * null;

符号 -> 自增(自减同理)

新值：

++a

原值：

a++

例： 陷阱问题

var c = 10; 

c++;

console.log(c++)

例：自增自减理解

​       // var n1 = 10, n2=20;

​        // var n = n1++; // 新值：n1 = 11 原值：n1++ = 10

​        // console.log('n='+n);

​        // //10

​        // console.log('n1='+n1);

​        // //11

​        // n = ++n1; //n1 = 12 ++n1 = 12

​        // console.log('n='+n);

​        // // 12

​        // console.log('n1='+n1);

​        // // 12

​        // n = n2 --;

​        // console.log('n='+n);

​        // //20

​        // console.log('n2='+n2);

​        // //19

​        // n = --n2;

​        // console.log('n='+n);

​        // //18

​        // console.log('n2='+n2);

​        // //18

##### ->逻辑运算(！&&  ||)

隐式类型转换：

为任意数据类型做两次非运算，从其他类型转换为Boolean

(Boolean值)

js中的与（&&）属于短路的与(第一个值为false不看第二个值)

js中的或（||）属于短路的与(第一个值为true不看第二个值)

(非Boolean值)

先转换为Boolean值，再运算(返回值为原值)

与（&&）运算：如果第一个值为true，则返回第二个值

​                          如果第一个值为false,返回第一个值

或（||）运算：如果第一个值为false，则返回第二个值

​                        如果第一个值为true,返回第一个值

##### ->赋值运算符(+=，-=，*=，/=)

##### ->关系运算符(>,<,>=,<=)

对于非数值比较会先转换成数字

如果符号两侧都是字符串，不会转换数字，而会比较字符串中的Unicode编码

任何值和NaN做比较都是false

##### ->相等运算符(==)

将比较值的类型转换为Number类型

Undefined 衍生至null，这两个值做相等判断会返回true

NaN不和任何值相等，包括它本身

isNaN()判断是否为NaN

##### ->不等运算符(!=)

将比较值的类型转换为Number类型

##### ->(===,!==)

不做值的类型转换，类型不同会返回false

##### ->三元运算符

如果比较的值为一个非boolean值那么会先转换成boolean再运算

#### 运算符优先级

##### ()>自增自减>普通运算符>关系运算符>相等，不等运算符>逻辑运算符>赋值运算符>三元运算>,

## 2.语句

代码块：js的代码块只用于分组

条件分支语句：if…else

​                         If…else if …else

​                         switch(条件为true如果不写break会继续向下执行）

​                        *注意switch的判断条件

​                       例：

​        let num = 69;

​      // 方式一

​        switch(num/10) {

​            case 6:

​                console.log('合格');

​                break;

​            default:

​                console.log('不合格');

​                break;

​        }

​       //方式二

​        switch(true) {

​            case num >= 60:

​                console.log('合格');

​                break;

​            default:

​                console.log('不合格');

​                break;

​        }

  for循环


