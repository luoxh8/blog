---
title: Python异步
date: 2018-08-21
updated: 2018-08-21
tags: Python
categories: 后端开发
keywords: 
description: 
top_img: /img/python_async.png
cover: /img/python_async.png
---

有时候在开发过程中，某些代码或功能需要异步执行，不阻塞主线程。

通常用多线程处理。而Python3多了“协程”的新东西，下面一一到来。

先看看在Python2.x怎么实现异步。



#### **1、使用threading模块**

多线程有两个模块thread和threading。

thread有一些缺陷，threading是改进版。我们直接使用threading即可。

先写个需要异步执行的方法。

```python
import time
 
def test(value):
    print('start test')
    time.sleep(3) #休息3秒
    print('input %s' % value)
```

这里休息3秒是模拟某些代码长时间运行，会造成长时间的阻塞。如下代码：

```python
if __name__ == '__main__':
    test('xxx')
    print('end input')
```

需要等待3秒，才输出"end input"。

那么，使用threading创建多线程如下：

```python
import threading
 
if __name__ == '__main__':
    t = threading.Thread(target=test, args=(u'xxx',))
    t.start()
    print('end input')
```

使用threading的Thread类实现多线程。具体效果大家可以自行测试。

这里我没说线程保护，线程锁的东西，大家有兴趣可以自行了解。



#### **2、继承threading.Thread类**

threading模块的Thread是可被继承的类。我们可以采用继承的方法实现多线程异步执行。

修改上面的test方法，将其封装到类中。

``` python
import threading
import time
 
class Test(threading.Thread):
    def __init__(self, value):
        threading.Thread.__init__(self)
        self.value = value
        
    #继承Thread类需要显示run方法，线程在开启后运行
    def run(self):
        print('start test')
        time.sleep(3) #休息3秒
        print('input %s' % self.value)
```

如下使用方法：

```python
if __name__ == "__main__":    
  test = Test(u'xxx')    
  test.start() #开启线程，会自动执行run方法    
  print('end input')
```

第2种用起来更加干净，便于维护。

相对第1种方法，我更喜欢第2种。





#### **3、使用异步框架**

Python出了名的库多，异步框架也不少。

有时候为了更好地实现异步的机制，可以使用异步框架。

例如Cerely、Tornado、Twisted等等。这里不讲怎么使用这些，只是顺便提一下。



#### **4、Python3.4用asyncio实现协程**

协程其实也是一种线程，其开销比threading小。而且它是Python3.4引入的新标准库 asyncio。

asyncio的编程模型是一个消息循环。

需要从asyncio模块中直接获取一个EventLoop的引用，然后把需要执行的协程扔到EventLoop中执行，实现了异步IO。

这个EventLoop相当于线程池。如下测试代码：



``` python
import asyncio
 
@asyncio.coroutine
def test(value):
    print("start test")
    
    #异步调用asyncio.sleep(3) 休息3秒
    r = yield from asyncio.sleep(3)
    print('input %s' % value)
 
loop = asyncio.get_event_loop() #获取EventLoop
c = test(u'xxx') #获取coroutine
 
#将coroutine丢进EventLoop中执行
loop.run_until_complete(c)
loop.close()
```

其中，需要使用协程的方法加上coroutine的装饰器。

该协程方法执行像方法一样执行的语句不是直接执行方法，只能获取到一个协程对象。

需要把该对象放到EventLoop中执行。该run_until_complete也可以接收list对象，批量执行协程方法。

看起来代码比多线程复杂？是的，所以到Python3.5引入新的关键字简化协程的代码。



#### **5、Python3.5引入async/await**

为了简化并更好地使用异步，到Python 3.5开始引入了新的语法async和await。

这些关键字可以让coroutine的代码更简洁易读。

1）async相当于@asyncio.coroutine；

2）await相当于yield from。



其他部分的代码不需要改变。将上面Python3.4的代码修改如下：

``` python
import asyncio
 
async def test(value):
    print("start test")
    r = await asyncio.sleep(3)
    print('input %s' % value)
 
loop = asyncio.get_event_loop() #获取EventLoop
c = test(u'xxx') #获取coroutine
 
#将coroutine丢进EventLoop中执行
loop.run_until_complete(c)
loop.close()
```

