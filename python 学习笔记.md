---
title: python 学习笔记 
tags: python
---

### python 基础
1. python list[a:b:c] 其中a代表起始位置， b代表结束位置 c代表步长
2. np.random.permutation(N) Randomly permute a sequence, or return a permuted range.np.random.randn(N)
3. df.groupby().agg() 和 df.groupby.apply()
### python install pip (in ubuntu)
1.python2
sudo apt-get install python-pip
2.python3
sudo apt-get install python3-venv python3-pip
### python3 pandas 
1. pivot_table 'rows=>index' and 'cols=>columns
2. Series.sort_values 替换 Series.order
python dict 方法
2. dict.get(key, default=None)
## python 常用包
### datetime, random
datetime.now() //获取当前时间
delta //时间差
strftime //将时间转为字符串
strptime //将字符串转为时间
seed(10) # seed(() 方法的实例
random()
randint(1,2)
randrange(1, 10) 

### itertools
1.迭代器
生成有限序列
```
>>> from itertools import count
>>> counter = count(start=13)
>>> next(counter)
13
>>> next(counter)
14
```
从一个有限序列中生成一个无限序列
```
>>> from itertools import cycle
>>> colors = cycle(['red', 'white', 'blue'])
>>> next(colors)
'red'
>>> next(colors)
'white'
>>> next(colors)
'blue'
>>> next(colors)
'red'
```
从无限的序列中生成有限序列：
```
>>> from itertools import islice
>>> colors = cycle(['red', 'white', 'blue'])  # infinite
>>> limited = islice(colors, 0, 4)            # finite
>>> for x in limited:                         
...     print(x)
red
white
blue
red
```
 2. 生成器

生成器算得上是Python语言中最吸引人的特性之一，生成器其实是一种特殊的迭代器，不过这种迭代器更加优雅。它不需要再像上面的类一样写__iter__()和__next__()方法了，只需要一个yiled关键字
```
def fib():
    prev, curr = 0, 1
    while True:
        yield curr
        prev, curr = curr, curr + prev

>>> f = fib()
>>> list(islice(f, 0, 10))
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
生成器表达式是列表推倒式的生成器版本，看起来像列表推导式，但是它返回的是一个生成器对象而不是列表对象。
```
>>> a = (x*x for x in range(10))
>>> a
<generator object <genexpr> at 0x401f08>
>>> sum(a)
285
```
注意: 区别于列表推倒式 ls = [x for x in range(10)]
 3. 总结
容器是一系列元素的集合，str、list、set、dict、file、sockets对象都可以看作是容器，容器都可以被迭代（用在for，while等语句中），因此他们被称为可迭代对象。
可迭代对象实现了__iter__方法，该方法返回一个迭代器对象。
迭代器持有一个内部状态的字段，用于记录下次迭代返回值，它实现了__next__和__iter__方法，迭代器不会一次性把所有元素加载到内存，而是需要的时候才生成返回结果。
生成器是一种特殊的迭代器，它的返回值不是通过return而是用yield。

## functools 
### 偏函数 partial 和 partialmethod
柯里化 : 部分参数应用
它指的是通过“部分参数应用”从现有函数派生出新函数的技术
```
from functools import partial

def add(x, y):
    return x + y

add_y = partial(add, 3)  # add_y 是一个函数
add_y(4)                 # 结果是 7
```
partialmethod 是 Python 3.4 中新引入的装饰器，作用基本类似于 partial， 不过仅作用于方法。举个例子就很容易明白：
```
class Cell(object):
    def __init__(self):
        self._alive = False
    @property
    def alive(self):
        return self._alive
    def set_state(self, state):
        self._alive = bool(state)
    set_alive = partialmethod(set_state, True)
    set_dead = partialmethod(set_state, False)

c = Cell()
c.alive         # False
c.set_alive()
c.alive         # True
```
### 装饰器相关
说到“接受函数为参数，以函数为返回值”，在 Python 中最常用的当属装饰器了。 functools 库中装饰器相关的函数是 update_wrapper、wraps，还搭配 WRAPPER_ASSIGNMENTS 和 WRAPPER_UPDATES 两个常量使用，作用就是消除 Python 装饰器的一些负面作用。

 1. wraps

```
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@decorator
def add(x, y):
    return x + y

add     # <function __main__.wrapper>
```
可以看到被装饰的函数的名称，也就是函数的 __name__ 属性变成了 wrapper， 这就是装饰器带来的副作用，实际上add 函数整个变成了 decorator(add)，而 wraps 装饰器能消除这些副作用：
```
def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@decorator
def add(x, y):
    return x + y

add     # <function __main__.add>
```
会更正的属性定义在 WRAPPER_ASSIGNMENTS 中：
```
>>> functools.WRAPPER_ASSIGNMENTS
('__module__', '__name__', '__doc__')
>>> functools.WRAPPER_UPDATES
('__dict__',)
```
2.update_wrapper
update_wrapper 的作用与 wraps 类似，不过功能更加强大，换句话说，wraps 其实是 update_wrapper 的特殊化，实际上 wraps(wrapped) 相当于 partial(update_wrapper, wrapped=wrapped, **kwargs)。

因此，上面的代码可以用 update_wrapper 重写如下：
```
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return update_wrapper(wrapper, func)
```
### 用于比较的 cmp_to_key 和 total_ordering

 1. cmp_to_key
cmp_to_key 是 Python 2.7 中新增的函数，用于将比较函数转换为 key 函数， 这样就可以应用在接受 key 函数为参数的函数中，比如 sorted、max 等等。 例如：
```
sorted(range(5), key=cmp_to_key(lambda x, y: y-x))      # [4, 3, 2, 1, 0]
```
2. total_ordering
total_ordering 同样是 Python 2.7 中新增函数，用于简化比较函数的写法。如果你已经定义了 __eq__ 方法，以及 __lt__、__le__、__gt__ 或者 __ge__() 其中之一， 即可自动生成其它比较方法。官方示例：
```
@total_ordering
class Student:
    def __eq__(self, other):
        return ((self.lastname.lower(), self.firstname.lower()) ==
                (other.lastname.lower(), other.firstname.lower()))
    def __lt__(self, other):
        return ((self.lastname.lower(), self.firstname.lower()) <
                (other.lastname.lower(), other.firstname.lower()))

