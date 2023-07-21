$PythonNote\quad\text I$

## Python基础语法

### 字面量

字面量：在代码中被写下来的固定的值

| 类型             | 描述                            | 说明                                   |
| ---------------- | ------------------------------- | -------------------------------------- |
| 数字             | 支持`int, float, complex, bool` | 复数(complex)如`4+3j`，用j结尾表示复数 |
| 字符串(String)   | 描述文本                        | 由任意数量字符组成                     |
| 列表(List)       | 有序可变数列                    | 使用频繁，可以有序记录一堆数据         |
| 元组(Tuple)      | 有序不可变数列                  |                                        |
| 集合(Set)        | 无序不重复集合                  |                                        |
| 字典(Dictionary) | 无序键值对集合                  |                                        |

### 注释

```python
# 单行注释
print("hello world")
"""
多行注释
（三个双引号）
"""
```

### 变量

在程序运行时能储存计算结果或能表示值的抽象概念

```py
# 变量名 = 值
a = 1
print("a = ", a) # 打印内容的不同部分可用逗号隔开
a = a + 1
```

### 数据类型

验证数据类型可用`type`语句，并且可以用（字符串）变量存储该结果

```py
print(type(1)) # <class 'int'>
print(type("abc")) # <class 'str'>
a = 3.14
a_type = type(a)
print(a_type) # <class 'float'>
```

type语句查看的是数据的类型，因为**变量无类型**，但是其存储的数据有

### 数据类型转换

数据类型（数字、字符串等）在特定场景可以相互转换

常见转换语句

| 语句（函数） | 说明                        |
| ------------ | --------------------------- |
| `int(x)`     | 将x转换为一个整数           |
| `float(x)`   | 将x转换为一个浮点数         |
| `str(x)`     | 将**对象**x转换为一个字符串 |

这些语句均有返回值，可以打印输出或者用变量存储

```py
# num to str
num_str = str(11)
print(type(num_str), num_str) # <class 'str'> 11
# str to num
str_num = int("11")
print(type(str_num), str_num) # <class 'int'> 11
# 只有表示数字的字符串可以转换成数字，否则将报错
# 整数与浮点数之间可以相互转换，但浮点数转为整数会丢失小数部分
print(int(1.11)) # 1
```

### 标识符

包括变量名、方法名、类名等等

规则：

1. 只能出现英文、（中文、）数字和下划线，且不能以数字开头
2. 大小写敏感
3. 不能使用关键字

### 运算符

算数运算符

| 运算符 | 描述               |
| ------ | ------------------ |
| +      | 加                 |
| -      | 减                 |
| *      | 乘                 |
| /      | 除（结果为浮点数） |
| //     | 取整除             |
| %      | 取余               |
| **     | 指数               |

赋值运算符

| 运算符 | 描述       |
| ------ | ---------- |
| =      | 赋值       |
| +=     | 加法赋值   |
| -=     | 减法赋值   |
| *=     | 乘法赋值   |
| /=     | 除法赋值   |
| %=     | 取模赋值   |
| **=    | 幂赋值     |
| //=    | 取整除赋值 |

### 字符串

#### 三种定义方式

1. 单引号定义

   ```python
   name = 'string'
   ```

2. 双引号定义

   ```python
   name = "string"
   ```

3. 三引号定义

   ```python
   name = """string"""
   # 本质上和多行注释写法一样，内部支持换行，但用变量接收就是字符串
   ```

**字符串的引号嵌套**

- 单（双）引号定义法，可以内含双（单）引号
- 可以使用转义字符将引号解除效用，变成普通字符串

#### 拼接

使用+号连接字符串变量和字符串字面量

```python
str1 = "abc"
str2 = "def"
print(str1 + str2) # abcdef
# 字符串不能直接通过加号与其他类型数据（整数，浮点数等）进行拼接
```

#### 格式化

通过占位符实现字符串快速拼接

