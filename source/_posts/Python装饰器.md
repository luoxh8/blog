---
title: Python装饰器
date: 2018-08-11
updated: 2018-08-11
tags: Python
categories: 后端开发
keywords: 
description: 
top_img: /img/python_decorator.jpg
cover: /img/python_decorator.jpg
---

一个简单的加法运算：

```python
def plus(a, b):
    return a + b
```



这个方法只是简单输出计算结果。我还想知道输入了什么参数。这时候就可以给plus方法加一个装饰器。

```python
def print_params(func):
    def warpper(a, b):
        print(u"第%s个参数是%s" % (1, a))
        print(u"第%s个参数是%s" % (2, b))
        func(a, b) #这句话很重要，继续执行传进来的函数
    return warpper
    
@print_params
def plus(a, b):
    return a + b
```

分析一下：执行到@print_params的时候，就把被装饰的函数(plus)传递给print_params函数。这个将返回warpper方法，然后再执行这个warpper方法。

在warpper方法中，也定义了两个参数，这个结构和plus一一对应。也就是通过这样的方式包装了一下，把plus的参数传递给warpper方法。

plus函数被拆成两部分，函数名部分从装饰器传入，参数部分从装饰器下的包装函数传入。

最后的结果，只执行warpper方法。该方法中先是打印参数，再执行func方法。若不写这一句话，将不会得到a+b的结果。因为没有执行该函数。

```python
def print_params(func):
    def warpper(a, b):
        print(u"第%s个参数是%s" % (1, a))
        print(u"第%s个参数是%s" % (2, b))
        #func(a, b) #注释这句话
    return warpper
    
@print_params
def plus(a, b):
    return a + b
```

执行一下 plus(1, 2)得到下面的结果，可以发现并没有执行到plus本身。

```shell
第1个参数是1
第2个参数是2
```

执行一下，将会报错。func参数不够。为了让装饰器有更好的拓展性和兼容性。修改如下：

```python
def print_params(func):
    def warpper(*args, **kw):
        for i, arg in enumerate(args):
            print(u"第%s个参数是%s" % (i+1, arg))
        for item, value in kw.items():
            print(u"%s = %s" % (item, value))
        func(*args, **kw)
    return warpper
    
@print_params
def plus(a, b, c):
    return a + b + c
```

用*args和**kw可以接受任意参数，再遍历打印结果。执行一下 warpper(5,2,3)。可以得到：

```shell
第1个参数是5
第2个参数是2
第3个参数是3
10
```

这个功能讲到这里，如果吸收消化了，可以再上一个阶梯：装饰器自身的参数。

大家可以发现，我们上面写的装饰器都是没有参数的，都是简单一个@print_params

若想要像普通的方法一样可以传递参数，例如@print_params(1)。这个就需要再对装饰器进一步包装。

这样包装之后，就有3层函数。Python编译器会自动判断装饰器是否带有参数。如下例子，我想判断plus的参数中是否包含我设定的值：

```python
def print_params(check_num):
    def __print_params(func):
        def warpper(*args, **kw):
            if check_num in args:
                print(u"参数中包含%s" % check_num)
            else:
                print(u"参数中不包含%s" % check_num)
        
            for i, arg in enumerate(args):
                print(u"第%s个参数是%s" % (i+1, arg))
            for item, value in kw.items():
                print(u"%s = %s" % (item, value))
            func(*args, **kw)
        return warpper
    return __print_params #需要返回装饰器
 
@print_params(1)
def plus(a, b, c):
    return a + b + c
```

用装饰器传递了一个参数：1。判断plus的参数中是否有包含1，再遍历打印结果。执行一下 warpper(5,2,1)。可以得到：

```shell
参数中包含1
第1个参数是5
第2个参数是2
第3个参数是1
8
```

这种方式要复杂一些。这三层方法中，第一层是普通方法，用于接收装饰器的参数；第二层是装饰器；第三层是对应的包装函数。每一层都要返回对应包装的函数。

当然，第一层的方法同样可以写成 *args, **kw的形式接收容易参数。不过这样写要复杂很多。



最后总结，python装饰器类似：

```python	
def f():
	return 'f'
  
def g(f):
  _f = f()
  return 'g' + _f
  
print(b(f)) # gf
```

