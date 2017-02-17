Dart 语言简易教程(一)： http://www.jianshu.com/p/8a62b1a2fd75
Dart 语言简易教程(二)： http://www.jianshu.com/p/b2153a32dd8b
Dart 语言简易教程(三)： http://www.jianshu.com/p/6d2495a0d3d7
Dart 语言简易教程(四):   http://www.jianshu.com/p/fdd046a6dc82
Dart 语言简易教程(五) http://www.jianshu.com/p/83adc77839b6
Dart 语言简易教程(六)  http://www.jianshu.com/p/78d317b2ea79

# Dart 语言简易教程(七)

## 泛型(Generics)
使用`<...> ` 的方式来定义泛型。

### 为什么使用泛型
1. 虽然Dart 语言中类型是可选的，但是明确的指明使用的是泛型，会让代码更好理解。
	```Dart
	var names = new List<String>();
	names.addAll(['Seth', 'Kathy', 'Lars']);
	// ...
	names.add(42); // Fails in checked mode (succeeds in production mode).
	```
2. 使用泛型减少代码重复。
	代码片段一：
	```Dart
	abstract class ObjectCache {
	  Object getByKey(String key);
	  setByKey(String key, Object value);
	}
	```

	代码片段二：
	```Dart
	abstract class StringCache {
	  String getByKey(String key);
	  setByKey(String key, String value);
	}
	```
	以上的两段代码可以使用泛型简化如下：
	```Dart
	abstract class Cache<T> {
	  T getByKey(String key);
	  setByKey(String key, T value);
	}
	```

### 用于集合类型(Using collection literals)
泛型用于List 和 Map 类型参数化。
 List: `<type> `
Map: `<keyType, valueType> `

例子：
```Dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```

### 在构造函数中参数化
List 类型的例子：
```Dart
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = new Set<String>.from(names);
```

Map 类型的例子：
```Dart
var views = new Map<int, View>();
```

### 泛型集合
Dart 语言的泛型是具体的，在运行时会包含泛型的实际类型。
```Dart
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
```

### 现在参数化类型
当实现一个泛型时，如果需要限制它参数的类型，可以使用`extends `关键字。
```Dart
// T must be SomeBaseClass or one of its descendants.
class Foo<T extends SomeBaseClass> {...}

class Extender extends SomeBaseClass {...}

void main() {
  // It's OK to use SomeBaseClass or any of its subclasses inside <>.
  var someBaseClassFoo = new Foo<SomeBaseClass>();
  var extenderFoo = new Foo<Extender>();

  // It's also OK to use no <> at all.
  var foo = new Foo();

  // Specifying any non-SomeBaseClass type results in a warning and, in
  // checked mode, a runtime error.
  // var objectFoo = new Foo<Object>();
}
```

## 库和可见性(Libraries and visibility)
使用`import ` 和 `library ` 机制可以方便的创建一个模块或分享代码。
一个Dart 库不仅能够提供相应的API，还可以包含一些以`_ `开头的变量用于在库内部使用。
每一个Dart 应用都是一个库，即使它没有使用库机制。
库可以方便是使用各种类型的包。

### 引用库
通过`import ` 语句在一个库中引用另一个库的文件。

import 的例子：
```Dart
import 'dart:html';
```

在`import `语句后面需要接上库文件的路径。
- 对Dart 语言提供的库文件以`dart:xx ` 格式
- 其它第三方的库文件使用`package:xx `格式

```Dart
import 'dart:io';
import 'package:mylib/mylib.dart';
import 'package:utils/utils.dart';
```

### 指定一个库的前缀(Specifying a library prefix)
当引用的库拥有相互冲突的名字，可以为其中一个或几个指定不一样的前缀。
```Dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;
// ...
Element element1 = new Element();           // Uses Element from lib1.
lib2.Element element2 = new lib2.Element(); // Uses Element from lib2.
```

