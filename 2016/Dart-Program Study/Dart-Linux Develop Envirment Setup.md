google Fuchsia系统 及 dart语言简介： 
http://www.jianshu.com/p/8d153c6a75d0

# Dart Linux 开发环境搭建
主要包括如下内容：
- 在linux平台安装Dart
- 在linux安装dart SDK
- “hello world” Sample

## 在linux平台安装Dart
### 使用apt-get 安装dart
依次在终端中执行如下的命令:

```shell
# Enable HTTPS for apt.
$ sudo apt-get update
$ sudo apt-get install apt-transport-https

# Get the Google Linux package signing key.
$ sudo sh -c 'curl https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -'

# Set up the location of the stable repository.
$ sudo sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'

$ sudo apt-get update
```

## 安装dart SDK
在终端中执行如下命令：
```shell
$ sudo apt-get install dart
```

## “hello world” sample
dart 为了方便开发，提供网页版本的开发，编译，运行环境。
在网页版中运行“hello world” 的方法：
1. 浏览器打开： https://dartpad.dartlang.org
2. 在编辑框中输入如下代码
```dart
void main() {
  print('Hello, World!');
}
```
3. 在右边将看到对应的输出。
![Dart "HelloWorld" Sample](http://upload-images.jianshu.io/upload_images/1940331-221c0acbd21e1bd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
