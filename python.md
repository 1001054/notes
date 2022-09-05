### 对象

对象由：标识、类型、value组成

本质：一个内存块，拥有特定的值，支持特定类型的相关操作。

1. 标识用于唯一标识对象，通常对应于对象在计算机内存中的地址。使用内置函数id(obj)可返回对象obj的标识。
2. 类型用于表示对象存储的 “数据” 的类型。类型可以限制对象的取值范围以及可执行的操作。可以使用type(obj)获得对象的所属类型。
3. 值表示对象所属的数据的信息。使用print(obj)可以直接打印出值。

### 引用

在Python中，变量是对象的引用。因为，变量存储的是对象的地址。变量通过地址引用了 “对象”。

变量位于栈内存，对象位于堆内存。

1. Python是动态类型语言：不需要显式的声明类型。根据变量引用的对象，解释器自动确定数据类型。
2. Python是强类型语言：每个对象都有数据类型，只支持该类型支持的操作。

### 标识符

用于变量、函数、类、模块的名称。

1. 区分大小写
2. 由字母、数字、下划线组成。不能由数字开头。
3. 不能使用关键字。
4. 以双下划线开头和结尾的名称通常有特殊含义，尽量避免这种写法。

规范：模块、包名和函数名为蛇形命名，类名为驼峰且首字母大写，常量名都为大写。

### 删除变量和垃圾回收机制

可以通过 del 语句删除不再使用的变量

```python
a = 123
del a
```

如果对象没有变量引用，就会被垃圾回收器回收，清空内存空间。

### 链式赋值

```python
x = y = 123
```

### 系列解包赋值

```python
a,b,c = 4,5,6
#变量交换
a,b = b,a
```

### 常量

Python不支持常量，即没有语法规则限制一个常量的值，我们只能约定常量的命名规则，以及在程序的逻辑上不对常量的值做出修改。

### 运算符

1. / ：浮点数除法
2. //：整数除法
3. **：幂

### 三种进制

1. 二进制：0b或0B
2. 八进制：0o或0O
3. 十六进制：0x或0X

### 类型转换

1. 整数转换：int(3.14)、float(1)
2. 自动转型：整数和浮点数混合运算时，表达式结果自动转型成浮点数。
3. 整数四舍五入：round(value)

（所有的转换和运算都不会改变原有的值，而是产生新的值）

### 逻辑运算符

1. （短路）逻辑或：or
2. （短路）逻辑与：and
3. 逻辑非：not

### 同一运算符

1. is：判断两个标识符是不是同一个对象
2. is not：判断两个标识符是不是引用不同的对象

### 整数缓存

Python仅仅对比较小的整数对象进行缓存。

```python
a = 100
b = 100
#此时a和b的地址相同
a is b 
```



1. 命令行：（范围是 [-5, 256]）
2. 文件或Pycharm：（范围是 [-5, 任意正整数]) 这是因为解释器做了一部分优化。

### 字符串编码

Python3直接支持Unicode，可以表示世界上任何书面语言的字符。Python3的字符默认就是16位Unicode编码，ASCII码是Unicode编码的子集。

1. 使用内置函数ord()可以把字符转换成对应的Unicode码
2. 使用内置函数chr()可以把十进制数字转换成对应的字符

### 引号创建字符串

1. 可以通过单引号或双引号创建字符串。

   ``` python
   a = "I'm a boy."
   b = 'He is a "BOY".'
   ```

   

2. 连续三个单引号或三个双引号，可以创建多行字符串。

   ```python
   resume = '''name="Tom"
   age=3
   address="Sun street"
   '''
   ```

### 转义字符

| 转义字符     | 描述             |
| ------------ | ---------------- |
| \\(在行尾时) | 续行符           |
| \\\\         | 反斜杠符号       |
| \\'          | 单引号           |
| \\"          | 双引号           |
| \\b          | 退格 (Backspace) |
| \\n          | 换行             |
| \\t          | 横向制表符       |
| \\r          | 回车             |

### 字符串拼接

``` python
#用加号
'a' + 'b'
#直接放在一起
'a' 'b'
#字符串复制
'a'*3
```

### 不换行打印

print方法会自动打印一个换行符，如果不想换行，可以加end参数。

```python
print("aa", end="")
print("bb", end="")
print("cc")
```

### 从控制台读取字符串

```python
name = input("请输入姓名：")
```

### 转型字符串

```python
#将数字转换位字符串
str(3.14)
```

### 从字符串中提取字符

字符串的本质是字符序列，可以通过在字符串后面添加 [ i ] , ( i 为索引位置)，提取字符。索引也可以为负数，此时索引为从右往左数。

### 字符串切片slice

格式为：string [起始偏移量 : 终止偏移量 : 步长]

1. 步长省略为1，string [0:] 终止偏移量省略到最后，string [:-1] 起始偏移量省略从头开始。都省略，只剩一个冒号为提取整个字符串。

2. string [::-1]，步长为负，从右到左反向提取。
3. 若起始偏移量和终止偏移量不在 [0, length-1] 这个范围，也不会报错。小于0当作0，超出范围则取最后一个索引。

### 字符串分割和合并

1. split() 可以基于指定分割符将字符串分割成多个子字符串（存储到列表中）。如果不指定分割符，则默认使用空白字符（换行符/空格/制表符）

   ```python
   a = "abc drf cc"
   a.split()
   ```

2. join() 的作用和split() 作用刚好相反，用于将一系列子字符串连接起来。

   ```python
   a = ['ad', 'sdf', 'sdf']
   ''.join(a)
   ```

3. 使用 + ，会生成新的字符串对象，因此不推荐使用。而 join 函数在拼接前会计算所有字符串的长度，仅新建一次对象。

### 字符串驻留机制和字符串比较

字符串驻留：相同的字符串，只会被存放在字符串驻留池中一次。（只支持那些仅包含下划线、字母和数字的字符串）

``` python
a = "abd"
b = "abd"
#true
a is b

c = "ab&"
d = "ab&"
#false
c is d
```

### 常用查找方法

```python
#字符串长度
len(a)

#是否以参数开头
a.startswith("abc")

#是否以参数结尾
a.endswith("abv")

#第一次出现指定字符串的位置
a.find('a')

#最后一次出现指定字符串的位置
a.rfind('a')

#参数出现的次数
a.count("a")

#所有字符全是字母或数字
a.isalnum()

#去除字符串首尾指定信息，无参去空格
a.strip()

#首字母大写
a.capitalize()

#每个单词首字母大写
a.title()

#所有字符全大写
a.upper()

#所有字符全小写
a.lower()

#所有字符大小写转换
a.swapcase()

#字符串排版
a="abc"
a.center(10,"*")      #'***abc****'
a.center(10)          #'   abc    '
a.ljust(10,"*")       #'abc*******'

#是否都是字母或汉字
isalpha()

#是否都是数字
isdigit()
```

