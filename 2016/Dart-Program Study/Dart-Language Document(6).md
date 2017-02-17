Dart 语言简易教程(一)： http://www.jianshu.com/p/8a62b1a2fd75
Dart 语言简易教程(二)： http://www.jianshu.com/p/b2153a32dd8b
Dart 语言简易教程(三)： http://www.jianshu.com/p/6d2495a0d3d7
Dart 语言简易教程(四):   http://www.jianshu.com/p/fdd046a6dc82
Dart 语言简易教程(五) http://www.jianshu.com/p/83adc77839b6

# Dart 语言简易教程(六)
## 对象
Dart 是一种面向对象的语言，并且支持基于mixin的继承方式。
Dart 语言中所有的对象都是某一个类的实例。所有的类有同一个基类--Object。
基于mixin的继承方式具体是指：一个类可以继承自多个父类。

使用`new `语句来构造一个类。
构造函数的名字可能是`ClassName `，也可以是`ClassName.identifier `。例如：
```Dart
var jsonData = JSON.decode('{"x":1, "y":2}');

// Create a Point using Point().
var p1 = new Point(2, 2);

// Create a Point using Point.fromJson().
var p2 = new Point.fromJson(jsonData);
```

使用`. `（dot） 来调用实例的变量或者方法。
```Dart
var p = new Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(new Point(4, 4));
```
使用`?. ` 来确认前操作数不为空。常用来替代`. `。
```Dart
// If p is non-null, set its y value to 4.
p?.y = 4;
```
使用`const `替代`new `来创建编译时的常量构造函数。
```Dart
var p = const ImmutablePoint(2, 2);
```
使用` runtimeType `方法，在运行中获取对象的类型。该方法将返回`Type ` 类型的变量。
```Dart
print('The type of a is ${a.runtimeType}');
```
### 实例化变量(Instance variables)
在类定义中，所有没有初始化的变量都会被初始化为`null `。
```Dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
```

类定义中所有的变量Dart 语言都会隐式的定义 setter 方法，针对非空的变量会额外增加 getter 方法。
```Dart
class Point {
  num x;
  num y;
}

main() {
  var point = new Point();
  point.x = 4;          // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
```

### 构造函数(Constructors)
声明一个和类名相同的函数，来作为类的构造函数。
```Dart
class Point {
  num x;
  num y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
```

`this `关键字指向了当前类的实例。
上面的代码可以简化为：
```Dart
class Point {
  num x;
  num y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
```

#### 默认构造函数(Default constructors)
如果类定义时，没有显式的定义构造函数，Dart 语言将提供默认的构造函数。默认的构造函数没有参数。

#### 构造函数不能继承(Constructors aren’t inherited)
Dart 语言中，子类不会继承父类的命名构造函数。如果不显式提供子类的构造函数，系统就提供默认的构造函数。

#### 命名的构造函数(Named constructors)
使用命名构造函数从另一类或现有的数据中快速实现构造函数。
```Dart
class Point {
  num x;
  num y;

  Point(this.x, this.y);

  // Named constructor
  Point.fromJson(Map json) {
    x = json['x'];
    y = json['y'];
  }
}
```

构造函数不能被继承，父类中的命名构造函数不能被子类继承。如果想要子类也拥有一个父类一样名字的构造函数，必须在子类是实现这个构造函数。

##### 调用父类的非默认构造函数
默认情况下，子类只能调用父类的无名，无参数的构造函数。父类的无名构造函数会在子类的构造函数前调用。如果initializer list 也同时定义了，则会先执行initializer list 中的内容，然后在执行父类的无名无参数构造函数，最后调用子类自己的无名无参数构造函数。即下面的顺序：
1. initializer list
2. superclass’s no-arg constructor
3. main class’s no-arg constructor

如果父类不显示提供无名无参数构造函数的构造函数，在子类中必须手打调用父类的一个构造函数。这种情况下，调用父类的构造函数的代码放在子类构造函数名后，子类构造函数体前，中间使用`: `(colon) 分割。
```Dart
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});

  // Prints:
  // in Person
  // in Employee
  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
```
上面的代码输出如下：
```Log
in Person
in Employee
```

