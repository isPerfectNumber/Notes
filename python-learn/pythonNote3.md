$PythonNote\quad\text{III}$

## 面向对象

### 类的定义、创建对象与成员变量

```python
# 1.设计一个类
class Student:
    name = None
    gender = None
    nationality = None
    native_place = None
    age = None

# 2.创建对象
stu_1 = Student()

# 3.对象属性赋值
stu_1.name = 'Tom'
stu_1.gender = 'male'
stu_1.nationality = 'China'
stu_1.native_place = 'Jiangsu'
stu_1.age = 18

# 4.获取对象记录的信息
print(stu_1.name)
print(stu_1.gender)
print(stu_1.nationality)
print(stu_1.native_place)
print(stu_1.age)
```

### 成员方法

```python
class Student:
    类的属性
    
    def 方法名(self, 形参1, ..., 形参N):
        方法体
```

成员方法的参数列表有一个`self`关键字，在定义成员方法时必须写入：

- `self`用来表示类对象实例自身
- 使用对象调用方法时，`self`会自动被传入（调用的时候不用专门写）
- 在方法内部想访问类的成员变量必须使用`self`

### 构造方法

python中的类可以使用`__init__()`方法，作为构造方法：

- 在创建类对象的时候会**自动执行**
- 创建对象时会将传入参数自动传递给`__init__`方法使用

```python
class Student:
    name = None
    age = None
    tel = None
    
    def __init__(self,name,age,tel):
        # 这里可以同时完成对成员变量的定义，上面可以省略
        self.name = name
        self.age = age
        self.tel = tel


stu = Student('Tom', 19, '93477012')
```

### 其他内置方法（魔术方法）

python中类有一些内置方法，各自有特殊功能，称之为魔术方法。包括：

- `__init__`：构造方法（参阅上方）

- `__str__`：字符串方法

  用于控制类转换为字符串的行为（类似于`toString`）

  ```python
  class Student:
      def __init__(self,name,age):
          self.name = name
          self.age = age
          
      def __str__(self):
          return f"name = {self.name}, age = {self.age}"
      
  stu = Student('Tom', 11)
  print(stu)			# name = Tom, age = 11
  print(str(stu))		# name = Tom, age = 11
  ```

- `__lt__`：小于、大于号比较

  两个自定义的类对象之间通常无法直接进行比较，但可以通过`__lt__`方法实现“运算符的重载”

  ```python
  class Student:
      def __init__(self,name,age):
          self.name = name
          self.age = age
          
      def __lt__(self, other):
          # other: 另一个类对象
          return self.age < other.age
      
  stu1 = Student('Tom', 18)
  stu2 = Student('Jerry', 19)
  print(stu1>stu2) # False
  ```

- `__le__`：小于等于、大于等于号比较——原理基本同上

- `__eq__`：==符号比较

  不实现`__eq__`方法，对象之间也可以比较，但比较的是对象的地址值。实现`__eq__`方法就可以根据需要判断两对象是否“相等”。实现原理基本同上。

- ……

### 面向对象的特性

- 封装、继承和多态

#### 封装

- 私有成员变量和方法（“`private`”）

只要在变量名/方法名之前用两个下划线`__`开头就可以完成私有化设置。

```python
class Phone:
    IMEI = None					# 序列号
    producer = None				# 厂商
    
    __current_voltage = None	# 当前电压
    
    def call(self):
        print('call someone')
        
    def __keep_single_core(self):
        print('CPU run with single core')
        
phone = Phone()					# 创建对象实例
phone.__keep_single_core()		# 使用私有成员方法报错
phone.__current_voltage = 33	# 私有变量赋值不报错，但【无效】
print(phone.__current_voltage)	# 获取私有变量的值会报错
```

私有成员无法被类对象使用，但可以被类中的其他成员使用（访问）。

#### 继承

继承表示从父类那里继承（复制）来成员变量和成员方法（不含私有）。**可以实现多继承。**

```python
class 类名(父类[1, 父类2, ..., 父类N]):
    类内容体
```

- `pass`关键字：用于“填补空白”的空语句

举例：

```python
class Phone:
    IMEI = None
    producer = None
    
    def call_by_5g(self):
        print('call by 5G')
        
class NFCReader:
    nfc_type = 5
    producer = 'xxx'
    
    def read(self):
        print('read NFC')
        
    def write(self):
        print('write NFC')
        
class RemoteControl:
    rc_type = '红外遥控'
    
    def control(self):
        print('remote control ON')
        
class MyPhone(Phone, NFCReader, RemoteControl):
    pass
```

