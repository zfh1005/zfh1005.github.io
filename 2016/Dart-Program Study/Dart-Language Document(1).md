在 InteIIiJ IDEA 中搭建 Dart 的开发环境： http://www.jianshu.com/p/fa275a08b083

# Dart 语言简易教程(一)

## 一个简单的dart 程序
```Dart
// Define a function.
printNumber(num aNumber) {
  print('The number is $aNumber.'); // Print to console.
}

// This is where the app starts executing.
main() {
  var number = 42; // Declare and initialize a variable.
  printNumber(number); // Call a function.
}
```
从这个程序里面我们可以看到如下的东西：
- 使用` // `来注释。同时` /* ...*/ `也可以用来注释。
- `num` 是一个数据类型，定义在语言中。同样的类型还有`String`，`int`，`bool`。
就是说Dart语言是有数据类型的概念的。这点与Python语言不同。
- `print()` 是显示输出的方法。
- `'...'`(或者`"..."`)，表示是有个 `string` 类型的数据。
这一点与 Python 中string 类型数据一样的使用方法。
- `var` 定义了一个变量，但是没有指定特定的数据类型。
这种用法是很灵活的，既可以像Java类似的语言采取强数据类型，也可以像Python那样在第一次赋值的时候确认数据类型。

> 按照Dart 的编程规范，使用2个空格来缩进。
> **这一点与Python 语言建议的4个空格不一样。**

## 一些重要的概念
- 所有的东西都是对象，无论是变量，数字，函数等。
所以的对象都是类的实例。
所有的对应都继承自内置的`Object`类。

- 程序中指定数据类型是为了指出自己的使用意图，并帮助语言进行语法检查。但是，指定类型不是必须的。
Dart 语言是弱数据类型。

- Dart 代码在运行前解析。
指定数据类型和编译时的常量，可以提高运行速度。

- Dart 程序有统一的程序入口: ` main() `。
这一点是C / C++语言相像。

- Dart 支持顶级的变量定义。
- Dart 没有` public ` ，` protected `，` and private `的概念。
但是如果变量或函数以下划线(`_`)开始，则该函数或变量属于这个包私有(`private`)的方法。

- Dart 中变量或函数以下划线(`_`)或字母开始，后面接上任意组合的下划线(`_`)，数字或字母。
这点与大部分的编程语言是一样的。

- 严格区分` expression ` 和 ` statement `

- Dart 的工具可以检查出警告信息(` warning `)和错误(` errors `)。
警告信息只是表明代码可能不工作，但是不会妨碍程序运行。
错误可以是编译时的错误，也可能是运行时的错误。编译的错误将阻止程序运行，运行时的错误将会以` exception `的方式呈现。

- Dart 使用 `;` 来分割语句
这点类似Java / C++, 但是与Python语言不同。

## 关键字
Dart 语言提供的关键字如下表所示：

| 1 | 2 |3 |4 | 5|
|--------|--------|--------|--------|--------|
|abstract |	continue | false| new    |	this|
| as |	default	| final	| null	| throw |
| assert |	deferred |	finally	| operator  |	true|
| async |	do	| for	| part|	try |
| async |	dynamic |	get |	rethrow |	typedef |
| await |	else |	if	| return |	var |
| break	| enum	| implements  | set |	void |
| case	| export |	import |	static |	while |
| catch	| external |	in	| super	| with |
| class	| extends |	is |	switch|	yield |
| const |	factory | library |	sync |	yield |


## 变量（` Variable `）
变量赋值的例子
```Dart
// The variable called name contains a reference to a String object with a value of “Bob”.
var name = 'Bob';
```

### 默认值
没有初始化的变量都会被赋予默认值 ` null `.
即使是数字也是如此， 因为在Dart 中数字也是一个对象。
``` Dart
int lineCount;
assert(lineCount == null);
// Variables (even if they will be numbers) are initially null.
```language
```

> Note: The assert() call is ignored in production mode. In checked mode, assert(condition) throws an exception unless condition is true.

### 可选类型
也可以在定义的时候指定变量的类型。
```Dart
String name = 'Bob';
```
指定数据类型可以更好的辨明自己的使用意图，编译器和IDE 工具可以根据这些类型信息来做检查，更早的发现问题。
如前文所说，通过指定类型，也可以减少编译和运行时间。

### 常量和固定值
1. 如果定义的变量不会变化，可以使用` final ` 或 ` const `来指明。
也可以使用` final ` 或 ` const `来代替类型声明。
	- ` final `的值只能被设定一次。
	- ` const` 是一个编译时的常量。( Const variables are implicitly final.)
	```Dart
	final name = 'Bob'; // Or: final String name = 'Bob';
	// name = 'Alice';  // Uncommenting this causes an error
	```

2. 通过对` const`类型做四则运算将自动得到一个` const`类型的值。
```Dart
const bar = 1000000;       // Unit of pressure (dynes/cm2)
const atm = 1.01325 * bar; // Standard atmosphere
```language
```

3. 可以通过` const`来创建常量的值
就是说const[] 本身是构造函数。
```Dart
// Note: [] creates an empty list.
// const [] creates an empty, immutable list (EIA).
var foo = const [];   // foo is currently an EIA.
final bar = const []; // bar will always be an EIA.
const baz = const []; // baz is a compile-time constant EIA.
// You can change the value of a non-final, non-const variable,
// even if it used to have a const value.
foo = [];
// You can't change the value of a final or const variable.
// bar = []; // Unhandled exception.
// baz = []; // Unhandled exception.
```
