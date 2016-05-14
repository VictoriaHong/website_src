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
