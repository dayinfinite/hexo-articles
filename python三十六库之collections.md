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

#### elements

> 返回重复其计数的许多倍的每个元素的迭代器。按任意顺序返回元素。如果元素数小于 1， elements()将会忽略它。

```bash

In [132]: c = Counter(a=4, b=2, c=0, d=-2)

In [133]: list(c.elements())
Out[133]: ['a', 'a', 'a', 'a', 'b', 'b']

```

#### most_common

> 返回列表n最常见元素及从其计数最常见到最低限度。如果未指定n ，则most_common()将返回计数器中的所有元素。具有同等数量的元素是任意排序：

```bash

In [134]: Counter('abracadabra').most_common(3)
Out[134]: [('a', 5), ('r', 2), ('b', 2)]

In [135]: Counter('abracadabra').most_common(4)
Out[135]: [('a', 5), ('r', 2), ('b', 2), ('c', 1)]

```

#### subtract

> 元素减去可迭代或从另一个映射（或计数器）。像dict.update()但减去计数而不是取代他们。输入和输出可能为零或负。

```bash

In [136]: c = Counter(a=4, b=2, c=0, d=-2)

In [137]: d = Counter(a=1, b=2, c=3, d=4)

In [138]:  c.subtract(d)

In [139]: c
Out[139]: Counter({'a': 3, 'b': 0, 'c': -3, 'd': -6})

```
### deque

> 返回从左到右初始化的新的双向队列，是堆栈和队列的泛化。 支持线程安全，高效的内存追加。

支持以下方法

- append()

- appendleft()

- clear()

- count()

- extend()

- extendletf()

- pop()

- popleft()

- remove()  deque中的元素向右移动n个位置。如果n是负数的向左移动

- reverse()

- rotate()

- maxlen()

除了以上所述，通过支持迭代、 pickling、 len(d)、 reversed(d)、 copy.copy(d)、 copy.deepcopy(d)、 检查成员用in运算符，和下标索引d [-1]。索引的访问两端都是 o (1)，但在中间部分速度就减慢到 o (n)。对于快速的随机访问，改用列表。


通过方法即可知道作用，这里说下 rotate此方法

```bash

In [142]: d = deque('ghi')

In [143]: d.rotate(1)

In [144]: d
Out[144]: deque(['i', 'g', 'h'])

In [145]: d.rotate(-3)

In [146]: d
Out[146]: deque(['i', 'g', 'h'])

In [147]: d.rotate(-2)

In [148]: d
Out[148]: deque(['h', 'i', 'g'])

```

### defaultdict

> defaultdict是内置的dict类的一个子类。 它覆盖一个方法并添加一个可写的实例变量.

> 第一个参数提供了default_factory属性的初始值; 它默认为None。 所有其余的参数都被视为如果它们被传递给字典构造函数，包括关键字参数。

默认接受参数， 用来指定dict的value值的类型

```bash

In [161]: s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

In [162]: d = defaultdict(list)

In [163]: for k, v in s:
     ...:     d[k].append(v)
     ...:

In [164]: d.items()
Out[164]: [('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]

In [173]: s = [('yellow', {'k': 1}), ('blue', {'v': 2}), ('red', {'p': 3})]

In [174]:

In [174]: for k, v in s:
     ...:     d[k] = v
     ...:

In [176]: d.items()
Out[176]: [('blue', {'v': 2}), ('red', {'p': 3}), ('yellow', {'k': 1})]

```

### namedtuple

> 命名元组赋予元组中每个位置的含义，并允许更具可读性的自编写代码。 它们可以在任何使用常规元组的地方使用，并且可以通过名称而不是位置索引来添加字段

```bash

In [178]:  Point = namedtuple('Point', ['x', 'y'], verbose=True)

In [179]: p = Point(11, 22)

In [180]: p.x
Out[180]: 11

In [181]: p.y
Out[181]: 22

```

其本质仍是tuple数据结构， 继承tuple的方法，有兴趣可以阅读下源码，由于平常没有使用，没有好的场景使用举例，但是初步观察使用在 坐标、域名表示、cvs文件等，应该是比较好的使用场景。其实建议使用namedtuple, 避免使用下标进行索引。

### Orderedict

> 有序的字典记得其插入顺序，可以使用与排序，以使已排序的字典.

目前还暂不明确使用场景。

```bash

In [226]: d
Out[226]: OrderedDict([('orange', 2), ('pear', 1), ('banana', 3), ('apple', 4)])

In [227]: b = dict({'banana': 3, 'apple':4, 'pear': 1, 'orange': 2})

In [229]: b
Out[229]: {'apple': 4, 'banana': 3, 'orange': 2, 'pear': 1}

In [230]: d
Out[230]: OrderedDict([('orange', 2), ('pear', 1), ('banana', 3), ('apple', 4)])

```

以上就是OrderedDict 与dict的对比