### 字符串格式化

```python
a = "名字是：{0}，年龄是：{1}，请{0}好好吃饭"
a.format("Jack", 18)

b = "名字是：{name}，年龄是：{age}"
b.format(name='Jack', age=18)
```

### 填充于对齐

^  <  > 分别是居中、左对齐、右对齐

冒号后面带填充的字符，只能是一个字符，不指定的话默认是用空格填充。

```python
"{:*>8}".format("235")
"I'm {0}, my phone number is {1:*^8}".format("Jack", "666")
```

### 数字格式化

1. {:.2f}：保留小数点后两位
2. {:+.2f}：带符号保留小数点后两位
3. {:.0f}：不带小数
4. {:0>2d}：数字补零（填充左边，宽度为2）
5. {:,}：以逗号分割的数字格式
6. {:.2%}：百分比格式
7. {:.2e}：指数记法

### 可变字符串

在Python中，字符串属于不可变对象，不支持原地修改，如果需要修改其中的值，只能创建新的字符串对象，或使用io.StringIO对象或array模块。

```python
import io
s = "Helloworld!"
sio = io.StringIO(s)
sio.getvalue()
#使指针指向第九个字符
sio.seek(9)
#替换字符
sio.write(".")
```

### 位运算符

```python
a = 0b11001
b = 0b01000
#按位或
c = a|b
#按位异或
c = a^b
#按位与
c = a&b
#位移
a<<2
```

### 序列

一种数据存储方式，用来存储一系列的数据。在内存中，序列就是一块用来存放多个值的连续内存空间。因为这些数据都是对象，所以序列中存储的实际上是这些对象的地址。

### 列表

1. 字符串和列表都是序列类型

2. 列表用于存储任意数量、任意类型的数据集合。

3. 列表是内置可变序列，是包含多个冤死的有序连续的内存空间。

4. 这些元素可以各不相同，可以是任意类型。

   ``` python
   #基本语法创建
   a = ["Jack", 13]
   #list()创建
   #使用list()可以将任何可迭代的数据转化成列表
   a = list()
   b = list(range(10))
   c = list("Hello")
   #推导式创建列表
   d = [x*2 for x in range(5) if x%2==0]
   ```

   

### range()创建整数列表

格式为：range(start, end, step)

1. start默认为0，step默认为1，负数为逆序。

2. 该方法返回的是一个range对象，而不是列表，通常使用list()方法将其转换成列表对象。

### 列表元素的增加与删除

当列表增加和删除元素时，列表会自动进行内存管理，减少了程序员的负担，但列表元素会大量移动，效率较低。如果只在列表尾部添加元素或删除元素，会大大提高列表的操作效率。

```python
a = [20, 30]
#在列表尾部添加新的元素
a.append(40)
#并不是真正的添加元素，而是创建了新的列表对象
a = a + [50, 60]
#将目标列表所有元素添加到本列表尾部，原地操作，不创建新的列表对象
a.extend([70, 80])
#将指定元素插入到列表对象的任意指定位置
a.insert(0, 10)
#乘法扩展
a = a * 3
#删除列表指定位置的元素
del a[0]
#删除并返回指定位置的元素，可以有参数
a.pop()
#删除首次出现的指定元素，若不存在该元素抛出异常
a.remove(20)
```

### 元素的访问

``` python
#可以通过索引直接访问元素，若超出范围则抛出异常
b = a[0]
#获得指定元素首次出现的索引位置
#也可以添加参数index(value, start)或index(value, start, end)
a.index(40)
#获得指定元素在列表中出现的次数
a.count(20)
#列表长度
len(a)
```

### 成员资格判断

判断列表中是否存在指定的元素，可以使用count()来返回元素个数，也可以使用 in 关键字。

``` python
a = [10, 20]
20 in a        #true
```

### 列表的遍历

```python
a = [1, 2, 3]
for x in a:
    print(x)
```

### 列表排序

1. 修改原列表，不新建列表的排序

   ```python
   a = [2, 1, 4, 5]
   #升序排序
   a.sort()
   #降序排序
   a.sort(reverse=True)
   #打乱顺序
   random.shuffle(a)
   ```

   

2. 建新列表的排序

   ```python
   #默认升序
   b = sorted(a)
   #降序
   c = sorted(a, reverse=True)
   ```

   

### reversed()返回迭代器

内置函数reversed()也支持进行逆序排列，与列表对象reverse()方法不同的是，内置函数不对原列表做任何修改，只是返回一个逆序排列的迭代器对象。

```python
a = [1, 2, 3]
b = reversed(a)
list(b)           #[3, 2, 1]
list(b)           #[]    只能迭代一次
```

### 最大值和最小值

```python
a = [1, 2, 3]
max(a)            #3
min(a)            #1
```

### 元组tuple

列表属于可变序列，可以任意修改列表中的元素。元组属于不可变序列，不能修改元组中的元素。

```python
#通过()创建元组，小括号可以省略
a = (1, 2, 3)
a = 1, 2, 3
#如果元组只有一个元素，则必须后面加逗号，这是因为解释器会把(1)解释为整数1，而(1,)解释为元组。

#通过内置函数，本质上是将其他类型对象转换为了typle对象
a = tuple()
a = tuple("abc")
a = tuple(range(3))
a = tuple([1, 2, 3])

#元组的删除
del b

#排序生成新的列表对象
sorted(a)
```

### zip

zip(列表1，列表2，...) 将多个列表对应位置的元素组合成为元组，并返回zip对象

```python
a = [1, 2, 3]
b = [4, 5, 6]
c = zip(a, b)
list(c)     #[(1, 4), (2, 5), (3, 6)] 

```

### 生成器推导式创建元组

生成器推导式生成的是一个生成器对象。可以将其转化为列表或者元组。可以使用该对象的\_\_next\_\_() 方法进行遍历，或者直接作为迭代器来使用。

``` python
s = (x*2 for x in range(5))
tuple(s)       #(0, 2, 4, 6, 8)
list(s)        #[] 因为生成器只能使用一次
```

### 元组总结

1. 核心特点：不可变序列
2. 访问速度和处理速度比列表快
3. 与整数和字符串一样，元组可以作为字典的健，列表则不能

### 字典

字典是 “键值对” 的无序可变序列。字典中每个对象都是一个 “键值对” ，包括 “键对象” 和 “值对象” 。

"健" 是任意不可变的数据，比如：整数、浮点数、字符串、元组。

