## Non-Overview

Python rookie is on her way :)

Wish me good luck with python!

### Basic

- print(var)

- print("join", "us")

  `>>> join us`

  "," is replaced by " " (space).

- sth = input()

  Get user input and assign it to sth.

- sth = input("hint:")

  Input after a hint.

- `>>> 'I\'m \"OK\"!'` --> `>>> I'm "OK"!`

- `>>> print(r'\\\t\\')` --> `>>> \\\t\\`

- multiple line string

  ```
  >>> print('''line1
  ... line2
  ... line3''')
  line1
  line2
  line3
  ```

  same as

  ```
  >>> print(r'''line1
  ... line2
  ... line3''')
  ```

- Dynamic & static programming language. 这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。例如Java是静态语言.

- String is passed by value

- Divide

  `>>> 9 / 3` --> `>>> 3.0`

  `/` return float type

  `>>> 10 // 3` --> `>>> 3`

  `//` return integer

- 在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。

  把Unicode编码转化为“可变长编码”的UTF-8编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。

  在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言。

- 由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。

  `'abc'` 是str，`b'abc'`内容一样，但是每个字符只占用一个字节。

  `>>> 'abc'.encode('ascii')` --> `>>> b'abc'`

  `>>> '中文'.encode('utf-8')` --> `>>> b'\xe4\xb8\xad\xe6\x96\x87'` 中文编码成ascii会报错，因为范围不够。

  反向是`decode()`。

  len(str)是求字符数。 len(b'str')是求字节数。

- `# -*- coding: utf-8 -*-` 按照UTF-8编码读取源代码，否则源代码中的中文输出可能会有乱码。

- 格式化输出字符串

  |%?|类型|
  |:--:|:--:|
  |%d|整数|
  |%f|浮点数|
  |%s|字符串|
  |%x|十六进制整数|

  `%%`转义成`%`。

  `>>> 'Age: %s. Gender: %s' % (25, True)` --> `>>> 'Age: 25. Gender: True'`

- 取出array最后一个元素，`a[-1]`
