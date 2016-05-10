---
title: Python学习笔记1
date: 2016-05-08 22:02:28
tags: [Notes, python]
categories: Tech Notes
---

## 有点杂乱的笔记，第一次读python小项目

关于shebang, click, decorator, random, ifmain, underscore用法, map & reduce & filter, self, yield, OOP, uuid, with.

感谢[@wilbeibi](http://www.wilbeibi.com)的帮助 <3

<!--more-->

### Content

- **What does `#!/usr/bin/python` mean?**

  `#!` is a mark for what interpreter to use for this script.

  `#!/usr/bin/python` is called **Shebang line**

  > If you have several versions of Python installed, /usr/bin/env will ensure the interpreter used is the first one on your environment's $PATH. The alternative would be to hardcode something like #!/usr/bin/python; that's ok, but less flexible.

  If you want to use: `$python myscript.py`, you don't need that line at all. The system will call python and then python interpreter will run your script.

  But if you intend to use: $./myscript.py, here is where you need it.

  Reference: [StackOverFlowAnswer](http://stackoverflow.com/questions/2429511/why-do-people-write-usr-bin-env-python-on-the-first-line-of-a-python-script)

- **[Python IF...ELIF...ELSE Statements](http://www.tutorialspoint.com/python/python_if_else.htm)**

  ```
  if expression1:
    statements(s)
  elif expression2:
    statements(s)
  else:
    statements(s)
  ```

- **Library: Click**

  Click is a command line library for Python.

  ```
  import click

  @click.command()
  @click.option('--count', default=1, help='Number of greetings.')
  @click.option('--name', prompt='Your name',
                help='The person to greet.')
  def hello(count, name):
      for x in range(count):
          click.echo('Hello %s!' % name)

  if __name__ == '__main__':
      hello()
  ```
  Run this script:

  `$ python hello.py --count=3`

  Get a hint: `Your name:`

  Input `John`

  Output:

  ```
  Hello John!
  Hello John!
  Hello John!
  ```

  Reference: [$ Click](http://click.pocoo.org/5/)

- **Decorator**

  Decorator Pattern 多用于wrapper class。在不改变原来class的基础上，添加新功能。一层一层包起来，可以变换顺序。

  Python的annotation类似于函数式编程。

  一个例子:

  ```
  def hello(fn)
    def wrapper():
      print "hello, %s" % fn.__name__
      fn()
      print "goodbye, %s" % fn.__name__

  @hello
  def foo():
    print "I am foo"

  foo()
  ```

  foo()是一个全局的方法，可以当做main函数执行，上面的代码相当于 foo = hello(foo)。

  输出：

  ```
  hello, foo
  I am foo
  goodbye, foo
  ```

  > hello(foo)返回了wrapper()函数，所以，foo其实变成了wrapper的一个变量，而后面的foo()执行其实变成了wrapper()。

  Decorator还可以包裹多层，加参数。

  Reference:
    1. [装饰器与函数式Python(译)](http://blog.xiayf.cn/2013/01/04/Decorators-and-Functional-Python/)
    2. [Python修饰器的函数式编程](http://coolshell.cn/articles/11265.html)


- **choice & randint**

  ```
  from random import randint
  from random import choice
  ```
  `randint(0, 23)` 0~23闭合区间中随机一个，可重复。

  `choice(list)` list中随机一个，可重复。

- **range**

  `range(1,5)` [1, 2, 3, 4]左闭右开。

- **list的一个用法**

  list = [1, 2, 3, "a"]

  list[2:3] --> [2, 3]

  list[::-1] --> ["a", 3, 2, 1]实现了逆序输出。

- **`if __name__ == "__main__":`**

  类似于java中的main函数。

  python是解释型语言。一行一行读出来，所以代码顺序会影响执行结果。当读到main时候，执行main的内容，所以main一般放在最后，不然有一部分代码还没读到会报错。

  关于编译器和解释器的区别：

  ![](http://i.imgur.com/gGRoMLS.png)

  关于执行顺序的一个例子：

  ```
  # file one.py
  def func():
      print("func() in one.py")

  print("top-level in one.py")

  if __name__ == "__main__":
      print("one.py is being run directly")
  else:
      print("one.py is being imported into another module")

  # file two.py
  import one

  print("top-level in two.py")
  one.func()

  if __name__ == "__main__":
      print("two.py is being run directly")
  else:
      print("two.py is being imported into another module")
  ```
  运行：

  `python one.py`

  得到：

  top-level in one.py

  one.py is being run directly

  先print再到main。

  运行：

  `python two.py`

  得到：

  top-level in one.py

  one.py is being imported into another module

  top-level in two.py

  func() in one.py

  two.py is being run directly

  先one的print再到one的main。two的print，到one.fuc(),最后到two的main。

  > when module one gets loaded, its `__name__` equals "one" instead of `__main__`.

  Reference: [what does if main do](http://stackoverflow.com/questions/419163/what-does-if-name-main-do)

- **Underscore in python**

  1.
  > `_single_leading_underscore`: weak "internal use" indicator. E.g. "from M import ..." does not import objects whose name starts with an underscore.

  Python doesn't have real private methods, so one underline in the start of a method or attribute means you shouldn't access this method, because it's not part of the API.

  2.
  > `single_trailing_underscore_`: used by convention to avoid conflicts with Python keyword, e.g. `Tkinter.Toplevel(master, class_='ClassName')`

  3.
  > `__double_leading_underscore`: when naming a class attribute, invokes name mangling.

  It makes a lot of confusion. It should not be used to create a private method. It should be used to avoid your method to be overridden by a subclass. So, when you create a method starting with __ it means that you don't want anyone to override it, it will be accessible only from inside the class where it was defined.

  子类中如果有个跟父类一样的double underscore 开头的方法，那么子类的是不能覆盖父类的方法的。

  4.
  > `__double_leading_and_trailing_underscore__`: "magic" objects or attributes that live in user-controlled namespaces. E.g. `__init__`, `__import__` or `__file__`. Never invent such names; only use them as documented.

  `__this__`是python自动调用的方法。

  Reference:

  1. [why have underscores before and after function](http://stackoverflow.com/questions/8689964/python-why-do-some-functions-have-underscores-before-and-after-the-functio)

  2. [style guide](https://www.python.org/dev/peps/pep-0008/)

- **`functionName.__name__`**

  One of attributes of this function.

- **map & reduce**

  Map(func, list) 是把一个function用在list所有元素上。

  Reduce(func, list) 是按照function的方法将list所有元素合并成一个。

  Filter(func, list) 是把运用function后return为true的元素筛选出来。

  Reference: [Lambda, filter, reduce and map](http://www.python-course.eu/lambda.php)

- **Python的OOP如何**

  对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。

  对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了。

  动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的。

  Reference: [继承和多态](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431865288798deef438d865e4c2985acff7e9fad15e3000)

- **yield**

  > `yield` is a keyword that is used like return, except the function will return a generator.

  产生一个generator，需要遍历一遍得到内容。或者转成list(generator)。

  Reference: [What does the yield keyword do in Python?](http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-python)

- **`self`**

  self 就代表要这个方法作用的Object。

  没有self的类似于static修饰的。不过牵扯到引用传递还是值传递的问题略复杂。反正list是可以共享的，int, string啥的不行。

- **uuid**

  The uuid module implements Universally Unique Identifiers as described in RFC 4122.


- **无题**
  ```
  with open(self.file_path, 'w') as f:
            f.write(self.code)
  ```

  `with` 类似于 `try catch`，可以执行才执行。
