.. Introduction
.. ===============================================================================

入门
===============================================================================

.. contents:: **目录**
   :local:


.. Simple example
.. --------------

简单的例子
--------------

.. note::


   你可以在命令行或者IPython/Jupyter notebook 中执行下面的代码。
   在上述环境中，可以用IPython的 `%timeit` 命令替代我写的 `custom one <code/tools.py>`_ 。
   译者：在IPython中用 `%timeit` 的方式，将添加到下文中。


..   You can execute any code below from the `code <code>`_ folder using the
..   regular python shell or from inside an IPython session or Jupyter notebook. In
..   such a case, you might want to use the magic command `%timeit` instead of the
..   `custom one <code/tools.py>`_ I wrote.

Numpy 即向量化（vectorization）。
如果你熟悉Python，向量化就是学Numpy时的难点。
因为你需要改变思维方式，并熟悉一些新的名词，比如向量、数组、视图以及通用函数等。

.. Numpy is all about vectorization. If you are familiar with Python, this is the
.. main difficulty you'll face because you'll need to change your way of thinking
.. and your new friends (among others) are named "vectors", "arrays", "views" or
.. "ufuncs".

我们从一个简单的例子开始：随机游走（random walk）。
实现随机游走的一种可选方式，是用面向对象的方法：先定义一个 `RandomWalker` 类，
并写一个 `walk` 方法，该方法每次调用即运行一个随机步，然后返回当前位置。
这样实现起来很简单且代码可读，但是很慢：

.. Let's take a very simple example, random walk. One possible object oriented
.. approach would be to define a `RandomWalker` class and write a walk
.. method that would return the current position after each (random) step. It's nice,
.. it's readable, but it is slow:

.. **Object oriented approach**

**面向对象的方案**

.. code:: python

   class RandomWalker:
       def __init__(self):
           self.position = 0

       def walk(self, n):
           self.position = 0
           for i in range(n):
               yield self.position
               self.position += 2*random.randint(0, 1) - 1
           
   walker = RandomWalker()
   walk = [position for position in walker.walk(1000)]

.. Benchmarking gives us:

测试一下耗时:

.. code:: pycon

   >>> from tools import timeit
   >>> walker = RandomWalker()
   >>> timeit("[position for position in walker.walk(n=10000)]", globals())
   10 loops, best of 3: 15.7 msec per loop


译者：以上代码中，作者用自定义的 `timeit` 测试运行时间，在ipython中可用 `%timeit` 替代:

.. code:: pycon

   >>> walker = RandomWalker()
   >>> %timeit [position for position in walker.walk(n=10000)]
       

.. **Procedural approach**

**面向过程的方案**

对这样一个简单的问题，我们可以去掉类定义，只写一个函数，计算每一个随机游走步的位置。

.. For such a simple problem, we can probably save the class definition and
.. concentrate only on the walk method that computes successive positions after
.. each random step.

.. code:: python

   def random_walk(n):
       position = 0
       walk = [position]
       for i in range(n):
           position += 2*random.randint(0, 1)-1
           walk.append(position)
       return walk

   walk = random_walk(1000)

这个新方法（相对前面的面向对象方案），并没有节约太多CPU时间，耗时相差不多。
这个细微的时间变化，可能是因为节省了Python内部实现面向对象的耗时。

.. This new method saves some CPU cycles but not that much because this function
.. is pretty much the same as in the object-oriented approach and the few cycles
.. we saved probably come from the inner Python object-oriented machinery.

.. code:: pycon

   >>> from tools import timeit
   >>> timeit("random_walk(n=10000)", globals())
   10 loops, best of 3: 15.6 msec per loop
   >>> # or in ipython 
   >>> %timeit random_walk(n=10000)

   
.. **Vectorized approach**

**向量化的方案**
   
我们可以利用 `itertools
<https://docs.python.org/3.6/library/itertools.html>`_ 模块进一步提高性能。
`itertools` 为实现高效的循环提供了一系列迭代器。
如果我们把随机游走视为所有随机步的累积和，那么可以先生成所有的随机步，但是不通过循环计算累积和，而是利用`accumulate` 来计算:

.. But we can do better using the `itertools
.. <https://docs.python.org/3.6/library/itertools.html>`_ Python module that
.. offers *a set of functions creating iterators for efficient looping*. If we
.. observe that a random walk is an accumulation of steps, we can rewrite the
.. function by first generating all the steps and accumulate them without any
.. loop:

