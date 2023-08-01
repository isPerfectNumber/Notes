$\text{MATLAB note I}$

## 基本操作与矩阵输入

### 运算符与表达式

运算符：`+, -, *, /, ^`

运算优先级：括号→指数→乘除→加减

如$\cos(\sqrt\frac{(1+2+3+4)^3}{5})$可以写成

```matlab
cos(((1+2+3+4)^3/5)^0.5)
```

$\sin(\sqrt\pi)+\ln(\tan(1))$可以写成

```matlab
sin(pi^0.5)+log(tan(1))
```

表达式的结果自动存入变量`ans`中。

### 变量

#### 定义

变量在赋值、调用之前不需要另外声明。

变量名大小写敏感，并且不能以数字开头，不能使用关键字命名。不可作为变量名的还有：

- `i，j`：复数

- `Inf`：无穷大$(\infty)$

- `eps`：2.2204e-016

- `NaN`：not a number

- `pi`：圆周率$\pi$

- 尽量不要使用内置函数名作为变量名，比如

  ```matlab
  cos='This string.'; % 此时"cos"作为一个变量名
  cos(8)	% ans = 'r'
  clear cos %将变量"cos"清除，此时"cos"是函数
  cos(8)
  ```

  - 注：`clear`会清空所有变量

在命令行窗口输入`whos`可以获取已经保存的变量列表。

#### 数据类型与`format`指令

```matlab
>>pi;
>>format long % 使用示例
```

| 类型     | 结果                         | 示例                  |
| -------- | ---------------------------- | --------------------- |
| `short`  | 显示小数点后四位             | 3.1416                |
| `long`   | 显示小数点后15位             | 3.141592653589793     |
| `shortE` | 显示小数点后四位的科学计数法 | 3.1416e+00            |
| `longE`  | 显示小数点后15位的科学计数法 | 3.141592653589793e+00 |
| `bank`   | 金额，显示小数点后2位        | 3.14                  |
| `hex`    | 十六进制数字                 | 400921fb54442d18      |
| `rat`    | 分数形式                     | 355/113               |

#### 其他操作

1. 在命令行末尾加`;`，该行运算结果不显示

   使用键盘`↑`键可以使用历史命令
2. 一些函数：
   - `clc`：清空命令行窗口
   - `clear`：清空工作区的所有变量
   - `who`：显示工作区的变量
   - `whos`：显示工作区的变量信息

### 向量与矩阵

```matlab
a = [1 2 3 4] 		% 行向量，数字时间也可用逗号隔开
b = [1; 2; 3; 4] 	% 列向量
a*b		% 求向量a,b内积
b*a		% 求向量a,b外积
```

$$
A=\begin{bmatrix}
 1 & 21 & 6\\
 5 & 17 & 9\\
 31 & 2 &7
\end{bmatrix}
$$

输入为

```matlab
A = [1 21 6;5 17 9;31 2 7]
```

#### 向量、矩阵元素的索引

以上面的矩阵$A$为例，

```matlab
A(1,2)			% A(i,j)取第i行第j列的元素
A([1 3], [1 3])	% 取A([m n],[p q])中第m,n行和第p,q列元素的交集组成矩阵

A(4)			% “列序优先”取A(n)中第n个元素
A([1 3 5])		% 取这些元素组成一个向量
A([1 3; 1 3]) 	% 取这些元素组成一个矩阵
```

#### 冒号操作

```matlab
%% 以下表达式结果为行向量
j:k		% [j, j+1, j+2, ..., j+k]
j:i:k	% [j, j+i, j+2i, ..., j+m*i]

%% 利用冒号生成矩阵
B = [1:5; 2:3:15; -2:0.5:0]

%% 字符串
str = 'a':2:'z'

%% 矩阵中取向量
A([3,:])	% 取矩阵A的第3行所有元素为行向量
A([2,:]) = [] % 删除矩阵A第2行元素
```

#### 矩阵的操作与运算

##### 矩阵的组合

有矩阵$A,B,F$，

- 当$F=[A,B]$时：`F = [A B]`
- 当$F=\begin{bmatrix}A\\B\end{bmatrix}$时：`F = [A; B]`

##### 矩阵的运算

有两矩阵$A,B$，

- 矩阵相加：`y1 = A+B`
- 矩阵相乘：`y2 = A*B`
- 矩阵点乘（同型矩阵对应位置元素相乘）：`y3 = A.*B`
- 矩阵相除：`y4 = A/B`（$A/B\approx AB^{-1}$）
- 矩阵点除（类似点乘）：`y5 = A./B`

有矩阵$A$和实数$a$，

- 所有元素与$a$运算：
  - 相加（减）：`x1 = A+a`；`x2 = A-a`
  - 相乘（除）：`x3 = A*a`（等价于`x3 = A.*a`）；`x4 = A/a`（等价于`x4 = A./a`）
  - 指数：`x5 = A^a`（$A^a$）
  - 点指数：`x6 = A.^a`（$A$中每个元素转为其$a$次幂）
- 转置：`C = A'`（即$C=A^T$）

#### 一些特殊矩阵