”值“ 可以是任意的数据，并且可以重复。

### 字典的创建

```python
a = {'name':'Jack', 'age':18}
b = dict(name='Jack', age=18)
c = dict([("name","Jack"), ("age",18)])

#通过zip()创建字典
k = ["name", "age"]
v = ["Jack", 18]
a = dict(zip(k, v))

#通过fromkeys创建值为空的字典
a = dict.fromkeys(['name', 'age'])
#{'name':None, 'age':None, 'job':None}
```

### 字典元素的访问

1. 通过键获得值，若键不存在，则抛出异常。

   ```python
   name = a['name']
   ```

   

2. 通过 get() 方法获得 “值” 。若键不存在，返回None。

   ```python
   name = a.get('name')
   #若不存在，返回第二个参数
   name = a.get('name', '匿名用户')
   ```

3. 列出所有的键值对

   ```python
   a.items()
   ```

   

4. 列出所有的键，列出所有的值

   ```python
   a.keys()
   a.values()
   ```

   

5. len() 返回键值对的个数

6. 检测一个键是否在字典中

   ```python
   'name' in a
   ```

   

### 字典元素的添加、修改、删除

1. 新增（或覆盖） “键值对” 

   ```python
   a['address'] = 'sun street'
   ```

   

2. 使用 update() 将新字典中所有键值对全部添加（或覆盖）到旧字典对象上。

   ```python
   a = {'name':'Jack', 'age':'18'}
   b = {'name':'Joey', 'age':'33', 'sex':'male'}
   a.update(b)
   ```

   

3. 字典中元素的删除

   ```python
   #删除指定键值对
   del(a['name'])
   #删除所有键值对
   a.clear()
   #删除指定并返回对应的“值对象"
   age = a.pop('age')
   ```

   

4. 随机删除和返回该键值对。

   ```python
   random = a.popitem()
   ```

   

### 序列解包

序列解包可以用于元组、列表、字典。可以方便的对多个变量赋值。

```python
a, b, c = 1, 2, 3
(a, b, c) = (1, 2, 3)
[a, b, c] = [1, 2, 3]
```

序列解包用于字典时，默认时对”键“进行操作。

```python
a = {'name':'Jack', 'age':18}
#获取键
name_key, age_key = a
#获取值
name_value, age_value = a.values()
#获取键值对（返回一个元组）
name_pair, value_pair = a.items()

```

### 字典原理

字典对象的核心是散列表，即稀疏数组（总有空白元素的数组），数组的每个单元叫做bucket。每个bucket有两部分：一个是键对象的引用，一个是值对象的引用。

由于所有bucket结构和大小一致，所以可以通过偏移量来读取指定bucket。

用哈希算法将 ”键“ 算出哈希值，转换为二进制后。如果散列表有8个位置，就按照后三位映射，如果发生哈希碰撞，往前找三位。

### 字典总结

1. 键必须是可散列的
   1. 数字、字符串、元组，都是可散列的
   2. 自定义对象需要支持下面三点：
      1. 支持hash()函数
      2. 支持通过\_\_eq\_\_() 方法检测相等性
      3. 若 a\=\=b 为真，则hash(a)\=\=hash(b)也为真
2. 字典在内存中开销巨大，典型的空间换时间
3. 键查询速度很快
4. 往字典里面添加新键可能导致扩容，导致散列表中键的次序变化。因此，不能在遍历字典的同时进行字典的修改。

### 集合

无序可变，不可重复。底层是字典实现，所有的元素是字典中的 ”键对象“ 。

```python
#集合的创建
a = {1,2,3}
#添加
a.add(4)
#将其他类型转换为集合
b = [1, 2, 3]
set(b)
#删除指定元素
a.remove(1)
#清空整个集合
a.clear()
#并集
a|b
a.union(b)
#交集
a&b
a.intersection(b)
#差集
a-b
a.difference(b)


```

### 单分支选择结构

```python
if 条件表达式:
    语句/语句块
```

条件表达式中，空的列表或字符串是False，不能有赋值操作符。

### 双分支选择结构

```python
if 条件表达式:
    语句1/语句块1
else:
    语句2/语句块2
```

### 三元条件运算符

条件为真的值  if  (条件表达式)  else  条件为假的值

```python
a = input("Please input a number:")
print("less than 10" if int(a) < 10 else "more than 10")
```

### 多分支选择结构

```python
if 条件表达式1:
    语句1/语句块1
elif 条件表达式2:
    语句2/语句块2
else:
    语句块3/语句块3

```

### 选择结构的嵌套

选择结构可以嵌套，缩进量决定了代码的从属关系。

```python
if 表达式:
    语句1
    if 表达式2:
        语句2
    else:
        语句3
else:
    语句4
```

### while循环

```python
while 条件表达式:
    循环体语句
```

### for循环

```python
for 变量 in 可迭代对象:
    循环体语句
```

### 可迭代对象

1. 序列。包含：字符串、列表、元组
2. 字典
3. 迭代器对象（iterator)
4. 生成器函数（generator）

### 循环 else

while、for循环可以附带一个else语句（可选）。如果for、while语句没有被break语句结束，则会执行else子句，否则不执行。

```python
while 条件表达式:
    循环体
else:
    语句块

#或者
for 变量 in 可迭代对象:
    循环体
else:
    语句块
```

### zip() 并行迭代

zip() 函数可以对多个序列进行并行迭代，在最短序列 ”用完“ 时就会停止。

```python
names = ("Jack", "Jone", "Max")
ages = (18, 19, 20)

for name,age in zip(names, ages):
    print("name:{0}, age:{1}".format(name, age))
```

### 推导式创建序列

从一个或多个迭代器快速创建序列的一种方法。它可以将循环和条件判断结合，从而避免冗长的代码。

### 列表推导式

```python
[表达式 for item in 可迭代对象]
或者 [表达式 for item in 可迭代对象 if 条件判断]
#例
point = [(x, y) for x in range(10) for y in range(10)]
```

### 字典推导式

```python
{key_expression : value_expression for expression in iterator}
#例
text = "HelloWorld!"
char_count = {char : text.count(char) for char in text}
```

### 集合推导式

```python
{表达式 for item in 可迭代对象 if 条件判断}
```

### 生成器推导式（生成元组）

```python
(x for x in range(100) if x%9==0)
```

一个生成器只能运行一次，第一次迭代可以得到数据，第二次迭代发现数据已经没有了。

### 函数的分类

1. 内置函数

   在python中直接可以使用的函数，str()、list()、len()

2. 标准库函数

   通过import语句导入库，然后使用其中定义的函数