```python
name = "Tom"
age = 12
str1 = "My name is %s" % name
str2 = "My name is %s, I'm %s years old" % (name, age) # 数字被转换为字符串拼接
print(str1) # My name is Tom
print(str2) # My name is Tom, I'm 12 years old
```

常用占位符有`%s, %d, %f`。

==字符串格式化的数字精度控制==

在占位符`%m.nd`中：

- m表示宽度，要求是数字，设置宽度若小于数字自身则不生效

  注意：小数点与小数部分都会算入宽度计算

- .n用来控制小数点精度，要求是数字，会四舍五入

#### 快速格式化

通过语法`f"内容{变量}"`格式快速格式化

```python
name = "Tom"
age = 12
print(f"My name is {name}, I'm {age} years old.") # 数字不做精度控制，原样输出
```

**对表达式格式化**

表达式：一条具有明确执行结果的代码语句

在无需使用变量进行数据存储的时候，可以直接格式化表达式

```python
print("1 + 1 = %d" % (1+1))
print(f"1 + 1 = {1+1}")
```

### `input`语句输入

使用`input()`语句可以从键盘获取输入，使用一个变量接收input语句获取的键盘输入

```python
name = input() # 该语句总是默认接收字符串
# name = input("What's your name?")
# 提示信息可以直接输入到input()语句中
print(f"My name is {name}")
```



## 流程控制

### 判断语句

**布尔类型与比较运算符**

布尔类型：`true`和`false`

比较运算符：`==, !=, >, <, >=, <=`

- 通过比较运算可以得到布尔类型的结果

**`if-elif-else`语句**

```python
if age>=18:
    print("adult")
elif age>=12:
    print("teenager")
elif age>=3:
    print("child")
else:
    print("infant")
```

**==注意==**	对代码块的缩进不能有错，否则会报错或产生错误结果（因为没有大括号来确定代码块的范围和归属）

该条件分支判断语句可以嵌套。

**`not`与`and`关键字**

- not ：“非”，条件取反，相当于“`!`”
- and：“与”，相当于“`&&`”



### 循环语句

==循环语句均支持嵌套。==嵌套需要根据缩进确定层次。

#### `while`循环

```python
i = 1
while i<=10:
    print(i)
    i+=1
```

#### `for`循环

**基础语法**

```python
for 临时变量 in 待处理数据集（序列）:
    循环语句体
```

它**不能定义循环条件**，只能从被处理的数据集中依次取出内容处理。故`for`循环被称为**遍历循环**。

案例如下：

```python
name = "Harry Potter"
for x in name:
    print(x)
# 效果为name中每个字符逐行输出
```

循环中的临时变量的作用域为在**循环内**。

#### `range`函数

通过`range`可以获取一个简单的数字序列。

```python
range(num) # 表示从0开始到num结束的数字序列（不包含num）
range(num1, num2) # 从num1到num2（不包含num2）
range(num1, num2, step) # 从num1到num2按步长step获取数字序列（不包含num2本身）
```

**使用注意：**

1. 所有参数都必须是整形，不能给出浮点数序列
2. `start`参数省略时，`step`参数也必须省略
3. `step`小于1没有任何意义（不能为负数或0）
4. `range()`的返回值类型是`range`而不是列表，可以通过`list()`将其转化为列表

==关键字：`continue`和`break`==

- continue：直接进入下一次循环
- break：直接退出本层循环



## 函数

函数：组织好的，可重复使用的，用来实现特定功能的代码块

### 函数的定义

函数必须**先定义后使用**

```python
def 函数名(传入参数列表):
    """
    函数说明
    :param x: 形参x的说明
    :param y: 形参y的说明
    :return: 返回值的说明
    """
    函数体
    return 返回值
```

### None类型

如果函数没有用`return`语句返回数据，函数**仍有返回值**，其类型为`<class 'NoneType'>`，字面量为None

