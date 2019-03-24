# 快速入门
## 基础语法

要特别注意相等运算符==。JavaScript在设计时，有两种比较运算符：
第一种是==比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
第二种是===比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。
由于JavaScript这个设计缺陷，不要使用==比较，始终坚持使用===比较。

另一个例外是NaN这个特殊的Number与所有其他值都不相等，包括它自己
唯一能判断NaN的方法是通过isNaN()函数：

这不是JavaScript的设计缺陷。浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数。要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：
`Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true`

JavaScript的设计者希望用null表示一个空的值，而undefined表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用null。undefined仅仅在判断函数参数是否传递的情况下有用。

如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量

'use strict';

## String
ES6标准新增了一种多行字符串的表示方法，用反引号 ` ... ` 表示

字符串是不可变的，如果对字符串的某个索引赋值，不会有任何错误，但是，也没有任何效果

## Array
大多数其他编程语言不允许直接改变数组的大小，越界访问索引会报错。然而，JavaScript的Array却不会有任何错误。在编写代码时，不建议直接修改Array的大小，访问索引时要确保索引不会越界

如果要往Array的头部添加若干元素，使用unshift()方法，shift()方法则把Array的第一个元素删掉

splice()方法是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

## Object
JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成

JavaScript用一个{...}表示一个对象，键值对以xxx: xxx形式申明，用,隔开。注意，最后一个键值对不需要在末尾加,，如果加了，有的浏览器（如低版本的IE）将报错

如果我们要检测xiaoming是否拥有某一属性，可以用in操作符
要判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用hasOwnProperty()方法

## 条件判断
JavaScript把null、undefined、0、NaN和空字符串''视为false，其他值一概视为true，因此上述代码条件判断的结果是true

## 循环
请注意，for ... in对Array的循环得到的是String而不是Number。

## Map和Set
Map和Set是ES6标准新增的数据类型

## iterable
for ... of循环是ES6引入的新的语法

for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性
```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```
for ... in循环将把name包括在内，但Array的length属性却不包括在内。
for ... of循环则完全修复了这些问题，它只循环集合本身的元素

foreach(function)
Array 回调函数参数依次是当前元素的值、当前索引、Array对象本身
Set 回调函数的前两个参数都是元素本身
Map的回调函数参数依次为value、key和map本身

# 函数
## 定义和调用
由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数

传入的参数比定义的少也没有问题

JavaScript还有一个免费赠送的关键字arguments，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。arguments类似Array但它不是一个Array

实际上arguments最常用于判断传入参数的个数

rest参数只能写在最后，前面用...标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量rest，所以，不再需要arguments我们就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是undefined）

## 变量作用域与结构赋值
JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部

实际上，JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性

全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。
减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中

由于JavaScript的变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量的
为了解决块级作用域，ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量

ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域

从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值
`var [x, y, z] = ['hello', 'JavaScript', 'ES6'];`
`let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];`
`let [, , z] = ['hello', 'JavaScript', 'ES6']`
```
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
```
```
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
```

使用解构赋值可以减少代码量，但是，需要在支持ES6解构赋值特性的现代浏览器中才能正常运行。目前支持解构赋值的浏览器包括Chrome，Firefox，Edge等

## 方法
```js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};
xiaoming.age(); // 25, 正常结果
getAge(); // NaN
```
JavaScript的函数内部如果调用了this，那么这个this到底指向谁？
答案是，视情况而定！
如果以对象的方法形式调用，比如xiaoming.age()，该函数的this指向被调用的对象，也就是xiaoming，这是符合我们预期的。
如果单独调用函数，比如getAge()，此时，该函数的this指向全局对象，也就是window。
更坑爹的是，如果这么写：
```javascript
var fn = xiaoming.age; // 先拿到xiaoming的age函数
fn(); // NaN
```
也是不行的！要保证this指向正确，必须用obj.xxx()的形式调用！
由于这是一个巨大的设计错误，要想纠正可没那么简单。ECMA决定，在strict模式下让函数的this指向undefined，因此，在strict模式下，你会得到一个错误

要指定函数的this指向哪个对象，可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array，表示函数本身的参数

另一个与apply()类似的方法是call()，唯一区别是：
- apply()把参数打包成Array再传入；
- call()把参数按顺序传入。

对普通函数调用，我们通常把this绑定为null

## 高阶函数
map/reduce
filter
sort()方法会直接对Array进行修改，它返回的结果仍是当前Array