3. 第三方库函数

   Python社区中提供的库，这些库可以被安装，然后通过import语句导入

4. 用户自定义函数

   开发中适应用户自身需求定义的函数。

### 函数的定义和调用

```python
def 函数名(参数列表):
    '''文档字符串'''
    函数体/若干语句
```

1. Python执行 def 时，会创建一个函数对象，并绑定到函数名变量上。
2. 参数列表
   1. 圆括号内是形式参数列表，有多个参数则使用逗号隔开
   2. 形式参数不需要声明类型，也不需要指定函数返回值类型
   3. 无参数，也必须保留空的圆括号
   4. 实参列表必须与形参列表一一对应
3. return 返回值
   1. 如果函数体中包含return语句，则结束函数执行并返回值
   2. 如果函数体中不包含return语句，则返回None值
   3. 要返回多个返回值，使用列表、元组、字典、集合将多个值“存起来”即可。
4. 调用函数之前，必须要先定义函数，即先调用 def 创建函数对象
   1. 内置函数对象会自动创建
   2. 标准库和第三方库函数，通过import导入模块时，会执行模块中的 def 语句

### 文档字符串（函数的注释）

在函数体的开始部分附上函数定义的说明。要使用三个单引号或三个双引号。中间可以加入多行文字进行说明。

可以调用 help(函数名.\_\_doc\_\_) 可以打印输出函数的文档字符串。

### 变量的作用域

变量起作用的范围是作用域，不同作用域同名变量之间互不影响。

1. 全局变量：

   1. 在函数和类定义之外声明的变量，作用域为定义的模块，从定义位置开始直到模块结束。

   2. 全局变量降低了函数的通用性和可读性。应尽量避免全局变量的使用。

   3. 全局变量一般做常量使用。

   4. 函数内要改变全局变量的值，使用global声明一下

      ```python
      a = 200
      def function():
          global a        #如果要在函数内改变全局变量，增加此声明
          a = 300
      
      print(a)
      ```

      

2. 局部变量：
   1. 在函数体中声明的变量。（包含形式参数）
   2. 局部变量的引用比全局变量快，优先考虑使用
   3. 如果局部变量和全局变量同名，则在函数内隐藏全局变量，只使用同名的局部变量

```python
def function():
    print(locals())        #打印所有的局部变量
    print(globals())       #打印所有的全局变量
```

### 参数的传递

函数的参数传递本质上就是：从实参到形参的赋值操作。Python中，所有的赋值操作都是 “引用的赋值” 。所以，Python中赋值操作都是 “引用的赋值”， 参数的传递都是 “引用传递”， 不是 “值传递”。  

1. 对 “可变对象” 进行 “写操作”， 直接作用于原对象本身。
2. 对 “不可变对象” 进行 “写操作”， 会产生一个新的 “对象空间”，并用新的值填充这块空间，即原引用会指向一个新的对象。

可变对象有：字典、列表、集合、自定义的对象等

不可变对象有：数字、字符串、元组、function等

### 浅拷贝和深拷贝

内置函数分别是：copy(浅拷贝)、deepcopy(深拷贝)

1. 浅拷贝：不拷贝子对象的内容，只是拷贝子对象的引用
2. 深拷贝：会连子对象的内存也全部拷贝一份，对子对象的修改不会影响源对象。

传递的参数是不可变对象时，实际上传递的是对象的引用。但在 “写操作” 时，会创建一个新的对象拷贝，这个拷贝使用的是 “浅拷贝” ，不是 “深拷贝”。

### 参数的类型

1. 位置参数

   函数调用时，实参默认按位置顺序传递，需要个数和形参匹配。

2. 默认值参数

   某些参数可以设置默认值，这样这些参数在传递时就是可选的。默认值参数必须放在位置参数后面。

   ```python
   def function(a, b, c = 10):
       print(a, b, c)
   ```

   

3. 命名参数

   ```python
   def function(a, b, c):
       print(a, b, c)
   
   function(c = 1, b = 2, a = 1)
   ```

   

4. 可变参数

   可变数量的参数。分为两种情况：

   1. *param（一个星号），将多个参数收集到一个 “元组” 对象中。

   2. **param（两个星号），将多个参数收集到一个 “字典” 对象中。

      ```python
      def function(a, b, *c):
          print(a, b, c)
          
      function(1, 2, 3, 4)
      
      def function2(a, b, **c):
          print(a, b, c)
          
      function2(1, 2, name = "Jack", age = 19)
      ```

      

5. 强制命名参数

   在带星号的 “可变参数” 后面增加新的参数，必须时 “强制命名参数”。（因为可变参数的数量不确定）

### lambda表达式

可以用来声明匿名函数，lambda函数是一种简单的、在同一行中定义函数的方法。lambda函数实际上生成了一个函数对象。

lambda表达式只允许包含一个表达式，不能包含复杂语句，该表达式的计算结果就是函数的返回值。

基本语法：

```python
#运算结果是返回值
lambda arg1, arg2, arg3... : <表达式>

#例
function = lambda a, b, c : a + b + c
print(function(1, 2, 3))
```

### eval()函数

功能：将字符串str当成是有效的表达式来求值并返回计算结果。

语法：eval(source[,globals[,locals]]) -> value

参数：

1. source：一个Python表达式或函数compile() 返回的代码对象
2. globals：可选。必须是dictionary
3. locals：可选。任意映射对象。

```python
dic = dict(a = 100, b = 200)
d = eval("a + b", dic)
print(d)
```

### 递归函数

递归函数指的是：自己调用自己的函数，在函数体内部直接或间接的自己调用自己。每个递归都包含以下两个部分：

1. 终止条件

   表示递归什么时候结束，一般用于返回值，不再调用自己。

2. 递归步骤

   把第n步的值和第n-1步相关联

### 嵌套函数（内部函数）

在函数内部定义函数。

```python
def f1():
    print('f1 running...')
    
    def f2():
        print('f2 running...')
        
    f2()

f1()
```

f2定义在f1中，就只能在f1中调用。

应用场景：

1. 封装 - 数据隐藏

   外部无法访问 “嵌套函数”

