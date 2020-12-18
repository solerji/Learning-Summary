JavaScript 基础

1.基本数据类型(6，5)

Number  

*运算问题，浮点数运算不准确

Number.MIN_VALUE, Number.MAX_VALUE

 5e-324 1.7976931348623157e+308

Null (typeof 输出object类型，为空)

Undefined(为未定义，typeof 输出undefined类型)

String

Boolean (true false 输入数字均为数字类型)



其他类型：

Object



强制类型转换：

->String

(1).toString()

不会影响原变量，会将转换结果返回

对于null和undefined没有toString()方法

(2).String(b)

变成参数返回该值，null返回"null",undefined返回"undefined"

对于Number和Boolean来说调用的是toString()方法

对于null和undefined不会调用toString()方法

->Number(数字类型的值在前端为了保持它值的一致，最好在使用前都对它的类型做一个标准转换，否则可能会导致数值不能正确使用，也可能导致浏览器报错)

(1).Number(a)

纯字符串直接转数字

带有字符串转换为NaN形式

字符串为空格会转为0

boolean转换为0/1

(2).字符串转数字

只读到数字结束的位数

parseInt()

parseFloat()

-> 进制转换

十六进制：0x

八进制：0

二进制可能会导致浏览器无法识别，IE内核可能会对此有不同处理

-> Boolean

1.Boolean()函数

NaN, null,0 => false

"", undefiend => false

*对象都是true

2.运算符

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





