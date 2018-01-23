---
title: python三十六库之functools
date: 2017-12-26 15:38:53
tags:
---


懒死了，懒死了，有好久没有更新，就将之前总结的文档放到blog上面充数了，不能这样呀！
好了，开始第二篇


> functools模块用于高阶函数：作用于或返回其他函数的函数。

functools模块有以下函数

- cmp_to_key
- partial
- reduce
- wraps
- total_ordering
- update_wrapper


### cmp_to_key
> 将旧风格的比较函数转换为key函数。


### partial
> 回一个新的partial对象，该对象在调用时将采用位置参数args和关键字参数关键字调用的func 。如果提供多个参数调用， 它们会被追加给 args。如果提供额外的关键字参数， 它们会扩展和覆盖 keywords。

```python
In [10]: def add(x, y, z):
    ...:     return x+y+z
    ...:

In [11]: sum = partial(add, 3, 4)

In [12]: sum(10)
Out[12]: 17

In [13]: sum = partial(add,)

In [14]: sum(10, 3, 4)
Out[14]: 17

In [15]:
```


### reduce
> 将两个参数的函数累加到序列的项中，从左到右，以便将序列减少为单个值。For example, reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]) calculates ((((1+2)+3)+4)+5). 左参数，x是累加值，右参数，y是来自序列的更新值。如果存在可选的初始化器，则它将放置在计算中序列的项目之前，并在序列为空时用作默认值。如果未提供初始化程序且序列只包含一个项目，则返回第一个项目。


```python
In [17]: def add(x, y):
    ...:     return x+y
    ...:

In [18]: list = [1, 2, 3, 4, 5]

In [19]: reduce(add, list)
Out[19]: 15

```

题外话，Python内部还有一个map函数，作用刚好的reduce类似，经常用来做大数据处理
用法如下:
```python
In [22]: def square(x) :
    ...:     return x**2
    ...:

In [23]: map(square, list)
Out[23]: [1, 4, 9, 16, 25]

In [26]: s1 = [1, 3, 5, 7, 9]

In [27]: s2 = [2, 4, 6, 8, 10]

In [28]: map(add, s1, s2)
Out[28]: [3, 7, 11, 15, 19]
```

### wraps

> 这是一个方便的函数，用于在定义包装器函数时调用update_wrapper()作为函数装饰器

> 可以避免由于本身装饰器漏洞导致函数名和文档混乱,建议使用装饰器，使用wraps

```python
In [37]: def wrapper(func):
    ...:     @wraps(func)
    ...:     def wrapper_function(*args, **kwargs):
    ...:         """this is a decorator function"""
    ...:         return func(*args, **kwargs)
    ...:     return wrapper_function
In [38]: @wrapper
    ...: def wrapped():
    ...:     """This is a decorated function"""
    ...:     print 'wrapped'
In [59]: wrapped.__name__
Out[59]: 'wrapped'

In [60]: wrapped.__doc__
Out[60]: 'This is a decorated function'

对比 原生装饰器如下

In [53]: def decorator(func):
    ...:     def wrapper(*args, **kwargs):
    ...:         """this is a decorator function"""
    ...:         return func(*args, **kwargs)
    ...:     return wrapper
    ...:

In [54]: @decorator
    ...: def wrapped():
    ...:     """This is a decorated function"""
    ...:     print 'wrapped'
In [55]: wrapped.__name__
Out[55]: 'wrapper'

In [56]: wrapped.__doc__
Out[56]: 'this is a decorator function'
```

### total_ordering
> 给定一个定义了一个或多个富比较排序方法的类，该方法提供了一个类装饰器。这简化了指定所有可能的富比较操作的工作量。

> 类必须定义__lt__()，__le__()，__gt__()或__ge__()此外，该类应提供一个__eq__()方法。

> 虽然这个装饰器使得容易创建行为良好的完全有序类型，但是以导致比较方法的较慢执行和更复杂的堆栈跟踪为代价的。如果性能基准测试表明这是给定应用程序的瓶颈，则实施所有六种丰富的比较方法可能提供一个容易的速度提升。

```python
In [4]: @total_ordering
   ...: class Student:
   ...:         def _is_valid_operand(self, other):
   ...:                 return (hasattr(other, "lastname") and
   ...:                         hasattr(other, "firstname"))
   ...:         def __eq__(self, other):
   ...:                 if not self._is_valid_operand(other):
   ...:                         return NotImplemented
   ...:                 return ((self.lastname.lower(), self.firstname.lower()) ==
   ...:                         (other.lastname.lower(), other.firstname.lower()))
   ...:         def __lt__(self, other):
   ...:                 if not self._is_valid_operand(other):
   ...:                         return NotImplemented
   ...:                 return ((self.lastname.lower(), self.firstname.lower()) <
   ...:                         (other.lastname.lower(), other.firstname.lower()))
   ...:

```

### update_wrapper
> 更新一个 wrapper 函数让其更像一个 wrapped 函数。

> 此函数的主要用途是在decorator函数中，它包装修饰的函数并返回包装器。如果不更新包装器函数，返回的函数的元数据将反射包装器的定义，而不是原函数的定义 ，这通常是没有意义的。

> update_wrapper()可以与除函数之外的可调用项一起使用。
