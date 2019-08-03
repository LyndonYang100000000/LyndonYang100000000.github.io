---
layout:     post
title:      Apache Thrift介绍及安装
subtitle:   thrift
date:       2019-08-03
author:     Lyndon
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Thrift
---

#### Thrift是什么

​	Thrift是FaceBook开源出的一种高效的，支持多种编程语言的远程过程调用框架。其数据传输采用二进制格式，相对于XML和JSON体积小，对于高并发，大数据量和多语言的情况下更加有优势。Thrift通过一个中间语言(IDL, 接口定义语言)来定义RPC的接口和数据类型，然后通过一个编译器生成不同语言的代码（目前支持C++,Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, Smalltalk和OCaml）,并由生成的代码负责RPC协议层和传输层的实现

#### 架构图

![](https://raw.githubusercontent.com/YangHao1992/YangHao1992.github.io/master/_img/Thrift.png)

​	Thrift实际上是实现了C/S模式，通过代码生成工具将接口定义文件生成服务器端和客户端代码（可以为不同语言），从而实现服务端和客户端跨语言的支持。用户在Thirft描述文件中声明自己的服务，这些服务经过编译后会生成相应语言的代码文件，然后用户实现服务（客户端调用服务，服务器端提服务）便可以了。其中protocol（协议层, 定义数据传输格式，可以为二进制或者XML等）和transport（传输层，定义数据传输方式，可以为TCP/IP传输，内存共享或者文件共享等）被用作运行时库

#### 下载安装

##### 安装boost库

参考:[http://www.cnblogs.com/LyndonYoung/articles/5288618.html](http://www.cnblogs.com/LyndonYoung/articles/5288618.html)

##### 安装依赖库

​		sudo apt-get install flex
　　sudo apt-get install bison
　　sudo apt-get install byacc
　　sudo apt-get install libssl-dev

​		在安装过程中报错缺哪些库安转即可

##### 下载源码

[http://thrift.apache.org/](http://thrift.apache.org/)

编译安装：sudo ./configure –>sudo make –>sudo make install

安装完毕后，thrift -version,若执行该命令报如下错误：thrift: error while loading shared libraries: libthriftc.so.0；则按照如下链接解决

[https://www.cnblogs.com/qq952693358/p/6193842.html](https://www.cnblogs.com/qq952693358/p/6193842.html)

Lyndon@test_slave:~$ thrift -version

Thrift version 0.10.0

安装成功，之后可参照

[http://www.cnblogs.com/LyndonYoung/articles/7977654.html](http://www.cnblogs.com/LyndonYoung/articles/7977654.html)

开始第一个thrift例子