| 语法            | 矩阵            |
| --------------- | --------------- |
| `linspace()`    |                 |
| `eye(n)`        | $n$阶单位阵     |
| `zeros(n1, n2)` | 零矩阵          |
| `ones(n1, n2)`  | 元素全为1的矩阵 |
| `diag()`        | 对角阵          |
| `rand()`        | 随机数矩阵      |

#### 矩阵函数

| 函数          | 作用                              |
| ------------- | --------------------------------- |
| `max(A)`      | 取$A$中每一列最大的元素组成行向量 |
| `max(max(A))` | 取$A$中最大元素                   |
| `min(A)`      | 取$A$中每一列最小的元素组成行向量 |
| `sum(A)`      | $A$每列元素求和组成行向量         |
| `mean(A)`     | $A$中所有元素的平均值             |
| `sort(A)`     | 将每列元素按升序排序              |
| `sortrows(A)` | 按照首列元素升序进行行交换        |
| `size(A)`     | 生成行向量$[行数,列数]$           |
| `length(A)`   | 行数与列数中较大的数              |
| `find(A==n)`  | 返回$A$中元素$n$的位置            |



## 结构化程式与自定义函数

### 流程控制

- `if, elseif, else`
- `for, switch, case, otherwise`
- `try, catch`
- `while`
- `break, continue, end, pause, return`

#### 逻辑运算符

- `<, <=, >, >=, ==, ~=`$\text{(not equal to)}$`, &&, ||`

#### 语法

- `if-elseif-else`

  ```matlab
  if condition1
  	statement1
  elseif condition2
  	statement2
  else condition3
  	statement3
  end
  ```

  ```matlab
  % example
  a = 3;
  if rem(a,2)==0			% remainder，取余
  	disp('a is even')	% display
  else
  	disp('a is odd')
  end
  ```

- `switch`

  ```matlab
  switch expression
  case value1
  	statement1
  case value2
  	statement2
  ...
  otherwise
  	statement
  end
  ```

  ```matlab
  % example
  input_num = 1;
  switch input_num
  case -1
  	disp('negative 1');
  case 0
  	disp('zero');
  case 1
  	disp('positive 1');
  otherwise
  	disp('other value')
  end
  ```

- `while`

  ```matlab
  while expression
  	statement
  end
  ```

  ```matlab
  % example
  n = 1;
  while prod(1:n) < 1e100
  	n = n+1;
  end
  ```

- `for`

  ```matlab
  for variable=start:increment:end
  	commands
  end
  ```

  ```matlab
  % example
  for n=1:10
  	a(n)=2^n;
  end
  disp(a)		% a是一个行向量
  ```

  - 预先给变量分配空间可以优化程序的运行速度

  ```matlab
  %% A: not pre-allocating
  tic
  for ii=1:2000
  	for jj=1:2000
  		A(ii,jj)=ii+jj;
  	end
  end
  toc
  
  %% B: pre-allocating
  tic
  A = zeros(2000,2000);
  for ii=1:size(A,1)
  	for jj=1:size(A,2)
  		A(ii,jj)=ii+jj;
  	end
  end
  toc
  ```

### 函数

![image-20230627142436682](assets\image-20230627142436682.png)

#### 自定义函数

新建脚本，使用关键字`function`定义函数，文件名与函数名保持一致。

```matlab
function x = freebody(x0,v0,t)
% calculation of free falling
% x0: initial displacement in m
% v0: initial velocity in m/sec
% t: the elapsed time in sec
% x: the depth of falling in m
x = x0 + v0.*t + 1/2*9.8*t.*t;
% 使用点乘则可以传入向量，函数返回结果为向量
```

在**命令行窗口**可以对函数调用

```matlab
% command window
>> freebody(0,0,10)

ans =

   490
   
>> freebody([0,1],[0,1],[10,20])

ans =

         490        1981
```

还可以定义具有多个返回值的函数

```matlab
function [a F] = acc(v2,v1,t2,t1,m)
a = (v2-v1)./(t2-t1);
F = m.*a;

[Acc Force] = acc(20,10,5,4,1)
```

#### 函数默认变量

| 变量名      | 用途                       |
| ----------- | -------------------------- |
| `inputname` | 存储函数输入值的变量       |
| `mfilename` | 正在运行的函数所在的文件名 |
| `nargin`    | 传入参数的数量             |
| `nargout`   | 输出参数的数量             |
| `varargin`  | 传入参数（向量）的长度     |
| `varargout` | 输出参数的长度             |

#### 匿名函数

一种不需要专门存放于`.m`文件中的函数，方便快速定义与使用

```matlab
f = @(x) exp(-2*x)	% 类型为function_handle
x = 0:0.1:2;
plot(x,f(x));
```



## 变量（进阶）与数据存取

<img src="assets\image-20230627145749366.png" alt="image-20230627145749366" style="zoom:67%;" />

### 变量的类型转换

![image-20230627150130773](assets\image-20230627150130773.png)

### 变量介绍（进阶）

#### 字符（`char`）

