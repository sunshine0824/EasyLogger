# EasyLogger

---

# 1. 介绍

EasyLogger是一款超轻量级(ROM<1.6K, RAM<0.3k)、高性能的C日志库，非常适合对资源敏感的嵌入式软件。相比log4c、zlog这些知名的C日志库，EasyLogger的功能更加简单，提供给用户的接口更少，但上手会很快，也非常适合应用于小型的非嵌入式软件。EasyLogger主要特性如下：

- 支持多种输出方式（终端、文件、串口、485、Flash...）；
- 日志内容可包含级别、时间戳、线程、进程信息；
- 日志输出被设计为线程安全的方式；
- 支持多种操作系统（[RT-Thread](http://www.rt-thread.org/)、UCOS、Linux、Windows...），也支持裸机平台；
- 日志支持 **RAW格式** ,可设定按 **标签** 进行过滤。

> 名词解释：
1、RAW格式：未经过格式化的原始日志。
2、标签：在软件中可以按照文件、模块、功能等方面，对需要打印的日志设定标签，实现日志分类。

# 2. 使用

### 2.1 参数配置

EasyLogger拥有过滤方式、输出格式、输出开关这些属性。

- 过滤方式支持按照标签、级别、关键词进行过滤；
- 可以动态的开启/关闭日志的输出；
- 可设定动态和静态的输出级别（静态：一级开关，通过宏定义；动态：二级开关，通过API接口）。

> 注：目前参数配置及输出方式都是单例模式，即全局只支持一种配置方式。此模式下，软件会较为简单，但是无法支持复杂的输出方式。

### 2.2 输出级别

参考Android Logcat，级别最高为0(Assert)，最小为5(Verbose)。

```
0.[A]：断言(Assert)
1.[E]：错误(Error)
2.[W]：警告(Warn)
3.[I]：信息(Info)
4.[D]：调试(Debug)
5.[V]：详细(Verbose)
```

### 2.3 输出过滤

#### 2.3.1 过滤级别

默认过滤级别为5(详细)，用户可以任意设置。在设置高优先级后，低优先级的日志将不会输出。例如：设置当前过滤的优先级为3(警告)，则只会输出优先级别为警告、错误、断言的日志。

#### 2.3.2 过滤标签

默认过滤标签为空字符串("")，即不过滤。当前输出日志的标签会与过滤标签做字符串匹配，日志的标签包含过滤标签，则该输出该日志。例如：设置过滤标签为WiFi，则系统中包含WiFi字样标签的（WiFi.BSP、WiFi.Protocol、Setting.WiFi）日志都会被输出。

#### 2.3.3 过滤关键词

默认过滤关键词为空字符串("")，即不过滤。检索当前输出日志中是否包含该关键词，包含则允许输出。

> 注：对于配置较低的MCU建议不开启关键词过滤（默认为不过滤），增加关键字过滤将会在很大程度上减低日志的输出效率。实际上过滤关键词功能交给上位机做会更轻松，所以后期的跨平台日志助手开发完成后，就无需该功能。

### 2.4 输出格式

输出格式支持：级别、时间、标签、进程信息、线程信息、文件路径、行号、方法名

> 注：默认为 **RAW格式**，RAW格式日志不支持标签过滤

### 2.5 输出方式

通过用户的移植，可以支持任何一种输出方式。只不过对于某种输出方式可能引入的新功能，目前需要用户自己实现，例如：文件转存，检索Flash日志等等。这些属于日志功能附带小工具，后期会以插件的形式逐步开源出来。下面简单对比下部分输出方式使用场景：

- 终端：方便用户动态查看，不具有存储功能；
- 文件与Flash：都具有存储功能，用户可以查看历史日志。但是文件方式需要文件系统的支持，而Flash方式更加适合应用在无文件系统的小型嵌入式设备中。

### 2.6 Demo

下图为在终端中输入命令来控制日志的输出及过滤器的设置，更加直观的展示了EasyLogger各项功能。

![easylogger](https://raw.githubusercontent.com/armink/EasyLogger/master/docs/images/EasyLoggerDemo.gif)

> 注：以上内容对应的API，可以打开[思维导图](http://naotu.baidu.com/viewshare.html?shareId=ausqm3j44f4k)看到更清晰的逻辑。

# 3. 后期

- 1、Flash存储：在[EasyFlash](https://github.com/armink/EasyFlash)中增加日志存储、读取功能，让EasyLogger与其无缝对接。使日志可以更加容易的存储在 **非文件系统** 中，并具有历史日检索的功能；
- 2、异步输出：目前日志输出与用户代码之间是同步的方式，这种方式虽然软件简单，也不存在日志覆盖的问题。但在输出速度较低的平台下，会由于增加日志功能，而降低软件运行速度。所以后期会增加 **异步输出** 方式，关键字过滤也会放到异步输出中去；
- 3、日志助手：开发跨平台的日志助手，兼容Linux、Windows、Mac系统，打开助手即可查看、过滤（支持正则表达式）、排序、保存日志等，计划使用[NW.js](http://www.oschina.net/p/nwjs)框架；
- 4、文件转档：文件系统下支持文件按容量转档，按时间区分；
- 5、配置文件：文件系统下的配置文件；
- 6、Arduino：增加Arduino lib，并提供其Demo；

# 4. 许可

GPL v3.0 Copyright (c) armink.ztl@gmail.com