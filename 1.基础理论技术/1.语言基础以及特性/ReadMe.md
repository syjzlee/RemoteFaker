<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Python](#python)
  - [语法基础](#%E8%AF%AD%E6%B3%95%E5%9F%BA%E7%A1%80)
  - [语言特性篇](#%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7%E7%AF%87)
    - [谈谈 Python 和其他语言的区别](#%E8%B0%88%E8%B0%88-python-%E5%92%8C%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [Python 2和3的区别](#python-2%E5%92%8C3%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [闭包](#%E9%97%AD%E5%8C%85)
    - [装饰器](#%E8%A3%85%E9%A5%B0%E5%99%A8)
    - [谈谈GC](#%E8%B0%88%E8%B0%88gc)
    - [GIL的理解(CPython特性，计算密集型用进程，IO密集用线程)](#gil%E7%9A%84%E7%90%86%E8%A7%A3cpython%E7%89%B9%E6%80%A7%E8%AE%A1%E7%AE%97%E5%AF%86%E9%9B%86%E5%9E%8B%E7%94%A8%E8%BF%9B%E7%A8%8Bio%E5%AF%86%E9%9B%86%E7%94%A8%E7%BA%BF%E7%A8%8B)
    - [Python传参](#python%E4%BC%A0%E5%8F%82)
    - [深拷贝和浅拷贝](#%E6%B7%B1%E6%8B%B7%E8%B4%9D%E5%92%8C%E6%B5%85%E6%8B%B7%E8%B4%9D)
    - [鸭子类型](#%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)
    - [猴子补丁](#%E7%8C%B4%E5%AD%90%E8%A1%A5%E4%B8%81)
    - [Python中的作用域](#python%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F)
    - [函数式编程](#%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)
      - [lambda函数](#lambda%E5%87%BD%E6%95%B0)
      - [map函数](#map%E5%87%BD%E6%95%B0)
      - [reduce函数](#reduce%E5%87%BD%E6%95%B0)
      - [filter函数](#filter%E5%87%BD%E6%95%B0)
    - [迭代器和生成器](#%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%92%8C%E7%94%9F%E6%88%90%E5%99%A8)
    - [协程](#%E5%8D%8F%E7%A8%8B)
    - [Python 面对对象编程](#python-%E9%9D%A2%E5%AF%B9%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B)
      - [封装](#%E5%B0%81%E8%A3%85)
      - [继承](#%E7%BB%A7%E6%89%BF)
      - [多态](#%E5%A4%9A%E6%80%81)
      - [类成员](#%E7%B1%BB%E6%88%90%E5%91%98)
      - [类成员的修饰符](#%E7%B1%BB%E6%88%90%E5%91%98%E7%9A%84%E4%BF%AE%E9%A5%B0%E7%AC%A6)
      - [类的特殊成员](#%E7%B1%BB%E7%9A%84%E7%89%B9%E6%AE%8A%E6%88%90%E5%91%98)
    - [Python自省指南](#python%E8%87%AA%E7%9C%81%E6%8C%87%E5%8D%97)
    - [Python中的元编程](#python%E4%B8%AD%E7%9A%84%E5%85%83%E7%BC%96%E7%A8%8B)
    - [设计模式](#%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
      - [单例模式](#%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F)
      - [工厂模式](#%E5%B7%A5%E5%8E%82%E6%A8%A1%E5%BC%8F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Python
## 语法基础
    * 各大网站或者书太多资源了，这是基础，不会都不能干活
## 语言特性篇
* 学习书籍推荐：[《流畅的Python》](https://book.douban.com/subject/27028517/)
### 谈谈 Python 和其他语言的区别
* Python是强类型、动态类型的语言
### Python 2和3的区别
* python2中，print是个特殊语句，python3中print是函数。
* python中有两种字符类型：字节字符串和文本字符串。

| 版本 | python2| python3 |
| -----| ---- | ---- |
| 字节字符串 | str | bytes |
| 文本字符串 | Unicode | str |
* python2中默认的字符串类型默认是ASCII，python3中默认的字符串类型是Unicode。
* python2中除法`/`的结果是整型，python3中是浮点类型。
* python2中默认类是旧式类，需要显式继承新式类（object）来创建新式类。python3中完全移除旧式类，所有类都是新式类，但仍可显式继承object类。
* 兼容问题：six库，2to3，__future__包
### 闭包
* “闭包”的本质就是函数的嵌套定义，即在函数内部再定义函数；保存了外部函数的状态信息，使函数的局部变量信息依然可以保存下来； 
* 闭包指延伸(延伸的意思是seriers在average函数用，但在average_nums中定义的)了作用域的函数，访问定义体之外定义的非全局变量。创建一个闭包必须满足以下几点:
    - 必须有一个内嵌函数
    - 内嵌函数必须引用外部函数中的变量
    - 外部函数的返回值必须是内嵌函数
```python
def average_num():
    series=[]
    def average(num):
        ## series自由变量,未在本地作用域绑定
        series.append(num)
        return sum(series)/len(series)
    return average
aver=average_num()
```
* nonlocal 关键字将变量count，total 手动标记为自由变量
```python
def average_num():
    count=0
    series=[]
    def average(num):
        nonlocal count  ## 列表等容器不需要重新nonlocal标记为自由变量
        series.append(num)
        count+=1
        return sum(series)/count, series, count
    return average
aver=average_num()
i = 0
while i < 5:
    print(aver(10))
    i += 1

结果：
(10.0, [10], 1)
(10.0, [10, 10], 2)
(10.0, [10, 10, 10], 3)
(10.0, [10, 10, 10, 10], 4)
(10.0, [10, 10, 10, 10, 10], 5)
```
* 顺便提下 global 关键字，Python 默认函数定义体中赋值的变量是局部变量，所以要在函数中使用全局变量，须用global声明。(下面这句去掉global就会报错，`local variable 'b' referenced before assignment`)
```python
b=9
def test(a):
    print(a)
    global b
    print(b)
    b=6
test(3)
```

https://www.cnblogs.com/bonelee/p/11581303.html

**https://www.cnblogs.com/xiaozao/p/9594069.html**
* 为什么要使用闭包 ——LJP 2020/06/05
    * **保存函数的状态信息，使函数的局部变量信息依然可以保存下来**
    * 闭包避免了使用全局变量
    * 闭包允许将函数与其所操作的某些数据（环境）关连起来
    * 一般来说，当对象中只有一个方法时，这时使用闭包是更好的选择（比用类来实现更优雅，此外装饰器也是基于闭包的一中应用场景）

### 装饰器
* 主要参考
    * [理解 Python 装饰器看这一篇就够了](https://www.cnblogs.com/zepc007/p/9763638.html)
* 闭包的一个重要应用就是装饰器，函数装饰器在导入模块时立即执行，被装饰的函数只在明确调用时执行。装饰器采用语法糖@，使用方便，用途多样，既可以自定义在装饰器中指定日志级别，也可以使用官方提供的预激协程装饰器等等。
``` python
## 手动实现一个带参数的装饰器
def use_arg(argument):
    def decorate(fun):
        def wrapper(*args,**kwargs):
            print('%s is running'%fun.__name__)
            print('传入的参数 %s'%argument)
            return fun(*args, **kwargs)  
        return wrapper
    return decorate
@use_arg('hahhaa')   # 语法糖@  相当于省去了 demo = use_arg('hahhaa')(demo)， 之后直接调用demo()即可。
def demo():
    pass
demo()
```
* 装饰器本质上是一个 Python 函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器的返回值也是一个函数/类对象。它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景，装饰器是解决这类问题的绝佳设计。有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码到装饰器中并继续重用。概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。

### [谈谈GC](https://blog.csdn.net/m0_46090675/article/details/109672544)
* 主要参考
    * [Python 进阶：浅析「垃圾回收机制」(上篇)](https://hackpython.com/blog/2019/07/05/Python%E8%BF%9B%E9%98%B6%EF%BC%9A%E6%B5%85%E6%9E%90%E3%80%8C%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6%E3%80%8D-%E4%B8%8A%E7%AF%87/)
* 主要使用**引用计数**进行垃圾回收
    * 引用计数就是：变量值被变量名关联的次数； 变量名与值内存地址的关联关系存放于栈区；变量值存放于堆区，内存管理回收的则是堆区的内容。简记左栈右堆
    * 直接引用指的是从栈区出发直接引用到的内存地址。
​    * 间接引用指的是从栈区出发引用到堆区后，再通过进一步引用才能到达的内存地址。如a=['xxx'],列表['xxx']就间接引用了'xxx'的地址
* 通过**标记-清理**解决容器对象产生循环引用的问题
    * 标记/清除算法的做法是当应用程序可用的内存空间被耗尽的时，就会停止整个程序，然后进行两项工作，第一项则是标记，第二项则是清除
    * python的垃圾回收是以引用计数为主、标记清除为辅的
    * python的标记清除会有两个双端链表的“容器”，引用为0就放在“不可达容器”，引用不为0就放在“可达容器”，然后如果一个应用在遍历时候还遍历到了“不可达容器”则把这个对象从该容器放回“可达容器”中，最后所以的遍历结束后进行清除阶段，清除阶段将“不可达容器”的所有内容删除释放内存。
* 通过**分代回收**以空间换时间的方式来提高垃圾回收的效率（回收依然是使用引用计数作为回收的依据）
    * 基于引用计数的回收机制，每次回收内存，都需要把所有对象的引用计数都遍历一遍，这是非常消耗时间的，于是引入了分代回收来提高回收效率，分代回收采用的是用“空间换时间”的策略
    * 在历经多次扫描的情况下，都没有被回收的变量，gc机制就会认为，该变量是常用变量，gc对其扫描的频率会降低，具体实现原理如下：
        * 分代指的是根据存活时间来为变量划分不同等级（也就是不同的代）。 新定义的变量，放到新生代这个等级中，假设每隔1分钟扫描新生代一次，如果发现变量依然被引用，那么该对象的权重（权重本质就是个整数）加一，当变量的权重大于某个设定得值（假设为3），会将它移动到更高一级的青春代，青春代的gc扫描的频率低于新生代（扫描时间间隔更长），假设5分钟扫描青春代一次，这样每次gc需要扫描的变量的总个数就变少了，节省了扫描的总时间，接下来，青春代中的对象，也会以同样的方式被移动到老年代中。也就是等级（代）越高，被垃圾回收机制扫描的频率越低。
    * 虽然分代回收可以起到提升效率的效果，但也存在一定的缺点：例如一个变量刚刚从新生代移入青春代，该变量的绑定关系就解除了，该变量应该被回收，但青春代的扫描频率低于新生代，这就到导致了应该被回收的垃圾没有得到及时地清理。
### GIL的理解(CPython特性，计算密集型用进程，IO密集用线程)
* 主要参考
    * 这个好通俗易懂[Python中的GIL(全局解释器锁)详解及解决GIL的几种方案](https://blog.csdn.net/qq_40808154/article/details/89398076)
    * [python中的GIL详解](https://www.cnblogs.com/SuKiWX/p/8804974.html)
* **GIL：又叫全局解释器锁，每个线程在执行的过程中都需要先获取GIL，保证同一时刻只有一个线程在运行，目的是解决多线程同时竞争程序中的全局变量而出现的线程安全问题。它并不是python语言的特性，仅仅是由于历史的原因在CPython解释器中难以移除，因为python语言运行环境大部分默认在CPython解释器中。**
* 缺陷 ：限制程序的多核执行，同一时间只能有一个线程执行字节码，CPU密集型程序（大量时间花在计算上）难以利用多核优势；但是IO期间会释放GIL，对IO密集型程序(大量时间花在网络传输上，资源调度)影响小。
* 规避影响策略：CPU密集型使用多进程+进程池，IO密集型采用多线程/协程的策略；cython扩展（将python转换为c代码）
*  GIL释放：Python会计算当前已执行的微代码数量，达到一定阈值后强制释放GIL；系统分配的时间片用完释放；IO操作时释放。
* 一个操作如果是一个字节码指令可以完成就是原子的，原子操作是可以保证线程安全的，有了GIL之后仍然要关注线程安全，必要时候需人为加锁。
```python
import threading
lock=threading.Lock()
n=[0]
def demo():
    #with lock:
    n[0]=n[0]+1
threads=[]
for i in range(5000):
    t=threading.Thread(target=demo)
    threads.append(t)
for t in threads:
    t.start()
print(n)
```
* 多线程threading中的join表示主线程一直等待全部的子线程结束之后才继续向下执行（代码中的time.time计算时间都是计算的主线程的时间）
* 剖析程序性能：内置profile/cprofile工具
### Python传参
* 主要参考
    - [Python传入参数的几种方法](https://blog.csdn.net/abc_12366/article/details/79627263)
* Python中参数传递方式：位置传递、默认参数、可变参数、关键字参数、命名关键字参数
* Python 函数参数传递实际叫传对象（call by object），可以把不可变对象传参理解为传值，可变对象参数理解为传引用。
### 深拷贝和浅拷贝
* 浅拷贝(copy)拷贝父对象，不会拷贝对象的内部的子对象。深拷贝(deepcopy)完全拷贝了父对象及其子对象。
```python
import copy
a = [1, 2, 3, 4, ['a', 'b']] #原始对象
b = a                       #赋值，传对象的引用
c = copy.copy(a)            #对象拷贝，浅拷贝
d = copy.deepcopy(a)        #对象拷贝，深拷贝
a.append(5)                 #修改对象a
a[4].append('c')            #修改对象a中的['a', 'b']数组对象
```
### 鸭子类型
https://www.cnblogs.com/olivertian/p/11627110.html
* 鸭子类型就是：如果走起路来像鸭子，叫起来也像鸭子，那么它就是鸭子（If it walks like a duck and quacks like a duck, it must be a duck）。鸭子类型是编程语言中动态类型语言中的一种设计风格，一个对象的特征不是由父类决定，而是通过对象的方法决定的。（重点关注接口，不关注对象）
* 作用： 例如list_b是一个列表，set_c是一个集合，他们都是可迭代类型，都可以通过list_a的extend方法拼接到list_a后面，这样就体现了python的灵活性了，因为按我们一般的思路，一个列表后面只能是拼接一个列表才对，可是这里却不这么限定，只要是个可迭代类型就都可以拼接，极大丰富了应用的范围。这就体现了鸭子类型的优势了，list和set都是可迭代类型（即都看起来像鸭子），只要是可迭代我就给你可拼接到一个列表的功能（只要是鸭子类型就可以做某件事
```python
class Dog(Animal):
    def run(self):
        print('Dog is running...')

class Cat(Animal):
    def run(self):
        print('Cat is running...')
```
### 猴子补丁
* 属性在运行时的动态替换，叫做猴子补丁（Monkey Patch）。猴子补丁的核心就是用自己的代码替换所用模块的源代码。
* Monkey patch就是在运行时对已有的代码进行修改，达到hot patch的目的。运行时动态替换模块的方法，运行时动态增加模块的方法（慎用）
```python
class Monkey:
    def hello(self):
        print('hello')

    def world(self):
        print('world')


def other_func():
    print("from other_func")



monkey = Monkey()
monkey.hello = monkey.world
monkey.hello()
monkey.world = other_func
monkey.world()
```
### Python中的作用域
* Python的作用域一共有4种: L （Local） 局部作用域，E （Enclosing） 闭包函数外的函数中，G （Global） 全局作用域，B （Built-in） 内建作用域。以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内建中找。
### 函数式编程
#### lambda函数
* lambda 定义了一个匿名函数，lambda 并不会带来程序运行效率的提高，只会使代码更简洁。使用lambda内不要包含循环，如果有，定义函数来完成，使代码获得可重用性和更好的可读性。lambda 是为了减少单行函数的定义而存在的。
```python
# 可以理解为冒号:前面的是参数，后面是返回结果，  lambda 参数:返回结果
list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
## [1, 4, 9, 16, 25, 36, 49, 64, 81]
```
#### map函数
* map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。
#### reduce函数
* reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是：
`reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`
#### filter函数
* filter()函数用于过滤序列。filter()接收一个函数和一个序列，把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。
```python
def is_odd(n):
    return n % 2 == 1
list(filter(is_odd, [1, 2, 4, 5, 6, 9, 10, 15]))
## 结果: [1, 5, 9, 15]
```
### 迭代器和生成器
* 主要参考
    - [Python中的可迭代对象、迭代器和生成器的异同点](https://blog.csdn.net/SL_World/article/details/86507872)
![relationships.png](https://i.loli.net/2019/08/25/a9HQ6d1vxPNzeSl.png)
* 可迭代对象和容器一样是一种通俗的叫法，并不是指某种具体的数据类型，可迭代对象实现了__iter__方法，该方法返回一个迭代器对象
```python
title = ['Python','Java','C++'] ## 列表是一个可迭代对象
isinstance(title,Iterable)      ## True
a = iter(title) ## 由可迭代对象的iter方法返回一个迭代器
>>> next(a)
Python
>>> next(a)
Java
```
* 迭代器有一种具体的迭代器类型，它是一个带状态的对象，他能在你调用next()方法的时候返回容器中的下一个值，**迭代器不会一次性把所有元素加载到内存**，而是需要的时候才生成返回结果(不同于容器)。任何实现了__iter__和__next__()方法的对象都是迭代器，__iter__返回迭代器自身，__next__返回容器中的下一个值，如果容器中没有更多元素了，则抛出StopIteration异常。迭代器每次调用next()方法的时候做两件事：为下一次调用next()方法修改状态，生成当前调用的返回结果。
* 生成器其实是一种特殊的迭代器，这种迭代器更加优雅，它不需要写__iter__()和__next__()方法了，只需要一个yiled关键字， 生成器一定是迭代器（反之不成立），**迭代器只能迭代取出数据，而生成器除了取数据功能还可以通过send()传入数据，传入的数据可在生成器内进行计算**。
### 协程
####yield
https://blog.csdn.net/SL_World/article/details/86507872 了解生成器函数，yield的工作原理

带有yield的函数就是生成器函数，生成器比迭代器多出的功能是利用send()传入数据，所以必须理解send()和yield。yield可以理解成一个断点，生成器的每次send()和next()都会从生成器函数当前的yield顺序执行到下一个yield断点处；具体看下述代码:
```python
def d():
    print('生成器函数开始')
    sum = 0
    value = yield sum
    sum = sum + value
    print('sum的值是：%d' % sum)
    value = yield (sum+5)
    print('最后一次传入的value值是： ', value)
    print('生成器函数结束')
c = d()          ## c是一个生成器，此行代码并不运行d()内容
a = c.send(None) ##
print('生成器传出的值:%d' % a)
a = c.send(10)
print('生成器传出的值:%d' % a)
c.send(10086)


>>>生成器函数开始
>>>生成器传出的值:0
>>>sum的值是：10
>>>生成器传出的值:15
>>>最后一次传入的value值是：  10086
>>>生成器函数结束
StopIteration
```
生成器yield函数自己的总结(第二次): 

    * 一句话总结就是 yield为断点，外部调用传值给yield左边变量，顺序执行后，到下一个yield处将右边表达式结果返回到外部。（左进向下右出）  
    * 生成器的每次 result = send(num)可以理解成两步，第一步将要传递的值num传递到中断的yield处（赋值号左边的变量），由yield进行赋值，然后生成器函数顺序执行到下一个yield处；  
    * 第二步将下一个中断点yield的表达式结果（赋值号和yield的右边的表达式）return回来返回给外部调用生成器函数的结果例如上面demo中的a变量；
    * 生成器函数都有一个不存在的yield断点，这个断点处需要外部调用生成器函数时候传入None值来标记生成器函数的开始。

简单来讲 result = send(num) 就是先进入生成器函数，将num传递给当前yield断点并且赋值给yield表达式的变量，然后在生成器函数中顺序执行直到遇到下一个yield表达式，然后在该yield处中断跳出生成器函数并把yield右边的计算结果return返回给外部程序result。（生成器函数必须以next(c)或者c.send(None)开始可以这么理解，生成器函数的起始位置有个不存在的yield断点，send(None)将None值传递给该yield断点，然后在实际的第一个yield断点处返回yield表达式）
https://www.liaoxuefeng.com/wiki/897692888725344/923057403198272 廖雪峰
* 主要参考
    - [Python协程深入理解](https://www.cnblogs.com/zhaof/p/7631851.html)
* 协程调用细节：实际上next()和send()在一定意义上作用是相似的，区别是send()可以传递yield表达式的值进去，而next()不能传递特定的值，只能传递None进去。因此，我们可以看做c.next() 和 c.send(None) 作用是一样的。第一次调用时，请使用next()语句或是send(None)，不能使用send发送一个非None的值，否则会出错的，因为没有Python yield语句来接收这个值。
* send执行的顺序。n1 = yield r这句话是从右往左执行的。当第一次send（None）时，启动生成器，从生成器函数的第一行代码开始执行，直到第一次执行完yield后，跳出生成器函数。这个过程中，n1一直没有定义。运行到send（1）时，进入生成器函数，此时，将yield r看做一个整体，赋值给它并且传回。此时即相当于把1赋值给n1，但是并不执行yield部分。下面继续从yield的下一语句继续执行，然后重新运行到yield语句，执行后，跳出生成器函数。即send和next相比，只是开始多了一次赋值的动作，其他运行流程是相同的。
```python
def consumer():
    r = 'hello'
    while True:
        n1=yield r  #这里的等式右边相当于一个整体，接受回传值
        if not n1:
            return
        print('[CONSUMER] Consuming %s...' % n1)
        r='%d00 OK' % n1
c = consumer()
c.__next__()
n=0
while n<3:
    n=n+1
    r1=c.send(n)
    print('[PRODUCER] Consumer return: %s' % r1)
c.close()
```
* 预激装饰器，使用协程之前必须预激，为了避免忘记，可以在协程上使用一个特殊的装饰器。
```python
from functools import wraps
def corountine(func):
    @wraps(func)                      
    ## functools.wraps 则可以将原函数对象的指定属性复制给包装函数对象, 默认有 __module__、__name__、__doc__,或者通过参数选择
    def primer(*args, **kwargs):       ## 把被装饰的生成器函数替换成这里的 primer 函数；调用 primer 函数时，返回预激后的生成器
        gen = func(*args, **kwargs)    ## 调用被装饰的函数，获取生成器对象。
        next(gen)                      ## 预激生成器
        return gen                     ## 返回生成器
    return primer
    
@corountine               ## 预激装饰器
def coro_average():
    total = 0.0
    count = 0
    average = None
    while 1:
        term = yield average
        total += term
        count += 1
        average = total/count
coro3 = coro_average()
print (coro3.send(5))
print (coro3.send(7))
print (coro3.send(10))
coro3.close()
```

#### yield from
#### [asyncio、async/await、aiohttp](https://www.liaoxuefeng.com/wiki/1016959663602400/1017970488768640)



### Python 面对对象编程
#### 封装
* 将内容封装到某处
* 从某处调用被封装的内容
```python
class Person:
    def __init__(self,name,age):
        self.name=name
        self.age=age

## 初始化一个实例时调用__init__方法
tim=Person('Tim',18)
## 将Bob,16分布封装到bob的name和age属性中
bob=Person('Bob',16)
```
#### 继承
* 对于面向对象的继承来说，其实就是将多个类共有的方法提取到父类中，子类仅需继承父类而不必一一实现每个方法。Python的类如果继承了多个类，那么其寻找方法的方式（即MRO,method resolution order (方法解析顺序)，在上述查找过程中，一旦找到，则寻找过程立即中断，便不会再继续找了）有两种，可以分别粗略理解为：深度优先和广度优先。具体实现细节可以看这篇[python多重继承C3算法](https://blog.csdn.net/fmblzf/article/details/52512145)
    * 当类是经典类时，多继承情况下，会按照深度优先方式查找，Python 2.x中默认都是经典类，只有显式继承了object才是新式类。
    * 当类是新式类时，多继承情况下，会按照广度优先方式查找Python 3.x中默认都是新式类，不必显式的继承object。
* 继承中super的调用顺序是与MRO-C3的类方法查找顺序一样的，super作用如下：
    - 如果子类继承父类不做初始化，那么会自动继承父类属性。
    - 如果子类继承父类做了初始化，且不调用super初始化父类构造函数，那么子类不会自动继承父类的属性。
    - 如果子类继承父类做了初始化，且调用了super初始化了父类的构造函数，那么子类也会继承父类的属性。
```python  调用查找顺序（python3 广度优先）
class A:
    def __init__(self):
        print('A')
class B(A):
    def __init__(self):
        print('B')
        super().__init__()   # B-A
class C(A):
    def __init__(self):
        print('C')
        super().__init__()  # C-A
class D(B, C):
    def __init__(self):
        print('D')
        super().__init__()   # D-B-C-A
```

```python
class A:
    def __init__(self):
        self.name = 'Father'

class B(A):
    pass

class C(A):
    def __init__(self):
        pass

class D(A):
    def __init__(self):
        super().__init__()

b = B()
print(b.name)    # Father

c = C()
# print(c.name)  #  AttributeError: 'C' object has no attribute 'name'

d = D()
print(d.name)   # Father

```
#### 多态
* Pyhon不支持多态并且也用不到多态，多态的概念是应用于Java和C#这一类强类型语言中，而Python崇尚“鸭子类型”。
#### 类成员
![leichengyuan.png](https://i.loli.net/2019/08/16/i4GtE972ScAjQhJ.png)
* 字段
	* 静态字段在内存中只保存一份
	* 普通字段在每个实例对象中都要保存一份
	* 应用场景： 通过类创建实例对象时，如果每个实例对象都具有相同的字段，那么就使用静态字段
```python
class Province:
    ## 静态字段
    country = 'China' 
    def __init__(self,name):
        ## 普通字段
        self.name = name
obj1 =Province('BeiJing')
print(obj1.name)
print(Province.country)
```
* 方法
    - 普通实例方法：由实例对象调用；至少一个self参数；执行普通方法时，自动将调用该方法的实例对象赋值给self；
    - 类方法：由类调用；至少一个cls参数；执行类方法时，自动将调用该方法的类复制给cls；
    - 静态方法：由类调用；无默认参数；
```python
class Foo:
    def __init__(self,name):
        self.name =name
    ## 定义普通实例方法,至少一个self参数
    def ord_func(self):
        print(self.name)
    ## 定义类方法，至少一个cls参数
    @classmethod
    def class_func(cls):
        print('类方法')
    ## 定义静态方法，无默认参数
    @staticmethod
    def static_fun(): 
        print('静态方法')
f = Foo('ord_func')
f.ord_func()
Foo.class_func()
Foo.static_fun()
```
* 属性
    - 定义时，在普通方法的基础上添加@property装饰器，属性仅有一个self参数，调用时无需括号。@property装饰器可以实现其他语言所拥有的getter，setter和deleter的功能（例如实现获取，设置，删除隐藏的属性）；通过@property装饰器可以对属性的取值和赋值加以控制,提高代码的稳定性。
```python
class Goods:
    ## 查看属性值 
    @property       
    def price(self):   
        print('@property')
    ## 修改、设置属性
    @price.setter                                         
    def price(self, value):       
        print('@price.setter')
    ## 删除属性
    @price.deleter   
    def price(self):      
       print('@price.deleter')                             
obj = Goods()
## 自动执行 @property 修饰的 price 方法，并获取方法的返回值 
obj.price
## 自动执行 @price.setter 修饰的 price 方法，并将2000赋值给方法的参数              
obj.price = 2000 
## 自动执行 @price.deleter 修饰的 price 方法  
del obj.price     
```
#### 类成员的修饰符
* _xxx 表示这是一个保护成员（属性或者方法），它不能用from module import * 导入，其他方面和公有一样访问；
* __xxx 这表示这是一个私有成员，它无法直接像公有成员一样随便访问，但可以通过`实例对象名._类名__xxx`这样的方式可以访问；
* __xxx__表示这是一个特殊成员，用来区别其他用户自定义的命名以防冲突，就是例如`__init__()、 __del__()`  这些特殊方法
#### 类的特殊成员
![魔法函数.png](https://i.loli.net/2019/08/16/aeglES6LYfQIz3U.png)
1. __doc __输出类的描述信息
2. __module __输出当前操作对象所在模块
3. __class __输出当前操作对象的类
4. __init __构造方法，通过类创建实例对象时，自动触发执行
5. __del __析构方法，当对象在内存中被释放时，自动触发执行，此方法一般无须定义，因为Python是一门高级语言，程序员在使用时无需关心内存的分配和释放，因为此工作都是交给Python解释器来执行，所以，析构函数的调用是由解释器在进行垃圾回收时自动触发执行的。
6. __call__对象后面加括号，触发执行，构造方法的执行是由创建对象触发的，即：对象 = 类名() ；而对于 __call__ 方法的执行是由对象后加括号触发的，即：对象() 或者 类()()
```python
class Foo:
    def __init__(self):
        pass
    def __call__(self,*args,**kwargs):
        print('__call__')
## 执行 __init__
obj6 = Foo() 
## 执行 __call__
obj6()
```
7. __dict__类或对象中的所有成员
```python
class Province:
    country = 'China'
    def __init__(self, name, count):
        self.name = name
        self.count = count
    def func(self, *args, **kwargs):
        print('func')
## 获取类的成员，即：静态字段、方法、
## 输出：{'__module__': '__main__', 'country': 'China', '__init__': <function Province.__init__ at 0x00000264B20F1E18>, 
## 'func': <function Province.func at 0x00000264B4638D08>, '__dict__': <attribute '__dict__' of 'Province' objects>, 
## '__weakref__': <attribute '__weakref__' of 'Province' objects>, '__doc__': None}
print(Province.__dict__)
## 输出：{'name': 'HeBei', 'count': 1000}
obj1 = Province('HeBei',1000)
print(obj1.__dict__)
obj2 = Province('HeNan', 3)
## 输出：{'name': 'HeNan', 'count': 3}
print(obj2.__dict__)
```
8. __str__如果一个类中定义了__str__方法，那么在打印对象时，默认输出该方法的返回值。
```python
class Foo:
    def __init__(self):
        pass
    def __str__(self):
        return '__str__'
obj=Foo()
print(obj)
```
9. `__getitem__、__setitem__、__delitem__`用于索引操作，如字典。以上分别表示获取、设置、删除数据。
9. `__getslice__、__setslice__、__delslice__`该三个方法用于分片操作，如列表。
10. __iter__用于迭代器，之所以列表、字典、元组可以进行for循环。
11. `__new__和__init__`
`__new__`是一个静态方法，而`__init__`是一个实例方法，`__new__`方法会返回一个创建的实例，而`__init__`什么都不返回，当创建一个新实例时调用`__new__`，初始化一个实例时用`__init__`。
### [Python自省指南](https://www.ibm.com/developerworks/cn/linux/l-pyint/index.html)
* 自省（Introspection）指运行时判断一个对象类型的能力，常见type，id，isinstance 等获取对象类型信息或 ispect 模块中的函数获取详细信息。
### Python中的元编程
https://www.cnblogs.com/gzl420/p/10915825.html  比较全面，主要要知道type；python所有皆对象包括类。
* 元类是创建类的类，元类允许我们控制类的生成（修改类的属性等），用type来定义元类，元类最常见的使用场景是 ORM 框架。
```python
## 通过元编程实现类自定义属性大写
class Base(type):
    def __new__(cls,name,bases,attrs):  #  参数依次是当前准备创建类的对象、创建类本身的名称、类继承的父类集合、创建类的方法集合
        upper_attrs={}
        for k,v in attrs.items():
            if not k.startswith("__"):
                upper_attrs[k.upper()]=v
            else:
                upper_attrs[k]=v
        return type.__new__(cls,name,bases,upper_attrs)
class Foo(metaclass=Base):
    foo=True
    def hello(self):
        print("world")
def hello():
    print("world")
print(dir(Foo))
```
当我们传入关键字参数 mataclass 时，魔术方法就生效了，这个参数会指示python解释器在创建这个Foo类时，要通过 
Base.__new__() 来创建，在上面这个new中，我们就可以修改类的定义。比如，加上新的方法啊或者修改自定义方法的大小写输出，然后，返回修改后的定义。  
   
#### 关于type以及元类的总结学习   
1. python一切皆对象，包括类；我们自己写的类其实也是相当于是由**内置元类type**生成的对象。
```python
class A:
    pass
    
print(A().__class__)
print(A.__class__)
print(int.__class__)

>>><class '__main__.A'>
>>><class 'type'>
>>><class 'type'>
```

2. 我们可以用类来创建实例对象，结合上一点类也是对象可以利用type()函数动态创建类这种对象出来。
```python
def func(obj):
    print(obj.name, obj.age)

A = type("my_class", (object, ), {'name': 'LJP', 'age': '999', 'func': func}) # 对应的参数依次是 类名，继承的父类，以及类的属性(类属性和类方法，注意是类属性不是实例化后的对象属性)
a = A()
print(a.__class__, a.name, a.age)  
a.func()

>>><class '__main__.my_class'> LJP 999
>>>LJP 999
```

3. python3创建自己的元类（元类是创建类的"东西"）
```python
def func(cls):      # 因为给类添加方法以后，在生成对应的实例对象调用方法时候第一个参数都是默认的实例对象本身self，所以这边的方法第一个参数是必须的。
    print(cls.country)
    print('元类添加的方法')

class my_metaClass(type):   # 建立自定义的元类，需要继承type类，利用type.__new__重新生成类
    """自己的元类"""
    def __new__(cls, class_name, father_class, attrs):
        print(class_name)
        print(father_class)
        attrs['func'] = func    # 利用元类生成的新类都添加方法 "func".
        print(attrs)   # 注意这边是可以接收到Base的类属性country的，这也是Django中Model的sql列属性的写法。
        return type.__new__(cls, class_name, father_class, attrs)


class Base(dict, metaclass=my_metaClass):
    country = "CHINA"

    def get_country(self):
        print(self.country)


class User(Base):
    def __init__(self, name, age):
        self.name = name
        self.age = age


user_a = User('LJP', 999)
print(user_a.country, user_a.name, user_a.age)
user_a.func()

>>>Base
>>>(<class 'dict'>,)
>>>{'__module__': '__main__', '__qualname__': 'Base', 'country': 'CHINA', 'get_country': <function Base.get_country at 0x1036bae18>, 'func': <function func at 0x1036ba6a8>}
>>>User
>>>(<class '__main__.Base'>,)
>>>{'__module__': '__main__', '__qualname__': 'User', '__init__': <function User.__init__ at 0x1036baea0>, 'func': <function func at 0x1036ba6a8>}
>>>CHINA LJP 999
>>>CHINA
>>>元类添加的方法
```
解析：  python3在利用自定义元类生成类的方法是通过 "关键字参数mataclass"来实现的；  具体过程是首先在当前类中（User）查找metaclass，如果没有找到就继续在父类中（Base）查找metaclass，找到了就利用
metaclass=my_metaClass定义的my_metaClass元类来创建User类（即__new__方法，这边要记得自定义元类要继承type类）。



4. 了解了元类，利用元类创建ORM[ORM理解、元类](https://www.cnblogs.com/Gaoqiking/p/10744253.html)

```python
class Field(object):
    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)

class StringField(Field):
    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')  # 调用super时，不用传入self


class IntegerField(Field):
    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')


class ModelMetaclass(type):
    def __new__(cls, name, bases, attrs):
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mappings  # 保存属性和列的映射关系
        attrs['__table__'] = name  # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)


class Model(dict, metaclass=ModelMetaclass):
    def __init__(self, **kw):
        print(kw)
        super(Model, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self, k, None))
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))


class User(Model):
    # 定义类的属性到列的映射：
    id = IntegerField('id')  # 为什么在元类那边可以遍历到id这个属性呢，因为这边写的是类属性。而元类的type的第三个参数正式类属性和类方法。
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')


# 创建一个实例：
u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
# 保存到数据库：
u.save()
```
总结: 元类是用来创建类的，而ORM这种需要读取 字段-数据库字段 映射关系的需求就刚好通过自定义的metaclass来扫描映射关系，并存储到自身的类属性如__table__、__mappings__中；这样每个继承基类Model的类就自动映射好了类属性字段与sql表字段的关系，只需要在Model里添加save、find、delete等成员方法或类方法即可，省去了在方法中重复编写扫描 代码字段与sql字段映射关系的代码。

[廖旭峰的设计编写ORM](https://www.liaoxuefeng.com/wiki/1016959663602400/1018490605531840)

### 设计模式
#### 单例模式
* 单例模式（Singleton Pattern）是一种常用的软件设计模式，该模式的主要目的是确保某一个类只有一个实例存在。当你希望在整个系统中，某个类只能出现一个实例时，单例对象就能派上用场。（None就是单例）
1. metaclass 实现
```python
class Singleton(type):
    _instances = {}
    def __call__(self, *args, **kwargs):
        print("父类的call")
        if self not in self._instances:
            self._instances[self] = super(Singleton, self).__call__(*args, **kwargs)
        return self._instances[self]

class Foo(metaclass=Singleton):
    pass
    def __init__(self):
        print("实例化的类对象")
foo1=Foo()   # 第一次创建元类Singleton的Foo类实例对象Foo，Foo()调用元类__call__方法，该__call__方法继续调用type的__call__方法使该元类的实例对象Foo仍然为可执行的函数Foo(),所以super(Singleton, self).__call__(*args, **kwargs)就会调用Foo自己的__init__方法，最终会打印出  "实例化的类对象" 这几个字
foo2=Foo()   # 第二次创建元类Singleton的Foo类实例对象Foo，Foo()调用元类__call__方法，此时cls._instances[]字典中已经存在Foo类，不会再调用type的__call__方法阻止了后续的Foo变为可执行的函数Foo(),进而不能调用自己的__init__方法， 此次没有打印出"实例化的类对象" 这几个字
print(id(foo1)==id(foo2))
```
总结：  
首先了解__call__函数作用， __call__函数作用是可以使类和类实例化的对象通过 实例对象()或者 类()() 的调用方式执行__call__函数的代码，即将类的实例化对象变为可以执行的函数（Call self as a function）;  
而我们知道利用自定义的元类metaclass创建的类其实就是自定义元类的对象，所以在上面的代码中Foo()就会调用元类Singleton的__call__函数, 之后的历程详情见上方的代码注释。
```
class A:
    def __call__(self, *args, **kwargs):
        print(args)
        print(kwargs)
        print('__call__方法')

a = A()
a(1,2,3, test=1)

>>>(1, 2, 3)
>>>{'test': 1}
>>>__call__方法
```
2. 函数装饰器实现
```python
def singleton(cls):
    _instance = {}
    def wrapper():
        if cls not in _instance:
            _instance[cls] = cls()
        return _instance[cls]
    return wrapper
    
@singleton
class Cls(object):
    def __init__(self):
        pass
cls1 = Cls()
cls2 = Cls()
print(id(cls1) == id(cls2))
```
3. import 方法
* Python 的模块就是天然的单例模式
#### [工厂模式](https://segmentfault.com/a/1190000013053013)
* 工厂模式是说调用方可以通过调用一个简单函数就可以创建不同的对象。工厂模式一般包含工厂方法和抽象工厂两种模式。
* 简单工厂的优点是可以使用户根据参数获得对应的类实例，避免了直接实例化类，降低了耦合性；缺点是可实例化的类型在编译期间已经被确定，如果增加新类 型，则需要修改工厂，不符合OCP（开闭原则）的原则。简单工厂需要知道所有要生成的类型，当子类过多或者子类层次过多时不适合使用。
```python
class Operation():
    def __init__(self, num1=0, num2=0):
        self._num1 = num1
        self._num2 = num2

    @property
    def num1(self):
        return self._num1

    @num1.setter
    def num1(self, num):
        self._num1 = num

    @property
    def num2(self):
        return self._num2

    @num2.setter
    def num2(self, num):
        self._num2 = num

    def getResult(self): 
        pass
        

class Add(Operation):
    def getResult(self):
        return self._num1 + self._num2

class Multi(Operation):
    def getResult(self):
        return self._num1 * self._num2

class OperationFactory():
    def chooseOperation(self,op):
        if op == '+':
            return Add()

        if op == '*':
            return Multi()

num1, num2 = 10, 7
optype = '+'
OpeFac = OperationFactory()   # 实例化工厂对象
OpeObject = OpeFac.chooseOperation(optype)   # 根据参数选择实例化对应的工厂产品对象
OpeObject.num1 = num1   # 给工厂产品对象进行组装
OpeObject.num2 = num2   # 给工厂产品对象进行组装
print(OpeObject.getResult())    # 获取工厂产品对象的计算结果
```