```matlab
s1 = 'h';	% 单引号中单个字符
s2 = 'H';
whos            
uint16(s1)	% 根据ASCII码转换
uint16(s2)
```

#### 字符串（`String`）

```matlab
s1 = 'Example';
s2 = 'String';
s3 = [s1 s2];		% ExampleString
s4 = [s1; s2];		% 错误使用 vertcat：串联的矩阵的维度不一致。

str = 'sardvark';
str(3)				% ans = r
'a' == str			% ans = [0 1 0 0 0 1 0 0]
str(str=='a') = 'Z'	% str = sZrdvZrk
```

#### 结构体（`Structure`）

```matlab
student.name = 'John';
student.id = 'jdo2@sfu.ca';
student.number = 301073268;
student.grade = [100,75,73;
				  95,91,85.5;
				  100,98,72];

% 可以使用下标记录同类型的多个结构体数据
student(2).name = 'Ann';
...
% 此时student是1*2的向量，向量的每个元素是一个结构体
```

##### 相关函数

![image-20230627152557075](assets\image-20230627152557075.png)

##### 结构体的嵌套

```matlab
a = struct('data',[3 4 7;8 0 1],'nest',...
    struct('testnum','test1',...
    'xdata',[4 2 8],'ydaya',[7 1 6]));
a(2).data=[9,3,2;7,6,5];
a(2).nest.testnum='test2';
a(2).nest.xdata=[3 4 2];
a(2).nest.ydata=[5 0 9];
```

所示结构为<img src="assets\image-20230627154125427.png" alt="image-20230627154125427" style="zoom:50%;" />

#### 阵列（`cell`）

与矩阵类似但其中元素可以存放不同数据类型；用花括号定义

```matlab
%% method 1
A(1,1)={[1 4 3; 0 5 8; 7 2 9]};
A(1,2)={'Anne Smith'};
A(2,1)={3+7i};
A(2,2)={-pi:pi:pi};
A
%% method 2
A{1,1}=[1 4 3; 0 5 8; 7 2 9];
A{1,2}='Anne Smith';
A{2,1}=3+7i;
A{2,2}=-pi:pi:pi;
A
```

该阵列结构为<img src="assets\image-20230627154541251.png" alt="image-20230627154541251" style="zoom: 50%;" />

##### 相关函数

![image-20230627155700966](assets\image-20230627155700966.png)

##### 多维阵列

![image-20230627155923380](assets\image-20230627155923380.png)

可以使用函数`cat()`简化操作。

![image-20230627160231732](assets\image-20230627160231732.png)

![image-20230627160328681](assets\image-20230627160328681.png)

##### 重构：`reshape()`

有两矩阵（阵列）$A$与$B$，$A$有$r_1$行，$c_1$列；$B$有$r_2$行，$c_2$列。当有$c_1r_1=c_2r_2$时可以使用`reshape()`函数进行重构。

```matlab
A = {'James', [1 2;3 4;5 6]; pi, magic(5)}
B = reshape(A,1,4) % 2*2 -> 1*4
```

#### 其他检查变量类型的函数

![image-20230627162044490](assets\image-20230627162044490.png)

### 数据保存与读取

将工作区的数据保存在文件中

#### `save`和`load`

- `save`

  ```matlab
  a = magic(4);
  save mydata1.mat
  save mydata2.mat -ascii
  ```

  将变量数据存到文件`mydata1.mat`和`mydata2.mat`中。第二种方式可用记事本打开阅读。

- `load`

  ```matlab
  load('mydata1.mat')
  load('mydata2.mat','-ascii')
  ```

#### Excel表格的存入与读取

##### 读取

<img src="assets\image-20230627162940092.png" alt="image-20230627162940092" style="zoom:50%;" />

```matlab
score = xlsread('04Score.xlsx')		% 表格内容存入变量
score = xlsread('04Score.xlsx','B2:D4')
```

##### 存入

使用`xlswrite()`函数

```matlab
M = mean(score')';		% 这里是两次转置
xlswrite('04Score.xlsx',M,1,'E2:E4');		% 存入变量
% (filename, variable, sheet, location)
xlswrite('04Score.xlsx',{'Mean'},1,'E1');	% 填写表头
% (filename, header, sheet, location)
```

效果为<img src="assets\image-20230627164319103.png" alt="image-20230627164319103" style="zoom:50%;" />

##### 从Excel获取文本信息

```matlab
[score header] = xlsread('04Score.xlsx')
```

<img src="assets\image-20230627164737657.png" alt="image-20230627164737657" style="zoom:67%;" />

#### 初阶文本内容I/O操作

![image-20230627165131973](assets\image-20230627165131973.png)

```matlab
fid = fopen('[filename]', '[permission]');
% permission includes: 'r','r+','w','w+','a','a+'
status = fclose(fid);
```

示例：

```matlab
x = 0:pi/10:pi;
y = sin(x);
fid  =fopen('sinx.txt','w');
for i=1:11
	fprintf(fid,'%5.3f %8.4f\n', x(i), y(i));
end
fclose(fid);
type sinx.txt
```

![image-20230627170338051](assets\image-20230627170338051.png)