```python
def greet():
    print("hello...")
    # return None 可以主动返回none值
result = greet()
print(type(result)) # <class 'NoneType'>
```

在判断上，`None`等同于布尔类型的`false`，一般可以在函数中主动返回`None`，**配合`if`判断做相关处理**。也可以**定义变量**，但暂时不需要变量有具体值。

### 变量的作用域

```python
a = 1 		# 全局变量
def printA():
    i = 3	# 局部变量
    global g = 4 # 使用global关键字在函数内定义全局变量
    print(a) # 1
    print(i) # 3
    print(g) # 4
def printB():
    a = 2
    print(a) # 2
	# print(i) // error: i is not defined
    print(g) # 4
```



## 数据容器

一种可以容纳多种数据的数据类型，容纳的每一份数据为1个元素。每个元素可以是任意类型的数据。

数据容器根据特点不同，即：

- 是否支持重复元素
- 是否可修改
- 是否有序

等，可分为五类，分别是：列表(`list`)，元组(`tuple`)，字符串(`str`)，集合(`set`)，字典(`dict`)

### `list`列表

**特点**

1. 容纳多个（不同类型）数据
2. 数据有序存储
3. 允许重复，可以修改

**定义**

```python
变量名 = [elem1, elem2, ...]
# 定义空列表
变量名 = []
变量名 = list()
```

- 以`[]`作为标识
- 列表内每个元素之间逗号隔开

列表可以一次存储多个数据，并且**可以为不同数据类型，支持嵌套**

```python
name_list = ['str1', 'str2', 'str3']
print(name_list)		# ['str1', 'str2', 'str3']
print(type(name_list))	# <class 'list'>

my_list = ['str', 123, True, [1, 2, 3]]
print(my_list)
print(type(my_list))
```

**下标（索引）取出数据**

```python
name_list = ['Tom', 'Lily', 'Rose']
# 正向下标从0开始递增
print(name_list[0]) # Tom
# 下标可以反向标记，末尾元素下标为-1
print(name_list[-1]) # Rose
print(name_list[-2]) # Lily
# 嵌套列表
sub_list = [1,2,3]
my_list = [sub_list, [4,5,6]]
print(my_list[0][1]) # 2
print(my_list[1][1]) # 5
```

**常用方法**

- 注：当函数定义为类(`class`)的成员，则将函数称之为“方法”

```python
class Student:
	def add(self, x, y):
        return x + y
# 方法通过对象名调用
stu = Student()
num = stu.add(1, 2)
```

功能：

| 方法 | 作用                                                         | 语法                                    |
| ---- | ------------------------------------------------------------ | --------------------------------------- |
| 查找 | 查找传入的元素所在位置的下标。<br>若元素不存在则报错“`ValueError`” | `list.index(elem)`                      |
| 修改 | 修改特定位置的元素值                                         | `list[index] = val`                     |
| 插入 | 在指定下标位置插入指定元素                                   | `list.insert(index, val)`               |
| 追加 | 将指定元素添加到列表尾部                                     | `list.append(val)`                      |
|      | 将其他数据容器内容取出，依次追加到列表尾部*                  | `list.extend(list2)`                    |
| 删除 | 删除列表指定位置元素                                         | `del list[index]`<br>`list.pop(index)`* |
|      | 删除某元素在列表中的第一个匹配项*                            | `list.remove(elem)`                     |
| 清空 | 清空列表                                                     | `list.clear()`                          |
| 计数 | 统计列表某元素的数量*                                        | `list.count(elem)`                      |
| 长度 | 获取列表长度                                                 | `len(list)`                             |

```python
# 追加方式2
my_list = [1,2,3]
my_list.extend([4,5,6])
print(my_list) # [1, 2, 3, 4, 5, 6]

# 删除方式2
# pop方法可以返回被删除元素的值
my_list = [1,2,3]
elem = my_list.pop(1) # 2
# 删除方式3
my_list = [1,2,3,2,3]
my_list.remove(2)
print(my_list) # [1, 3, 2, 3]

# 计数
my_list = [1,1,1,2,3]
cnt = my_list.count(1) # 3
```