2. 贯彻 DRY (Don't Repeat Yourself) 原则

   嵌套函数，可以让我们在函数内部避免重复代码。

3. 闭包

### nonlocal关键字

nonlocal  用来声明外层的局部变量

global      用来声明全局变量

### LEGB规则

Python在查找 “名称” 时，是按照 LEGB 规则查找的：

1. Local：指的是函数或者类的方法内部
2. Enclosed：指的是嵌套函数（一个函数包裹另一个函数，闭包）
3. Global：指的是模块中的全局变量
4. Build in：指的是Python为自己保留的特殊名称

### 类的定义

```python
class Student:
    
    def __init__(self, name, age):  #self必须位于第一个参数
        self.name = name
        self.age = age
        
	def print_age(self):
        print(self.age)
        

student = Student("Jack", 19) #调用构造函数
```

1. \_\_init\_\_() 方法：初始化创建好的对象，初始化指的是：给实例属性赋值
2. \_\_new\_\_() 方法：用于创建对象，==一般无需主动定义该方法==

3. 创建好的Python对象包含如下部分：
   1. id (identity 识别码)
   2. type (对象类型)
   3. value (对象的值)
      1. 属性（attribute)
      2. 方法（method)

### 实例属性

实例属性是从属于实例对象的属性，也称为 “实例对象”。他的使用有如下几个要点：

1. 实例属性一般在\_\_init\_\_() 方法中通过如下代码定义：

   self.实例对象 = 初始值

2. 在本类的其他实例方法中，也是通过self进行访问：

   self.实例属性名

3. 创建实例对象后，通过实例对象访问：

   object1 = 类名()     #创建对象，调用\_\_init\_\_() 初始化属性

   object1.实例属性名 = 值  #可以给已有属性赋值，也可以==新加属性==

### 实例方法

实例方法是从属于实例对象的方法。实例方法的定义格式如下：

```python
def 方法名(self, 形参列表):
    函数体
    

对象.方法名(实参列表)   #方法的调用格式
```

1. 定义实例方法时，第一个参数必须为self。和前面一样，self指当前的实例对象
2. 调用实例方法时，不需要也不能给self传参。self由解释器自动传参。

3. 实例对象的方法调用本质：

   ```python
   student = Student()
   student.print_age()
   #解释器翻译为：
   Student.print_age(student)
   ```

4. 函数是类外的语句块，方法是类内定义的语句块。

5. dir(object) 可以获得对象的所有属性、方法
6. object.\_\_dict\_\_ 对象的属性字典
7. pass 空语句
8. isinstance (对象，类型)  判断 “对象” 是不是 “指定类型”

### 类对象

在定义一个类后，解释器会创建一个类对象。

### 类属性

类属性从属于 “类对象” 的属性，也称为 “类变量” 。类属性从属于类对象，可以被所有实例对象共享。

```python
#类属性的定义方式
class 类名:
    类变量名 = 初始值
    
#例
class Student:
    school = "Sunk"
    count = 0
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        Student.count += 1
```

在类中或者类的外面，可以通过：“类名.类变量名” 来读写。

### 类方法

类方法从属于 “类对象” 的方法。类方法通过装饰器@classmethod来定义

```python
class Student:
    company = "Sunk"
    
    @classmethod
    def printCompany(cls):
        print(cls.company)
        
#方法调用
Student.printCompany()
```

1. @classmethod 必须位于方法上面一行
2. 第一个参数cls必须有；指的就是 “类对象” 本身
3. 类方法中访问实例属性和实例方法会导致错误
4. 子类继承父类方法时，传入cls是子类对象，而非父类对象。

### 静态方法

Python中允许定义与 "类对象" 无关的方法，称为 “静态方法”

“静态方法” 和在模块中定义的普通函数没有区别，只不过 “静态方法” 放到了 “类的名字空间里面” ，需要通过 “类调用”

```python
class Student:
    
    @staticmethod
    def add(a, b):
        print("{0} + {1} = {2}".format(a, b, a + b))
        
        
#方法调用
Student.add(1, 1)
```

1. @staticmethod 必须位于方法上面一行
2. 静态方法中访问实例属性和实例方法会导致错误

### 析构函数和垃圾回收机制

用于实现对象被销毁时所需的操作，比如释放对象占用的资源，（例如：打开的文件资源、网络连接等）

Python实现自动的垃圾回收，当对象没有被引用时（引用计数为0)，由垃圾回收器调用\_\_del\_\_方法

也可以通过del语句主动删除对象，从而保证调用\_\_del\_\_方法，系统会自动提供\_\_del\_\_方法，一般不需要自定义析构方法

```python
class Student:
    def __del__(self):
        print("销毁对象：{0}".format(self))
       
#调用
student = Student()
del student
```

### \_\_call\_\_方法和可调用对象

定义了call方法的对象，称为 “可调用对象” ，即该对象可以像函数一样被调用。

```python
class SalaryAccount:
    def __call__(self, *args, **kwargs):
        print("print the salary")
        return 2000


s = SalaryAccount()
s()

```

### 没有重载方法

Python中，方法的参数没有类型，参数的数量也可以由可变参数控制。因此没有方法重载。

如果在类体中定义了多个重名的方法，只有最后一个方法有效。

### 方法的动态性

Python是动态语言，因此可以动态的为类添加新的方法，或者动态的修改类的已有方法。

```python
class Student:
    def work(self):
        print("work...")


def play(s):
    print("play...")


def work(s):
    print("work harder...")


Student.play = play
student = Student()
# 相当于Student.play(student)
student.play()
# 修改work方法
Student.work = work
student.work()


```

### 私有属性和私有方法（实现封装）

python对于类的成员没有严格的访问控制限制，关于私有属性和私有方法，有如下要点：

1. 通常约定，两个下划线开头的属性是私有的，其他的为公开的
2. 类的内部可以访问私有属性（方法）
3. 类外部不能直接访问私有属性（方法）
4. 类外部可以通过 “_类名\_\_私有属性（方法）名” 访问私有属性（方法）

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.__age = age

    def __work(self):
        print("work...")


student = Student("Jack", 19)
print(student.name)
print(student._Student__age)
student._Student__work()

```

### @property装饰器

将一个方法的调用方式变为 “属性调用”

```python
class Student:
    @property
    def school(self):
        return "Sunk"


student = Student()
print(student.school)
print(type(student.school))

