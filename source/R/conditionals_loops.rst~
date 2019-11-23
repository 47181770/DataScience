Conditionals and Loops 条件循环
-------------------------------------

1. 条件执行
~~~~~~~~~~~~~~

- if语句/单条件执行

 如果满足条件，则执行程序，否则执行下一个条件并退出，其中条件必须是一个被解析为T或F的表达式，所以尽可能让条件简单:
 条件控制语句写法：if (condition) {expr1} else {expr2}
 else不能单独成一行，它的前边必须有内容

.. code:: r

 if ('条件表达式为True'){
    '执行语句'
 }else{
    '执行语句'
 }

 # if...else...也可以写成一行
 x <- 3
 y <- if (x==3) 7 else 8
 print('y is the right number: 7')
 #输出: x is the right number: 7

2. 多条件执行
~~~~~~~~~~~~~~

    多条件if...else嵌套结构用来实现更复杂的处理：
    2.1 if(条件1）{语句块1} else if(条件2){语句块2} ... else{语句块}
    2.2 if(条件) {if(条件1){语句块1} else{语句块2} else if(条件2}{if(条件3}...else...} else...

参考如下多条件样例

.. code:: r

  x <- 100
  y <- 99

  if (x<y) {
    print('x is less than y')
  } else if (x == y) {
    print('x is equal to y')
  } else {
    print('x is larger than y')
  }

  # 输出结果：[1] "x is larger than y"
  

3. 嵌套条件
~~~~~~~~~~~~~~~~~~~


参考如下多条件样例嵌套条件Nested conditionals:

.. code:: python

 if x < y:
     STATEMENTS_A
 else:
     if x > y:
         STATEMENTS_B
     else:
         STATEMENTS_C

4. for循环
~~~~~~~~~~~~~~~~

   for循环的语法格式：for(var in seq){ expr } var为循环变量，seq为向量表达式，通常为一个序列 expr是一个表达式语句组，expr随着var的取不同的seq结果向量的值而被多次重复执行。

.. code:: r

     # 执行1到1000的求和值
     sum <- 0
     for (var in 1:1000)
       sum = sum + var
     print(sum)
     # 返回 500500

5. 生成Tables
~~~~~~~~~~~~~~~

- 生成2 :sup:`x` 次方，x为0-10的数字，最大生成的值为2 :sup:`10` =1024

.. code:: python

 for x in range(11):   # Generate numbers 0 to 10
     print(x, '\t', 2**x)


6. While语句
~~~~~~~~~~~~~~~~~~

猜数字（100以内）游戏

.. tip::

 如何快速的猜对答案，提示算法：二分查找

.. code:: python

    import random
    input_number = input('请输入数字: ')
    # 设定一个数字，看怎么在最短循环次数内猜对
    game_number = random.randint(1, 100)
    guess = int(input_number)
    while guess != game_number:
        if guess > game_number:
            guess_new = int(input('猜大了,请重新输入数字: '))
        else:
            guess_new = int(input('猜的数字小了，请重新输入数字: '))
        guess = guess_new

    print('恭喜你，猜对了')



7. 怎样选择for或while
~~~~~~~~~~~~~~~~~~~~~~~~

- for循环：知道开始循环之前执行程序体的最大次数，是确定性迭代

- while循环：不确定性迭代，无法确定执行次数的上限


8. break continue pass exit()
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **break**

  * break语句用在while和for循环中
  * 打破了最小封闭for或while循环
  * break语句用来终止循环语句，即循环条件没有False条件或者还没被完全递归完，也会停止执行循环语句
  * 如果使用的是嵌套循环，break语句在深层嵌套中，则break会停止执行最深层的循环，并开始执行下一行代码


.. code:: python

 for i in range(8):
    print("---i1 is--{}---printed--".format(i))
    for j in range(6):
        print("-j1-{}---printed".format(j))
        if j > 3:
            print("--j2-{}---printed".format(j))
            break

|
  执行结果说明: 第一层循环--i1 is-- 5次全部执行完成，第二层for循环执行前5次（值为0-4）执行完，在j >= 4时，因为符合break条件，所以退出次循环

- **continue**

  * 跳出本次循环
  * continue 语句用在while和for循环中
  * continue 语句跳出本次循环，而break跳出整个循环

.. code:: python

 for i in range(8):
    print("---i1 is--{}---printed--".format(i))
    for j in range(6):
        print("-j1-{}---printed".format(j))
        if j > 3:
            print("--j2-{}---printed".format(j))
            continue
            print("--j3-{}---printed".format(j))


 # 注意观察i1 j2 j3值在循环中的变化

|

 执行结果说明: continue退出本次循环，跳过当前循环的剩余语句，然后继续下一轮第一层循环


- **pass**

  * 什么都不做， do nothing

.. code:: python

 for i in range(6):
     print("---i1 is--{}---printed--".format(i))
     for j in range(6):
         print("-j1-{}---printed".format(j))
         if j > 3:
             print("--j2-{}---printed".format(j))
             pass
             print("--j3-{}---printed".format(j))


-- **exit()**

   * 结束整个程序

.. code:: python

 for i in range(6):
     print("---i1 is--{}---printed--".format(i))
     for j in range(6):
         print("-j1-{}---printed".format(j))
         if j > 3:
             print("--j2-{}---printed".format(j))
             exit()
             print("--j3-{}---printed".format(j))

|

参考：
   1. Python 条件语句与循环，`Conditionals_and_Loops`_

.. _Conditionals_and_Loops: http://www.openbookproject.net/books/bpp4awd/ch04.html#conditionals-and-loops

