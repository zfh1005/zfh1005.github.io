Dart 语言简易教程(一)： 
http://www.jianshu.com/p/8a62b1a2fd75

# Dart 语言简易教程(二)
## 内建数据类型（Built-in types）
Dart 语言内建了下面集中类型
- numbers
- strings
- booleans
- lists (also known as arrays)
- maps
- runes (for expressing Unicode characters in a string)
- symbols

### Number 类型
Dart 类型包括如下两类
- int
取值范围：-2^53 to 2^53

- double
64 位长度的浮点型数据，符合IEEE 754 标准。

int 和 double 类型都是 num 类型的子类。
num 类型包括的操作包括： `+`, `-`, `*`, `/` 以及位移操作` >> `.
num 类型 有如下常用方法 abs(), ceil()和 floor()。完整的使用方法请参见：dart:math 包的使用说明。

int 类型不能包含小数点`.`.

num类型操作例子：
```Dart
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
```

num 类型按位操作的例子：
```Dart
assert((3 << 1) == 6);  // 0011 << 1 == 0110
assert((3 >> 1) == 1);  // 0011 >> 1 == 0001
assert((3 | 4)  == 7);  // 0011 | 0100 == 0111
```

### Strings 类型
Dart 的String 是 UTF-16 编码的一个队列。

Dart语言定义的例子：
```Dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

String 类型可以使用 `+` 操作：
```Dart
var s1 = 'String ' 'concatenation'
         " works even over line breaks.";
assert(s1 == 'String concatenation works even over '
             'line breaks.');

var s2 = 'The + operator '
         + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

可以使用三个`‘` 或`“`来定义多行的String 类型。
(这点和Python 语言类似。)
```Dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

可以使用`r` 来修饰String类型，表 表明是“raw” 类型字符串：
```Dart
var s = r"In a raw string, even \n isn't special.";
```

String 类型是 compile-time 的常量。
可以在编译是才给String类型赋值。

```Dart
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = const [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
```

### booleans 类型
Dart 的布尔类型名字是` bool `，可能的取值包括”ture“ 和 ”false“。
”bool“ 类型是 compile-time 的常量。

Dart 是强bool 类型检查，只有bool 类型的值是”true“ 才被认为是true。
```Dart
var name = 'Bob';
if (name) {
  // Prints in JavaScript, not in Dart.
  print('You have a name!');
}
```
在production mode 中上面的代码将不会输出任何东西，因为`name != true`。
checked mode 中上面的代码将会出现异常，因为`name`不是bool 类型。

### Lists 类型
在 Dart　语言中，具有一系列相同类型的数据被称为 List 对象。
Dart List 对象类似JavaScript 语言的 array 对象。

定义list的例子：
```Dart
var list = [1, 2, 3];
```

Dart list 对象的第一个元素的位置是`0`，最后个元素的索引是`list.lenght - 1`。
```Dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

### Maps 类型
Map　类型将keys 和 values 关联在一起。
keys 和 values 可以是任意类型的对象。
像其它支持Map 的编程语言一样，Map 的 key 必须是唯一的。

Map 对象的定义：
```Dart
var gifts = {
// Keys      Values
  'first' : 'partridge',
  'second': 'turtledoves',
  'fifth' : 'golden rings'
};

var nobleGases = {
// Keys  Values
  2 :   'helium',
  10:   'neon',
  18:   'argon',
};
```

也可以使用Map 对象的构造函数 Map() 来创建Map 对象：
```Dart
var gifts = new Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = new Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

添加新的key-value 对：
```Dart
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
```

检查key 是否在Map 对象中：
```Dart
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
```

使用` .lenght ` 来获取key-value 对的数量：
```Dart
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
```

### Runes 类型
Dart 中 ` runes 	` 是UTF-32字符集的string 对象。
`codeUnitAt ` 和 `codeUnit ` 用来获取UTF-16字符集的字符。
使用` runes ` 来获取UTF-32字符集的字符。

```Dart
main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
```
上面例子的输出结果是：
```Dart

[55357, 56399]
[128079]
          
```

### Symbols 类型
一般程序中不会使用` Symbol `类型。
` Symbol `类型跟在`#`后面。
