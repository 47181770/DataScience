Conditionals and Loops 条件循环
-------------------------------------

1. 条件执行
~~~~~~~~~~~~~~

- if语句

 如果满足条件，则执行程序，否则退出:


.. code:: python

 if ('条件表达式为True'):
    '执行语句'
 else:
    '执行语句'


 x = 3
 if x == 3:
    print('x is the right number: {}'.format(x))
 else:
    print('x is the wrong number: {}'.format(x))
 #输出: x is the right number: 3

2. 多条件执行
~~~~~~~~~~~~~~


3. 嵌套条件
~~~~~~~~~~~~~~~~~~~


4. 循环Loop
~~~~~~~~~~~~~~~~


5. Tables
~~~~~~~~~~~~~~~


6. While语句
~~~~~~~~~~~~~~~~~~

7. 怎样选择for或while
~~~~~~~~~~~~~~~~~~~~~~~~

- for循环：知道开始循环之前执行程序体的最大次数，是确定性迭代

- while循环：不确定性迭代，无法确定执行次数的上限


8. break continue pass
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **break**

  * break语句用在while和for循环中
  * 打破了最小封闭for或while循环
  * break语句用来终止循环语句，即循环条件没有False条件或者还没被完全递归完，也会停止执行循环语句
  * 如果使用的是嵌套循环，break语句在深层嵌套中，则break会停止执行最深层的循环，并开始执行下一行代码

.. code:: python

 for i in range(5):
    print("---i1 is--{}---printed--".format(i))
    for j in range(6):
        print("-j1-{}---printed".format(j))
        if j > 3:
            print("--j2-{}---printed".format(j))
            break

 结果说明: 第一层循环--i1 is-- 5次全部执行完成，第二层for循环执行前5次（值为0-4）执行完，在j >= 4时，因为符合bread条件，所以退出次循环