**遍历**

`while`循环

利用下标索引取出，可定义一个变量表示下标，循环条件为 下标值<列表元素数量

```python
i = 0
while i<len(my_list):
    elem = my_list[i]
    {elem处理}
    i += 1
```

`for`循环

```python
for elem in my_list:
    {elem处理}
```

细节：

- 循环控制
  - `while`循环可以**自定循环条件**并自行控制
  - `for`循环**不能自定循环条件**
- 无限循环
  - `while`可以通过条件控制做到无限循环
  - `for`理论上不可以，因为被遍历的容器容量不是无限的
- 使用场景
  - `while`适用于任何想要循环的场景
  - `fo`适用于遍历数据容器的场景或简单的固定次数循环场景

### `tuple`元组

**特点**

元组同list列表一样可以封装多个不同类型元素（并且允许元素重复），但**一旦定义完成就不可被修改**

**定义**

```python
变量名 = (elem1, elem2, ...)
# 定义空元组
变量名 = ()
变量名 = tuple() 
# 注意：元组中只有一个数据，要在这个数据后面单独加一个逗号
t1 = ('hello', )
print(type(t1)) # <class 'tuple'>
t2 = ('hello')
print(type(t2)) # <class 'str'>
```

**相关操作**

| 方法 | 作用               | 语法              |
| ---- | ------------------ | ----------------- |
| 查找 | 查找数据返回位置   | `tup.index(elem)` |
| 计数 | 统计某数据出现次数 | `tup.count(elem)` |
| 长度 | 获取元组长度       | `len(tup)`        |

注意事项：

- 不可对元组元素进行修改，否则报错（但是可以修改元组中嵌套的list中的数据）

### `str`字符串

字符串是字符的容器，一个字符串可以存放任意数量的**字符**，长度任意

字符串支持通过下标（索引）访问对应字符（反向下标可用）

和元组类似，字符串是**无法修改**的数据容器。

**常用操作**

| 方法 | 作用                                                         | 语法                               |
| ---- | ------------------------------------------------------------ | ---------------------------------- |
| 查找 | 查找元素                                                     | `str.index(elem)`                  |
| 替换 | 将字符串1的全部内容替换为字符串2<br>其中参数字符串1是调用该方法的字符串的子串<br>该方法产生新字符串，即该方法**存在返回值**，可以用其他变量接收 | `str.replace(str1,str2)`           |
| 分割 | 按照指定的分隔符字符串，将字符串划分成多个字符串，并存入**返回的列表对象** | `str.split(ls)`                    |
| 规整 | 去除字符串头尾的指定字符串，或空格与回车符（参数默认值）*    | `str.strip(str1)`<br>`str.strip()` |
| 计数 | 统计字符串中某子串出现的次数                                 | `str.count(sstr)`                  |
| 长度 | 获取串长                                                     | `len(str)`                         |

```python
# 规整
# 头尾部分的字符串任意某个字符，若是传入的字符串参数的子串即可被清除
my_str = "12hello world21"
my_list = my_str.strip("12")
print(my_list) # ['hello world']
```

### 序列

指内容连续、**有序，可使用下标索引**的一类数据容器

==常用操作：切片==

即从序列中取出一个子序列

**语法：`序列[起始下标:结束下标:步长]`**

- 步长为1可省略不写；起始、结束下标不写时默认为序列的头、尾处
- 取出的序列不含结束下标处的元素
- 步长可以用负数表示，此时反向取元素，起始与结束下标也要反向标记
- 该操作不会影响序列本身，而是产生一个新序列

```python
my_list = [0,1,2,3,4,5,6]
res1 = my_list[1:4] # [1, 2, 3]
res2 = my_list[1:6:2] # [1, 3, 5]
res3 = my_list[::-1] # 等于将原序列直接倒置
```

