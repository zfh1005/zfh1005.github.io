Dart 语言简易教程(一)： 
http://www.jianshu.com/p/8a62b1a2fd75

Dart 语言简易教程(二)： 
http://www.jianshu.com/p/b2153a32dd8b

# Dart 语言简易教程(三)
## 函数(Functions)
Dart 是一个面向对象的语言，所以即使是函数也是对象，函数属于`Function`对象。
可以通过函数来指定变量或者像其它的函数传递参数。

函数实现的例子：
```Dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

可以去掉形式参数数据类型和返回值的数据类型。
下面的例子演示了这些：
```Dart
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

如果函数只有单个语句，可以采用简略的形式：
```Dart
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```
> The `=> expr;` syntax is a shorthand for `{ return expr;}`.

函数可以有两中类型的参数：
- 必须的
必须的参数放在参数列表的前面。
- 可选的
可选的参数跟在必须的参数后面。

### 可选的参数
可以通过名字或位置个函数指定可选参数。

#### 可选的名字参数
在调用函数时，可以指定参数的名字及相应的取值，采用`paramName: value `的格式。
例如函数定义：
```Dart
/// Sets the [bold] and [hidden] flags to the values
/// you specify.
enableFlags({bool bold, bool hidden}) {
  // ...
}
```
函数调用：
```Dart
enableFlags(bold: true, hidden: false);
```

#### 可选的位置参数
将参数使用`[]` 括起来，用来表明是可选位置参数。
例如下面的例子，函数定义：
```Dart
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

调用函数不包含第三个参数：
```Dart
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
```
调用函数包含第三个参数：
```Dart
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
```

#### 参数默认值
可以定义包含默认位置参数或默认名字参数的函数。参数的默认值必须是编译时的静态值。
假如定义函数时，没有指定默认的参数值，则参数值默认为`null` 。
- 使用冒号(`: `)来设置默认名字参数。
	```Dart
// Sets the [bold] and [hidden] flags to the values you
// specify, defaulting to false.
enableFlags({bool bold: false, bool hidden: false}) {
// ...
}
// bold will be true; hidden will be false.
enableFlags(bold: true);
```
- 使用等号(`= `)来设置默位置字参数。
```Dart
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}
assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
```

也可以将`lists ` 及`maps `类型作为默认值。
如下面的例子：
```Dart
doStuff({List<int> list: const[1, 2, 3],
         Map<String, String> gifts: const{'first':  'paper',
                                          'second': 'cotton',
                                          'third':  'leather'}}) {
  print('list:  $list');
  print('gifts: $gifts');
}

main() {
  // Use the default values for both parameters.
  doStuff();

  // Use the default values for the "gifts" parameter.
  doStuff(list:[4,5,6]);
  
  // Don't use the default values for either parameter.
  doStuff(list: null, gifts: null);
}
```

对应输出结果是：
```Dart
list:  [1, 2, 3]
gifts: {first: paper, second: cotton, third: leather}
list:  [4, 5, 6]
gifts: {first: paper, second: cotton, third: leather}
list:  null
gifts: null
```

#### main() 函数
所以的APP 都必须有一个`mian() `函数，作为APP 的应用接入点。
`main() `函数返回void 类型，并且包含可选的`List< String > ` 类型的参数。

`main() `函数不包含参数的例子：
```Dart
void main() {
  querySelector("#sample_text_id")
    ..text = "Click me!"
    ..onClick.listen(reverseText);
}
```

`main() `函数包含参数的例子：
```Dart
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
```

#### 传递函数给函数
- 可以将一个函数作为一个参数传递给另一个函数。例如：
	```Dart
	printElement(element) {
	  print(element);
	}

	var list = [1, 2, 3];

	// Pass printElement as a parameter.
	list.forEach(printElement);
	```

- 也可以将函数赋值给一个变量。例如：
	```Dart
	var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
	assert(loudify('hello') == '!!! HELLO !!!');
	```

#### 变量作用范围
嵌套的函数中可以访问包含他的函数中定义的变量。例如：
```Dart
var topLevel = true;

main() {
  var insideMain = true;

  myFunction() {
    var insideFunction = true;

    nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
```

#### 变量闭合
函数可以返回一个函数。例如：
```Dart
/// Returns a function that adds [addBy] to the
/// function's argument.
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}

```

#### 函数返回值
所以的函数都会有返回值。
如果没有指定函数返回值，则默认的返回值是`null `。
没有返回值的函数，系统会在最后添加隐式的return 语句。
