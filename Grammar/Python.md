- [变量](#变量)
- [字符串](#字符串)
- [字符串方法](#字符串方法)
- [字符串格式化](#字符串格式化)
- [f-string格式](#f-string格式)
- [字符串、列表、元组转换](#字符串列表元组转换)
- [字符串切片](#字符串切片)
- [字符串函数](#字符串函数)
- [计算符号](#计算符号)
- [小数](#小数)
- [列表](#列表)
- [列表方法](#列表方法)
- [列表推导式](#列表推导式)
- [列表实现堆栈](#列表实现堆栈)
- [列表实现队列](#列表实现队列)
- [del语句](#del语句)
- [元组](#元组)
- [元组方法](#元组方法)
- [集合](#集合)
- [字典](#字典)
- [字典方法](#字典方法)
- [判断 IF](#判断-if)
- [循环 for](#循环-for)
- [循环 while](#循环-while)
- [判断 match](#判断-match)
- [函数](#函数)
- [解包实参列表](#解包实参列表)
- [闭包](#闭包)
- [异常](#异常)
- [文件路径操作](#文件路径操作)
- [模块](#模块)
- [类](#类)
- [JSON](#json)
- [NameSpace](#namespace)
- [调用CMD命令](#调用cmd命令)

### 变量

```python
name = "name"
name = 'name'
name1 = 1
name = True
print(name1)
type(name)                    # 查看变量类型
a,b = 1,2                     # 批量赋值
a if a>b else b               # 三元表达式:如果a>b则输出a,否则输出b
```

### 字符串

```python
print("hello world")
print("hello \nworld")        # \n特殊作用字符
print("hello \'world\'")      # \加符号可以输出符号
print(r"hello \'world\'")     # r符号可输出原始内容
print(f"{a},{b}")             # f-strings格式输出，{}使用变量表达式
print(u"123456")              # 使用unicode编码，防止乱码
print("apple单价为%.02f,banana单价为%.02f,一共%.02f" % (price,weight,money))
print("""
Usage:py [options]
      -h
      -H hostname
""")                          #"""符号可以批量格式输出
print("123" + "book")         #字符串或变量可通过+符号相连
print("123" "123")            #邻近字符串可通过空格相连
print("UI" * 3)               #字符串可通过*符号进行复制
list1 = [1]
list2 = [2]
print(list1 + list2)            # 列表相加
print(list1*3)                  # 列表复制
```

### 字符串方法

```python
x = "hello book"
x.capitalize()                  # 首字母大写
x.casefold()                    # 全部字母小写，可以其他语言使用
x.title()                       # 每个单词首字母大写
x.swapcase()                    # 大小写翻转
x.upper()                       # 全大写
x.lower()                       # 全小写
x.center(10)                    # 字符少于原字符串，直接输出，大于原字符，则居中, 可带参数为填充字符
x.ljust(12)                     # 左对齐
x.zfill(12)                     # 填充字符
x.rjust(12)                     # 右对齐
x.rjust(18, "C")                # 填充字符
x.lstrip()                      # 清除左空格
x.rstrip()                      # 清除右空格
x.lstrip(" hell")               # 带参数的是按字符匹配，有则删除无则输出
x.strip()                       # 清除左右空格
x.removeprefix("he")            # 清除前缀
x.removesuffix("rd")            # 清除后缀
x.partition(".")                # 从左至右查找，以参数为中心分割为三段
x.rpartition(".")               # 从右至左
x.split(".")                    # 参数作为分割符，以列表返回
x.split(".", 1)                 # 参数作为分割符，分割次数，以列表返回
x.splitlines()                  # 按行输出
".".join([1, 3, 4])             # 用'.'拼接字符串
```

### 字符串格式化

```python
b = "hello word"
"{} {}".format("hello", "world")    # '.format'方式格式化字符串
"{0} {1} {0}".format("1", "2")      # 带顺序
"{name}".format(name="name")        # 带变量名
"{{}}".format()                     # 输出花括号
"{0:>10}".format("hello")           # 索引；对齐方向；宽度
"{:010}".format(520)                # ':'填充选项;字符
"{0:x=10}".format(520)              # 'x'为填充字符
```

### f-string格式

.format与f-string格式表达方式一致

```python
a = 20000
b = 'book'
print(f'{a!s}')                         # 'a'转换为str(),相当于str(a)
print(f'{a!a}')                         # 'a'转换为acsii()
print(f'{a!r}')                         # 'a'抓换为repr()
print(f'{a!a:<10} book')                # 10表示空格，强制字段在可用空间内左对齐(这是大多数对象的默认值)
print(f'{a!a:>10} book')                # 强制字段在可用空间内右对齐(这是数字的默认值)
print(f'{a:=10} book')                  # 强制在符号(如果有)之后数码之前放置填充。 这被用于以 '+000000120' 形式打印字段。 这个对齐选项仅对数字类型有效。 这是当 '0' 紧接在字段宽度之前时的默认选项。
print(f'{a:^10} book')                  # 强制字段在可用空间内居中。
print(f'{a:+}')                         # 表示标志应该用于正数和负数。
print(f'{a:-}')                         # 表示标志应仅用于负数（这是默认行为）。
print(f'{a: }')                         # ' ' 表示应在正数上使用前导空格，在负数上使用减号。

# 整数类型分隔符
print(f'{a:,}')                         # 使用逗号作为千位分隔符
print(f'{a:#}')                         # '#' 选项可让“替代形式”被用于执行转换。 替代形式会针对不同的类型分别定义。 此选项仅适用于整数、浮点数和复数类型。对于整数类型，当使用二进制、八进制或十六进制输出时，此选项会为输出值分别添加相应的 '0b', '0o', '0x' 或 '0X' 前缀。
print(f'{a:_}')                         # '_' 选项表示对浮点表示类型和整数表示类型 'd' 使用下划线作为千位分隔符。 对于整数表示类型 'b', 'o', 'x' 和 'X'，将为每 4 个数位插入一个下划线。 对于其他表示类型指定此选项则将导致错误。

# 字符串类型可用
print(f'{b:s}')                         # 字符串格式。这是字符串的默认类型，仅限字符串可用，可以省略。
print(f'{b:}')                          # None 和 's' 一样。

# 整数类型可用
print(f'{a:b}')                         # 二进制格式。 输出以 2 为基数的数字。
print(f'{a:c}')                         # 字符。在打印之前将整数转换为相应的unicode字符。
print(f'{a:d}')                         # 十进制整数。 输出以 10 为基数的数字。
print(f'{a:o}')                         # 八进制格式。 输出以 8 为基数的数字。
print(f'{a:x}')                         # 十六进制格式。 输出以 16 为基数的数字，使用小写字母表示 9 以上的数码。
print(f'{a:X}')                         # 十六进制格式。 输出以 16 为基数的数字，使用大写字母表示 9 以上的数码。 在指定 '#' 的情况下，前缀 '0x' 也将被转为大写形式 '0X'。
print(f'{a:n}')                         # 数字。 这与 'd' 相似，不同之处在于它会使用当前区域设置来插入适当的数字分隔字符。
print(f'{a:}')                          # None 和 'd' 相同。

# 浮点数类型可用
print(f'{a:.2f}')                       # 定点表示法。 对于一个给定的精度值 p，将数字格式化为在小数点之后恰好有 p 位的小数形式。如未指定精度，则会对 float 采用小数点之后 6 位精度
print(f'{a:.2F}')                       # 定点表示法。 与 'f' 相似，但会将 nan 转为 NAN 并将 inf 转为 INF。
print(f'{a:.2e}')                       # 科学计数法。 对于一个给定的精度值 p，将数字格式化为以字母 'e' 分隔系数和指数的科学计数法形式。
print(f'{a:.2E}')                       # 科学计数法。 与 'e' 相似，不同之处在于它使用大写字母 'E' 作为分隔字符。
print(f'{a:.2g}')                       # 常规格式。 对于给定精度 p >= 1，这会将数值舍入到 p 个有效数位，再将结果以定点表示法或科学计数法进行格式化，具体取决于其值的大小。 精度 0 会被视为等价于精度 1。
print(f'{a:.2G}')                       # 常规格式。 类似于 'g'，不同之处在于当数值非常大时会切换为 'E'。 无穷与 NaN 也会表示为大写形式。
print(f'{a:.2%}')                       # 百分比。 将数字乘以 100 并显示为定点 ('f') 格式，后面带一个百分号。

# 字符串等号表达式
print(f'{a=}')                          # '=' 为表达式表示方式
print(f'{a=:10}')                       # ':' 跟格式说明符
```

### 字符串、列表、元组转换

```python
list("1234")
list((1, 2, 3, 4, 5))
tuple("1234")
tuple([1, 3, 4, 5])
str([1, 2, 4])
str((1, 3, 4))
```

### 字符串切片

```python
word = 'python'
print(word[0])
print(word[-1])
print(word[0:2])
print(word[:2])
print(word[4:])
print(word[-2:])
```

### 字符串函数

```python
len()
format()
print("" % ())
print(f'')
```

### 计算符号

```python
print(14 // 3)
print(14 % 3)
print(14 + 3)
print(14 - 3)
print(14 * 3)
print(14 ** 3)
print(14 / 3)

x is y                      # 判断是同一对象， 依据ID值
x is not y                  # 判断否同一对象
"a" in "as"                 # a包含在as中
"a" not in "as"             # a不包含在as中

max()                       # 输出最大值
min()                       # 输出最小值
len()                       # 计算长度，len存在最大值，2^63-1
sorted()                    # 排序但不改变原列表
all()                       # 判断元素全为真则True
any()                       # # 判断元素有真则为True
sea = ["spring", 'summer', 'fall', 'winter']
enumerate(sea)              # 枚举对象
list(enumerate(sea))        # 形成带序号的元组
list(enumerate(sea, 10))    # 形成带序号的元组，序号从10开始[(1,"spring"), (2,"summer")]
zip(x, y)                   # 将两个列表组合起来形成元组[(2,1), (2,3), (6,4)]
zipped = itertools.zip_longest(x, y, z)  # 不会丢掉值，并且输出为none
print(list(zipped))
```

### 小数

```python
a = 14 / 3
print(round(a,1))             # round函数会使用四舍五入的方式进行小数保留
a1 = 3.141592
print("{:.1f}".format(a1))    # 使用fromat格式化函数进行小数保留
print(f'{a1:.1f}')            # 使用f-strings的方式进行小数保留
```

### 列表

```python
list = [1,2,3,4,5,6]
print(list[0])
print(list[-3:])                    # 列表和内置sequence类型一致,支持索引和切片
print(list[:])                      # 浅拷贝,返回列表所有元素的新列表
list + [4,5,6,7]                    # 列表可通过+符号进行合并
list[5] = 8                         # 可赋值和修改
list[-1]                            # 负号为倒数
list[0:]                            # 索引0到所有
list[:3]                            # 索引3之前所有
list[0:5:2]                         # 索引0到5间隔为2
list[2:5] = [8,8,8]                 # 切片赋值,索引2到5赋值
list[2:5] = []                      # 切片赋值为空,删除操作
list[:] = []                        # 切片删除列表
list1 = [[1,2,3,3],[1,2,3,4]]       # 列表嵌套
print(list1[0][0])                  # 索引方式 
```

### 列表方法

```python
fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi', 'apple', 'banana']
fruits.count('apple')           	# 统计出现的次数
fruits.index('banana')          	# 查询索引值
fruits.index('banana', 4) 
fruits.insert(1,2)              	# 插入
fruits.remove("2")              	# 删除
fruits.extend(["123","123"])    	# 批量添加
fruits.reverse()                	# 倒序
fruits.sort()                   	# 正序
fruits.append('grape')          	# 添加
fruits.pop()                    	# 弹出指定索引值内容,或者删除最后一位
fruits.clear()                  	# 清除列表
fruits.copy()                   	# 复制列表

import copy
matrix = [[1, 2, 3], [3, 4, 5], [5, 6, 7]]
matrix1 = matrix.copy()             # 列表的方法copy，浅拷贝，数据引用
matrix1 = copy.copy(matrix)         # copy模块引用式拷贝
matrix1 = copy.deepcopy(matrix)     # 同时对子对象拷贝
```

### 列表推导式

结果为列表[],i即为表达式,for中的i存放在i中;创建新列表放到旧列表；for列表则是原列表挨个挨个修改

```python
list = [i for i in range(10)]
list = [i[1] for i in matrix]
list = [i for i in range(10) if i % 2 == 0]  			# 可有条件，执行顺序for->if->i
list = [col for raw in matrix for col in raw]  			# 等同于
list = []
for raw in matrix:
    for col in raw:
        list.append(col)

```

### 列表实现堆栈

```python
stack = [3, 4, 5]
stack.append(6)
stack.append(7)
stack.pop()
```

### 列表实现队列

```python
from collections import deque
queue = deque(["Eric", "John", "Michael"])
queue.append("Terry")           # Terry arrives
queue.append("Graham")          # Graham arrives
queue.popleft()                 # The first to arrive now leaves
queue.popleft()                 # The second to arrive now leaves
```

### del语句

```python
a = [-1, 1, 66.25, 333, 333, 1234.5]
del a[0]                            # 删除索引值为0的元素
del a[2:4]                          # 删除2-4之间的元素
del a[:]                            # 清空列表
del a                               # 删除变量
```

### 元组

```python
empty = ()                          # 创建空元组
singleton = 'hello',                # 创建元组，元组打包
t = 12345, 54321, 'hello!'          # 创建元组
x, y, z = t                         # 元组解包,将元组的内容赋值给多个变量，同样适用于任何序列类型，包括列表，字符串，元组，字典
q, w, *e = t                        # *e将多余的赋值给e
rhy = (1, 2, 3, 4, 3, 5, 6, 3, 1)
rhy1 = (1, 3, 4, 5, 6)            
w = rhy, rhy1                       # 嵌套元组
rhy = [x for x in rhy]              # 可使用列表推导式
_ = (1, 2)                          # "_" 为匿名变量
```

### 元组方法

```python
tu = (1,2,3,4,5)
tu.count()
tu.index()
```

### 集合

```python
from asyncore import file_dispatcher
import itertools

s = set("asdfas")                   # 设置集合
s1 = [1, 1, 3, 4, 5]
len(s1) == len(set(s1))             # 判断列表是否具有重复元素，集合具有去重作用
s.isdisjoint(set("as"))             # 判断是否有相同元素，有则假无则真
s.isdisjoint("as")                  # 判断是否有相同元素，有则假无则真，传入可迭代对象即可
s.issubset("as")                    # 是否为s的子集
s.issuperset("asd")                 # 是否为s的超集
s.union({1, 3, 7})                  # 并集, 支持多参数
s.intersection({1, 3})              # 交集， 支持多参数
s.difference({1, 3})                # 差集，支持多参数
s <= set("")                        # 子集， 使用运算符两侧必须为集合类型
s < set("")                         # 真子集
s > set("")                         # 超集
s >= set("")                        # 真超集
s | {""}                            # 并集
s & {""}                            # 交集
s - {""}                            # 差集
s ^ {""}                            # 对称差集
s.update([1, 3], "43")              # 集合不能重复，且为无序
s.intersection_update([])           # 使用交集更新
s.difference_update([])             # 使用差集更新
s.symmetric_difference_update([])
s.add("12")                         # 单纯的插入，整体插入
s.remove([1, 3])                    # 指定删除元素，没有则抛出异常
s.discard([1])                      # 静默处理
s.pop()                             # 随机弹出
s.clear()                           # 清空集合
hash(1)                             # 获取哈希值，不可变可以得hash值，可变对象不可求
s = frozenset({})                   # 不可变集合
```

### 字典

```python
tel = {}
tel = {'jack': 4098, 'sape': 4139}
tel["jack"]
del tel['sape']
list(tel)
sorted(tel)
'guido' in tel
'jack' not in tel
dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])

knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
    print(k, v)

for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)

questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
    print('What is your {0}?  It is {1}.'.format(q, a))
```

### 字典方法

```python
y.pop()                             # 弹出第一个键
y.pop('a','no')                     # 如果没有a输出后面的值
y.popitem()                         # 弹出键值对
del y["1"]                          # 删除字典某个键值对
y.clear()                           # 清除字典
y.update({1: "1"})                  # 更新元素
y.get('c', 'no')                    # 如没有c则输出没有
y.setdefault('c', 'code')           # 如果没有c则输出code并写入字典
y.items()                           # 输出键值对
y.keys()                            # 输出键
y.values()                          # 输出值、
len(y)                              # 判断键值对长长度
list(y)                             # 仅获取键列表
'c' in y                            # 是否在字典中
e = iter()
next(e)                             # 迭代器，执行一次输出一个值
```

### 判断 IF

```python
x = 1
if x < 0:
    x = 0
    print("nav")
elif x == 0:
    print("loop")
else:
    print("book")

print("book!") if 3>1 else print("hello!")
```

### 循环 for

```python
for i in range(0,10,2):                     # enumerate()函数
    print(i)


users = {'Hans': 'active', 'Éléonore': 'inactive', '景太郎': 'active'}
for user, status in users.copy().items():   # Strategy:  Iterate over a copy
    if status == 'inactive':
        del users[user]

active_users = {}                           # Strategy:  Create a new collection
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```

### 循环 while

```python
i =1
while(i <=5):
    play = int(input("请输入：石头1；剪刀2；布3；"))
    cpt=random.randint(1, 3)
    if ((play == 1) and (cpt ==2) or ((play ==2) and(cpt ==3)) or ((play == 3) and(cpt == 1))):
        print("你真是太棒了！")
        if i<=5:
            print("你赢了%d" % i)
        i+=1
    elif play == cpt:
        print("再来一次吧！")
    else:
        print("你真可怜，再来一次吧！")
  
i =1
num=0
while i<=100:
    if i%2 !=0:
        num = num +i
    i+=1
print("100之间的偶数相加=%d" % num)

i = 1
while i <=5:
    j = 1
    while j <=i:
        print( "*" ,end=" " )
        j+=1
    print()
    i+=1

i = 0
while i<10:
    print("nihao")
    i += 1
else:
    print("wobuhao")            # 但while不为真时执行

i =0
while i< len("abcd"):
    print("abcd"[i])
    i += 1
for i in range(1,5,2):
    print(i)
for i in range(1,5,-2):         # 倒序输出
    print(i)
```

### 判断 match

```python
def http_error(status):
    match status:
        case 402 | 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the internet"
```

### 函数

```python
# 函数
def fib(n):
    """Print a Fibonacci series up to n."""     # 函数第一个字符串为文档字符串,即docstring,用于描述函数作用等
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()
fib(123)

# 定义函数默认值
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        reply = input(prompt)
        if reply in {'y', 'ye', 'yes'}:
            return True
        if reply in {'n', 'no', 'nop', 'nope'}: 
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)

# 函数共享值
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))

def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L

# 关键字参数(kwarg=value)
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
parrot(1000)                                          # 1 positional argument,不能对同一个参数多次赋值
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword

def cheeseshop(kind, *arguments, **keywords):           # *name为元组形式,**name为字典形式,*name必须在**name前面
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw, ":", keywords[kw])
cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")

# 函数特殊参数
def combined_example(pos_only, /, standard, *, kwd_only):
    print(pos_only, standard, kwd_only)
def kwd_only_arg(*, arg):                                   # 仅关键字参数
    print(arg)
def pos_only_arg(arg, /):                                   # 仅位置参数
    print(arg)
def standard_arg(arg):                                      # 位置或关键字参数均可
    print(arg)
```

### 解包实参列表

```python
args = [3, 6]
list(range(*args))

def parrot(voltage, state='a stiff', action='voom'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.", end=' ')
    print("E's", state, "!")
d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
parrot(**d)
```

### 闭包

```python
def func6():
    x = 888

    def func61():
        print(x)
    return func61

func62 = func6()
func62()

def func7():
    x = 0
    y = 0

    def inner(x1, y1):
        nonlocal x, y  # x,y的值会传递给上层函数，保存起来，形成记忆作用
        x += x1
        y += y1
        print(x, y)
    return inner
func8 = func7()
func8(1, 2)
func8(1, 2)
```

### 异常

```python
try:
    1 / 0
except ZeroDivisionError as e:              # 将错误信息交给e
    print(e)

try:
    pass
except (ZeroDivisionError, ValueError):     # 多个异常同时处理 
    pass

try:
    pass
except ZeroDivisionError:                   # 多个异常分别处理
    pass
except ValueError:
    pass

try:
    1 / 0
except:
    print("抓住")
else:
    print("没抓住")

try:
    1 / 0
except:
    print("抓住")
else:
    print("没抓住")
finally:                            # 主要用于收尾工作
    print("抓没抓住都有")

raise ValueError("0")               # 自定义错误类型

s = "fin"
assert s == "fin"                   # 判断异常，没有无值，有则报错

try:
    raise NameError('HiThere')
except NameError:
    print('An exception flew by!')
    raise                           # raise 直接触发异常
```

### 文件路径操作

```python
from pathlib import Path

p = Path.cwd()                      # 获取当前工作路径
p = p / "ac.txt"                    # 拼接路径

f = open("a.txt", 'w')
f.write("asd")                      # 写入
f.writelines(["1", '2'])            # 按行写入
f.read()                            # 读取
f.readline()                        # 按行读取
f.flush()                           # 将缓存写入，未关闭文件的情况下
f.seek(0)                           # 修改文件指针位置，0起始，1当前位置，2文件末尾
f.truncate(10)                      # 截断操作，截取到10之前

p.is_dir()                          # 判断是否为文件夹
p.is_file()                         # 判断是否为文件
p.exists()                          # 判断文件/文件夹存在
p.name                              # 文件全称
p.stem                              # 文件名称不带后缀
p.suffix                            # 文件后缀
p.parent                            # 文件的父目录
p.parents                           # 文件的父目录们，返回值为集合
p.parts                             # 目录切片，返回值为元组
Path("./a.txt")                     # 相对路径
Path("./a.txt").resolve()           # 转换为绝对路径
Path.iterdir()                      # 当前文件夹中的文件及文件夹，返回集合
p.mkdir()                           # 创建文件夹
p.mkdir(exist_ok=True)              # 如存在则不创建文件夹
p.mkdir(parents=True, exist_ok=True)  # 创建级联目录，存在则不创建
p.rename("a.txt")                   # 重命名
p.replace("")                       # 移动文件并重命名
p.parent.rmdir()                    # 删除目录
p.unlink()                          # 删除文件
p.glob("*.txt")                     # 查找当前文件夹内容

with open("a.txt", "w") as f:       # with即便是出现错误也会写入
        f.write("hello!")


import pickle

a = ""
b = ""
c = ""
with open("a.pkl", "wb") as f:          # 将对象写入pkl文件，必须是wb二进制写入
        pickle.dump((a, b, c), f)       # 可用元组


with open("a.pkl", "rb") as f:          # 将对象读取到内存，必须是rb二进制读取
        a = pickle.load(f)
```

### 模块

```python
import datetime                                 # 导入模块
from datetime import time,datetime              # 导入模块的某个方法
from datetime import time as tm                 # 导入模块的某个方法
import datetime as timeA                        # 模块别名
import importlib; importlib.reload(modulename)  # 重载模块
dir()                                           # 列出模块包含的方法变量
import builtins; dir(builtins)                  # 列出内置函数
```

### 类

```python
class MyClass:
    """A simple example class"""                        # 文档字符串
    i = 12345                                           # 类变量

    def __init__(self, realpart, imagpart):             # 初始化函数
        self.r = realpart                               # 实例变量
        self.i = imagpart

    def f(self):                                        # 类方法
        return 'hello world'

print(MyClass.i)

"""
类对象支持两种操作：属性引用和实例化。

属性引用:    使用 Python 中所有属性引用所使用的标准语法: obj.name。 有效的属性名称是类对象被创建时存在于类命名空间中的所有名称。
实例化:      使用函数表示法。 可以把类对象视为是返回该类的一个新实例的不带参数的函数。
"""

class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>

class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>

class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # private copy of original update() method

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # provides new signature for update()
        # but does not break __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```

### JSON

```python
import json
a = {1:"a",2:"b",3:"3"}
json.dumps({"c": 0, "b": 0, "a": 0}, sort_keys=True)
json.dumps("\"foo\bar")
json.dumps(['foo', {'bar': ('baz', None, 1.0, 2)}])
json.dumps([1, 2, 3, {'4': 5, '6': 7}], separators=(',', ':'))
json.dumps({'4': 5, '6': 7}, sort_keys=True, indent=4)

json.loads('["foo", {"bar":["baz", null, 1.0, 2]}]')
json.loads('"\\"foo\\bar"')

print(a)
```

### NameSpace

```python
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
# 注意，局部 赋值（这是默认状态）不会改变 scope_test 对 spam 的绑定。 nonlocal 赋值会改变 scope_test 对 spam 的绑定，而 global 赋值会改变模块层级的绑定。
# 而且，global 赋值前没有 spam 的绑定。
```

### 调用CMD命令

```python
import os

os.system('dir')                # 调用CMD执行命令，无返回结果
result = os.popen('dir')        # 调用CMD执行命令，返回结果
result.read()                   # 读取命令输出的结果

import subprocess

# 'subprocess.run' 为 'Popen' 的封装
subprocess.run(args, *, stdin=None, input=None, stdout=None, stderr=None, capture_output=False, shell=False, cwd=None, timeout=None, check=False, encoding=None, errors=None, text=None, env=None, universal_newlines=None, **other_popen_kwargs)

subprocess.Popen(args, bufsize=-1, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=True, shell=False, cwd=None, env=None, universal_newlines=None, startupinfo=None, creationflags=0, restore_signals=True, start_new_session=False, pass_fds=(), *, group=None, extra_groups=None, user=None, umask=-1, encoding=None, errors=None, text=None, pipesize=-1, process_group=None)

result = subprocess.run('dir',shell=True,capture_output=True,text=True,encoding='utf8')

# 'capture_output' 标准输出和错误输出捕获，不能与 'stdout' 、 'stderr' 同时使用, 'shell' 表示调用系统命令终端执行， 'text' 表示返回值为文本形式
result = subprocess.run(['net','user',],shell=True,capture_output=True,text=True)

result = subprocess.run('',shell=True,stdin=subprocess.PIPE,stdout=subprocess.PIPE,stderr=subprocess.PIPE,text=True)

# 返回值，须在subprocess中定义才可
result.stdout                   # 标准输出
result.stderr                   # 错误输出
result.returncode               # 执行返回码
result.check_returncode()       # 检查返回码，非0返回错误
out,err = result.communicate()  # 同时输出标准输出和错误输出

```