##### 初始化器列表(Initializer list)
除了调用父类的构造函数，也可以通过Initializer list 在子类的构造函数运行前来初始化实例的变量值。如下所示：
```Dart
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
```
上面的代码运行结果为：
```Log
3.605551275463989
```

#### 重定向构造函数
有时候构造函数的目的只是重定向到该类的另一个构造函数。
重定向构造函数的函数体是空的。
```Dart
class Point {
  num x;
  num y;

  // The main constructor for this class.
  Point(this.x, this.y) {
    print("Point($x, $y)");
  }

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}

void main() {
  var p1 = new Point(1, 2);
  var p2 = new Point.alongXAxis(4);
}
```

上面代码对应的输出结果：
```Dart
Point(1, 2)
Point(4, 0)
```

#### 静态构造函数(Constant constructors)
如果类的对象不会发生变化，可以构造一个编译时的常量构造函数。
为了实现上述过程，定义格式如下：
1. 将所有的类的变量定义为final 类型。
2. 定义const 类型的构造函数。
```Dart
class ImmutablePoint {
  final num x;
  final num y;
  const ImmutablePoint(this.x, this.y);
  static final ImmutablePoint origin =  const ImmutablePoint(0, 0);
}
```

### 工厂构造函数(Factory constructors)
`factory ` 关键字的功能，当实现构造函数但是不想每次都创建该类的一个实例的时候使用。
```Dart
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to the _ in front
  // of its name.
  static final Map<String, Logger> _cache =
  <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = new Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) {
      print(msg);
    }
  }
}
void main() {
  var p1 = new Logger("1");
  p1.log("2");


  var p2 = new Logger("11");
  p2.log("21");

}
```

上面代码的输出结果：
```Dart
2
21
```

## 方法(Methods)
方法是对象提供的函数功能。
### Getters and Setters
`get() `和`set() `方法是Dart 语言提供的专门用来读取和写入对象的属性的方法。
每一个类的实例，系统都隐式的包含有get()和set() 方法。
`get() `和`set() `的例子：
```Dart
class Rectangle {
  num left;
  num top;
  num width;
  num height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right             => left + width;
      set right(num value)  => left = value - width;
  num get bottom            => top + height;
      set bottom(num value) => top = value - height;
}

main() {
  var rect = new Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -3);
}
```

上面例子对应的输出为：
```Dart
Observatory listening on http://127.0.0.1:33439
Unhandled exception:
'file:///opt/program/Dart/0910.dart': Failed assertion: line 20 pos 10: 'rect.left == -3' is not true.
#0      _AssertionError._throwNew (dart:core-patch/errors_patch.dart:27)
#1      _AssertionError._checkAssertion (dart:core-patch/errors_patch.dart:34)
#2      main (file:///opt/program/Dart/0910.dart:20:10)
#3      _startIsolate.<anonymous closure> (dart:isolate-patch/isolate_patch.dart:261)
#4      _RawReceivePortImpl._handleMessage (dart:isolate-patch/isolate_patch.dart:148)

Process finished with exit code 255
```

### 抽象方法(Abstract methods)
抽象方法类似与Java语言中的interface 的概念。在当前不具体的实现方法，只是写好定义接口，具体实现留着调用的人去实现。
可以使用`abstract `关键字定义抽象方法。

抽象方法的例子：
```Dart
abstract class Doer {
  // ...Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // ...Provide an implementation, so the method is not abstract here...
  }
}
```

### 重载操作
可以重载的操作符如下表所示：
1 | 2 | 3 | 4
--- | --- | --- | ---
<	| +  | 	|	[]
>	| /	 |  ^	| []=
<= 	| ~/ |  &	| ~
>=	| *	 | <<	| ==
–	| %	 | >> | 	 

如果定义矢量的话，可以定义`+ ` 方法我操作两个矢量的实例。

