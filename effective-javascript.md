### 理解javascript的浮点数

1. Javascript使用64位双精度保存数字（1位符号位，11位指数位，52位有效数），计算时会把操作数转成二进制的表示法，遇到无法完整表示的数字，就会丢失精度
2. 在进行位运算时，会把数字转成32位的整数进行计算

### 当心隐式的强制转换

1. 对象通过 valueOf 方法强制转换为数字，通过 toString 方法强制转换为字符串。
2. 具有 valueOf 方法的对象应该实现 toString 方法，返回一个 valueOf 方法产生的数字的 字符串表示。 
3. 测试一个值是否为未定义的值，应该使用 typeof 或者与 undefined 进行比较而不是使 用真值运算

### 原始类型优于封装对象


获取和设置原始类型值的属性会隐式地创建封装对象

"hello".someProperty = 17 会隐式地创建一个 new String() 封装对象

### 避免对混合类型使用==运算符


### 了解分号插入的局限


### 尽量少用全局变量

1. ES5 提供了全局的JSON对象来读写JSON格式的数据
2. 可以使用全局对象来做平台特性检测

```other
if(!this.JSON) {

	this.JSON = {
	
		parse: ...,

		stringify: ...

	
	}	
}

```


### 始终声明局部变量


始终使用var来申明变量，在方法内部直接给未定义变量赋值，会导致意外地创建全局变量

使用lint来控制代码规范

### 避免使用 with


避免使用 with 语句

使用简短的变量名代替重复访问的对象

显式地绑定局部变量到对象属性上，而不要使用 with 语句隐式地绑定它们

### 熟练掌握闭包


### 理解变量声明提升


JavaScript 隐式地提升 (hoists) 声明部分到封闭函数的顶部，而将赋值留在原地。

### 使用立即调用的函数表达式创建局部作用域


```other
function wrapElement(a) {
	var result = [] ;
	for (var i = 0, n = a.length; i < n; i++) {
		// 使用 IIFE 创建一个局部作用域
		// 在函数外不能使用break或者continue
		// 如果代码块引用了 this 或特别的 arguments 变量，IIFE 将会改变它们的含义
		(function(j) {
			result[i]=function(){ return a[j]; };
		})(i)
	}
}
```


### 当心命名函数表达式笨拙的作用域


可以不使用命名函数

ES5以后的正确环境中这类问题已经不存在

### 当心局部块函数声明笨拙的作用域


官方指定函数声明只能出现在其他函数或者程序的最外层

### 避免使用 eval 创建局部变量


eval 会在调用它的函数作用域中创建局部变量，这会导致程序的不稳定，可以将eval语句包含在立即执行函数中避免这个情况

### 间接调用 eval 函数优于直接调用


直接调用可能产生危害，直接调用 eval 函数的能力可能很容易被滥用。例如，对一个来自网络的源字符串进行求 值，可能会暴露其内部细节给一些未受信者

f = eval

f(src)

可以让eval失去对局部变量的访问能力

简洁调用方式：(0,eval)(src)

### 理解函数，方法和构造函数调用之间的不同


方法调用中的this时具体的调用对象

函数调用this绑定到全局变量

ES5的严格模式中, this不能被绑定到全局变量window上，调用this会返回undefined

调用构造方法时，其中的 this 将被绑定到一个全新的对象中，并将其返回

### 高阶函数


### 使用 call 方法自定义接收者来调用方法


### 使用 apply 方法通过不通数量的参数调用函数


### 使用 arguments 创建可变参数的函数


### 永远不要修改arguments对象


arguments 不是标准的 Array 对象，不能调用.shift()

arguments的别名不会因为你对其进行修改而改变，这会导致无法察觉的错误，使用[].slice.call(arguments, ...) 对 arguments 进行复制，来获取需要的arguments 中的参数

### 使用变量保存arguments的引用


### 使用bind提取具有确定接收者的方法


### 使用bind实现函数柯里化


// 通过bind(null)进行函数柯里化，忽略函数的接收者