### `set`集合

**特点**

- 元素不能重复，且无序，故**不支持下标索引访问**
- 允许修改
- 可以容纳不同类型数据

**定义**

```python
变量名 = {elem, elem, ...}
# 空集合
变量名 = set()
```

**常用操作**

| 方法     | 作用                                                         | 语法                           |
| -------- | ------------------------------------------------------------ | ------------------------------ |
| 添加     | 将指定元素添加到集合中                                       | `set.add(elem)`                |
| 移除     | 将集合中指定的元素从集合中移除                               | `set.remove(elem)`             |
| 随机取出 | 从集合中**随机**取出一个元素，集合本身被修改，方法返回被取出的元素 | `set.pop()`                    |
| 清空     | 清空集合                                                     | `set.clear()`                  |
| 取差集   | 取出2个集合的差集（集合1有而集合2没有），返回新集合          | `set1.difference(set2)`        |
| 消除交集 | 在集合1中删除与集合2中相同的元素                             | `set1.difference_update(set2)` |
| 合并     | 将集合1，2合并新集合，返回二者并集（不修改集合1，2）         | `set1.union(set2)`             |
| 统计     | 统计集合中元素数量                                           | `len(set)`                     |

**遍历**

只能用for循环遍历

```python
for elem in set1:
    {处理elem}
```

### `dict`字典/映射

**特点**

- 同样用`{}`定义，但存储的元素是键值对
- 键值不允许重复，重复添加等于覆盖原有数据（即**可以修改**）
- 不能使用下标索引（无序），可以通过key值取得对应的value

**定义**

```python
my_dict = {
    key1: value1,
    key2: value2,
    key3: value3
}
# 从key获取value值
val = my_dict[key2] # value2
# 定义空字典
my_dict = {}
my_dict = dict()
```

key和value可以为任意数据类型（但key不可为字典），字典可以嵌套。

**常用操作**

| 方法      | 作用                                              | 语法              |
| --------- | ------------------------------------------------- | ----------------- |
| 添加/更新 | 添加/更新元素                                     | `dict[Key] = Val` |
| 删除      | 将字典指定键值对删除，返回指定key的value          | `dict.pop(Key)`   |
| 清空      | 清空字典                                          | `dict.clear()`    |
| 获取key   | 获取字典中所有的key<br>可以借助for对字典进行遍历* | `dict.keys()`     |
| 统计      | 获取字典键值对数量                                | `len(dict)`       |

```python
# 既可先获取所有key遍历，也可直接对字典遍历
for key in my_dict:
    {...}
#字典无法使用while遍历 
```

### 总结对比

|          | `list`列表 | `tuple`元组 | `str`字符串 | `set`集合 | `dict`字典 |
| -------- | ---------- | ----------- | ----------- | --------- | ---------- |
| 下标索引 | √          | √           | √           |           |            |
| 可重复   | √          | √           | √           |           |            |
| 可修改   | √          |             |             | √         | √          |
| 元素类型 | 任意       | 任意        | 仅字符      | 任意      | 键值对     |

|        | 使用场景                    |
| ------ | --------------------------- |
| 列表   | 一批数据，可修改、可重复    |
| 元组   | 一批数据，不可修改、可重复  |
| 字符串 | 一串字符                    |
| 集合   | 一批数据，**去重**存储      |
| 字典   | 一批数据，可通过ky检索value |

### 通用操作

1. `len(容器)`：统计元素个数

2. `max(容器)`：获取最大元素（数字和字符，下同）

3. `min(容器)`：获取最小元素

4. 通用类型转换：将给定容器转换成函数名对应的容器

   - `list(容器)`，`tuple(容器)`，`str(容器)`，`set(容器)`

     注意：字典转换成列表、元组、集合时只将所有键并在新容器中，值会丢失；但转换成字符串时可以保留