## 闭包
```javascript
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
```
在函数lazy_sum中又定义了函数sum，并且，内部函数sum可以引用外部函数lazy_sum的参数和局部变量，当lazy_sum返回函数sum时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力
当我们调用lazy_sum()时，每次调用都会返回一个新的函数，即使传入相同的参数

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}
var results = count();
var f1 = results[0];//16
var f2 = results[1];//16
var f3 = results[2];//16
```
全部都是16！原因就在于返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了4，因此最终结果为16
返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变
```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}
var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
```

在没有class机制，只有函数的语言里，借助闭包，同样可以封装一个私有变量。我们用JavaScript创建一个计数器:
```javascript
'use strict';
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
```
闭包还可以把多参数的函数变成单参数的函数
```javascript
'use strict';
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}
// 创建两个新函数:
var pow2 = make_pow(2);
var pow3 = make_pow(3);
console.log(pow2(5)); // 25
console.log(pow3(7)); // 343
```

JavaScript支持函数，可以用JavaScript用函数来写计算
```javascript
'use strict';
// 定义数字0:
var zero = function (f) {
  return function (x) {
    return x;
  }
};
// 定义数字1:
var one = function (f) {
  return function (x) {
    return f(x);
  }
};
// 定义加法:
function add(n, m) {
  return function (f) {
    return function (x) {
      return m(f)(n(f)(x));
    }
  }
}
// 计算数字2 = 1 + 1:
var two = add(one, one);
// 计算数字3 = 1 + 2:
var three = add(one, two);
// 计算数字5 = 2 + 3:
var five = add(two, three);
// 给3传一个函数,会打印3次:
(three(function () {
  console.log('print 3 times');
}))();
// 给5传一个函数,会打印5次:
(five(function () {
  console.log('print 5 times');
}))();
```

## 箭头函数
如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错
```
// SyntaxError:
x => { foo: x }
```
因为和函数体的{ ... }有语法冲突，所以要改为：`x => ({ foo: x })`

箭头函数完全修复了this的指向
由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略

## 生成器
调用generator对象有两个方法，一是不断地调用generator对象的next()方法
第二个方法是直接用for ... of循环迭代generator对象，这种方式不需要我们自己判断done

因为generator可以在执行过程中多次返回，所以它看上去就像一个可以记住执行状态的函数，利用这一点，写一个generator就可以实现需要用面向对象才能实现的功能
generator还有另一个巨大的好处，就是把异步回调代码变成“同步”代码

# 标准对象
```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```
number、string、boolean、function和undefined有别于其他类型。特别注意null的类型是object，Array的类型也是object，如果我们用typeof将无法区分出null、Array和通常意义上的object——{}

所以闲的蛋疼也不要使用包装对象！尤其是针对string类型！！！
包装对象用new创建,创建后的类型为'object'
Number()、Boolean和String()被当做普通函数，把任何类型的数据转换为number、boolean和string类型

- 不要使用new Number()、new Boolean()、new String()创建包装对象；
- 用parseInt()或parseFloat()来转换任意类型到number；
- 用String()来转换任意类型到string，或者直接调用某个对象的toString()方法；
- 通常不必把任意类型转换为boolean再判断，因为可以直接写if (myVar) {...}；
- typeof操作符可以判断出number、boolean、string、function和undefined；
- 判断Array要使用Array.isArray(arr)；
- 判断null请使用myVar === null；
- 判断某个全局变量是否存在用typeof window.myVar === 'undefined'；
- 函数内部判断某个变量是否存在用typeof myVar === 'undefined'。

任何对象都有toString()方法吗？null和undefined就没有！确实如此，这两个特殊值要除外，虽然null还伪装成了object类型
number对象调用toString()报SyntaxError
遇到这种情况，要特殊处理一下：
```javascript
123..toString(); // '123', 注意是两个点！
(123).toString(); // '123'
```
## Date
在JavaScript中，Date对象用来表示日期和时间.当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。
0表示一月，1表示二月
使用Date.parse()时传入的字符串使用实际月份01~12，转换为Date对象后getMonth()获取的月份值为0~11

## RegExp

## JSON 
要输出得好看一些，可以加上参数，按缩进输出`JSON.stringify(xiaoming, null, '  ');`
如果我们还想要精确控制如何序列化小明，可以给xiaoming定义一个toJSON()的方法，直接返回JSON应该序列化的数据
JSON.parse()还可以接收一个函数，用来转换解析出的属性

# 面向对象编程
JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程

`xiaoming.__proto__ = Student;`把xiaoming的原型指向了对象Student，看上去xiaoming仿佛是从Student继承下来的

JavaScript的原型链和Java的Class区别就在，它没有“Class”的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已

在编写JavaScript代码时，不要直接用obj.__proto__去改变一个对象的原型，并且，低版本的IE也无法使用__proto__
Object.create()方法可以传入一个原型对象，并创建一个基于该原型的新对象，但是新对象什么属性都没有，因此，我们可以编写一个函数来创建xiaoming

## 创建对象
用一种构造函数的方法来创建对象 

要让创建的对象共享一个hello函数，根据对象的属性查找原则，我们只要把hello函数移动到xiaoming、xiaohong这些对象共同的原型上就可以了，也就是Student.prototype

还可以编写一个createStudent()函数，在内部封装所有的new操作

## 原型继承
JavaScript的原型继承实现方式就是：
1. 定义新的构造函数，并在内部用call()调用希望“继承”的构造函数，并绑定this；
2. 借助中间函数F实现原型链继承，最好通过封装的inherits函数完成；
3. 继续在新的构造函数的原型上定义新方法。

## class继承
新的关键字class从ES6开始正式被引入到JavaScript中。class的目的就是让定义类更简单。

不是所有的主流浏览器都支持ES6的class。如果一定要现在就用上，就需要一个工具把class代码转换为传统的prototype代码，可以试试[Babel](https://babeljs.io/)这个工具。

# 浏览器

在编写JavaScript的时候，就要充分考虑到浏览器的差异，尽量让同一份JavaScript代码能运行在不同的浏览器中

## 浏览器对象
window

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的User-Agent字符串。

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如8、16、24。

- location.protocol; // 'http'
- location.host; // 'www.example.com'
- location.port; // '8080'
- location.pathname; // '/path/index.html'
- location.search; // '?a=1&b=2'
- location.hash; // 'TOP'
要加载一个新页面，可以调用location.assign()。如果要重新加载当前页面，调用location.reload()方法非常方便 

document.getElementById
document.getElementsByTagName
document.cookie
由于JavaScript能读取到页面的Cookie，而用户的登录信息通常也存在Cookie中，这就造成了巨大的安全隐患，这是因为在HTML页面中引入第三方的JavaScript代码是允许的
为了解决这个问题，服务器在设置Cookie时可以使用httpOnly，设定了httpOnly的Cookie将不能被JavaScript读取。这个行为由浏览器实现，主流浏览器均支持httpOnly选项，IE从IE6 SP1开始支持。
为了确保安全，服务器端在设置Cookie时，应该始终坚持使用httpOnly

任何情况，你都不应该使用history这个对象了

## 操作DOM
最常用的方法是document.getElementById()和document.getElementsByTagName()，以及CSS选择器document.getElementsByClassName()
第二种方法是使用querySelector()和querySelectorAll()，需要了解selector语法，然后使用条件来获取节点，更加方便

用innerHTML时要注意，是否需要写入HTML。如果写入的字符串是通过网络拿到了，要注意对字符编码来避免XSS攻击
第二种是修改innerText或textContent属性，这样可以自动对字符串进行HTML编码，保证无法设置任何HTML标签
两者的区别在于读取属性时，innerText不返回隐藏元素的文本，而textContent返回所有文本。另外注意IE<9不支持textContent

使用appendChild，把一个子节点添加到父节点的最后一个子节点
可以使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到referenceElement之前

调用父节点的removeChild把自己删掉

## 操作表单
但是，对于单选框和复选框，value属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用checked判断

## 操作文件
HTML5的File API提供了File和FileReader两个主要对象，可以获得文件信息并读取文件

## AJAX
如何实现跨域请求

## Promise
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001436512391628944d5da9a5654a35b0ace38246f30b9c000/l)
除了串行执行若干异步任务外，Promise还可以并行执行异步任务,用Promise.all([*,*])实现
有些时候，多个异步任务是为了容错。比如，同时向两个URL读取用户的个人信息，只需要获得先返回的结果即可。这种情况下，用Promise.race()实现

## Canvas
通常在<canvas>内部添加一些说明性HTML代码，如果浏览器支持Canvas，它将忽略<canvas>内部的HTML，如果浏览器不支持Canvas，它将显示<canvas>内部的HTML

在使用Canvas前，用canvas.getContext来测试浏览器是否支持Canvas

getContext('2d')方法让我们拿到一个CanvasRenderingContext2D对象，所有的绘图操作都需要通过这个对象完成
如果需要绘制3D怎么办？HTML5还有一个WebGL规范，允许在Canvas中绘制3D图形,canvas.getContext("webgl");

使用Canvas绘制k线图

## 基本模块
global
process
fs
stream
http
cryto 提供通用的加密和哈希算法

## Web开发
### koa
koa是Express的下一代基于Node.js的web框架
Express是第一代最流行的web框架，它对Node.js的http进行了封装
和Express相比，koa 1.0使用generator实现异步，代码看起来像同步
超前地基于ES7开发了koa2，和koa 1相比，koa2完全使用Promise并配合async来实现异步


### mocha
是JavaScript的一种单元测试框架，既可以在浏览器环境下运行，也可以在Node.js环境下运行

### WebSocket
### REST
### MVVM
关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把开发者从操作DOM的繁琐步骤中解脱出来！

常用的MVVM框架有：
- Angular：Google出品，名气大，但是很难用；
- Backbone.js：入门非常困难，因为自身API太多；
- Ember：一个大而全的框架，想写个Hello world都很困难。

