---
title: windows网络编程(1)-select
tags:
  - windows
  - select
  - 网络编程
  - 完成端口
categories:
  - 网络编程
  - windows
date: 2019-11-25 19:15:13
---
### 概述

如果你想在Windows平台上构建服务器应用，那么I/O模型是你必须考虑的
Windows操作系统提供了五种I/O模型，分别是：

* 选择（select）
* 异步选择（WSAAsyncSelect）
* 事件选择（WSAEventSelect）
* 重叠I/O（Overlapped I/O）
* 完成端口（Completion Port) 

每一种模型适用于一种特定的应用场景。程序员应该对自己的应用需求非常明确，综合考虑到程序的扩展性和可移植性等因素，作出自己的选择。

### 选择（select）

**Winsock中最常见的 I/O模型。核心便是利用 select 函数，实现对 I/O的管理**

>利用`select`函数来判断某`Socket`上是否有数据可读，或者能否向一个套接字写入数据，防止程序在`Socket`处于阻塞模式中时，在一次`I/O`调用（如`send`或`recv`、`accept`等）过程中，被迫进入“锁定”状态；同时防止在套接字处于非阻塞模式中时，产生`WSAEWOULDBLOCK`错误

#### select 函数原型
```c++
int select(
  __in          int nfds,
  __in_out      fd_set* readfds,
  __in_out      fd_set* writefds,
  __in_out      fd_set* exceptfds,
  __in          const struct timeval* timeout
);
```

其中，第一个参数`nfds`会被忽略。之所以仍然要提供这个参数，只是为了保持与`Berkeley`套接字兼容
后面大家看到有三个`fd_set`类型的参数：

* 检查可读性（readfds）
* 检查可写性（writefds）
* 用于例外数据（exceptfds）

<!-- more -->

##### fd_set

`fd_set`结构的定义如下：
```c++
typedef struct fd_set {
  u_int fd_count;
  SOCKET fd_array[FD_SETSIZE];
} fd_set;

#define FD_SETSIZE      64
```

所以`fd_set`结构中最多只能监视64个套接字

##### readfds

`fdset`代表着一系列特定套接字的集合。其中，`readfds`集合包括符合下述任何一个条件的套接字：

* 有数据可以读入
* 连接已经关闭、重设或中止
* 假如已调用了`listen`，而且一个连接正在建立，那么`accept`函数调用会成功

##### writefds

`writefds`集合包括符合下述任何一个条件的套接字：

* 有数据可以发出
* 如果已完成了对一个非锁定连接调用的处理，连接就会成功

##### exceptfds

`exceptfds`集合包括符合下述任何一个条件的套接字：
* 假如已完成了对一个非锁定连接调用的处理，连接尝试就会失败
* 有带外（`Out-of-band，OOB`）数据可供读取

#### 例子

举个例子，假设我们想测试一个套接字是否“可读”，必须将自己的套接字增添到`readfds`集合中，
然后调用`select`函数并等待其完成。`select`完成之后，再次判断自己的套接字是否仍为`readfds`集合的一部分。
若答案是肯定的，则表明该套接字“可读”，可立即着手从它上面读取数据。

在三个参数中（readfds、writefds 和 exceptfds），任何两个都可以是空值（`NULL`）；
但是，至少有一个不能为空值！在任何不为空的集合中，必须包含至少一个套接字句柄；
否则，`select`函数便没有任何东西可以等待。最后一个参数`timeout`对应的是一个指针，它指向一个`timeval`结构，
用于决定`select`最多等待 I/O操作完成多久的时间。如`timeout`是一个空指针，那么`select`调用会无限
期地“锁定”或停顿下去，直到至少有一个描述符符合指定的条件后结束。

##### timeval

对 timeval 结构的定义如下：

* `tv_sec`字段以秒为单位指定等待时间
* `tv_usec`字段则以毫秒为单位指定等待时间

若将超时值设置为`(0,0)`，表明`select`会立即返回，出于对性能方面的考虑，应避免这样的设置

##### select 函数返回值

`select`成功完成后，会在`fdset`结构中，返回刚好有未完成的`I/O`操作的所有套接字句柄的总量。
若超过`timeval`设定的时间，便会返回`0`。若`select`调用失败，都会返回`SOCKET_ERROR`，
应该调用`WSAGetLastError`获取错误码！

##### 相关宏用法

用`select`对套接字进行监视之前，必须将套接字句柄分配给一个`fdset`的结构集合，
之后再来调用`select`，便可知道一个套接字上是否正在发生上述的 I/O 活动。
`Winsock`提供了下列宏操作，可用来针对 I/O 活动，对`fdset`进行处理与检查：

* `FD_CLR(s, *set)`：从set中删除套接字s
* `FD_ISSET(s, *set)`：检查s是否set集合的一名成员；如答案是肯定的是，则返回`TRUE`
* `FD_SET(s, *set)`：将套接字s加入集合`set`
* `FD_ZERO( * set)`：将`set`初始化成空集合

例如，假定我们想知道是否可从一个套接字中安全地读取数据，同时不会陷于无休止的“锁定”状态，便可使用`FDSET`宏，将自己的套接字分配给`fdread`集合，再来调用`select`。要想检测自己的套接字是否仍属`fdread`集合的一部分，可使用`FD_ISSET`宏。采用下述步骤，便可完成用`select`操作一个或多个套接字句柄的全过程：
1) 使用`FDZERO`宏，初始化一个`fdset`对象
2) 使用`FDSET`宏，将套接字句柄加入到`fdset`集合中
3) 调用`select`函数，等待其返回……`select`完成后，会返回在所有`fdset`集合中设置的套接字句柄总数
并对每个集合进行相应的更新
4) 根据 select的返回值和`FDISSET`宏，对`fdset`集合进行检查
5) 知道了每个集合中“待决”的 I/O 操作之后，对 I/O 进行处理
然后返回步骤1 )，继续进行`select`处理

`select`函数返回后，会修改`fdset`结构，删除那些不存在待决 I/O 操作的套接字句柄。
这正是我们在上述的步骤 (4) 中，为何要使用`FDISSET`宏来判断一个特定的套接字是否仍在集合中的原因

### 后记

项目源码和文章主体内容来源于：[book-code](https://github.com/wyrover/book-code/tree/master/IOCP%E5%AE%8C%E6%88%90%E7%AB%AF%E5%8F%A3)
变更内容：

- 基于原项目进行验证性测试
- 升级测试代码解决方案到`vs2019`
- 代码中补充注释说明
- `txt`格式讲义转为`markdown`

本文对应的源码地址：[LearnWindowsSocket](https://github.com/xiangtianlong/LearnWindowsSocket)