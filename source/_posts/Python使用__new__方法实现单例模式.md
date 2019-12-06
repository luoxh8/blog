---
title: Python使用__new__方法实现单例模式
date: 2018-06-25
updated: 2018-07-22
tags: Python
categories: 后端开发
keywords: 
description: 
top_img: /img/singleton.jpg
cover: /img/singleton.jpg
---


单例模式意思是不管实例化多少次只实例化单个实例的特殊类。这种模式在软件中设计比较常见，主要为了管理和控制系统资源，避免重复实例化。例如日志类，每个日志对象都有自己的保存路径等配置。我们不想重复实例化多个日志对象，在整个系统中只想使用同个日志对象即可。你可以定义一个全局变量，也可以实现单例模式。每次实例化日志类都是同一个日志对象。

接下来看看Python如何实现单例模式。

这里采用__new__方法，先看看__new__是何方神圣。如下代码：

```python
class A(object):
    def __init__(self):
        print('init')
        
    def __new__(cls, *args, **kwargs):
        print('new %s' % cls)
        return object.__new__(cls, *args, **kwargs)
        
#实例化测试
a = A()
```



结果将输出：

```python
new <class '__main__.A'>
init
```



有几个地方需要注意一下：

1）__new__方法是静态类方法，虽然没有加静态类方法的装饰器；

2）A继承object类，虽然通常可以省略object字样。但这里省略就无法执行__new__方法。若A继承其他类，同样可以使用object的__new__方法，也可以使用父类的__new__方法；

3）在实例化对象的时候，__new__方法在__init__方法之前执行；

4）__new__必须返回一个对象，才能给__init__初始化。



我们可以从从单词的意思理解这两个方法，new是新的意思，``init是初始化。在__new__方法生成一个新的对象给__init__初始化内容。我们可以通过控制__new__方法生成的对象来实现单例模式。只要在__new__方法每次返回同一个对象即可。如下代码：

```python
class A(object):
    __instance = None
    
    def __new__(cls, *args, **kwargs):
        #判断__instance是否有None
        if cls.__instance is None:
            #若为None，则new一个对象写入到该变量
            cls.__instance = object.__new__(cls, *args, **kwargs)
            
        #返回该变量中的对象
        return cls.__instance
```



我们可以写如下代码测试：

```python
a = A()
a.value = 2
 
b = A()
print(b.value)
 
print(a is b)
print(a)
print(b)
```



得到如下结果：

```python
2
True
<__main__.A object at 0x024959F0>
<__main__.A object at 0x024959F0>
```



从上面可以看出a和b两个对象指向同一个地址，说明它们是同一个对象。

继续拓展，上面提到实例化对象时先执行__new__生成对象，再执行__init__初始化。若我们在__init__中初始化话对象的属性值，第二次通过实例化获取同个单例会不会有影响？答案是“会”，用代码说话，如下代码：

```python
class A(object):
    __instance = None
        
    def __new__(cls, *args, **kwargs):
        #判断__instance是否有None
        if cls.__instance is None:
            #若为None，则new一个对象写入到该变量
            cls.__instance = object.__new__(cls, *args, **kwargs)
            
        #返回该变量中的对象
        return cls.__instance
        
    def __init__(self):
        self.value = 1
        
a = A()
a.value = 2 #修改value属性为2
 
b = A() #再次获取实例
print(b.value)
```



结果将输出1，这意味__init__初始化会影响属性的内容。在使用单例模式时，尽量避免初始化属性或者初始化属性之前先判断是否存在该属性。如下代码：

```python
class A(object):
    __instance = None
        
    def __new__(cls, *args, **kwargs):
        #判断__instance是否有None
        if cls.__instance is None:
            #若为None，则new一个对象写入到该变量
            cls.__instance = object.__new__(cls, *args, **kwargs)
            
        #返回该变量中的对象
        return cls.__instance
        
    def __init__(self):
        #判断value属性是否存在，不存在则初始化
        if not hasattr(self, 'value'):
            self.value = 1
        
a = A()
a.value = 2 #修改value属性为2
 
b = A() #再次获取实例
print(b.value)
```



这次得到的结果为2。

更多细节大家可自行实践总结。当然，还可以通过其他手段实现单例模式。例如，静态类方法、装饰器等。
