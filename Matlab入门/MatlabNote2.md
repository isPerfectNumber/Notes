$\text{MATLAB note II}$

## 绘图基础

### 绘制图形

#### `plot()`

- `plot(x,y)`表示坐标$(x,y)$对应的点
- `plot(y)`也可表示坐标$(x,y)$对应的点，其中`x=[1...n], n=length(y)`

举例：

```matlab
plot(cos(0:pi/20:2*pi));
```

<img src="assets\image-20230627175905866.png" alt="image-20230627175905866" style="zoom: 50%;" />

其中传入的$y$是长度为40的向量，故$x$取1~40的自然数，$y=\cos(\frac {\pi x}{20})$.

- `hold on/off`

  可以在一个窗口同时画出多个图形

  ```matlab
  hold on
  plot(cos(0:pi/20:2*pi));
  plot(sin(0:pi/20:2*pi));
  hold off
  ```

<img src="assets\image-20230627180648457.png" alt="image-20230627180648457" style="zoom: 50%;" />

#### dot style

![image-20230627180942908](assets\image-20230627180942908.png)

```matlab
hold on
plot(cos(0:pi/20:2*pi),'or--');	% circle, dashed, red
plot(sin(0:pi/20:2*pi),'xb:');	% cross, dotted, bule
hold off
```

<img src="assets\image-20230627181430374.png" alt="image-20230627181430374" style="zoom:50%;" />

#### `legend()`

多图线时用`legend()`函数生成图例：`legend('L1',...)`

```matlab
x = 0:0.5:4*pi;
y = sin(x);
h = cos(x);
w = 1./(1+exp(-x));
g = (1/(2*pi*2)^0.5).*exp((-1.*(x-2*pi).^2)./(2*2^2));
plot(x,y,'bd-', x,h,'gp:', x,w,'ro-', x,g,'c^-');

legend('sin(x)','cos(x)','Sigmoid','Gauss function');
```

<img src="assets\image-20230627182008218.png" alt="image-20230627182008218" style="zoom:50%;" />

#### 标题与坐标轴

- 标题：`title()`
- 坐标轴：`xlabel(), ylabel(), zlabel()`

```matlab
x = 0:0.1:2*pi;
y1 = sin(x);
y2 = exp(-x);
plot(x,y1,'--*', x,y2,':o');

xlabel('t = 0 to 2\pi');
ylabel('values of sin(t) and e^{-x}');
title('Function Plots of sin(t) and e^{-x}');
legend('sin(t)','e^{-x}');
```

<img src="assets\image-20230627182526399.png" alt="image-20230627182526399" style="zoom:50%;" />

#### 文本与注解

借用LaTex语法生成图线注解

```matlab
x = linspace(0,3);
y = x.^2.*sin(x);
plot(x,y);
line([2,2],[0,2^2*sin(2)]);
str = '$$ \int_{0}^{2} x^2\sin(x)\mathrm{d}x $$';
text(0.25,2.5,str,'Interpreter','latex');
% 开头两个参数代表注解文本起始点坐标
annotation('arrow','X',[0.32,0.5],'Y',[0.6,0.4]);
% 箭头[起始点x, 结束点x],[起始点y, 结束点y]
```

<img src="assets\image-20230627183621899.png" alt="image-20230627183621899" style="zoom:50%;" />

#### 图线属性调整

- 字体、文字大小、图线粗细、坐标分度等

MATLAB中绘制的图主要包括三部分：幕布(figure)，轴(axes)，图线(line)

<img src="assets\image-20230628094313012.png" alt="image-20230628094313012" style="zoom:50%;" />

可以在打开的图中“编辑”→“图形属性”查看图的各种属性。

##### 找到对象的辨识码

![image-20230628094810920](assets\image-20230628094810920.png)

相关函数：

![image-20230628094839526](assets\image-20230628094839526.png)

##### 获取/修改属性

- 获取：`get()`

  ```matlab
  x = linspace(0,2*pi,1000);
  y = sin(x);
  plot(x,y);
  h = plot(x,y);
  get(h)
  ```

  在命令行窗口可以获取到`h`的各种属性参数（line）

  <img src="assets\image-20230628095216522.png" alt="image-20230628095216522" style="zoom: 50%;" />

  输入指令`get(gca)`可以获取该图关于轴（axes）的相关属性

- 修改：`set()`

  - 修改坐标范围

    ```matlab
    set(gca,'XLim',[0,2*pi]);
    set(gca,'YLim',[-1.2,1.2]);
    
    %% alternative:
    xlim([0,2*pi]);
    ylim([-1.2,1.2]);
    ```

  - 修改字号

    ```matlab
    set(gca,'FontSize',25);
    ```

  - 修改分度值

    ```matlab
    set(gca,'XTick',0:pi/2:2*pi);
    set(gca,'XTickLabel',0:90:360);
    %%
    set(gca,'FontName','tex');
    set(gca,'XTickLabel',{'0','\pi/2','\pi','3\pi/2','2\pi'});
    ```

  - 修改曲线

    ```matlab
    set(h,'LineStyle','-.','LineWidth',7.0,'Color','g');
    
    %% alternative:
    plot(x,y,'-.g', 'LineWidth',7.0);
    ```

