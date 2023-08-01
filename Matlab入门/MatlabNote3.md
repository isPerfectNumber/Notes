$\text{MATLAB note III}$

## 数值微积分

### 多项式微积分

#### 多项式的表示

多项式在MATLAB中使用向量表示。比如对于多项式$f(x)=x^3-2x-5$，在MATLAB中可以表示成

```matlab
p = [1 0 -2 -5];	% 元素为多项式中的系数，其顺序按照所在项指数递减
```

可以绘制关于多项式的函数图像。如对于$f(x)=9x^3-5x^2+3x+7(-2\leq x\leq5)$，有

```matlab
a = [9,-5,3,7]; x = -2:.01:5;
f = polyval(a,x);
plot(x,f,'LineWidth',2);
xlabel('x'); ylabel('f(x)');
```

<img src="assets\image-20230628152520256.png" alt="image-20230628152520256" style="zoom:50%;" />

#### 多项式的微分：`polyder()`

$$
f(x)=a_nx^n+a_{n-1}x^{n-1}+\dots+a_1x+a_0\\
f'(x)=a_nnx^{n-1}+a_{n-1}(n-1)x^{n-2}+\dots+a_1
$$

给一多项式函数$f(x)=5x^4-2x^2+1$，可以用`polyder()`函数获取其对应的导函数多项式$(f'(x)=20x^3-4x)$等价向量：

```matlab
p = [5 0 -2 0 1];
polyder(p)
% ans = [20 0 -4 0]
polyval(polyder(p),7)	% 求该导数在x=7处的值
```

#### 多项式的积分：`polyint()`

$$
f(x)=a_nx^n+a_{n-1}x^{n-1}+\dots+a_1x+a_0\\
\int f(x)=\frac1{n+1}a_nx^{n+1}+\frac1na_{n-1}x^n+\dots+a_0x+C
$$

依旧给函数$f(x)=5x^4-2x^2+1$，可用`polyint()`函数求其积分式，注意需要向函数提供积分式中的常量$C$

```matlab
p = [5 0 -2 0 1];
polyint(p,3);
polyval(polyint(p,3),7)
```

### 数值的微积分

#### 数值的微分

$$
f'(x_0)=\lim_{h\to0}\frac{f(x_0+h)-f(x)}h
$$

函数`diff()`可以计算向量中每相邻两元素之差

```matlab
x = [1 2 5 2 1];
diff(x)
% ans = [1 3 -3 -1]
```

故可用来粗略计算函数某点的斜率

```matlab
x0 = pi/2; h = 0.1;
x = [x0 x0+h];
y = [sin(x0) sin(x0+h)];
m = diff(y)./diff(x)
```

当$h$越小，计算出来的斜率就越接近$x_0$处的导数。

##### 二次/三次微分

注：每次求导$x$对应的向量中元素应少一个

```matlab
x = -2:.005:2; y = x.^3;
m = diff(y)./diff(x);
m2 = diff(m)./diff(x(1:end-1));

plot(x,y, x(1:end-1),m, x(1:end-2),m2);
xlabel('x'); ylabel('y');
legend('f(x) = x^3','f''(x)','f''''(x)');
```

<img src="assets\image-20230628200155574.png" alt="image-20230628200155574" style="zoom:50%;" />

#### 数值的积分

$$
s=\int_a^bf(x)\mathrm{d}x\approx\sum_{i=0}^mf(x_i)\int_a^bL_i(x)\mathrm{d}x
$$

$\text{e.g.}:f(x)=\int_0^24x^3\mathrm{d}x=x^4|^2_0=2^4-0^4$

- 中值法：`sum()`

  <img src="assets\image-20230628202801787.png" alt="image-20230628202801787" style="zoom:50%;" />

  ```matlab
  h = 0.05; x = 0:h:2;
  midpoint = (x(1:end-1)+x(2:end))./2;
  y = 4*midpoint.^3;
  s = sum(h*y)
  ```

- 梯形法：`trapz()`

  <img src="assets\image-20230628203316622.png" alt="image-20230628203316622" style="zoom:50%;" />

  ```matlab
  h = 0.05; x = 0:h:2; y = 4*x.^3;
  s = h*trapz(y)
  
  %% alternative
  h = 0.05; x = 0:h:2; y = 4*x.^3;
  trapezoid = (y(1:end-1)+y(2:end))/2;
  s = h*sum(trapezoid)
  ```

- 辛普森法

  <img src="assets\image-20230628210041309.png" alt="image-20230628210041309" style="zoom:50%;" />

  ```matlab
  h = 0.05; x = 0:h:2; y = 4*x.^3;
  s = h/3*(y(1)+2*sum(y(3:2:end-2))+4*sum(y(2:2:end))+y(end));
  ```

- 三者比较

  ![image-20230628210510913](assets\image-20230628210510913.png)

#### 函数指针与`integral()`

可以借用`@`向函数中传入函数参数

```matlab
function [y] = xy_plot(input,x)
% xy_plot receives the handle of a function and plots that function of x
y = input(x); plot(x,y,'r--');
xlabel('x'); ylabel('function(x)');
end

%%
xy_plot(@sin, 0:0.01:2*pi);
% xy_plot(sin, 0:.01:2*pi); 错误
```

可以借助`integral()`函数与函数指针求积分
$$
\text{e.g.:}\int_0^2\frac1{x^3-2x-5}\mathrm{d}x
$$

```matlab
y = @(x) 1./(x.^3-2*x-5);
integral(y,0,2)
```

 还可以使用`integral2()`和`integral3()`进行多重积分
$$
\text{e.g.:}f(x,y)=\int_0^\pi\int_{\pi}^{2\pi}(y\sin(x)+x\cos(y))\mathrm{d}x\mathrm{d}y
$$

```matlab
f = @(x,y) y.*sin(x)+x.*cos(y);
integral2(f,pi,2*pi,0,pi) % (f,x1,x2,y1,y2)，按照积分先后次序
```

$$
\text{e.g.:}f(x,y,z)=\int_{-1}^1\int_0^1\int_0^\pi(y\sin(x)+z\cos(y))\mathrm{d}x\mathrm{d}y\mathrm{d}z
$$

```matlab
f = @(x,y,z) y.*sin(x)+z.*cos(y);
integral3(f,0,pi,0,1,-1,1) % (f,x1,x2,y1,y2,z1,z2)，按照积分先后次序
```



## 方程式求根

### 解析解

利用`syms`或`sym()`可以将变量定义为一个符号变量（变量可以存在输出结果中）

```matlab
syms x;	% x = sym('x');
x+x+x	% ans = 3*x
```

- 可以使用`solve()`函数对变量求解。

例如求解$y=x\sin(x)-x=0$：

```matlab
syms x
y = x*sin(x)-x;
solve(y,x)	% (equation,symbol)
```

一元二次方程组也可以求解。例如
$$
\begin{cases}
x-2y=5\\
x+y=6
\end{cases}
$$

```matlab
syms x y
eq1 = x-2*y-5;
eq2 = x+y-6;
A = solve(eq1,eq2,x,y);
A.x		% 17/3
A.y		% 1/3
```

也可以求解含参方程。例如在$ax^2-b=0$中用$a,b$表示$x$：

```matlab
syms x a b
solve(a*x^2-b,x)	% 第二个参数表示待求解未知数
```

- 可以使用`diff()`计算函数的微分

```matlab
syms x
y = 4*x^5;
yprime = diff(y)
```

- 使用`int()`函数计算函数的积分

如令$z=\int y\mathrm{d}x=\int x^2e^x\mathrm{d}x,z(0)=0$

```matlab
syms x;
y = x^2*exp(x);
z = int(y);
z = z-subs(z,x,0) % sub(z,x,0):用0取代x，即求出z(0)的值
```

### 数值解

- 可以使用`fsolve()`函数与匿名函数（指针）求解。

```matlab
f2 = @(x) (1.2*x+0.3+x*sin(x));
fsolve(f2,0) % (function_handle, initial guess)
% 提供初值利用迭代法求解
```

- `fzero()`也可以得到数值解，底层原理与上面有区别。无法解出不变号零点解

```matlab
f = @(x) x.^2;
fzero(f,0.1) % (function_handle, initial guess)
fsolve(f,0)
```

<img src="assets\image-20230629115539592.png" alt="image-20230629115539592" style="zoom: 50%;" /><img src="assets\image-20230629115610461.png" alt="image-20230629115610461" style="zoom: 50%;" />

```matlab
f = @(x) x.^2;
options=optimset('MaxIter',1e3,'TolFun',1e-10);
%						迭代次数			误差
fsolve(f,0.1,options)
fzero(f,0.1,options)
```

<img src="assets\image-20230629120010997.png" alt="image-20230629120010997" style="zoom:50%;" />

- 专门求解多项式的函数：`roots()`

例如求解函数$f(x)=x^5-3.5x^4+2.75x^3+2.125x^2-3.875x+1.25$的零点。

```matlab
roots([1 -3.5 2.75 2.125 -3.875 1.25])
```

<img src="assets\image-20230629120242356.png" alt="image-20230629120242356" style="zoom:50%;" />

==该函数只能求解多项式。==

#### 数值解求解的运作方法

![image-20230629120637732](assets\image-20230629120637732.png)

二分法：$r=\frac{l+u}2$

- $f(x)$在$[l,u]$上连续
- $f(l)\cdot f(u)<0$

牛顿法：$x_{n+1}=x_n-\frac{f(x_n)}{f'(x_n)}$

- $f(x)$连续且$f'(x)$可求

**解析解与数值解优缺点比较**

![image-20230629114059558](assets\image-20230629114059558.png)

### 递归函数

```matlab
function output = fact(n)
% fact recursively finds n!
if n==1
	output = 1;
else
	output = n*fact(n-1);
end
end
```



## 线性方程组与线性系统

### 线性方程组求解

$$
\begin{cases}
3x-2y=5\\
x+4y=11
\end{cases}\Rightarrow
\begin{bmatrix}
 3 & -2\\
 1 &4
\end{bmatrix}
\begin{bmatrix}
 x\\y 
\end{bmatrix}
=\begin{bmatrix}
 5 \\ 11 
\end{bmatrix}\quad(Ax=b)
$$

#### 初等行变换

使用函数`rref()`求解线性方程组（得到行最简式）
$$
\begin{cases}
x+2y+z=2\\
2x+6y+z=7\\
x+y+4z=3
\end{cases}\Rightarrow
\begin{bmatrix}
 1 & 2 & 1 &2 \\
 2 & 6 & 1 &7 \\
 1 & 1 & 4 &3
\end{bmatrix}
$$

```matlab
A = [1 2 1;2 6 1;1 1 4];
b = [2;7;3];
R = rref([A,b])
```

#### $LU$法求解

- 原理

  在$Ax=b$中，令$A=L^{-1}U$（其中$L$是下三角矩阵，$U$是上三角矩阵）

  故$Ax=b\Rightarrow L^{-1}Ux=b$，令$y=Ux$

  在方程$L^{-1}y=b$求解$y$，在通过$Ux=y$求解$x$

- 使用`lu()`函数获得$L,U$

  ```matlab
  A = [1 1 1;2 3 5;4 6 8];
  [L, U, P] = lu(A);
  inv(L)
  U
  ```

#### 矩阵的左除：`\`或`mldivide()`

```matlab
A = [1 2 1;2 6 1;1 1 4];
b = [2;7;3];
x = A\b
```

也可以借助逆矩阵求解。

```matlab
x = inv(A)*b
```

**相关求解函数**

![image-20230629152915174](assets\image-20230629152915174.png)

### ==线性系统==

$$
\begin{cases}
2\cdot2-12\cdot4=x\\
1\cdot2-5\cdot4=y
\end{cases}\Rightarrow
\begin{bmatrix}
2&-12\\1&-5
\end{bmatrix}
\begin{bmatrix}
2\\4
\end{bmatrix}
=\begin{bmatrix}
x\\y
\end{bmatrix}\quad(Ab=y)
$$

其中$A$为系统矩阵，输出的矩阵$y$受$A$影响。

- $Av_i=\lambda_iv_i$求$A$的特征值$\lambda_i\in\mathbb{R}$
- $b=\sum\alpha_iv_i,\alpha_i\in\mathbb{R}$
- $Ab=\sum\alpha_iAv_i=\sum\alpha_i\lambda_iv_i$

可以通过`eig()`函数求矩阵的特征值

```matlab
A = [2 -12;1 -5];
[v,d] = eig(A)
```

其中$d$是特征值的对角矩阵，矩阵$v$每一列是对应的右特征向量，使得$Av=vd$



## 统计

### 叙述统计学

![image-20230629165518802](assets\image-20230629165518802.png)

#### 中心趋势

![image-20230629165542048](assets\image-20230629165542048.png)

![image-20230629170420042](C:\Users\30235\AppData\Roaming\Typora\typora-user-images\image-20230629170420042.png)

#### 建立图表

```matlab
x = 1:14;
freqy = [1 0 1 0 4 0 1 0 3 1 0 0 1 1];
subplot(1,3,1); bar(x,freqy); xlim([0 15]);
subplot(1,3,2); area(x,freqy); xlim([0 15]);
subplot(1,3,3); stem(x,freqy); xlim([0 15]);
```

![image-20230629202419696](assets\image-20230629202419696.png)

#### boxplot

![image-20230629202842788](assets\image-20230629202842788.png)

```matlab
marks = [80 81 81 84 88 92 92 94 96 97];
boxplot(marks);
prctile(marks,[25 50 75]);
```

<img src="assets\image-20230629202938444.png" alt="image-20230629202938444" style="zoom:50%;" />

#### 偏态分布

![image-20230629204708553](assets\image-20230629204708553.png)

使用`skewness()`函数可以大致得出数据的偏态情况

```matlab
X = randn([10 3])*3;
X(X(:,1)<0, 1) = 0; X(X(:,3)>0, 3) = 0;
boxplot(X, {'Right-skewed','Symmetric','Left-skewed'});
y = skewness(X)
```

<img src="assets\image-20230629204913330.png" alt="image-20230629204913330" style="zoom:50%;" />

#### ==峰度==



### 推论统计学

#### 假设检验

对于一个事件，设置虚无假设$H_0$和对立假设$H_1$，然后设置一个置信区间（在该区间内$H_0$成立），$H_0$成立的概率为置信程度

##### t-test

```matlab
load stockreturns;
x1 = stocks(:,3);
x2 = stocks(:,10);
boxplot([x1 x2],{'3','10'});
[h,p] = ttest2(x1,x2)
% h = 0时虚无假设成立，h = 1时虚无假设不成立
% p为虚无假设成立的概率
```

<img src="assets\image-20230629211433096.png" alt="image-20230629211433096" style="zoom:50%;" />

##### 双尾测试

<img src="assets\image-20230629211640212.png" alt="image-20230629211640212" style="zoom:50%;" />

##### 单尾测试

<img src="assets\image-20230629211714363.png" alt="image-20230629211714363" style="zoom:50%;" />



