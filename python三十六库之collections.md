---
title: python三十六库之collections
date: 2017-12-10 23:57:32
tags:
---

一直要督促自己深入学习python常用库，由于懒癌晚期，从来没有开始，今天就从这篇开始，定期更新。

## Collections

> 按照官方解释是高性能的容器数据类型

共包含一下数据结构容器

- namedtuple

- deque

- Counter

- OrderedDict

- defaultdict


### Counter

> 用于可计数可哈希的字典类型，无序的集合， 元素被存储为字典键，计数被存储为字典值， 计数可以是任何整数值，包括零或负数。

```bash

In [70]: for word in ['red', 'blue', 'red', 'green', 'blue', 'blue']:
    ...:     c[word] += 1
    ...:

In [71]: c
Out[71]: Counter({'blue': 3, 'green': 1, 'red': 2})

```

Counter内建方法分别为 elements() most_common() subtract()


### deque

> 返回从左到右初始化的新的双向队列，是堆栈和队列的泛化。 支持线程安全，高效的内存追加。


### defaultdict

> defaultdict是内置的dict类的一个子类。 它覆盖一个方法并添加一个可写的实例变量.

> 第一个参数提供了default_factory属性的初始值; 它默认为None。 所有其余的参数都被视为如果它们被传递给字典构造函数，包括关键字参数。

### namedtuple

> 命名元组赋予元组中每个位置的含义，并允许更具可读性的自编写代码。 它们可以在任何使用常规元组的地方使用，并且可以通过名称而不是位置索引来添加字段