- 注意：多个父类中，如果存在同名成员，默认以继承顺序（从左到右）为优先级。即：先继承的保留，后继承的覆盖

##### 复写（重写）父类成员

子类中重新定义父类中同名的属性或方法，可以实现父类成员的复写。

```python
class Phone:
    IMEI = None
    procuder = 'nokia'
    
    def call_by_5g(self):
        print('call by 5G')
        
class MyPhone(Phone):
    producer = 'Apple'				# 复写父类属性 
    
    def call_by_5g(self):
        print('NEW call by 5G')		# 复写父类方法
```

一旦复写父类成员，那么子类类对象调用成员的时候，就会调用复写之后的成员。如果要使用被复写的父类成员，需要特殊的调用方式。

1. 通过类名调用

   使用成员变量：`父类名.成员变量`

   使用成员方法：`父类名.成员方法(self)`

2. 使用`super()`调用

   使用成员变量：`super().成员变量`

   使用成员方法：`super().成员方法()`

注：只能在子类内部调用父类的同名成员

#### 类型注解

##### 变量类型注解

- `变量: 类型`

```python
# 基础数据类型注解
var_1: int = 10
var_2: float = 3.14
var_3: bool = True
var_4: str = 'python'

# 类对象类型注解
class Student:
    pass

stu: Student = Student()

# 基础容器类型注解
## 简易注解
my_list: list = [1,2,3]
my_tuple: tuple = (1,2,3)
## 详细注解
my_list: list[int] = [1,2,3]
my_tuple: tuple[str,int,bool] = ('python',666,True)
# 元组类型设置类型详细注解需要把每个元素标记出来
my_dict: dict[str,int] = {'python',123}
# 字典类型设置类型详细注解需要标注键和值的类型
```

- 注释中注解

```python
class Student:
    pass

var_1 = random.randint(1,10)	# type: int
var_2 = json.loads(data)		# type: dict[str,int]
var_3 = func()					# type: Student
```

一般在无法直接看出变量类型之时会添加变量的类型注解。

类型注解只是帮助IDE工具对代码进行类型推断，协助做代码提示，并不会真正对类型做验证和判断。即该注解仅仅是提示性的，不会导致错误。

##### 函数形参、返回值注解

```python
def 方法名(形参1: 类型, 形参2: 类型, ...) -> 返回值类型:
    pass
```

##### Union类型注解

使用Union可以定义联合类型注解

使用方式：

- 导包：`from typing import Union`
- 使用：`Union[类型, ..., 类型]`

```python
from typing import Union

my_list: list[Union[str, int]] = [1,2,'python']
my_dict: dict[str, Union[str,int]] = {'name':'Tom', 'age':31}

def func(data: Union[int,str]) -> Union[int,str]:
    pass
```

#### 多态

完成某个行为时，使用不同对象会得到不同状态

```python
class Animal:
    def speak(self):
        """动物说话"""
        pass	# 空实现（接口）
    
class Dog(Animal):
    def speak(self):
        print('汪汪汪')
        
class Cat(Animal):
    def speak(self):
        print('喵喵喵')
        

def make_noise(animal:Animal):
    animal.speak()
    
dog = Dog()
cat = Cat()
make_noise(dog)		# 汪汪汪
make_noise(cat)		# 喵喵喵
```

多态常作用在继承关系上，以父类做定义声明，以子类做实际工作，用以获得同一行为，不同状态

**抽象类（接口）：**包含抽象方法（空方法）的类，用于顶层设计，以便子类做具体实现，是对子类的软性约束，可以配合多态使用。



## SQL

### 数据库

指存储数据的库，作用是组织并存储数据，按照库→表→数据三个层级组织。这种数据组织形式的工具软件，称为数据库管理系统。

数据库（软件）提供数据组织存储能力，SQL语言是操作数据库的工具语言。

操作数据库的SQL语言可以基于功能分为4类：

- 数据定义：DDL(Data Definition Language)

  库的创建删除、表的创建删除等

- 数据操纵：DML(Data Manipulation Language)

  新增数据、删除数据、修改数据等

- 数据控制：DCL(Data Control Language)

  新增用户、删除用户、密码修改、权限管理等

- 数据查询：DQL(Data Query Language)

  基于需求查询和计算数据

**SQL语法特征**

- SQL语言大小写不敏感
- 可以单行或多行书写，最后以分号结尾
- 支持注释：
  - 单行注释：`-- content`（“`--`”之后一定有空格）
  - 单行注释：`# content`（# 后面可以不加空格，建议加上）
  - 多行注释：`/* content */`