5. 排序：`sorted(容器, [reverse = True])`
   - 默认升序排序，传入`[reverse = True]`参数可以将所得排序结果倒置（即降序排列）
   - 返回值是列表对象



## 函数进阶

### 函数的多返回值

```python
def test_return():
    return 1, 2

x, y = text_return()
print(x) # 1
print(y) # 2

"""
按照返回值顺序，用对应顺序的多个变量接收即可
变量之间用逗号隔开，支持不同类型的数据返回
"""
```

### 函数的多种传参方式

根据**使用方式**上的不同，函数有4种常见参数使用方式：

- 位置参数
- 关键字参数
- 缺省参数
- 不定长参数

#### 位置参数

调用函数的时候根据函数定义的参数位置传递参数

```python
def user_info(name, age, gender):
    print(f'name: {name}, age: {age}, gender: {gender}')

user_info('Tom', 20, 'male')
```

注意：传递的参数和定义的参数顺序必须一致

#### 关键字参数

函数调用时通过“键 = 值”形式传递参数

作用：可以让函数更加清晰、容易使用，同时**清除了参数的顺序要求**

```python
def user_info(name, age, gender):
    print(f'name: {name}, age: {age}, gender: {gender}')

# 关键字传参
user_info(name='Tom', age=20, gender='male')
# 可以按照不同顺序
user_info(age=20, gender='male', name='Tom')
# 可以和位置参数混用，位置参数必须在前且匹配参数顺序。关键字参数之间不存在先后顺序
user_info('Tom', age=20, gender='male')
# 位置参数匹配的参数必须连续
"""
user_info('Tom', 'male', age=20) 
TypeError: user_info() got multiple values for argument 'age'
"""
```

#### 缺省参数

也叫默认参数，用于定义函数，为参数提供默认值，调用函数时可以不传递该默认参数的值（注意：所有位置参数必须出现在默认参数之前，包括函数定义和调用）

当调用函数时没有传递参数，就会默认使用缺省参数对应的默认值

```python
def user_info(name, age, gender='male'):
    print(f'name: {name}, age: {age}, gender: {gender}')

user_info('Tom', 20)
user_info('Rose', 19, 'female')
```

#### 不定长参数

也叫可变参数，用于不确定调用的时候会传递多少个参数（不传参也可以）的场景

类型：

- 位置传递

  ```python
  def user_info(*args):
      print(args)
      
  # ('Tom', )
  user_info('Tom')
  # ('Tom', 18)
  user_info('Tom', 18)
  ```

  注意：传进的**所有参数**都会被`args`变量收集，它会根据传进参数的位置合并为一个**元组**，`args`是元组类型

- 关键字传递

  ```python
  def user_info(**kwargs):
      print(kwargs)
      
  # {'name':'Tom', 'age':18, 'id':110}
  user_info(name='Tom', age=18, id=110)
  ```

  注意：参数是“键 = 值”形式的情况下，所有的“键 = 值”都会被`kwargs`接受，组成**字典**

### 函数作为参数传递

```python
def test_func(compute):
    result = compute(1,2)
    print(result)

def compute(x,y):
    return x+y

test_func(compute) # 3
```

函数本身可以作为参数传入另一个函数中进行使用。**作用在于传入计算逻辑而非传入数据**。

### lambda匿名函数

函数定义中

- `def`关键字可以定义带有名称的函数，函数可以基于名称重复使用
- `lambda`关键字可以定义匿名函数（无名称），函数**只能临时使用一次**

定义语法：

```python
lambda 传入参数: 函数体(一行代码)
```

- `lambda`是关键字，表示定义匿名函数
- 传入参数表示匿名函数的形参，如：`x,y`表示接收2个形参
- 函数体（就是函数的执行逻辑）**只能写一行**

```python
def test_func(compute):
    result = compute(1,2)
    print(result)

test_func(lambda x,y: x+y) # 3
```