dir(Student)    # ['__doc__', '__eq__', '__ge__', '__gt__', '__le__', '__lt__', '__module__']
```

## re 正则
5.subprocess http://docs.python.org/2/library/subprocess.html 调用shell命令的神器
6.pdb 调试
7.traceback 调试
8.pprint 漂亮的输出
9.logging 日志

## threading和multiprocessing  多线程

 1. 直接创建threading.Thread类的对象
 
```
from threading import Thread
import time
def run(a = None, b = None) :
  print a, b 
  time.sleep(1)

t = Thread(target = run, args = ("this is a", "thread"))
#此时线程是新建状态

print t.getName()#获得线程对象名称
print t.isAlive()#判断线程是否还活着。
t.start()#启动线程
t.join()#等待其他线程运行结束
```

 2. 通过继承Thread类
```
from threading import Thread
import time

class MyThread(Thread) :
  def __init__(self, a) :
    super(MyThread, self).__init__()
    #调用父类的构造方法
    self.a = a

  def run(self) :
    print "sleep :", self.a
    time.sleep(self.a)

t1 = MyThread(2)
t2 = MyThread(4)
t1.start()
t2.start()
t1.join()
t2.join()
```
 3. 针对join()函数用法的实例
```
# encoding: UTF-8
import threading
import time

def context(tJoin):
    print 'in threadContext.'
    tJoin.start()
    # 将阻塞tContext直到threadJoin终止。
    tJoin.join()
    # tJoin终止后继续执行。
    print 'out threadContext.'

def join():
    print 'in threadJoin.'
    time.sleep(1)
    print 'out threadJoin.'

tJoin = threading.Thread(target=join)
tContext = threading.Thread(target=context, args=(tJoin,))
tContext.start()
```

 4. multiprocessing.dummy
```
import time
from multiprocessing.dummy import Pool as ThreadPool
#给线程池取一个别名ThreadPool
def run(fn):
  time.sleep(2)
  print fn

if __name__ == '__main__':
  testFL = [1,2,3,4,5]
  pool = ThreadPool(10)#创建10个容量的线程池并发执行
  pool.map(run, testFL)
  pool.close()
  pool.join()
```

 5. The Process class
```
from multiprocessing import Process

def f(name):
    print('hello', name)

if __name__ == '__main__':
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()
```

```
from multiprocessing import Process
import os

def info(title):
    print(title)
    print('module name:', __name__)
    print('parent process:', os.getppid())
    print('process id:', os.getpid())

def f(name):
    info('function f')
    print('hello', name)

if __name__ == '__main__':
    info('main line')
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()
```

### urllib/urllib2/httplib

http库，httplib底层一点，推荐第三方的库requests

12.os/sys 系统，环境相关
stdin,stdout,stderr

13.queue 队列
task_done()
意味着之前入队的一个任务已经完成。由队列的消费者线程调用。每一个get()调用得到一个任务，接下来的task_done()调用告诉队列该任务已经处理完毕。
如果当前一个join()正在阻塞，它将在队列中的所有任务都处理完时恢复执行（即每一个由put()调用入队的任务都有一个对应的task_done()调用）。

join()
阻塞调用线程，直到队列中的所有任务被处理掉。

只要有数据被加入队列，未完成的任务数就会增加。当消费者线程调用task_done()（意味着有消费者取得任务并完成任务），未完成的任务数就会减少。当未完成的任务数降到0，join()解除阻塞。

put(item[, block[, timeout]])
将item放入队列中。

如果可选的参数block为True且timeout为空对象（默认的情况，阻塞调用，无超时）。
如果timeout是个正整数，阻塞调用进程最多timeout秒，如果一直无空空间可用，抛出Full异常（带超时的阻塞调用）。
如果block为False，如果有空闲空间可用将数据放入队列，否则立即抛出Full异常
其非阻塞版本为put_nowait等同于put(item, False)

get([block[, timeout]])
从队列中移除并返回一个数据。block跟timeout参数同put方法
其非阻塞方法为｀get_nowait()｀相当与get(False)

empty()
如果队列为空，返回True,反之返回False

14.pickle/cPickle 序列化工具
15.hashlib md5, sha等hash算法
16.cvs
17.json/simplejson  python的json库，据so上的讨论和benchmark，simplejson的性能要高于json
18.timeit 计算代码运行的时间等等
19.cProfile python性能测量模块
20.glob 类似与listfile，可以用来查找文件
21.atexit 有一个注册函数，可用于正好在脚本退出运行前执行一些代码
22.dis python 反汇编，当对某条语句不理解原理时，可以用dis.dis 函数来查看代码对应的python 解释器指令等等。

### python 内置序列函数
enumerate， sorted， zip，reversed

### 列表推导式
[expr for val in collection if condition]

