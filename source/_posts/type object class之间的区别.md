---
title: type object class之间的区别
date: 2018-08-25
updated: 2018-08-25
tags: Python
categories: 后端开发
keywords: 
description: 
---


![type-class-object关系图](/img/type-class-object关系图.png)

type函数是python内置的顶层函数之一，不需要导包就可以使用，该函数也是python为何如此魔法的原因。


下面代码为例：

```python
a = 1
type(a) # int
type(int) # type
```


```python
b = 'abc'
type(b) # str
type(str) # type
```

由此可得出，type生成了int，int生成1
因此是由type生成了类对象，再有类对象生成了实例
type->class->obj



我们可以生成两个类，Student类和MyStudent类，Student类是MyStudent类的父类，我们在分别查看下他们的基类

```python
class Student:
    pass
class MyStudent(Student):
    pass
MyStudent.__bases__
Out[10]: (__main__.Student,)
Student.__bases__
Out[11]: (object,)
```



由此我们可以发现object是最顶层的基类

type既是一个类，也是一个对象
既然type也是一个类，我们查看下type的基类是谁

``` python
type.__base__
Out[14]: object
```

我们可以发现type的基类是object
那我们在查看object是由谁生成的

```  python
type(object)
Out[15]: type
object.__bases__
Out[16]: ()
```

object又是由type生成的，也就形成了一开始图片的样子


