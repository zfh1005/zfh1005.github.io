# google Fuchsia系统 及 dart语言简介


## Fuchsia 是啥？
Fuchsia的报道文章：
http://www.tomshardware.com/news/google-fuchsia-new-operating-system,32475.html

google正在开发的第三个操作系统: 
Fuchsia。主要的想用统一的OS来操作IOT设备，移动设备，PC等。

github 地址: 
https://github.com/fuchsia-mirror

Fuchsia采用Magenta kernel，这是一个“Little Kernel”，与Android，chromeOS采用的 Linux kernel的不同。Magenta kernel 是更轻量，更底层，更通用的。Linux 基金会推出的针对IOT的系统 Zephyr（参考网站： https://www.linuxfoundation.org/news-media/announcements/2016/02/linux-foundation-announces-project-build-real-time-operating-system），对google来说，显得太笨重:linux kernel里面的很多东西不是google需要的。

## 为什么推出Fuchsia？
推出Fuchsia 的原因有三个：
- 统一各个平台（IOT，移动设备，PC）
- 避免出现目前Android面临的碎片化的问题
- 控制开发语言（使用自己的开发语言）

## dart 语言
而drat语言可能是其的理想选择。
官方网站： https://www.dartlang.org

Dart 是谷歌在 2011 年推出的编程语言，是一种结构化 Web 编程语言，允许用户通过 Chromium 中所整合的虚拟机（Dart VM）直接运行 Dart 语言编写的程序，免去了单独编译的步骤。以后这些程序将从 Dart VM 更快的性能与较低的启动延迟中受益。

设计目标包括如下几点：
- 提供丰富的开发库
- 简化编程任务
- 更直观的编程语言
- 开发稳定的应用
