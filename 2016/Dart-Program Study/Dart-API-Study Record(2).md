Dart API 学习记录(一)  http://www.jianshu.com/p/388d986c0f48

# Dart API 学习记录(二)
## `dart:async `库
`dart:async `库是Dart 语言支持异步编程的基础。
库中提供了Future 类和Stream 类。这两个类是理解Dart 语言异步机制的基础。

在使用前async 库的功能前需要在代码中添加如下语句:
```Dart
import 'dart:async'
```

### Future 类
Future 对象返回值表明当前时刻的计算结果可能还不可用。
Future 实例会在计算结束后再返回结果，而不是现在立即返回。

Future 通常用来操作冗长的计算，比如大规模的I/O 操作或与用户进行交互。

下面的例子将返回一个Future:
```Dart
import 'dart:io';

void bindServer(){
  HttpServer.bind('127.0.0.1', 4444)
      .then((server) => print('${server.isBroadcast}'))
      .catchError(print);
}
void main(){
  bindServer();
}
```
在例子中`Future.then `注册了一个回调函数，当bind() 方法执行完成后开始运行。回调函数的功能是: 当bind()执行成功后会返回一个HttpServer 的对象，在回调函数中会返回该HttpServer的一个熟悉值。

### Stream 类
一个Stream 对象提供了一个异步数据的队列。在这个队列中可能包含事件(鼠标点击)，大数据块.

利用Stream 方法来读取文件的例子：
```Dart
import 'dart:async';
import 'dart:io';
import 'dart:convert';

void readFile(f){
  Stream<List<int>> stream = new File(f).openRead();
  stream.transform(UTF8.decoder).listen(print);
}
void main(){
  String filename = "/opt/program/Dart/dart-code/test_data/temp.py";
  readFile(filename);
```

## Asynchronous Programming: Futures
以下的内容来源于下列地址:
https://www.dartlang.org/tutorials/language/futures

### 要点总结
- Dart 语言是单线程的。
- 同步的代码可能导致代码阻塞。
- 可以使用Future 方法来执行异步操作。
- 在异步方法里使用`await() `来暂停代码执行，直到Future 执行完成。
- 也可使用Future 的`then() `方法来实现上面的目的。
- 在异步方法里使用`try...catch... `来获取异常。
- 也可是使用Future 的`catchError() `方法来实现上面的目的。
- 可以就多个异步的方法链接起来顺序执行。

### 简介
可能导致代码阻塞的例子：
```Dart
void printDailyNewsDigest(){
// Synchronous code
    String news = gatherNewsReports(); // Can take a while.
    print(news);

}
String gatherNewsReports(){
  return "<gathered news goes here>";
}
String printWinningLotteryNumbers(){
  return "Winning lotto numbers: [23, 63, 87, 26, 2]";
}
String printWeatherForecast(){
  return "Tomorrow's forecast: 70F, sunny";
}
String printBaseballScore(){
  return "Baseball score: Red Sox 10, Yankees 0";
}


void main(){
  printDailyNewsDigest();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}
```

对应输出为:
```Log
<gathered news goes here>
Winning lotto numbers: [23, 63, 87, 26, 2]
Tomorrow's forecast: 70F, sunny
Baseball score: Red Sox 10, Yankees 0
```

### 使用说明
Future 代表着在将来会获取到一个值。
当Future 被调用时，会发生：
1. 将需要完成的工作进行队列化并返回一个Future 的对象。
2. 当待执行的工作完成并获取到最后的值后，Future 对象以这个值结束。

可以通过下面的方式获取Future 的返回值。
1. 使用async 和 await
2. 使用Future 提供的API

### 使用async 和 await实现异步通信
async 和 await 是Dart 语言中异步编程支持的关键字。使用这两个关键字可以在不使用Future API 的情况下实现异步编程。

使用async 和 await实现异步通信的例子：
```Dart
import 'dart:html';
import 'dart:async';

printWinningLotteryNumbers() {
  print('Winning lotto numbers: [23, 63, 87, 26, 2]');
}

printWeatherForecast() {
  print('Tomorrow\'s forecast: 70F, sunny.');
}

printBaseballScore() {
  print('Baseball score: Red Sox 10, Yankees 0');
}

// Imagine that this function is more complex and slow. :)
Future gatherNewsReports_async() async {
  String path =     'https://www.dartlang.org/f/dailyNewsDigest.txt';
  return (await HttpRequest.getString(path));
}

Future printDailyNewsDigest_async() async {
  String news = await gatherNewsReports_async();
  print(news);
}

void asynchronousCode(){
  printDailyNewsDigest_async();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}

main() {
  asynchronousCode();
}
```

上面的例子对应的输出结果：
```Dart
Winning lotto numbers: [23, 63, 87, 26, 2]
Tomorrow's forecast: 70F, sunny.
Baseball score: Red Sox 10, Yankees 0
<gathered news goes here>
```

