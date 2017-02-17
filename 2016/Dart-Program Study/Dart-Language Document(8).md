Dart 语言简易教程(一)： 
http://www.jianshu.com/p/8a62b1a2fd75

Dart 语言简易教程(二)： 
http://www.jianshu.com/p/b2153a32dd8b

Dart 语言简易教程(三)： 
http://www.jianshu.com/p/6d2495a0d3d7

Dart 语言简易教程(四):   
http://www.jianshu.com/p/fdd046a6dc82

Dart 语言简易教程(五) 
http://www.jianshu.com/p/83adc77839b6

Dart 语言简易教程(六)  
http://www.jianshu.com/p/78d317b2ea79

Dart 语言简易教程(七)  
http://www.jianshu.com/p/023cea0b4a52

# Dart 语言简易教程(八)
## 可调用的类(Callable classes)
Dart 语言中为了能够让类像函数一样能够被调用，可以实现`call() `方法。
实例代码：
```Dart
class WannabeFunction {
  call(String a, String b, String c) => '$a $b $c!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out');
}
```
上面代码的输出结果为：
```Log
Hi there, gang!
```

### 隔离
Dart 语言中每个app都运行在独立的隔离空间中，每个隔离空间都有自己的内存堆栈，确保每个隔离空间的状态不会被其它的隔离空间访问。

隔离空间的概念和类似与Chrome 浏览器的沙盒概念，每个应用独自运行在自己的空间里，互不干扰。

这样做的好处是：安全，减少了代码复杂性。


## 类型定义(Typedefs)
在Dart 语言中，函数也是一个对象，和字符串或数字一样。

原始代码：
```language
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
```

使用了typedef 的代码：
```Dart
typedef int Compare(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
```

因为`typedef `只是简单的别名，通过用来检查函数类型。
例子如下：
```Dart
typedef int Compare(int a, int b);

int sort(int a, int b) => a - b;

main() {
  assert(sort is Compare); // True!
}
```

## 元数据(Metadata)
使用元数据给代码添加更多的信息。
元数据是以`@ `开始的修饰符。在`@ ` 后面接着编译时的常量或调用一个常量构造函数。

目前Dart语言提供的三个可用的@修饰符：
- @deprecated
- @override
- @proxy

使用`@deprecated `修饰符的例子：
```Dart
class Television {
  /// _Deprecated: Use [turnOn] instead._
  @deprecated
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {
    print('on!');
  }
}
```

元数据可以修饰library(库), class(类), typedef(类型定义), type parameter(类型参数), constructor(构造函数), factory(工厂函数), function(函数), field(作用域), parameter(参数), 或者 variable declaration(变量声明)。

### 定义自己的元数据
通过`library `来定义一个库，在库中定义一个相同名字的类，然后在类中定义const 修饰的相同名字的方法。

元数据定义的方法：
```Dart
library todo;

class todo {
  final String who;
  final String what;

  const todo(this.who, this.what);
}
```

元数据使用的例子：
```Dart
import 'todo.dart';

@todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
```


## 注释
Dart 支持三种注释类型：
- 单行注释
- 多行注释
- 文档注释

### 单行注释
单行注释以`// `开头，从`// `开始到一行结束的所以内容都会被Dart 编译器忽略。
```Dart
main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
```

### 多行注释
单行注释以`/* `开头，以`*/ `结束。从`/* `开头到`*/ `结束间的所有内容都会被Dart 编译器忽略掉。
```Dart
main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = new Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
```

### 文档注释
文档注释以`/** `或`/// `开头
```Dart
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
class Llama {
  String name;
  /**
   * Feeds your llama [Food].
   * The typical llama eats one bale of hay per week.
   **/
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
```