#### 绘制多窗口图

多次使用`figure`可以唤出多个窗口，每个窗口绘制一个图

```matlab
x = -10:0.1:10;
y1 = x.^2-8;
y2 = exp(x);
figure,plot(x,y1);
figure,plot(x,y2);
```

可以设置唤出窗口的位置与大小。

```matlab
figure('Position',[left,bottom,width,height]);
```

#### 单窗口绘制子图

可以在一个窗口上绘制多个子图

```matlab
subplot(m, n, x); % 在窗口上绘制m行n列第x个位置的子图（行序优先排列）
```

```matlab
t = 0:0.1:2*pi;
x = 3*cos(t);
y = sin(t);
subplot(2,2,1); plot(x,y); axis normal;
subplot(2,2,2); plot(x,y); axis square;
subplot(2,2,3); plot(x,y); axis equal;
subplot(2,2,4); plot(x,y); axis equal tight;
```

<img src="assets\image-20230628102107207.png" alt="image-20230628102107207" style="zoom:50%;" />

对子图样式控制的指令

![image-20230628102419714](assets\image-20230628102419714.png)

#### 图的保存

```matlab
saveas(gcf,'<filename>','<formattype>');
```

如果图是高解析度，建议使用`print`



## 绘图进阶

![image-20230628103029500](assets\image-20230628103029500.png)

### 特殊图表

#### 对数图

```matlab
x = logspace(-1,1,100); % x从10^{-1}取到10^1，取100个
y = x.^2;
subplot(2,2,1); plot(x,y); title('Plot');
subplot(2,2,2); semilogx(x,y); title('Semilogx');
subplot(2,2,3); semilogy(x,y); title('Semilogy');
subplot(2,2,4); loglog(x,y); title('Loglog');
```

<img src="assets\image-20230628112637497.png" alt="image-20230628112637497" style="zoom:50%;" />

#### 双y轴图：`plotyy()`

```matlab
x = 0:0.01:20;
y1 = 200*exp(-0.05*x).*sin(x);
y2 = 0.8*exp(-0.5*x).*sin(10*x);
[AX,H1,H2] = plotyy(x,y1,x,y2);
set(get(AX(1),'YLabel'),'String','Left Y-axis');
set(get(AX(2),'YLabel'),'String','Right Y-axis');
title('Labeling plotyy');
set(H1,'LineStyle','--');
set(H2,'LineStyle',':');
```

<img src="assets\image-20230628113006373.png" alt="image-20230628113006373" style="zoom:50%;" />

#### 直方图

```matlab
y = randn(1,1000); % 产生符合正态分布的随机数
subplot(2,1,1); hist(y,10); title('Bins = 10');
subplot(2,1,2); hist(y,50); title('Bins = 50');
```

<img src="assets\image-20230628113704955.png" alt="image-20230628113704955" style="zoom:50%;" />

#### 柱状图

（一般型）

```matlab
x = [1 2 5 4 8];
y = [x; 1:5]; % [1 2 5 4 8; 1 2 3 4 5];
subplot(1,3,1); bar(x); title('A bargraph of vector x');
subplot(1,3,2); bar(y); title('A bargraph of vector y');
subplot(1,3,3); bar3(y); title('A 3D bargraph');
```

![image-20230628114102966](assets\image-20230628114102966.png)

（压栈型与水平型）

```matlab
x = [1 2 5 4 8];
y = [x; 1:5];
subplot(1,2,1); bar(y,'stacked'); title('Stacked');
subplot(1,2,2); barh(y); title('Horizontal');
```

<img src="assets\image-20230628114802050.png" alt="image-20230628114802050" style="zoom:50%;" />

#### 饼图

```matlab
a = [10 5 20 30];
subplot(1,3,1); pie(a);
subplot(1,3,2); pie(a, [0,0,0,1]); % 参数向量中1对应的部分分离
subplot(1,3,3); pie3(a, [0,0,0,1]);
```

![image-20230628115035209](assets\image-20230628115035209.png)

#### 极坐标图

```matlab
x = 1:100; theta = x/10; r = log10(x);
subplot(2,2,1); polar(theta,r);

theta = linspace(0,2*pi); r = cos(4*theta);
subplot(2,2,2); polar(theta,r);

theta = linspace(0,2*pi,6); r = ones(1,length(theta));
subplot(2,2,3); polar(theta,r);

theta = linspace(0,2*pi); r = 1-sin(theta);
subplot(2,2,4); polar(theta,r);
```

