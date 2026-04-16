# 纹波现象

**纹波原因**：按照上述最少拍系统设计，保证了 $E_1(z)$ 在采样时刻的无差控制，但是通常无法保证控制器的输出 $E_2(z) = D(z) E_1(z)$ 平滑，且通常 $\color{red} E_2(z)$ 是在平均值上下来回波动的。

为了保证无纹波的平滑输出，需要 $e_2(nT)$ 在有限周期后不波动（即为常值）。
$$
{\color{red}D(z)} = \dfrac{\Phi(z)}{\Phi_e(z){\color{red}G(z)}}
$$
产生原因：分析可知，纹波现象是 $G(z)$ 的**单位圆内左半平面的零点**引起的，虽然稳定，但是振荡。

---

**例题**
$$
G_0(s) = \dfrac{10}{s^2+s}
$$
设计最少拍

```matlab
% matlab
clear;clc;
ts = 1;
s = tf('s');
z = tf('z',ts);
g0 = 10/(s^2+s);
gz = c2d(g0,ts,'zoh');
rz = z/(z-1);
phie = (1-z^(-1));
phi = 1-phie;
dz = phi/(phie*gz);
zpk(dz)
uz = dz*phie*rz;
[n1,d1] = tfdata(uz,'v');
uk = impz(n1,d1,10)
impz(n1,d1,10)
```

$$
u_k =
\begin{bmatrix}
0.2718 & -0.2952 & 0.2121 & -0.1523 & 0.1094 & -0.0786 & 0.0564 & -0.0405 & 0.0291 & -0.0209
\end{bmatrix}
$$

显然，控制量u[k]虽然稳定，但是不停波动，导致系统输出比如出现纹波。

纹波起因是由于$U(z)$中包含了$z=-0.7183$的极点，它来自$G(z)$的$z=-0.7183$的零点。

# 无纹波附加条件

为了满足无纹波，在最少拍设计的方法基础上，还需要满足以下两个条件：

**1）被控对象本身 $G_0(s)$ 有能力实现无纹波跟踪**

比如，如果要想对速度信号$r(t) =t$设计无纹波最少拍控制，则希望$G_0(s)$ 在ZOH常值输出信号作用下可以稳态输出等速变化量，因此$G_0(s)$ 必须至少包含一个积分环节。同理，如果要对$r(t) = 0.5t^2$ 设计无纹波最少拍控制，则$G_0(s)$ 中至少包含两个积分环节。

总结，若输入信号为
$$
r(t) = R_0+R_1 t + \dfrac{1}{2}R_2t^2+\cdots + \dfrac{1}{(q-1)!}R_{q-1}t^{q-1}
$$
则无纹波控制的前提（必要条件）是：
$$
被控对象G_0(s) 中至少包含 q-1个积分环节
$$
这个并非充要条件，如果想要保证无纹波，还需要设计特殊的脉冲传递函数。

**==2）闭环脉冲传递函数 $\Phi(z)$ 附加条件==**

考虑控制器输出信号：
$$
E_2(z) = D(z) \Phi_e(z) R(z)
$$
为了实现无纹波，我们希望经过$l$个采样周期后，控制输入$E_2(z)$ 进入稳态，即：
$$
e_2(lT) = e_2((l+1)T) = \cdots = constant
$$
因此，与前面最少拍设计同理，根据[[FIR]]判定定理，此时就需要多一个条件： $\color{red} D(z)\Phi_e(z)$ 是$z^{-1}$ 的有限多项式。

进一步，代入 $D(z)$，原条件进一步化为：要求 $\color{red} \dfrac{\Phi(z)}{G(z)}$ 是有限多项式。

显然 $G(z)$ 的零点可能会导致成为无限多项式，设 $G(z) = \dfrac{P(z)}{Q(z)}$，则原条件进一步化为：<span style="color:red">要求 $\Phi(z)$ 的零点应对消 $G(z)$ 的零点，</span>即：
$$
无纹波附加条件：{\color{red} \Phi(z) = P(z) M(z)}
$$
其中，$M(z)$ 是待定多项式，$P(z)$表示广义对象$G(z)$的所有非零零点。

## 总结

1. 当设计最少拍无纹波系统时，$\color{red}\Phi(z)$ 除了应满足最少拍要求的形式外，还需要满足**无纹波条件**：$\color{red} \Phi(z) = P(z) M(z)$，即要求包含 $G(z)$ 的全部非零零点。
2. 无纹波条件所增加的零点数，会导致无纹波最少拍系统比有纹波最少拍系统的调节时间要长。

# 例题1

单位阶跃输入：$R(z) = \dfrac{1}{1-z^{-1}}$

设
$$
D(z) \Phi_e(z) = \dfrac{E_2(z)}{R(z)} = a_0 + a_1 z^{-1} + a_2 z^{-2}
$$
代入$R(z)$，得：
$$
E_2(z) = a_0 + (a_0+a_1)z^{-1} +(a_0+a_1+a_2)(z^{-2} + z^{-3} +\cdots)
$$

说明，从第二排开始，$e_2(k)$进入稳态。