### 使用闭包而不是字符串来封装代码


多余，没人会这么做

### 不要信赖函数对象的 toString 方法


在一个引擎中能正确显示源代码，在其他浏览器引擎可能会失败，不要信赖函数的toString获取的源代码

### 避免使用非标准的栈检查属性


arguments.callee, arguments.caller 工程里没什么用处，用来观察调用栈也不靠谱

### 理解 prototype, getPrototypeOf 和 __proto__  之间的不同


prototype 是原型对象

getPrototypeOf()获取原型对象（ES5）

__proto__  获取原型对象(非标准方法)

### 使用getPrototypeOf不要使用 __proto__  


使用 getPrototypeOf 确保 _proto__不被污染

### 始终不要修改 _proto_


Object.create()用来给对象设定自定义的原型

### 让 new 操作符与构造函数无关


如果创建对象是没有使用new关键字，构造函数中的this 将会是全局对象，并且还会返回无意义的undefined** ** 

如果是严格模式，构造函数中的this将会是undefined ，这会导致程序错误

### 在原型中存储方法


现代的 Javascript 引擎优化了原型查找，所以讲方法存放在每个实例中，并不会有速度上的优势，反而会带来占用更多内存的问题

### 使用闭包存储私有数据


需要把方法定义在每个实例中，这将导致方法的扩散，不过这个代价是值得的

```other
function User(name, passwordHash) {
    this.toString = function() {
        return "[User " + name + "]";
    }
    this.checkPassword = function (password) {
        return hash(password) === passwordHash;
    }
}
```


### 只将实例状态存储在实例中


例：一个树形结构的类将其子节点数据保存在实例中

### 认识到this变量的隐式绑定问题


```other
function CSVReader(separators) {
    this.separators = separators || [","];
    this.regexp = new RegExp(this.separators.map(function(sep){
        console.log({sep})
        return "\\" + sep[0];
    }).join("|"));
}

CSVReader.prototype.read = function (str) {
    var lines = str.trim().split(/\n/);
    return lines.map(function(line) {
        console.log(this.regexp);
		 // 这里的 this 绑定的是当前 funtion 的作用域环境
        return line.split(this.regexp);
    });
}

var reader = new CSVReader();
const rs = reader.read("a,b,c\nd,e,f\n");
console.log({rs});
```


### 在子类的构造函数中调用父类构造函数


```other
function SpaceShip(scene, x, y) {
	// 调用父类构造函数，把接收者绑定到新对象（返回的新对象就是这里的this）
	Actor.call(this, scene, x, y);
	this.pointers = 0;
}
```


### 不要重用父类的属性名称


留意父类使用的所有属性名

###  避免继承标准类


继承Array的子类并不能调用Array里的length属性，因为Array是一个Object类型，Array只是一个被打上了Class属性是Array的Object对象

### 将原型视为实现细节


### 避免使用轻率的猴子补丁


### 使用Object的直接实例构造轻量级的字典


其他如Array类型 也是Object类型，for...in会读取其原型上的实例，所以应该使用Object的直接实例（Object.prototype的直接子类）

### 使用null原型以防止原型污染


__proto__ 是非标准的方法，不是都是可移植的。并且不要用__proto__ 作为对象的key

### 使用hasOwnProperty方法以避免原型污染


hasOwnProperty 检查对象的属性，可以避免原型污染，不会检查原型链上的属性

### 使用数组不要使用字典来存储有序集合


### 绝不要在Object.prototype中增加可枚举的属性


```other
// 往Object添加方法，导致污染了自身
Object.prototype.allKeys = function () {
    var result = [];
    for (var key in this) {
        result.push(key);
    }
    return result;
}
console.log(({a:1,b:2,c:3}).allKeys());

// 定义一个函数替代这个方法

// 或者使用Object.defineProperty
Object.defineProperty(Object.prototype, "allKeys", {
    value: function () {
        var result = [];
        for (var key in this) {
            result.push(key);
        }
        return result;
    },
    writable: true,
    enumerable:false,
    configurable: true
});
console.log(({a:1,b:2,c:3}).allKeys());
```