```

### get 和 set

```python
class Student:
    def __init__(self, name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_name(self, name):
        self.__name = name


student = Student("Jack")
print(student.get_name())
student.set_name("Jone")
print(student.get_name())

```

```python
class Student:
    def __init__(self, name):
        self.__name = name

    @property
    def name(self):
        return self.__name

    @name.setter
    def name(self, name):
        self.__name = name


student = Student("Jack")
print(student.name)
student.name = "Jone"
print(student.name)

```

### 继承

```python
class 子类类名(父类1,父类2,...):
    类体
    
#例
class Person:
    def __init__(self, name):
        self.name = name

        
    def print_name(self):
        print(self.name)
        
        
class Student(Person):
    def __init__(self, name, school):
        #必须显式的调用父类初始化方法，否则解释器不会调用
        Person.__init__(self, name)
        self.school = school
        

    #重写方法
    def print_name(self):
        print("My name is {0}.".format(self.name))
        
    
#打印继承的层次结构
print(Student.mro())
```

### object类

1. __str\_\_() 方法

   用于返回一个对于 “对象的描述” ，对应于内置函数 str()，经常用于 print()方法，查看对象的信息。__str\_\_() 可以重写。

   ```python
   class Student:
       def __init__(self, name):
           self.name = name
           
       def __str__(self):
           return "学生：{0}".format(self.name)
       
   student = Student("Jack")
   print(s)
   ```

   

2. MRO() 方法

   Python支持多继承，如果父类中有相同名字的方法，在子类中没有指定父类名时，解释器将 “从左向右” 按顺序搜索。

   MRO (Method Resolution Order): 方法解析顺序。

3. Super() 方法

   在子类中，如果想要获得父类的方法时，可以通过 super() 来做

   super() 代表父类的定义，不是父类对象。

### 多态

```python
class Animal:
    def move(self):
        print("Moving...")
        
class Bird(Animal):
    def move(self):
        print("Flying...")
        
class Frog(Animal):
    def move(self):
        print("Jumping...")
        
def do(p):
    if isinstance(p, Person):
        p.eat()
    else:
        print("Can not move...")
```

### 特殊方法

| 方法                   | 说明       | 例子           |
| ---------------------- | ---------- | -------------- |
| __init\_\_             | 构造方法   | p = Person()   |
| __del\_\_              | 析构方法   | 对象回收       |
| __repr\_\_,\_\_str\_\_ | 打印，转换 | print(a)       |
| __call\_\_             | 函数调用   | a()            |
| __getattr\_\_          | 点号运算   | a.xxx          |
| __setattr\_\_          | 属性赋值   | a.xxx = value  |
| __getitem\_\_          | 索引运算   | a[key]         |
| __setitem\_\_          | 索引赋值   | a[key] = value |
| __len\_\_              | 长度       | len(a)         |

```python
#加号运算符重载
class Student:
    def __init__(self, name):
        self.name = name
        
    def __add__(self, other):
        if isinstance(other, Person):
            return "{0} and {1}".format(self.name, other.name)
        else:
            return "Can not add..."
        
        
student1 = Student("Jack")
student2 = Student("Jone")
print(student1 + student2)
```

### 特殊属性

Python对象中包含了很多双下划线开始和结束的属性，这些是特殊属性，有特殊用法。

| 特殊方法                 | 含义                   |
| ------------------------ | ---------------------- |
| obj.__dict\_\_           | 对象的属性字典         |
| obj.__class\_\_          | 对象所属的类           |
| class.__bases\_\_        | 类的基类元组（多继承） |
| class.__base\_\_         | 类的基类               |
| class.__mro\_\_          | 类的层次结构           |
| class.__subclasses\_\_() | 子类列表               |

### 浅拷贝和深拷贝

1. 变量的赋值操作

   只是形成两个变量，实际还是指向同一个对象。

2. 浅拷贝

   Python 拷贝一般都是浅拷贝，拷贝时，对象包含的子对象内容不拷贝。因此，源对象和拷贝对象会引用同一个子对象

3. 深拷贝

   使用copy模块的deepcopy函数，会递归拷贝对象中包含的子对象。源对象和拷贝对象所拥有的子对象也不同。

### 工厂模式

工厂模式实现了创建者和调用者的分离，使用专门的工厂类将选择实现类、创建对象进行统一的管理和控制。

```python
class AnimalFactory:
    def create(self, kind):
        if kind == "cat":
            return Cat()
        elif kind == "dog":
            return Dog()
        else:
            return "Unknown Animal kind"
        
class Cat:
    pass

class Dog:
    pass


factory = AnimalFactory()
cat = factory.create("cat")
dog = factory.create("dog")
```

### 单例模式

核心作用是确保一个类只有一个实例，并且提供一个访问该实例的全局访问点。

单例模式只生成一个实例对象，减少了对系统资源的开销。当一个对象的产生需要比较多的资源，如读取配置文件、产生其他依赖对象时，可以产生一个 “单例对象” ，然后永久驻留内存中，从而极大的降低开销。

```python
class Singleton:
    __obj = None
    __init_flag = True

    def __new__(cls, *args, **kwargs):
        if cls.__obj is None:
            cls.__obj = object.__new__(cls)

        return cls.__obj

    def __init__(self, name):
        if Singleton.__init_flag:
            self.name = name
            Singleton.__init_flag = False


#实际上是相同的对象
a = Singleton("A")
b = Singleton("B")
print(a)
print(b)

```

### 异常处理

```python
try:
    被监控的可能引发异常的语句块
except Exception1 as e:
    异常处理语句块
except Exception2 as e:
    异常处理语句块
except BaseException:
    处理可能遗漏的异常
    
#例
while True:
    a = input("Please input your num:")
    try:
        if a.isdigit():
            if a == "88":
                break
        else:
            raise Exception("Is is not all numbers...")
    except Exception as e:
        print(e) 


```

### try...except...else结构

如果try块中没有抛出异常，则执行else块。如果抛出异常，则执行except块，不执行else块。

```python
try:
    a = input("a: ")
    b = input("b: ")
    c = float(a)/float(b)
except Exception as e:
    print(e)
else:
    print("a / b = ", c)
```

### try...except...finally结构

finally块无论是否发生异常都会被执行，通常用来释放try块中申请的资源。

### 常见异常

1. SyntaxError：语法错误
2. NameError：尝试访问一个没有申明的变量
3. ZeroDivisionError：除数为0错误（零除错误）
4. ValueError：数值错误
5. TypeError：类型错误
6. AttributeError：访问对象的不存在属性
7. IndexError：索引越界异常
8. KeyError：字典的关键字不存在

### with 上下文管理

with上下文管理可以自动管理资源，在with 代码块执行完毕后自动还原进入该代码之前的现场或上下文。不论何种原因跳出with块，不论是否有异常，总能保证资源正常释放。在文件操作、网络通信香港的场合非常常用。

```python
#语法结构
with context_expr [as var]:
    语句块
    
#例
with open("d:/bb.txt") as f:
    for line in f:
        print(line)
```

### traceback

使用Traceback模块打印异常信息

```python
import traceback

try:
    print("step1")
    num = 1 / 0
except:
    traceback.print_exc()
    
#例
try:
    print("step1")
    num = 1 / 0
except:
    #a为附加，w会覆盖，r为只读
    with open("d:/a.txt", "a") as f:
        traceback.print_exc(file = f)
```

### 自定义异常类

```python
class AgeError(Exception):
    def __init__(self, errorInfo):
        Exception.__init__(self)
        self.errorInfo = errorInfo
        
    def __str__(self):
        return str(self.errorInfo) + " Age is not correct. It should be between 1 and 150."
    
#如果为True，则模块是作为独立文件运行，可以执行测试代码
if __name__ == "__main__":
    age = int(input("Please input an age: "))
    if age < 1 or age > 150:
        raise AgeError(age)
    else:
        print("Age is : ", age)
```

### 文本文件和二进制文件

按文件中数据组织形式，文件可以分为文本文件和二进制文件两大类

1. 文本文件存储的是普通 ”字符“ 文本，python默认为unicode 字符集（两个字节表示一个字符），可以使用记事本程序打开（word不是文本文件）。
2. 二进制文件把数据内容用 ”字节“ 进行存储，无法用记事本打开。必须使用专用的软件解码。常见的有：MP4视频文件、MP3音频文件、JPG图片、doc文档等。

### 创建文件对象 open()

open() 函数用于创建文件对象，基本语法格式如下：

```python
#如果只是文件名，代表在当前目录下的文件
#也可以录入绝对路径
open(文件名, 打开方式)
#例
f = open(r"d:\b.txt", "a")
#r表示路径为原始字符串，可以减少 "\"的输入
#第二哥参数中没有b，默认表示是文本文件
```

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 读模式                                                       |
| w    | 写模式，如果文件不存在则创建；如果存在，则重新写内容         |
| a    | 追加模式，如果文件不存在则创建；如果文件存在，则在文件末尾追加内容 |
| b    | 二进制模式（可与其他模式组合使用）                           |
| +    | 读、写模式（可与其他模式组合使用）                           |

如果没有增加模式 ”b"，则默认创建的是文本文件对象，处理的基本单元是 “字符”。如果是二进制模式 “b" ，则创建的是二进制文件对象，处理的基本单元是 "字节"

### 文本文件的写入

写入步骤：

1. 创建文件对象
2. 写入数据
3. 关闭文件对象

```python
#例
f = open("a.txt", "a")
s = "Hello\nWorld\n"
f.write(s)
f.close()
```

### 文字编码

1. ASCII : 7位表示一个字符，最高位为0。只能表示128个字符。
2. ISO8859-1：8位表示1个字符，能表示256个字符。兼容ASCII。
3. GB***：兼容ISO8859-1.英文1个字节，汉字2个字节。
4. UTF-8：变长编码，1-4个字节表示1个字符；英文1个字节，汉字3个字节。
5. Unicode：定长编码，2个字节表示1个字符。与ISO不兼容。UTF-8相当于是Unicode的实现。

### 中文乱码问题

windows操作系统默认的编码是GBK，Linux操作系统默认是UTF-8。当我们用open()时，调用的是操作系统打开的文件，默认是GBK。

```python
#如果想以utf-8的方式写入
f = open(r"b.txt", "w", encoding = "utf-8")
f.write("Hello世界")
f.close()
```

### 文件的换行写入

```python
f = open(r"d:\bb.txt", "w", encoding = "utf-8")
s = ["Hello\n", "World\n"]
f.writelines(s)
f.close()
```

### close() 关闭文件流

由于文件底层是由操作系统控制，所以打开的文件对象必须显式调用 close() 方法关闭文件对象。当调用 close() 方法时，首先会把缓冲区数据写入文件（也可以直接调用 flush() 方法），再关闭文件，释放文件对象。

为确保打开的文件对象正常关闭，一般结合异常机制的 finally 或者 with 关键字实现无论何种情况都能关闭打开的文件对象。

```python
try:
    f = open(r"c.txt", "a")
    str = "HelloWorld!"
    f.write(str)
except BaseException as e:
    print(e)
finally:
    f.close()
    
    
#或者使用with，不论什么原有跳出with块，都能确保文件正确的关闭
#并且可以在代码块执行完毕后自动还原进入该代码块时的现场
s = ["Hello\n", "World\n"]
with open(r"d:\bb.txt", "w") as f:
    f.writelines(s)

```

### 文本文件的读取

文件的读取一般使用如下三个方法：

1. read([size])

   从文件中读取 size 个字符，并作为结果返回。如果没有 size 参数，则读取整个文件。读取到文件末尾，会返回空字符串。

2. readline()

   读取一行内容作为结果返回。读取到文件末尾，会返回空字符串。

3. readlines()

   文本文件中，每一行作为一个字符串存入列表中，返回该列表。

```python
#文件较小时，一次将文件内容读入到程序中
with open(r"c.txt", "r", encoding="utf-8") as f:
    s = f.read()
    print(s)

#按行读取一个文件
with open(r"c.txt", "r") as f:
    while True:
        fragment = f.readline()
        if not fragment:
            break
        else:
            print(fragment, end="")

#使用迭代器（每次返回一行）读取文本文件
with open(r"c.txt", "r") as f:
    for a in f:
        #print会自动加换行，所以将end设置为""
        print(a, end="")
        
#例
with open("c.txt", "r") as f:
    s = f.readlines()
    lines = [line.rstrip() + "  #" + str(index) + "\n" for index, line in enumerate(s)]

with open("d.txt", 'w') as f:
    f.writelines(lines)

```

 ### 二进制文件的赋值

```python
with open("threadPool.png", "rb") as r:
    with open("threadPool_copy.png", "wb") as w:
        w.writelines(r.readlines())

```

### 文件对象的常用方法

```python
seek(offset[,whence])
# 把文件指针移动到新的位置，offset 表示相对于whence的多少个字节的偏移量
# off为正往结束方向移动，为负往开始方向移动
# whence不同的值代表不同含义：
# 0：从文件头开始计算（默认值）
# 1：从当前位置开始计算
# 2：从文件尾开始计算
tell()
# 返回文件指针的当前位置
truncate([size])
# 不论指针在什么位置，只留下指针前 size 个字节的内容，其余全部删除
# 如果没有传入size，则当指针当前位置到文件末尾内容全部删除
```

### 使用pickle序列化

```python
#obj就是要被序列化的对象，file指的是存储的文件
pickle.dump(obj, file)
#从file读取数据，反序列化成对象
pickle.load(file)

#例
import pickle
with open(r"data.dat", "wb") as f:
    a = "Hello"
    b = 123
    c = [1, 2, 3]
    pickle.dump(a, f)
    pickle.dump(b, f)
    pickle.dump(c, f)
    
with open(r"data.dat", "rb") as f:
    a = pickle.load(f)
    b = pickle.load(f)
    c = pickle.load(f)
    print(a, b, c)

#序列化前和序列化后的对象id不同
```

### CSV文件的操作

CSV（Comma Separated Values) 是逗号分隔符文本格式，常用于数据交换、Excel文件和数据库数据的导入和导出。与Excel文件不同，CSV文件中：

1. 值没有类型，所有值都是字符串
2. 不能指定字体颜色等样式
3. 不能指定单元格的宽高，不能合并单元格
4. 没有多个工作表
5. 不能嵌入图像图表

```python
import csv

with open(r"file.csv", "r") as f:
    table = csv.reader(f)
    for row in table:
        print(row)
        
with open("table2.csv", "w", newline="") as f:
    table = csv.writer(f)
    table.writerow(["name", "age"])
    table.writerow(["JJJ", "29"])
    table.writerow(["KKK", "222"])
    
    
```

### os和os.path模块

os模块可以帮助我们直接对操作系统进行操作。可以直接调用操作系统的可执行文件、命令，直接操作文件、目录等。在系统运维的核心基础。

```python
#os.system调用windows系统的记事本程序
import os
os.system("notepad.exe")
#调用windows系统中的ping命令
os.system("ping www.baidu.com")
#直接调用可执行文件
os.startfile(r"C:\Program File\......")
```

### OS模块-文件和目录操作

可以通过文件对象实现对于文件内容的读写操作。如果，还需要对文件和目录做其他操作，可以使用os和os.path模块。

os模块下常用操作文件的方法

| 方法名            | 描述                           |
| ----------------- | ------------------------------ |
| remove(path)      | 删除指定文件                   |
| rename(src, dest) | 重命名文件或目录               |
| stat(path)        | 返回文件的所有属性             |
| listdir(path)     | 返回path目录下的文件和目录列表 |

os模块下关于目录操作的相关方法，汇总如下：

| 方法名                          | 描述                              |
| ------------------------------- | --------------------------------- |
| mkdir(path)                     | 创建目录                          |
| makedirs(path1/path2/path3/...) | 创建多级目录                      |
| rmdir(path)                     | 删除目录                          |
| removedirs(path1/path2...)      | 删除多级目录                      |
| getcwd()                        | 返回当前工作目录:current work dir |
| chdir(path)                     | 把path设为当前工作目录            |
| walk()                          | 遍历目录树                        |
| sep                             | 当前操作系统所使用的路径分隔符    |



### os.path模块

os.path模块提供了目录相关（路径判断、路径切分、路径链接、文件夹遍历）的操作

| 方法               | 描述 |
| ------------------ | ---- |
| isabs(path)        | 判断path是否绝对路径 |
| isdir(path)        | 判断path是否为目录 |
| isfile(path)       | 判断path是否为文件 |
| exists(path)       | 判断指定路径的文件是否存在 |
| getsize(filename)  | 返回文件的大小 |
| abspath(path)      | 返回绝对路径 |
| dirname(p)         | 返回目录的路径 |
| getatime(filename) | 返回文件的最后访问时间 |
| getmtime(filename) | 返回文件的最后修改时间 |
|walk(top,func,arg)| 递归方式遍历目录 |
|join(path,*paths)| 连接多个path |
|split(path)| 对路径进行分割，以列表形式返回 |
|splitext(path)| 从路径中分割文件的扩展名 |

```python
#打印出工作空间中所有py文件
path = os.getcwd()
py_names = [file for file in os.listdir(path) if file.endswith(".py")]
for name in py_names:
    print(name)
```

### walk()递归遍历所有文件和目录

返回一个3个元素的元组迭代器，（dirpath, dirnames, filenames)

1. dirpath: 要列出指定目录的路径
2. dirnames: 目录下所有文件夹
3. filenames: 目录下的所有文件

```python
#打印工作空间所有文件和目录的路径
import os

path = os.getcwd()
list_files = os.walk(path)

for (dirpath, dirnames, filenames) in list_files:
    for dirname in dirnames:
        print(os.path.join(dirpath, dirname))
    for filename in filenames:
        print(os.path.join(dirpath, filename))

```

### shutil模块（拷贝和压缩）

shutil模块是python标准库中提供的，主要用来做文件和文件夹的拷贝、移动、删除等。还可以做文件和文件夹的压缩、解压缩操作。

os模块提供了对目录或文件的一般操作。shutil模块作为补充，提供了移动、复制、压缩、解压等操作，这些os模块都没有提供。

```python
import shutil
import zipfile

shutil.copyfile("a.txt", "a_copy.txt")
shutil.copytree("movie_a", "movie_b")
shutil.copytree("movie/HongKong", "movie_b", ignore=shutil.ignore_patterns("*.txt", "*.html"))

#压缩
shutil.make_archive("movie_to/TT", "zip", "movie_from/GG")
#或
z1 = zipfile.ZipFile("d:/a.zip", "w")
z1.write("1.txt")
z1.write("2.txt")
z1.close()
#解压缩
x = zipfile.ZipFile("d:/a.zip", "r")
x.extractall("notes")
x.close()


```

### 模块

当导入一个模块时，模块中的代码都会被执行。不过，如果再次导入这个模块，则不会再次执行。

一个模块无论导入多少次，这个模块在整个解释器进程内有且仅有一个实例对象。

```python
from 模块名 import 成员1，成员2，...
#动态导入
s = "math"
m = __import__(s)
#以上方法在python2和python3中有差异，会导致意外错误。如果需要动态导入可以使用importlib模块
import importlib
a = importlib.import_module("math")
```

### 包package的使用

1. pycharm中创建包：在要创建包的地方单击右键：New-->Python package即可。pycharm会自动生成带有__init\_\_.py文件的包。
2. 导入包的本质其实是 “导入了包的__init\_\_.py” 文件。也就是说，“import pack1" 意味着执行了包 pack1 下面的 \_\_init\_\_.py 文件。这样，可以在 \_\_init\_\_.py 中批量导入我们需要的模块，而不再需要一个个的导入。
3. __init\_\_.py的三个核心作用：
   1. 作为包的标识，不能删除
   2. 用来实现模糊导入
   3. 导入包实质是执行__init\_\_.py文件，可以在在该文件中做这个包的初始化、以及需要统一执行代码。
4. 可以在init文件中定义__all\_\_变量，该变量是一个列表，所包含的模块会被 from package import * 所导入。

### sys.path 和模块搜索路径

解释器找某个模块文件的顺序：

1. 内置模块
2. 当前目录
3. 程序的主目录
4. pythonpath目录（如果已经设置了pythonpath环境变量）
5. 标准连结库目录
6. 第三方库目录（site-packages目录）
7. .pth文件的内容（如果存在的话）
8. sys.path.append()历史添加的目录