<img src="assets\image-20230628115426440.png" alt="image-20230628115426440" style="zoom:50%;" />

#### 阶梯图与茎状图

```matlab
x = linspace(0,4*pi,40); y = sin(x);
subplot(1,2,1); stairs(y);
subplot(1,2,2); stem(y);
```

![image-20230628115816951](assets\image-20230628115816951.png)

#### 棒状图与误差棒

```matlab
load carsmall
subplot(1,2,1); boxplot(MPG,Origin);

x = 0:pi/10:pi; y = sin(x);
e = std(y)*ones(size(x));
subplot(1,2,2); errorbar(x,y,e);
```

![image-20230628120404475](assets\image-20230628120404475.png)

### 颜色调节

#### 填充色

```matlab
t = (1:2:15)'*pi/8;
x = sin(t);
y = cos(t);
fill(x,y,'r');	% 范围内填充红色
axis square off;
text(0,0,'STOP','Color','w','FontSize',80,...
	'FontWeight','bold','HorizontalAlignment','center');
```

<img src="assets\image-20230628120808080.png" alt="image-20230628120808080" style="zoom:50%;" />

#### 图像调色

```matlab
subplot(1,2,1);
[x,y] = meshgrid(-3:.2:3, -3:.2:3);
z = x.^2+x.*y+y.^2;
surf(x,y,z);
box on;
zlabel('z'); xlim([-4 4]); xlabel('x');
ylim([-4 4]); ylabel('y');

subplot(1,2,2);
imagesc(z); axis square; xlabel('x'); ylabel('y');
```

![image-20230628143459170](assets\image-20230628143459170.png)

`colorbar`命令可以显示图片的颜色与数据的对应关系

```matlab
imagesc(z); axis square;
xlabel('x'); ylabel('y');
colorbar;
```

<img src="assets\image-20230628143904994.png" alt="image-20230628143904994" style="zoom:50%;" />

`colormap([Name])`中传入不同等参数可以实现不同配色

<img src="assets\image-20230628144115937.png" alt="image-20230628144115937" style="zoom:50%;" />

### 3D图形

相关函数

![image-20230628144429511](assets\image-20230628144429511.png)

#### `plot3()`

绘制三维空间中的曲线

```matlab
x=0:0.1:3*pi;
z1=sin(x); z2=sin(2*x); z3=sin(3*x);
y1=zeros(size(x)); y3=ones(size(x)); y2=y3./2;
plot3(x,y1,z1,'r', x,y2,z2,'b', x,y3,z3,'g'); grid on;
xlabel('x-axis'); ylabel('y-axis'); zlabel('z-axis');
```

<img src="assets\image-20230628145026421.png" alt="image-20230628145026421" style="zoom:50%;" />

#### 三维曲面

命令`meshgrid()`可以将$x$和$y$在不同方向上固定，类似形成平面坐标系

```matlab
x = -3.5:0.2:3.5;
y = -3.5:0.2:3.5;
[X,Y]=meshgrid(x,y);
Z = X.*exp(-X.^2-Y.^2);
subplot(1,2,1); mesh(X,Y,Z);
subplot(1,2,2); surf(X,Y,Z);
```

![image-20230628150101896](assets\image-20230628150101896.png)

#### 等高线图

```matlab
x = -3.5:0.2:3.5;
y = -3.5:0.2:3.5;
[X,Y]=meshgrid(x,y);
Z = X.*exp(-X.^2-Y.^2);
subplot(1,2,1); mesh(X,Y,Z); axis square;
subplot(1,2,2); contour(X,Y,Z); axis square;
```

![image-20230628150451837](assets\image-20230628150451837.png)

还可以通过不同指令画出不同等高线

```matlab
x = -3.5:0.2:3.5;
y = -3.5:0.2:3.5;
[X,Y]=meshgrid(x,y);
Z = X.*exp(-X.^2-Y.^2);
subplot(1,3,1); contour(Z,[-.45:.05:.45]); axis square;
subplot(1,3,2); [C,h]=contour(Z);
clabel(C,h); axis square;
subplot(1,3,3); contourf(Z); axis square;
```

![image-20230628150758004](assets\image-20230628150758004.png)

也可以借助`meshc()`和`surfc()`直接在坐标平面上显示等高线。

#### 观察角度：`view()`

```matlab
sphere(50); shading flat;
light('Position',[1 3 2]);
light('Position',[-3 -1 3]);
material shiny;
axis vis3d off;
set(gcf,'Color',[1 1 1]);
view(-45,20);
```

<img src="assets\image-20230628151216390.png" alt="image-20230628151216390" style="zoom:50%;" />

