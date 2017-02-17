Dart 语言简易教程(一)： http://www.jianshu.com/p/8a62b1a2fd75
Dart 语言简易教程(二)： http://www.jianshu.com/p/b2153a32dd8b
Dart 语言简易教程(三)： http://www.jianshu.com/p/6d2495a0d3d7
Dart 语言简易教程(四):   http://www.jianshu.com/p/fdd046a6dc82
Dart 语言简易教程(五) http://www.jianshu.com/p/83adc77839b6
Dart 语言简易教程(六)  http://www.jianshu.com/p/78d317b2ea79
Dart 语言简易教程(七)  http://www.jianshu.com/p/023cea0b4a52
Dart 语言简易教程(八)  http://www.jianshu.com/p/d28292662be5

# Dart API 学习记录(一)
所有资料基于当前最新的版本：https://api.dartlang.org/stable/1.19.1/index.html

在官方提供的的API 中，有如下3个包的使用频率最高。
- dart:core: Core functionality such as strings, numbers, collections, errors, dates, and URIs.
- dart:html: DOM manipulation for web apps.
- dart:io: I/O for command-line apps.

其中`dart:core` 库是Dart 语言初始已经包含的库。其它的任何包在使用前都需要加上import 语句。
例如需要使用`dart:html `,可以使用如下的命令：
```Dart
import 'dart:html';
```

使用官方提供的pub 工具可以安装丰富的第三方库。第三方库的地址为:pub.dartlang.org.

Dart 语言当前提供的库：

包名 | 包含的内容 | (自己理解)
--- | --- | ---
dart:async | Support for asynchronous programming, with classes such as Future and Stream. | 异步编程支持，提供的Future 和 Stream 类。
dart:collection | Classes and utilities that supplement the collection support in dart:core. | 对dart:core 提供更多的集合支持
dart:convert | Encoders and decoders for converting between different data representations, including JSON and UTF-8. | 不同类型(JSON,UTF-8)间的字符编码，解码支持。
dart:core | Built-in types, collections, and other core functionality for every Dart program. | Dart语言内建的类型，对象以及dart 语言核心的功能。
dart:developer | Interact with developer tools such as the debugger and inspector. | 开发者交互工具，包括debugging工具和代码检查工具。
dart:html | HTML elements and other resources for web-based applications that need to interact with the browser and the DOM (Document Object Model). | HTML 元素与浏览器交互。
dart:indexed_db | A client-side key-value store with support for indexes. | 保存用于支持‘ client-side key-value’ 的数据。
dart:io | File, socket, HTTP, and other I/O support for server applications. | 服务器端文件，socket, HTTP 及其它I/O 操作支持。
dart:isolate | Concurrent programming using isolates: independent workers that are similar to threads but don't share memory, communicating only via messages. | 用于隔离不同线程间的数据。
dart:js | Support for interoperating with JavaScript. | 与JavaScript 语言间交互的接口。
dart:js_util | Utility methods to efficiently manipulate typed JSInterop objects in cases where the name to call is not known at runtime. You should only use these methods when the same effect cannot be achieved with @JS annotations. These methods would be extension methods on JSObject if Dart supported extension methods. | 用于处理JavaScript语言中的‘JSInterop objects’，只有在使用'JSInterop objects'无法实现的时候才使用。
dart:math | Mathematical constants and functions, plus a random number generator. | 数字常量及函数，提供随机数算法。
dart:mirrors | Basic reflection in Dart, with support for introspection and dynamic invocation. | Dart语言的反射功能支持，包括自反和动态调用。
dart:svg | Scalable Vector Graphics: Two-dimensional vector graphics with support for events and animation. | 事件和动画的矢量图像支持。
dart:typed_data | Lists that efficiently handle fixed sized data (for example, unsigned 8 byte integers) and SIMD numeric types. | 指定数据长度的数据处理。
dart:web_audio | High-fidelity audio programming in the browser. | 浏览器中高保真音频支持。
dart:web_gl | 3D programming in the browser. | 浏览器中3D支持。
dart:web_sql | An API for storing data in the browser that can be queried with SQL. | 浏览器中SQL查询支持。