```mathematica
(*mathematica验证*)
Clear["Global`*"]
Dphi = a0+a1*z^(-1)+a2*z^(-2);
rz = z/(z-1);
uz = Dphi*rz;
uzseries = Series[uz,{z,Infinity,5}] 
```

$$
U(z) = a_0 + \frac{a_0 + a_1 + a_2}{z^5}+ \frac{a_0 + a_1 + a_2}{z^4}+ \frac{a_0 + a_1 + a_2}{z^3}+ \frac{a_0 + a_1 + a_2}{z^2}+ \frac{a_0 + a_1}{z}
$$

# 例题2

单位斜坡输入：$R(z) = \dfrac{Tz^{-1}}{(1-z^{-1})^2} $

设
$$
D(z) \Phi_e(z) = \dfrac{E_2(z)}{R(z)} = a_0 + a_1 z^{-1} + a_2 z^{-2}
$$
代入$R(z)$，分析同上。

```matlab
% matlab
clear;clc;
T = 1;
a0 =1; a1 = 2; a2 =3;
z = tf('z',T);
rz = T*z^(-1)/((1-z^(-1))^2);
Dphi = a0+a1*z^(-1)+a2*z^(-2);
uz = Dphi*rz;
[n,d]=tfdata(uz,'v');
impz(n,d,10)
```

也可以用mathematica分析级数展开：

```mathematica
Clear["Global`*"]
Dphi = a0+a1*z^(-1)+a2*z^(-2);
rz = T*z^(-1)/((1-z^(-1))^2);
uz = Dphi*rz;
(* 展开为 z^-1 的幂级数 *)
uzSeries = Series[uz, {z, Infinity, 6}]
```

$$
\begin{aligned}
U(z) &= \frac{(6a_0 + 5a_1 + 4a_2)T}{z^6}+ \frac{(5a_0 + 4a_1 + 3a_2)T}{z^5}+ \frac{(4a_0 + 3a_1 + 2a_2)T}{z^4}\\
&+\frac{(3a_0 + 2a_1 + a_2)T}{z^3}+ \frac{(2a_0 + a_1)T}{z^2}+ \frac{a_0 T}{z}
\end{aligned}
$$

# 例题3

$$
G_0(s) = \dfrac{10}{s^2+s},T=1s,r(t) = 1(t),设计最少拍无纹波数字控制器
$$

**答案**：

**1）利用matlab计算G(z)**

```matlab
clear;clc;
s = tf('s'); ts = 1; z = tf('z',ts);
g0 = 10/(s^2+s);
gz = c2d(g0,ts,'zoh'); % Zeros:(z+0.7183) 
```

**2）利用mathematica求待定系数**

```mathematica
phi = a*z^-1*(1+0.7183*z^-1);
phie = (1-z^-1)*(1+f1*z^-1);
expr = phi-1+phie;
eqs = Thread[CoefficientList[expr,z^-1]==0];
sol = Solve[eqs,{a,f1}]
```

$$
\begin{cases}
a = 0.581971 \\
f_1 = 0.418029
\end{cases}
$$

**3）利用mathematica求D(z)，分析U(z)**

```mathematica
(*mathematica*)
a = 0.581971; f1 = 0.418029;
phi = a*z^-1*(1+0.7183*z^-1);
phie = (1-z^-1)*(1+f1*z^-1);
gz = 3.6788*(z+0.7183)/((z-1)*(z-0.3679));
dz = phi/(phie*gz);
Simplify[dz]
rz = z/(z-1);
uz = dz*phie*rz;
Simplify[uz]
```

$$
\begin{cases}
D(z) = \dfrac{-0.0582003 + 0.158196\, z}{0.418029 + z}\\
U(z) = 0.158196 - \dfrac{0.0582003}{z}
\end{cases}
$$

**3’）利用matlab求D(z)，分析U(z)**

因为知道采样周期，也可以直接使用matlab：

```matlab
clear;clc;
s = tf('s'); ts = 1; z = tf('z',ts);
g0 = 10/(s^2+s);
gz = c2d(g0,ts,'zoh');       % Zeros:(z+0.7183) 
a = 0.581971; f1 = 0.418029;
phi = a*z^-1*(1+0.7183*z^-1);
phie = (1-z^-1)*(1+f1*z^-1);
dz = phi/(phie*gz);
rz = z/(z-1);
uz = dz*phie*rz;
[n1,d1] = tfdata(uz,'v');
impz(n1,d1,10)
```

$$
U(k) = [0.1582,-0.0582,0,0,0,0,0,0,0,0]
$$

我们发现，收敛比有纹波的慢了一拍，这是因为为了匹配$\Phi(z)$中增加了$z^{-1}$ 的次幂的原因。

**推广**：在无纹波时的 $\Phi(z)$ 要比有纹波增加了 $k$ 阶，其中 $k$ 就是单位圆内的 $G(z)$ 的零点数目。

# 无纹波设计步骤

1）求出广义对象的脉冲传递函数；

2）将$G(z)$中的纯滞后环节$z^{-l}$、所有零点$1-Z_iz^{-1}$（$z=1$除外)，设计$\Phi(z)$的主体部分；

3）根据典型输入形式、$G(z)$的不稳定极点（$z=1$除外），设计$\Phi_e(z)$的主体部分；

4）然后根据$\Phi(z)=1-\Phi_e(z)$的匹配原则，分别在$\Phi(z)$和$\Phi_e(z)$的中增加附加的匹配部分：
$$
\Phi_e(z) =  \underbrace{(1-z^{-1})^m  \prod_i (1-P_iz^{-1})}_{\color{red}主体部分} *\underbrace{ (1+f_1 z^{-1}+f_2z^{-2}+\cdots + f_pz^{-p})}_{\color{blue}匹配部分}\\
\Phi(z) =  \underbrace{z^{-l} \prod_{i} (1-Z_iz^{-1})}_{\color{red}主体部分}* \underbrace{(a_0 + a_1 z^{-1}+a_2 z^{-2}+\cdots + a_q z^{-q})}_{\color{blue}匹配部分}
$$
5）根据幂次相等的匹配原则，求解匹配部分的待定系数。

**综合题**，参考[[CCS_C07最少拍控制器设计]]中的最后的综合题。