---
title: Python学习笔记1
date: 2016-05-08 22:02:28
tags: [Notes, python]
categories: Tech Notes
---

##

<!--more-->

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

  list[::-1] --> ["a", 3, 2, 1]

- `if __name__ == "__main__":`

  类似于java中的main函数。

  python是解释型语言。

- _functionName_

- functionName.__name__

- map & reduce

- Python的OOP如何
