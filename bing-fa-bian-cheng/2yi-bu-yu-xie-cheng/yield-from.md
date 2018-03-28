##  yield和yield from 用法

### 迭代器(iterator)

讲到迭代器，就需要区别几个概念:`iterable`,`iterator`,`itertion`, 看着都差不多，其实不然。下面区分一下。

* iterable

  这个是`可迭代对象`,属于python的名词，范围也很广，可重复迭代，满足如下其中之一的都是`iterable`

  - 可以`for`循环: `for i in iterable`
  - 可以按`index`索引的对象，也就是定义了`__getitem__`方法，比如`list,str`;
  - 定义了`__iter__`方法。可以随意返回。
  - 可以调用`iter(obj)`的对象，并且返回一个`iterator`

* iterator

  迭代器对象`,也属于python的名词，只能迭代一次。需要满足如下的`迭代器协议`

  * 定义了`__iter__`方法，但是必须**返回自身**
  * 定义了`next`方法,在python3.x是`__next__`。**用来返回下一个值**，并且当没有数据了，抛出`StopIteration`
  * 可以保持当前的状态

* itertion

  就是`迭代`,一个接一个(one after another),是一个通用的概念，比如一个循环遍历某个数组

凡是可作用于for循环的对象都是Iterable类型；

凡是可作用于next()函数的对象都是Iterator类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是Iterable但不是Iterator，不过可以通过iter()函数获得一个Iterator对象。



### yield

generator的函数，在每次调用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的`yield`语句处继续执行。

* 创建生成器的方法:

  * yield 关键字

  * 生成器表达式

    (i for i in range(5))

  * 自定义一个有next方法和iter方法的类


* #### next方法

  * 语法：

    next(iterator[, default])

  * 作用：

    next()  返回迭代器的下一个项目。 iterator - 可迭代对象.  default - 可选，用于设置在没有下一个元素时返回该默认值，如果不设置，又没有下一个元素则会触发 StopIteration 异常。

    因为generator保存的是算法，每次调用`next()`，就计算出下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。

  * `return`：生成器中也可以包含return语句，但是不能出现在yield表达式中，当执行到return语句时如果有finally语块则执行，之后会抛出StopIteration异常。不再继续其他的迭代生成。

    ​

*  ### iter方法

  - 语法:

    ​	iter(object[, sentinel])

  - 作用:

    返回一个iterator对象。 

    ​

*  ### send方法

  * 语法:

    send(value)

  * 作用:

    send方法会首先把上一次挂起的yield语句的返回值通过参数设定,从而实现与生成器方法的交互。但是需要注意，在一个生成器对象没有执行next方法之前，由于没有yield语句被挂起，所以执行send方法会报错.除法执行send(None).

    ​

* ### throw方法

  * 作用：

    它的实现手段是通过向生成器对象在上次被挂起处，抛出一个异常。之后会继续执行生成器对象中后面的语句，直至遇到下一个yield语句返回。如果在生成器对象方法执行完毕后，依然没有遇到yield语句，抛出StopIteration异常。

* ### close方法

  close方法会在生成器对象方法的挂起处抛出一个GeneratorExit异常。GeneratorExit异常产生后，系统会继续把生成器对象方法后续的代码执行完毕。

  需要注意的是，GeneratorExit异常的产生意味着生成器对象的生命周期已经结束。因此，一旦产生了GeneratorExit异常，生成器方法后续执行的语句中，不能再有yield语句，否则会产生RuntimeError。




### yield from

* 出现原因

  生成器能够很容易分为多个拥有send和throw方法的子生成器，像一个大函数可以分为多个子函数一样简单。Python的生成器是协程`coroutine`的一种形式，但它的局限性在于只能向它的直接调用者yield值。这意味着那些包含yield的代码不能想其他代码那样被分离出来放到一个单独的函数中。

* 对于简单的迭代器，`yield from iterable`本质上等于`for item in iterable: yield item`的缩写版，如下所示：

  ```python
  def g(x):
      yield from range(x, 0, -1)
      yield from range(x)

  print(list(g(5)))
  ```

* 然而，不同于普通的循环，yield from允许子生成器直接从调用者接收其发送的信息或者抛出调用时遇到的异常，并且返回给委派生产器一个值，如下所示：

  ```python
  def accumulate():#子生成器
      tally = 0
      while True:
          next = yield
          #print(next)
          if next is None:
              return tally
          tally = tally+next
  def gather_tallies(tallies):
      while True:
          tally = yield from accumulate()
          tallies.append(tally)
  tallies = []
  acc = gather_tallies(tallies)
  next(acc) #激活生成器
  for i in range(4):
      acc.send(i)
  acc.send(None)#结束
  for i in range(5):
      acc.send(i)
  acc.send(None)
  print(tallies)
  ```

*  **利用yield from从生成器读取数据**

  ```
  def reader():
      # 模拟从文件读取数据的生成器
      for i in range(4):
          yield '***{}***'.format(i)

  def reader_wrapper(g):
      # 循环迭代从reader产生的数据 
      for v in g:
          yield v

  wrap = reader_wrapper(reader())
  for i in wrap:
      print(i)
  ```

  结果：
  '''
  ***1***
  ***2***
  ***3***
  ***4***
  '''
  我们可以用yield from语句替代reader_wrapper(g)函数中的循环，如下：
  def reader_wrapper(g):
      yield from g
  效果是一样的
  ```

  ```

* **利用yield from语句向生成器（协程）传送数据 **

  ```python
  def writer():
      # 读取send传进的数据，并模拟写进套接字或文件
      while True:
          w = (yield)    # w接收send传进的数据
          print('>> ', w)
          
  def writer_wrapper(coro1):
      coro1.send(None)  # 生成器准备好接收数据
      while True:
          try:
              x = (yield)  # x接收send传进的数据
              coro1.send(x)  # 然后将x在send给writer子生成器
          except StopIteration:    # 处理子生成器返回的异常
              pass
              
  #包装器也是个生成器，上面所有复杂的写法也可以用yield from替换：
  def writer_wrapper(coro2):
      yield from coro2
  ```

* **利用yield from向生成器传送数据--处理异常**

  ```python
  class SpamException(Exception):
      pass

  def writer():
      while True:
          try:
              w = (yield)
          except SpamException:
              print('***')
          else:
              print('>> ', w)
              
  def writer_wrapper(coro1):
      # 手工处理异常被抛给子生成器
      coro1.send(None)    # 生成器准备好接收数据
      while True:
          try:
              try:
                  x = (yield)
              except Exception as e:   # 捕获异常
                  coro1.throw(e)
              else:
                  coro1.send(x)
          except StopIteration:
              pass
              
  w = writer()
  wrap = writer_wrapper(w)
  wrap.send(None)  # "prime" the coroutine
  for i in [0, 1, 2, 'spam', 4]:
      if i == 'spam':
          wrap.throw(SpamException)
      else:
          wrap.send(i)
          
  同样的可以吧writer_wrapper方法写成：
  def writer_wrapper(coro):
      yield from coro
  ```

   看到这里大概能理解yield from显示处理传值给子生成器以及抛出异常给子生成器的意思了。

  总之，这是一个魔法语句，它也是协程的重要组成部分，至于协程，还需要继续学习。