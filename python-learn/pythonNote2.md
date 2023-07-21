$PythonNote\quad\text{II}$

## Python文件操作

### 文件的编码

计算机只能识别0和1，文件使用编码技术（密码本）将内容翻译成0和1存入

编码技术即翻译规则，记录了如何将内容翻译成二进制，以及如何将二进制翻译回可识别内容

计算机中可用编码为：

- UTF-8，GBK，Big5等

不同编码将内容翻译成二进制也是不同的。

可以使用记事本查看文件的编码方式。UTF-8是目前全球通用的编码格式。

### 文件的读取

操作系统以文件为单位管理磁盘中的数据。文件可以分为：

- 文本文件、视频文件、音频文件、图像文件、可执行文件等多种类别

文件操作主要包括：

- 打开文件
- 读写文件
- 关闭文件（`close()`方法）

等步骤。

#### 打开：`open()`函数

使用`open`函数可以打开一个已经存在的文件或者创建一个新文件。

```python
open(name, mode, encoding)
```

- name：指打开的目标文件名的字符串（可以包含文件所在的具体路径）
- mode：设置打开文件的格式（访问格式）：只读“`r`”、写入“`w`”、追加“`a`”等
- encoding：编码格式（主要使用UTF-8）

```python
f = open('python.txt', 'r', encoding="UTF-8")
# encoding的顺序不是第三位，所以不能使用位置参数，用关键字参数直接指定
print(type(f)) # <class '_io.TextIOWrapper'>
```

- 注意：此时“`f`”是`open`函数的文件对象，拥有属性和方法，可以借助对象名进行访问。

#### 读操作相关方法

- `read()`方法

  ```python
  f = open('python.txt')
  f.read(num)
  ```

  `num`表示要从文件中读取的数据的长度。如果没有传入`num`就表示读取文件中的所有数据。

  `read()`方法执行时会有指针记录读取位置，下次使用该方法与`readlines()`方法时会从上次读取到的位置继续往后读

- `readlines()`方法

  ```python
  f = open('python.txt')
  content = f.readlines()
  print(type(content)) # <class 'list'>
  print(content) #['hello world\n', 'abcd\n', 'aaa\n']
  
  # 关闭文件
  f.close()
  ```

  可以按照行的方式把**整个文件的内容一次性读取**，返回一个**列表**，其中每一行的数据为一个元素

- `readline()`方法

  一次读取一行

  可以通过for循环读取文件行

  ```python
  for line in open('python.txt', 'r'):
      print(line)
  # 每一个line临时变量记录了文件的一行数据
  ```

- `with open`语法

  ```python
  with open('python.txt', 'r') as f:
      # f指对该打开文件的对象设置一个别名
      f.readlines()
  # 通过在with open语句块中对文件操作
  # 可以在操作完成后自动关闭文件，避免遗忘使用close方法
  ```

### 文件的写入/追加

```python
# 1.打开文件
f = open('python.txt', 'w')
"""
w模式下文件不存在会创建新文件；文件存在会清空原有内容
a模式下文件不存在会创建新文件；文件存在会追加写入，不清空原有内容
"""

# 2.文件写入
f.write('hello world')

# 3.内容刷新
f.flush()
```

注意：

- 直接调用`write`内容并未真正写入文件，而是会积攒在程序的内存中
- 当调用`flush`的时候内容才会真正写入文件
- 这样做的目的是避免频繁操作硬盘导致效率下降（攒一堆之后一次性写入）
- `close()`方法包含了`flush()`的功能，调用该方法时可以自动刷新保存



## 异常

### 捕获异常

对可能出现的bug进行提前准备、提前处理

重新出现异常有两种情况：

- 程序停止运行
- 对出现的异常进行提醒，整个程序正常运行

捕获异常的作用在于：提前假设某处可能出现异常，提前做好准备，当真的出现异常可以有后续手段进行不久

#### 捕获常规（所有）异常

基本语法：

```python
try:
    可能发生错误的代码
except:
    如果出现异常执行的代码
[else:]		# alternative
    如果不出现异常执行的代码
[finally:]	# alternative
    不论是否有异常都会执行
```

举例：

```python
try:
    f = open('abc.txt','r')
except:		# except Exception as e:
    print('FileNotFoundError')
    f = open('abc.txt','w')
else:
    content = f.readlines()
finally:
    f.close()
```