### 避免在枚举期间修改对象


for...in 可能在不同的环境中有不同的便利顺序

### 循环数组应该用for而不是for...in


### 迭代优于循环


使用默认的map,filter,forEach 方法，代替for循环

也可以定义自己的迭代抽象，没有break,continue时 可以使用some,every进行变种

### 在类数组对象上复用通用的数组方法


下面的arrayLike和"abc"字符串 都是类数组对象

其中的arrayLike满足类数组对象的特征，索引值需要是有顺序的，length需要被正确的设置

```other
var arrayLike = {0: "a", 1: "b", 2: "c", 3:"d", length: 4};
var result = Array.prototype.map.call(arrayLike, function(s) {
    return s.toUpperCase();
})

console.log({ result });

var result = Array.prototype.map.call("abc", function(s) {
    return s.toUpperCase();
})

console.log({ result });
```


这样的对象可以通过调用Array上的原型方法来进行操作

### 数组字面量优于数组构造函数


var array = [1, 2, 3, 4, 5] 比 new Array(1, 2, 3, 4, 5) 优秀


1. 更直观更好看
2. 如果有人重新定义过Array构造函数
3. new Array(17) 表示一个长度为17的没有元素的数组
4. ["hello"] 和 new Array("hello") 相同
5. [17] 和 new Array(17) 的行为不同

### 保持一致的约定


### 将undefined看作「没有值」


在允许0，NaN，或者“”为有效参数的地方，不要通过!!真值测试的方式来实现参数默认值

采用测试undefined的方式来提供参数默认值

### 接收关键字参数的选项对象


传入对象参数更好

使用extend函数提取对象参数中的值，简化代码

### 避免不必要的状态


尽可能使用无状态的API

什么是有状态的API和无状态的API？

### 使用结构类型设计灵活的接口


### 区分数组对象和类数组对象


在一个允许多个全局对象的环境中，如果有多个Array副本，修改了Array的原型继承，使用 instanceof Array ，可能并不能准确的测试出这个对象是否是原始的真数组。

ES5中引入了isArray, 使用Array.isArray来判断一个对象是否是真数组，而不是一个类数组的对象。

不是ES5环境中，使用Object.prototype.toString方法测试一个对象是否为数组

```other
var toString = Object.prototype.toString;

function isArray(x) {
	return toString.call(x) === "[objecet Array]";
}
```


Object.prototype.toString 函数使用对象内部的[[Class]]属性创建结果，他比instance of可靠

### 避免过度的强制转换


### 支持方法链


```other
// 通过共享原型对象，来创建一个监视对象的工具库
var guard = {
    guard: function(x) {
        if (!this.test(x)) {
            throw new TypeError("expected " + this);
        }
    }
}

var uint32 = Object.create(guard);
uint32.test = function (x) {
    return typeof x === "number" && x === (x >>> 0);
};
uint32.toString = function () {
    return "uint32";
}
// arrayLike 的监视对象
var arrayLike = Object.create(guard);
arrayLike.test = function (x) {
    return typeof x === "object" && x && uint32.test(x.length);
}

// 在guard工具中创建链方法
guard.or = function(other) {
    // 链式调用需要返回相同的类型，在这里是同样的guard原型
    var result = Object.create(guard);
    var self = this;
    result.test = function(x) {
        return self.test(x) || other.test(x);
    };
    result.toString = function () {
        return self + " or " + other;
    }
    return result;
}

function sa() {
    var foo = 123;
    uint32.or(arrayLike).guard(foo);
}
sa();
```


### 不要阻塞浏览器主线程


### 在异步队列使用嵌套或者命名的回调函数


### 当心丢弃错误


异步的API接收一个处理错误的函数，作为异步Api的参数

### 对异步循环使用递归


异步循环中的递归不会导致调用栈过大而报错

### 使用计数器执行并行操作


### 使用Promise


### 不要同步的调用异步函数


```other

```

