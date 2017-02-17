Dart 语言简易教程(一)： 
http://www.jianshu.com/p/8a62b1a2fd75

Dart 语言简易教程(二)： 
http://www.jianshu.com/p/b2153a32dd8b


Dart 语言简易教程(三)： 
http://www.jianshu.com/p/6d2495a0d3d7

Dart 语言简易教程(四):   
http://www.jianshu.com/p/fdd046a6dc82

# Dart 语言简易教程(五)
## 流程控制语句
Dart 可用的流程控制语句：
- if and else
- for loops
- while and do-while loops
- break and continue
- switch and case
- assert
- try-catch and throw

### if and else
Dart 支持`if ` 及 `else `的多种组合。
和Python，Java，C++ 都有类似的操作。

例子：
```Dart
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
```
 > 如前面的教程里面提到的那样，Dart 中只有变量值为true 的时候才会返回true 的结果。

### for 循环
for 循环的例子：
```Dart
var message = new StringBuffer("Dart is fun");
for (var i = 0; i < 5; i++) {
  message.write('!');
}
```

除了常规的for 循环外，针对可以序列化的操作数，可以使用` forEach() ` 方法。
当不关心操作数的当前的下标的时候，`forEach() `方法是很简便的。
例子：
```Dart
var collection = [0, 1, 2];
for (var x in collection) {
  print(x);
}
```

### While and do-while
while 循环的例子：
```Dart
main() {
  var _temp = 0;
  while(_temp < 5){

    print("this is loop: " + (_temp).toString());
    _temp ++;
  }
}
```

do-while 循环的例子：
```Dart
main() {
  var _temp = 0;

  do{
    print("this is loop: " + (_temp).toString());
    _temp ++;
  }
  while(_temp < 5);
}
```

上面的两个例子都对应如下输出：
```Log
this is loop: 0
this is loop: 1
this is loop: 2
this is loop: 3
this is loop: 4
```
### Break 和 continue
break 用来跳出循环
```Dart
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
```

continue 用来跳过本次循环
```Dart
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

### Switch 和 case
Dart 中switch / case 语句使用`== ` 操作来比较整数，字符串或其它编译过程中的常量，从而实现分支的作用。
switch / case 语句的前后操作数必须是相同的类型的对象实例(即使其中一个操作数属于另一个对象的子类的实例，比较操作也不行。)

每一个非空的case 子句(不是case 判断语句本身，而是case 语句下面的实际操作。)最后都必须跟上break 语句。
```Dart
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break causes an exception!!

  case 'CLOSED':
    executeClosed();
    break;
}
```

```Dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

使用switch / case 语句，配合 continue 语句。可以实现类似 goto 语句的功能。
```Dart
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
    // Continues executing at the nowClosed label.

nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
```

### 断言(assert)
Dart 语言通过使用assert 语句来中断正常的执行流程，中断的前提是：assert 判断的条件为 false 。
```Dart
// Make sure the variable has a non-null value.
assert(text != null);
```

assert 判断的条件可以是任何可以转化为 boolean 类型的对象，即使是函数也可以(此时判断的是函数返回值)。

如果assert 的判断为true, 则继续执行下面的语句。反之则会丢出一个异 AssertionError 。

## 异常(Exceptions)
Dart 代码可以丢出或捕获异常。
异常表明发生了一些不预期的错误。
如果不捕获异常，一旦异常发生，则程序和中断执行并停止运行。

Dart 代码不会在代码中指定代码将会丢出什么异常，所以调用系统代码时，不会强制要求处理异常。这点与Java 的处理方式是不同的。

Dart 提供 Exception 和 Error 类型来处理异常。自己也可以定义属于自己的异常类型。

### Throw
用于抛出异常。
```Dart
throw new FormatException('Expected at least 1 section');
```

也可以通过throw 语句释放任意的类型:
```Dart
throw 'Out of llamas!';
```

因为抛出异常属于表达式，可以将throw 语句放置在 `=> `语句中：
```Dart
distanceTo(Point other) =>
    throw new UnimplementedError();
```

#### 捕获(Catch)
基本用法与Python 相同。
将可能出现异常的代码放置到try 语句中，然后在气候跟上 catch 语句来处理可能出现的异常。
可以通过 `on `语句来指定需要捕获的异常类型，使用`catch ` 来处理异常。

```Dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
```

可以向`catch() `传递1个或2个参数。第一个参数表示：捕获的异常的具体信息，第二个参数表示：异常的堆栈跟踪(stack trace)。

```Dart
  ...
} on Exception catch (e) {
  print('Exception details:\n $e');
} catch (e, s) {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
```

`rethrow `语句用来处理一个异常，同时希望这个异常能够被其它调用的部分使用。
如下的代码：
```Dart
final foo = '';

void misbehave() {
  try {
    foo = "You can't change a final variable's value.";
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    rethrow; // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
```

#### Finally
Dart 的`finally `用来执行那些无论异常是否发生都执行的操作。
```Dart
try {
  breedMoreLlamas();
} catch(e) {
  print('Error: $e');  // Handle the exception first.
} finally {
  cleanLlamaStalls();  // Then clean up.
}
```