#### 捕获指定异常

基本语法：

```python
try:
    可能发生错误的代码
except Error1 as e:
    如果出现异常1执行的代码
except Error2 as e:
    如果出现异常2执行的代码
```

举例：

```python
try:
    print(name)
    1/0
except NameError as e:
    print('variable not defined')
except ZeroDivisionError as e:
    print('division by zero')
```

try块中如果出现异常，之后的代码不会再执行

#### 捕获多个异常

```python
try:
    可能发生错误的代码
except (Error1, Error2, ...) as e:
    如果出现异常执行的代码
```

### 异常的传递

```python
def func01():
    print('func01 start')
    num = 1/0
    print('func01 end')

def func02():
    print('func02 start')
    func01()
    print('func02 end')

def main():
    try:
        func02()
	except Exception as e:
        print(e)

main()
```

异常具有传递性。当函数`func01()`发生异常并没有对其捕获处理，异常会传递到`func02()`，若`func02()`也没有对其处理，异常将会传递到`main()`函数。

==当所有函数都没有捕获到异常时，程序报错。==



## 模块与包

### 模块

模块是一个python文件，能定义函数、类和变量，也可以包含可执行代码。

python有很多不同模块，每个模块都可以帮助我们快速实现一些功能。可以认为一个模块就是一个工具包，每个工具包都有不同工具供使用以实现不同功能。

#### 模块的导入

模块在使用前需要导入。模块与其功能通常通过“.”确定层级关系，导入语句一般写在开头位置。

```python
[from 模块名] import [模块|类|变量|函数|*] [as 别名]
```

**常用组合形式：**

- `import 模块名`

```python
import Module1
import Module2, Module3

Module1.method()
```

举例：

```python
import time

print('start')
time.sleep(1) # 通过“模块名.”就可以访问模块中的全部功能
print('end')
```

- `from 模块名 import 功能名（类、变量、方法）`

举例：

```python
from time import sleep

print('start')
sleep(1)
print('end')
```

- `from 模块名 import *`

  代表把模块的所有功能导入

举例：

```python
fron time import *
print('start')
sleep(5)	# 不需要通过模块名访问功能
print('end')
```

- `import 模块名 as 别名`
- `from 模块名 import 功能名 as 别名`

```python
# 模块定义别名
import 模块名 as 别名

# 功能定义别名
from 模块名 import 功能 as 别名
```

举例：

```python
# 模块别名
import time as tt
tt.sleep(2)
print('hello')

# 功能别名
from time import sleep as sl
sl(2)
print('hello')
```

#### 自定义模块与导入

每个python文件都可以作为一个模块，模块的名字就是文件的名字，所以文件名必须符合标识符命名规则。

```python
# my_module.py
def test(a,b):
    print(a+2*b)
    
# demo.py
import my_module
# from my_module import test
my_module.test(1,3)
```

- 注意：当导入多个模块时，如果模块内有同名功能，当调用这个同名功能的时候，调用的是**后导入的模块的功能**

```python
# my_module1.py
def test(a,b):
    print(a+2*b)
    
# my_module2.py
def test(a,b):
    print(a-2*b)
    
# demo.py
print(test(1,2)) # -3
```

#### `__main__`变量

自定义模块时，往往会写一些可执行语句用于测试模块的功能。而导入模块时，被导入模块的可执行语句会自动运行。当我们不想在导入模块时就让模块中用于测试的可执行语句运行，则可以借助`__main__`变量。

```python
# my_module.py
def test(a,b):
    print(a+2*b)

if __name__ == '__main__':
    test(1,2)
    
# demo.py
from my_module import test
```

`__name__`是一个内置变量，当直接运行该文件时，它的值为`'__main__'`，而导入该模块时其值与`'__main__'`不同。故可以借助这个变量实现导入与直接运行时执行情况的分离。

#### `__all__`变量

如果一个模块文件中有`__all__`变量（列表），当使用`from xxx import *`导入时，只能导入这个列表中的元素

```python
# my_module.py
__all__ = ['test_A']
def test_A():
    print('testA')

def test_B():
    print('testB')
    
# demo.py
from my_module import *
test_A()
# test_B() 不能调用
# 当手动导入不在__all__中的函数时可以使用
# 此时无法通过直接“import 模块名”导入模块
```

### 包

从物理上看，包就是一个文件夹，但其中包含了一个`__init__.py`文件，该文件夹可以用于包含多个模块文件。