### 引用库的一部分
如果只需要使用库的一部分内容，可以有选择的引用。
- `show ` 关键字：只引用一点
- `hide ` 关键字：除此之外都引用

例子如下：
```Dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

### 延迟加载库(Deferred loading a library)
延迟加载机制，可以在需要使用的时候再加载库。

使用延迟加载库的原因：
- 减少应用启动时间
- 只加载必要的功能

为了实现延迟加载，必须使用`deferred as ` 关键字。
```Dart
import 'package:deferred/hello.dart' deferred as hello;
```
然后在需要使用的时候调用`loadLibrary() `方法。
```Dart
greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

可以在代码中多次调用`loadLibrary() `方法。但是实际上它只会被执行一次。

使用延迟加载的注意事项：
- 延迟加载的内容只有在加载后才存在。
- Dart 隐式的将`deferred as `改为了`deferred as namespace`。

## 异步(Asynchrony support)
Dart 语言是目前少数几个支持异步操作的语言。

一般使用`async `函数和`await `表达式实现异步操作。

### 'asynchronous '功能
Dart 库提供`asynchronous `的功能。该功能提供接口来进行耗费时间的操作，二调用的主代码不用等待耗时操作执行完成后才进行操作。该功能返回`Future `或`Stream `对象。

可以通过如下的方式来获取`asynchronous `功能返回的`Future `对象的值。
- 使用`async `函数和`await `表达式。
- 使用`Future `功能提供的API。

可以通过如下的方式来获取asynchronous `功能返回的`Stream `对象的值。
- 使用`async ` 和一个异步的循环(`await for`)。
- 使用`Stream `的相关API。

代码使用了`async `或`await `就是异步处理，虽然代码看起来像是同步处理的。
```Dart
await lookUpVersion()
```

必须在一个使用了`async `关键字标记后的函数中来使用`await `表达式。
```Dart
checkVersion() async {
  var version = await lookUpVersion();
  if (version == expectedVersion) {
    // Do something.
  } else {
    // Do something else.
  }
}
```

`await `表达式可以与`try `，`catch `和`finally `语句搭配在一起使用。
```Dart
try {
  server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4044);
} catch (e) {
  // React to inability to bind to the port...
}
```

### 声明异步功能(Declaring async functions)
一个异步函数必须是一个被`async `修饰符标记的函数。

虽然异步的函数中可能执行耗时的操作，但是函数本身在调用后将会立即返回，即使函数体一条语句也没执行。

给函数添加`async `修饰符将使函数返回一个`Future `类型。
修改前的代码：
```Dart
String lookUpVersionSync() => '1.0.0';
```

修改后的代码：
```Dart
Future<String> lookUpVersion() async => '1.0.0';
```

### 在`Future ` 中使用`await ` 表达式
`await `表达式的格式：
```Dart
await expression
```

在异步的代码里可以多次使用`await ` 表达式。
```Dart
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

在`await `表达式中，表达式通常是一个`Future `。如果表达式不是`Future ` 类型，它将自动被包装为`Future `类型。
`Future `类型表明了回传值的的类型是是一个对象。

如果`await `不工作，请确认`await `处于`async `函数中。即使是在main函数中，规则同样实用。
```Dart
main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```

### 在`Streams `中使用异步循环
异步循环的格式：
```Dart
// parameter ‘expression ’ must have type Stream.
await for (variable declaration in expression) {
  // Executes each time the stream emits a value.
}
```
上面代码中的函数参数 'exprenssion ' 必须是一个'Stream '类型。

异步循环的执行流程如下：
1. 等待 stream 发出数据
2. 执行函数体，并将变量的值变更为`1. `里面的数据。
3. 重复`1. `，`2. `直到stream 对象被关闭。

在上面的`3. `中的效果可以使用`break `语句和`return `语句来实现。

如果异步循环不工作，请确认异步循环处于`async `函数中。即使是在main函数中，规则同样实用。
```Dart
main() async {
  ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  ...
}
```