下面的例子就是重载了`+ ` 和`- `操作的例子：
```Dart
class Vector {
  final int x;
  final int y;
  const Vector(this.x, this.y);

  /// Overrides + (a + b).
  Vector operator +(Vector v) {
    return new Vector(x + v.x, y + v.y);
  }

  /// Overrides - (a - b).
  Vector operator -(Vector v) {
    return new Vector(x - v.x, y - v.y);
  }
}

main() {
  final v = new Vector(2, 3);
  final w = new Vector(2, 2);

  // v == (2, 3)
  assert(v.x == 2 && v.y == 3);

  // v + w == (4, 5)
  assert((v + w).x == 4 && (v + w).y == 5);

  // v - w == (0, 1)
  assert((v - w).x == 0 && (v - w).y == 1);
}
```

如果重载了`== `，同时也需要重载对象的`hashCode `的`get() ` 方法。

### 抽象类（Abstract classes）
使用`abstract ` 关键字定义一个抽象类，抽象类是不能实例化的。
抽象类通常用来定义接口。

假如需要将抽象类实例化，需要定义一个`factory constructor `。

抽象类通常会包含一些抽象的方法。
下面是包含抽象方法的例子：
```Dart
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // ...Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
```

没有使用`abstract ` 关键字的类，即使包含抽象方法，也可以被实例化。
```Dart 
class SpecializedContainer extends AbstractContainer {
  // ...Define more constructors, fields, methods...

  void updateChildren() {
    // ...Implement updateChildren()...
  }

  // Abstract method causes a warning but
  // doesn't prevent instantiation.
  void doSomething();
}
```

### 隐式的接口(Implicit interfaces)
每一个类都隐式的定义一个接口，这个接口包含了这个类的所有实例成员和它实现的所以接口。

如果相应创建一个类A, 这个类A 支持类B 提供的API函数，但是不继承B 的实现，则类A 需要继承类B 的接口。

继承的例子如下：
```Dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Imposter implements Person {
  // We have to define this, but we don't use it.
  final _name = "";

  String greet(who) => 'Hi $who. Do you know who I am?';
}

greetBob(Person person) => person.greet('bob');

main() {
  print(greetBob(new Person('kathy')));
  print(greetBob(new Imposter()));
}
```
上面代码对应的输出为：
```Log
Observatory listening on http://127.0.0.1:33001
Hello, bob. I am kathy.
Hi bob. Do you know who I am?

Process finished with exit code 0
```

### 扩展类(Extending a class)
使用`extends ` 关键字来创建一个子类，`super `关键子来指定父类。
```Dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ...
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ...
}
```
子类可以重载实例的方法，getters，setter。

可以使用`override `关键字来注释这个重载的方法。

### 枚举类型(Enumerated types)
枚举类型是一种特殊的类，通常用来表示相同类型的一组常量值。
每个枚举类型都用于一个`index `的getter，用来标记元素的元素位置。第一个枚举元素的标是0 。
```Dart
enum Color {
  red,
  green,
  blue
}

assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
```

获取枚举类中所有的值，使用`value `常数。
```Dart
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
```

因为枚举类里面的每个元素都是相同类型，可以使用switch 语句来针对不同的值做不同的处理。
```Dart
enum Color {
  red,
  green,
  blue
}
// ...
Color aColor = Color.blue;
switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor);  // 'Color.blue'
}
```

枚举类型不能继承，实例化。

### 使用’mixins‘ 功能给类添加新的功能
`mixins `是一种方便重用一个类的代码的方法。
使用`with ` 关键字来实现`mixins `的功能。

`with `用法的实例：
```Dart
class Musician extends Performer with Musical {
  // ...
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

### 类变量和方法(Class variables and methods)
使用`static `关键字来使用类范围内的变量及方法

#### 类常量
类常量的作用范围是类的内部。
类常量只有在被使用的时候才会被调用。
```Dart
class Color {
  static const red =
      const Color('red'); // A constant static variable.
  final String name;      // An instance variable.
  const Color(this.name); // A constant constructor.
}

main() {
  assert(Color.red.name == 'red');
}
```

#### 类方法
可以将类方法当做编译时的常量使用。
```Dart
import 'dart:math';

class Point {
  num x;
  num y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

main() {
  var a = new Point(2, 2);
  var b = new Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(distance < 2.9 && distance > 2.8);
}
```