从逻辑上看，包的本质依然是模块

![image-20230701144637928](C:\Users\30235\AppData\Roaming\Typora\typora-user-images\image-20230701144637928.png)

作用：当模块文件过多时，包可以帮助管理模块。

#### 自定义包

新建包的步骤：

- 新建包（文件夹）
- 新建包内模块（要新建文件`__init__.py`）

```python
# my_package
# my_module1.py
print(1)

def info_print1():
    print('my_module1')
    
# my_module2.py
print(2)


def info_print2():
    print('my_module2')
```

导入包

- `import 包名.模块名`

  ```python
  import my_package.my_module1
  import my_package.my_module2
  
  my_package.my_module1.info_print1()
  my_package.my_module2.info_print2()
  ```

- `from 包名 import 模块名`

  ```python
  from my_package import my_module1
  from my_package import my_module2
  
  my_module1.info_print1()
  my_module2.info_print2()
  ```

- `from 包名.模块名 import 功能名`

  ```python
  from my_package.my_module1 import info_print1
  from my_package.my_module2 import info_print2
  
  info_print1()
  info_print2()
  ```

- `from 包名 import *`

  注意：必须在`__init__.py`文件中添加`__all__=[]`控制允许导入的模块列表；`__all__`对第一种方式无效

  ```python
  # __init__.py
  __all__ = ['my_module2']
  
  # test.py
  from my_package import *
  
  my_module2.info_print2()
  ```

#### 安装第三方包

第三方包：非python官方内置的包，可以安装它们来扩展功能，提高开发效率

安装方法：

- 在命令提示符内：
  - `pip install 包名`
  - `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名`
- 在PyCharm中安装



## 数据可视化

### `json`数据格式

json是一种轻量级的数据交互格式，可以按照json指定的格式去组织和封装数据，本质上是一个带有特定格式的字符串。

主要功能：json是一种在各个编程语言中流通的数据格式，负责不同编程语言中数据传递和交互，是一种非常良好的中转数据格式。

#### 数据转化

json格式的数据要求严格，形式上和python的列表和字典很像。

```json
// json数据格式可以是
{"name":"admin","age":18}
// 也可以是
[{"name":"admin","age":18}, {"name":"root","age":18}, {"name":"张三","age":20}]
```

- python数据和json数据的相互转化

```python
# 导入json模块
import json

# 准备符合json格式要求的python数据
data = [{"name":"Tom","age":16}, {"name":"Jerry","age":20}]

# 通过json.dumps(data)方法把python数据转化为json数据
data = json.dumps(data)
# 通过json.loads(data)方法把json数据转化为python数据
data = json.loads(data)
# 可转换的有列表和字典
```

### `pyecharts`模块

如果想要做出数据可视化效果图，可以借助pyecharts模块来完成

安装pyecharts包：`pip install pyecharts`

查看官方画廊：`https://gallery.pyecharts.org/#/README`

### 折线图

#### 构建基础折线图

```python
from pyecharts.charts import Line

# 创建一个折线图对象
line = Line()
# 添加x轴数据
line.add_xaxis(['China', 'the Usa', 'Japan'])
# 添加y轴数据
line.add_yaxis("GDP", [30, 20, 10])
# 通过render方法将代码生成为图像
line.render()
```

运行之后可以生成一个`html`文件，用浏览器打开即可看到生成的图像。

<img src="C:\Users\30235\AppData\Roaming\Typora\typora-user-images\image-20230702205139086.png" alt="image-20230702205139086" style="zoom: 50%;" />

#### 全局配置

全局配置选项可以通过`set_global_opts`方法来进行配置

```python
# 创建一个折线图对象
# 添加x轴数据
# 添加y轴数据
# 设置全局配置项（简易示例）
line.set_global_opts(
    title_opts=TitleOpts(title="GDP展示", pos_left="center", pos_bottom="1%"),
    legend_opts=LegendOpts(is_show=True),
    toolbox_opts=ToolboxOpts(is_show=True),
    visualmap_opts=VisualMapOpts(is_show=True)
)

# 通过render方法将代码生成为图像
line.render()
```

效果：

![image-20230702210544352](C:\Users\30235\AppData\Roaming\Typora\typora-user-images\image-20230702210544352.png)

详细配置项可以查阅相关文档。

#### 数据处理