.. code:: python

   def random_walk_faster(n=1000):
       from itertools import accumulate
       # Only available from Python 3.6
       steps = random.choices([-1,+1], k=n)
       return [0]+list(accumulate(steps))

    walk = random_walk_faster(1000)

实际上，我们刚刚 **向量化** 了这个函数。
我们没有循环的生成一系列的步子，然后叠加到当前位置上，而是先生成了所有步子，然后用  `accumulate
<https://docs.python.org/3.6/library/itertools.html#itertools.accumulate>`_
一次计算所有的累积和。
去掉循环后，快了很多：

.. In fact, we've just *vectorized* our function. Instead of looping for picking
.. sequential steps and add them to the current position, we first generated all the
.. steps at once and used the `accumulate
.. <https://docs.python.org/3.6/library/itertools.html#itertools.accumulate>`_
.. function to compute all the positions. We got rid of the loop and this makes
.. things faster:

.. code:: pycon

   >>> from tools import timeit
   >>> timeit("random_walk_faster(n=10000)", globals())
   10 loops, best of 3: 2.21 msec per loop

我们减少了85%的计算时间，很不错。
但这个版本更大的优势，是可以很简单的过渡到numpy的向量化操作。
我们只需将itertools对应到numpy中的操作即可。

.. We gained 85% of computation-time compared to the previous version, not so
.. bad. But the advantage of this new version is that it makes numpy vectorization
.. super simple. We just have to translate itertools call into numpy ones.

.. code:: python
       
   def random_walk_fastest(n=1000):
       # No 's' in numpy choice (Python offers choice & choices)
       steps = np.random.choice([-1,+1], n)
       return np.cumsum(steps)

   walk = random_walk_fastest(1000)
           

这个转变不难，但获得了大约500倍的性能提升：

.. Not too difficult, but we gained a factor 500x using numpy:
 
.. code:: pycon

   >>> from tools import timeit
   >>> timeit("random_walk_fastest(n=10000)", globals())
   1000 loops, best of 3: 14 usec per loop

这本书将在代码或者问题的层次，讲述向量化。
这跟自定义的向量化是有区别的，具体内容将在后文详述。

.. This book is about vectorization, be it at the code or problem level. We'll
.. see this difference is important before looking at custom vectorization.


.. Readability vs speed

可读性 vs 速度
--------------------

在开始下一章之前，我先提个醒：等你熟悉了numpy，你可能需要面对的一个问题: 代码可读性。
Numpy是很强大的库，但是这也会带来可读性差的问题。
如果写代码的时候不写注释，估计几周（或者几天）后就不知道自己写的函数是做什么的。
举个例子，你可以看出下面的函数是在做什么吗？
第一个函数可能可以，第二个就难说了。

.. Before heading to the next chapter, I would like to warn you about a potential
.. problem you may encounter once you'll have become familiar with numpy. It is a
.. very powerful library and you can make wonders with it but, most of the time,
.. this comes at the price of readability. If you don't comment your code at the
.. time of writing, you won't be able to tell what a function is doing after a few
.. weeks (or possibly days). For example, can you tell what the two functions
.. below are doing? Probably you can tell for the first one, but unlikely for the
.. second (or your name is `Jaime Fernández del Río
.. <http://stackoverflow.com/questions/7100242/python-numpy-first-occurrence-of-subarray>`_
.. and you don't need to read this book).

.. code:: python
          
   def function_1(seq, sub):
       return [i for i in range(len(seq) - len(sub)) if seq[i:i+len(sub)] == sub]

   def function_2(seq, sub):
       target = np.dot(sub, sub)
       candidates = np.where(np.correlate(seq, sub, mode='valid') == target)[0]
       check = candidates[:, np.newaxis] + np.arange(len(sub))
       mask = np.all((np.take(seq, check) == sub), axis=-1)
       return candidates[mask]

第二个函数实际上就是第一个函数的向量化优化版本。
相对第一个，性能提升大约10倍，但是几乎不可读。

.. As you may have guessed, the second function is the
.. vectorized-optimized-faster-numpy version of the first function. It is 10 times
.. faster than the pure Python version, but it is hardly readable.
