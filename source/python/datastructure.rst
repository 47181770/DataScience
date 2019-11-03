数据结构
----------

1. 变量和类型
~~~~~~~~~~~~~~

变量是一种存储数据的载体。计算机中的变量是实际存在的数据或者说是存储器中存储数据的一块内存空间，变量的值可以被读取和修改，这是所有计算和控制的基础。计算机能处理的数据有很多种类型，除了数值之外还可以处理文本、图形、音频、视频等各种各样的数据，那么不同的数据就需要定义不同的存储类型。Python中的数据类型很多，而且也允许我们自定义新的数据类型，如下为我们常用的Python数据类型

 * 整形
 * 浮点型
 * 字符串型
 * 布尔型
 * 复数型

变量的使用：


2. 变量命名
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Python需要使用**合法**标识符给变量命名，标识符就是用于给程序中变量、类、方法命名的符号

- **Python 保留字**

-----------------------------------------

如下为Python保留字，不能用于变量、函数等对象名字。

.. raw:: html

 <font color="red">
 import class def and global nonlocal  not
 with as in from return
 if elif else assert pass break continue or yield
 try except finally raise for while lambda is  del</font>


关键字列表随时查：

.. code:: python

 #导入keyword 模块
 import keyword
 #显示所有关键字
 print(type(keyword.kwlist))
 <class 'list'>
 keyword.kwlist
 ['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

- 对命名的PEP 8要求与硬性规则

  1. PEP 8:

     a. 用小写字母拼写，多个单词用下划线连接
     b. 受保护的实例属性用单个下划线开头
     c. 私有的实例属性用两个下划线开头

  2. 硬性规则:

     a. 变量名由字母（广义的Unicode字符，不包括特殊字符）、数字和下划线构成，数字不能开头
     b. 大小写敏感
     c. 与关键字（有特殊含义的单词）和系统保留字（如函数、模块等的名字）不冲突

3. 变量使用
~~~~~~~~~~~~~~~~~~~~~~~

- 变量使用

- 变量转换

4. 运算符
~~~~~~~~~~~~~~~~~

加减乘除等


5. 字符串与常用数据结构
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


  1. 列表

  2. 元组

  3. 字典

  4. 集合