上面代码的执行流程图：
![async-await](http://upload-images.jianshu.io/upload_images/1940331-b1604b3c4bb805ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 程序开始执行。
2. 主函数调用`asynchronousCode() `函数。
3. `asynchronousCode() `函数调用`printDailyNewsDigest_async() `函数，然后函数立即返回一个Future 对象。
4. `asynchronousCode() `函数其它中接下来的代码依次开始执行。以此执行`  printWinningLotteryNumbers() `，`printWeatherForecast() `，`printBaseballScore() `函数，并返回对应的结果。
5. `printDailyNewsDigest_async() `函数体开始执行。
6. 当执行到await 语句时：程序暂停，等待`gatherNewsReports_async() `函数执行结果。
7. 一旦`gatherNewsReports_async() `函数的Future 执行完成，`printDailyNewsDigest_async() `函数将继续执行。
8. 当`printDailyNewsDigest_async() `函数执行完成后，程序结束。

#### 处理error
如果Future 的函数执行中发生了error，怎Future 的返回值中就会包含error 信息。
可以通过try-catch 语句来处理这些异常。

处理异常的例子：
```Dart
import 'dart:html';
import 'dart:async';

Future printDailyNewsDigest() async {
  try {
    String news = await gatherNewsReports();
    print(news);
  } catch (e) {
    // ... handle error ...
  }
}

main() {
  printDailyNewsDigest();
  printWinningLotteryNumbers();
  printWeatherForecast();
  printBaseballScore();
}

printWinningLotteryNumbers() {
  print('Winning lotto numbers: [23, 63, 87, 26, 2]');
}

printWeatherForecast() {
  print('Tomorrow\'s forecast: 70F, sunny.');
}

printBaseballScore() {
  print('Baseball score: Red Sox 10, Yankees 0');
}

// Imagine that this function is more complex and slow. :)
Future gatherNewsReports() async {
  String path =
      'https://www.dartlang.org/f/dailyNewsDigest.txt';
    String content = await HttpRequest.getString(path);
    return content;
}

```

#### 顺序执行
可以使用多个await 语句来确认上一条一句结束了才执行下面的语句。

例子如下：
```Dart
// Sequential processing using async and await.
main() async {
  await expensiveA();
  await expensiveB();
  doSomethingWith(await expensiveC());
}
```

### Future API 实现异步通信
await / async 在Dart 1.9版本里面才添加进来。

可以通过`then() 方法来注册一个回调函数。这个回调函数将在Future 完成后开始执行。

#### 处理error
在Future API 中提供了`catchError() `方法来处理Future 执行中的异常。

#### 调用多个Future 类型的返回值

##### 使用多个then() 方法来链接多个函数。
```Dart
expensiveA().then((aValue) => expensiveB())
            .then((bValue) => expensiveC())
            .then((cValue) => doSomethingWith(cValue));
```

上面的代码等待expensiveA 执行完成后开始执行expensiveB, 在expensiveB doSomethingWith。

##### 调用Future.wait() 方法来链接多个函数。
```Dart
// Parallel processing using the Future API
Future.wait([expensiveA(), expensiveB(), expensiveC()])
      .then((List responses) => chooseBestResponse(responses))
      .catchError((e) => handleError(e));
```

上面的代码等待expensiveA(), expensiveB(), expensiveC() 都执行完成后才开始执行then() 中的例子。

## Asynchronous Programming: Streams
以下的内容来源于下列地址:
https://www.dartlang.org/tutorials/language/streams

### 要点
1. Stream 提供数据的异步队列
2. 数据队列里面包含用户交互数据，或从文件中读取的数据。
3. 可以使用`await for ` 或`listen() `来处理stream 对象的数据。
4. Stream API 提供错误处理。
5. 包含两种类型的Stream：单个订阅，广播Stream

### 接受Stream 事件
虽然Stream 对象的创建有很多种方法，但是使用方法却是相同的。使用方法: 使用	`await for `语句去操作一个Stream 对象的事件的迭代对象，就像for 循环操作迭代对象一样。

`await for `语句只能操作使用`async `关键字标记的函数。
例子如下：
```Dart
import 'dart:async';

Future<int> sumStream(Stream<int> stream) async {
  var sum = 0;
  await for (var value in stream) {
    sum += value;
  }
  return sum;
}

Stream<int> countStream(int to) async* {
  for (int i = 1; i <= to; i++) {
    yield i;
  }
}

main() async {
  var stream = countStream(10);
  var sum = await sumStream(stream);
  print(sum); // 55
}
```

### 错误事件
当没有收到新的事件时，Stream 对象会结束运行。
当接受到新事件的时候，系统胡通知Stream 收到了新的事件。
当读取到的Stream 事件在await for 循环执行时，当Stream 结束后，await for 循环也就结束了。

当Stream 对象运行过程中有错误发生，大部分的Stream 将停止。同时Stream 对象将至少发送一条错误信息。
可以使用try-catch 语句间await for 语句包括起来，用这种方法来处理error 信息。

处理Stream 错误的例子：
```Dart
import 'dart:async';

Future<int> sumStream(Stream<int> stream) async {
  var sum = 0;
  try {
    await for (var value in stream) {
      sum += value;
    }
  } catch (error) {
    return -1;
  }
  return sum;
}

Stream<int> countStream(int to) async* {
  for (int i = 1; i <= to; i++) {
    if (i == 4) {
      throw "Whoops!";  // Intentional error
    } else {
      yield i;
    }
  }
}

main() async {
  var stream = countStream(10);
  var sum = await sumStream(stream);
  print(sum); // -1
}

```

### Stream 对象处理
可以很方便的通过Stream 的await for 循环来依次处理相应的事件。

下面的例子，可以方便找到 数值 > 0的数值的下标位置。
```Dart
import 'dart:async';

Future<int> lastPositive(Stream<int> stream) async {
  var lastValue = null;
  await for (var value in stream) {
    if (value < 0) continue;
    lastValue = value;
  }
  return lastValue;
}

main() async {
  var data = [1, -2, 3, -4, 5, -6, 7, -8, 9, -10];
  var stream = new Stream.fromIterable(data);
  var lastPos = await lastPositive(stream);
  print(lastPos); // 9
}
```

### Stream 两种类型
#### 单个订阅类型 (Single subscription streams)
大部分的Stream 实例会包含整个事件队列的一部分。每一个事件必须在正确的位置，并且不允许有任何数据遗失。这种类型类似于阅读文件或接受网页服务。
这是类似于TCP socket，不允许有数据遗失。

这种类型的Stream，只能被监听一次。
假如再次监听，将会导致之前的事件队列丢失，并导致Stream 不再接受数据。

#### 广播类型(Broadcast streams)
这种类型的Stream 可以只一次处理一个事件。
可以对这种类型实施多次监听。

### Stream 的处理方法
完整方法列表:

返回值 | 函数
--- | ---
`Future<T\>` | get first
`Future<T>` | get last
`Future<T> `| get single
`Future<int>` | get length
`Future<bool>` | get isEmpty
`Future<T>` | firstWhere(bool test(T event), {T orElse()})
`Future<T>` | lastWhere(bool test(T event), {T orElse()})
`Future<T>` | singleWhere(bool test(T event), {T orElse()})
`Future<T>` | reduce(T combine(T previous, T element))
`Future` | fold(initialValue, combine(previous, T element))
`Future<String>` | join([String separator = ""])
`Future<bool>` | contains(Object needle)
`Future` | forEach(void action(T element))
`Future<bool>` | every(bool test(T element))
`Future<bool>` | any(bool test(T element))
`Future<List<T>>` | toList()
`Future<Set<T>>` | toSet()
`Future<T>` | elementAt(int index)
`Future` | pipe(StreamConsumer<T> consumer)
`Future` | drain([var futureValue])

简单的实例：
```Dart
Future<bool> contains(Object element) async {
  await for (var event in this) {
    if (event == element) return true;
  }
  return false;
}

Future forEach(void action(T element)) async {
  await for (var event in this) {
    action(event);
  }
}

Future<List<T>> toList() async {
  var result = <T>[];
  await this.forEach(result.add);
  return result;
}

Future<String> join([String separator = ""]) async {
  return (await this.toList()).join(separator);
}
```

### Stream 对象的修改方法
方法列表：

返回值 | 函数
--- | ---
`Stream<T> ` | where(bool test(T event))
`Stream ` | map(convert(T event))
`Stream ` | expand(Iterable expand(T element);
`Stream<T> ` | take(int count)
`Stream<T> ` | skip(int count)
`Stream<T> ` | takeWhile(bool test(T element))
`Stream<T> ` | skipWhile(bool test(T element))
`Stream<T> ` | distinct([bool equals(T previous, T next)])
`Stream ` | asyncExpand(Stream expand(T element))
`Stream ` | asyncMap(Future convert(T event))
`Stream ` | timeout(Duration timeLimit, {void onTimeout(EventSink sink)})
`Stream<T> ` | handleError(Function onError, {bool test(error)})
`Stream ` | transform(StreamTransformer<T, dynamic> streamTransformer)

使用这些函数，阅读文件的例子：
```Dart
import 'dart:io';
import 'dart:async';
import 'dart:convert';

main(args) async {
  var file = new File(args[0]);
  var lines = file
      .openRead()
      .transform(UTF8.decoder)
      .transform(const LineSplitter());
  await for (var line in lines) {
    if (!line.startsWith('#')) {
      print(line);
    }
  }
}
```

### The listen() method
Stream 的listen() 方法是更底层的方法，其它的Stream的方法都是在listen 之上定义的。

创建一个Stream 类型时，可以只继承Stream 类，并实现listen 方法。



