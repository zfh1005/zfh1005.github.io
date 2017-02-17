Dart Linux 开发环境搭建： 
http://www.jianshu.com/p/f923b839f5f1

# 在 InteIIiJ IDEA 中搭建 Dart 的开发环境
## InteIIiJ IDEA 简介
InteIIiJ IDEA 是很出名的用IDE,在JAVA，Python，Android开发者中有很广泛的使用。
InteIIiJ IDEA的主要优点：

- 多语言支持
InteIIiJ IDEA官方默认的是做JAVA开发的，IntelliJ在业界被公认为最好的java开发工具之一。
尤其在智能代码助手、代码自动提示、重构、J2EE支持、各类版本工具(git、svn、github等)、JUnit、CVS整合、代码分析、 创新的GUI设计等方面的功能可以说是超常的。
通过各种插件能够很方便的进行JAVA，Python，Javascript，HTML，CSS, MySQL，Go，PHP， Lua，Perl等众多语言的开发。

- 插件支持
凭借非常多的使用者，在 InteIIiJ IDEA 的插件社区越来越多的组织和个人加入到开发者的队伍中。
在插件官网上：http://plugins.jetbrains.com/ 上可以找到各类1800+的各类插件。

## InteIIiJ IDEA 的 Dart 的开发环境搭建
InteIIiJ IDEA 的 Dart 的开发环境搭建，主要分为如下几个部分：
1. Dart SDK 安装
2. IDEA 的 Dart 插件安装
3. Dart 开发环境设置
4. HelloWorld sample

>  本文的相关涉及的软件版本信息：
> 1. IntelliJ IDEA version
IntelliJ IDEA 2016.2.2
Build #IU-162.1628.40, built on August 16, 2016
JRE: 1.8.0_76-release-b216 amd64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o

> 2. Dart version
![Dart version](http://upload-images.jianshu.io/upload_images/1940331-fd95cde8ccdd4c54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 3. PC Information
![ PC Information](http://upload-images.jianshu.io/upload_images/1940331-f0a7715161cb27ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面将依照这个顺序开始讲述。

### Dart SDK 安装
Dart SDK 的安装过程可以参照上一次分享中方法来操作，这里就不再赘述。
Dart 开发环境搭建： http://www.jianshu.com/p/f923b839f5f1

### IDEA 的 Dart 插件安装
依照如下步骤安装 Dart 的插件
1. 依次选择 File -> Setting 打开 IDEA 设定的界面
2. 选择 “Plugins” 选项，在输入输入“Dart”并点击搜索按钮。
![dart_plugins_search.png](http://upload-images.jianshu.io/upload_images/1940331-ec551361b9dc2fbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 在新弹出来的窗口中点击蓝色的安装按钮，等待出现如下的画面后重启IDEA
![dart_install.png](http://upload-images.jianshu.io/upload_images/1940331-47197a7194b8f3da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Dart 开发环境设置
依照如下步骤设置 Dart 开发环境
1. 在重启后打开的 IDEA 中，依次选择 File -> New -> Project 打开 IDEA 的新建工程界面。
2. 在界面的左边的语言的列表中选择Dart语言
![dart_language.png](http://upload-images.jianshu.io/upload_images/1940331-a1b5762c534c5dd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 在出现的界面中选择 Dart SDK 安装路径。
如果是按照上一篇里面的方法安装的 Dart SDK， 则 SDK 的路径是：/usr/lib/dart/。
使用其它的方法安装，请选择对应的安装路径。
![dart_sdk_setting.png](http://upload-images.jianshu.io/upload_images/1940331-e5dff19e76d0e5ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 点击”Next“，在出现的界面中填写 Project 的名字及保存路径。
![project_path.png](http://upload-images.jianshu.io/upload_images/1940331-8f1e332ec08b25d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 点击”Finish“， 等待出现工程界面。
![demo_project.png](http://upload-images.jianshu.io/upload_images/1940331-fd6e31eb46fefea4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### HelloWorld sample
依照如下步骤开发 HelloWorld sample。
1. 选择工程名”Demo“右击，然后点击依次点击 New -> Dart File。
2. 输入需要的文件名。
![file_name.jpg](http://upload-images.jianshu.io/upload_images/1940331-3d3b4156542e62fa.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 在文件中输入如下内容
```dart
void main(){
 print("Hello World!");
}
```
正确的文件：
![right_main.jpg](http://upload-images.jianshu.io/upload_images/1940331-43458e120f47d4be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果代码中有任何的问题，将会在 "Error Information Area" 进行显示，依照错误提示进行修正。
![error_project.jpg](http://upload-images.jianshu.io/upload_images/1940331-82574faaf4734469.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4. 确认文件没有错误后，在按下快捷键`Ctrl + Alt + F10`，在界面的下方将看到程序的输出结果。
![run_result.jpg](http://upload-images.jianshu.io/upload_images/1940331-620a92671baeb08b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


到此在InteIIiJ IDEA 搭建 Dart 的开发环境的相关介绍接已经结束了，接下来来将会一步步的开始介绍Dart语言的相关知识，敬请期待。
