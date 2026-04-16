# Z反变换定义

由Z变换反向求解相应**离散序列**的过程：
$$
 {\color{red}f(nT)} = Z^{-1}[ F(z) ] 
$$
则**采样信号**为：
$$
f^*(t)=f(t)\delta_T(t) = \sum_{n=0}^{\infty} {\color{red}f(nT)}\delta(t-nT)
$$
注意：Z变换和Z反变换的**唯一性，只是对于采样序列**，而非对于原始连续信号。

# 反变换方法

## 方法1：部分分式法

又称查表法，根据给定的$E(z)$，展开部分分式以便查表。

由于$Z$变换表中，$E(z)$ 分子部分普遍含有$z$，所以可以先将$\color{red}\dfrac{E(z)}{z}$ 展开部分分式，然后再将每一项乘以$z$，得到$E(z)$ 的部分分式展开。为了能够更好的匹配Z变换表，务必优先提出$z$，然后再部分分式展开。否则，可能无法匹配Z变换表。例如：
$$
\dfrac{z}{(z+1)(z+2)} = \dfrac{-1}{z+1} + \dfrac{2}{z+2}
$$


**前提**：**原分式需为<span style="color:red">真分式</span>（分子次数<span style="color:red">低于</span>分母）**。

如果不是真分式，需要先作长除法。

### 例题

1. （==无重根情形==）求Z反变换$f(nT)$：$F(z) = \dfrac{(1-e^{-aT}) {\color{red}z} }{(z-1)(z-e^{-aT})}$

	答案：
	$$
	\begin{aligned}
	{\color{red}\dfrac{F(z)}{z}} &= \dfrac{A}{z-1}+\dfrac{B}{z-e^{-aT}} = \dfrac{1}{z-1}-\dfrac{1}{z-e^{-aT}}\\
	F(z) &= \dfrac{z}{z-1}-\dfrac{z}{z-e^{-aT}}\\ 
	f(nT) & = 1-e^{-anT}\Rightarrow f^*(t)= \sum_{n=0}^{\infty} (1-e^{-anT})\delta(t-nT)\\
	\end{aligned}
	$$
	关于系数$A,B$的求法，可以采用如上的匹配系数法，也可以使用留数定理来求，对于==无重根==的情况：
	$$
	已知两个单极点：z_1 = 1, z_2 = e^{-aT}\\
	A = (z-z_1) \left. \dfrac{F(z)}{z} \right|_{z=z_1} =\left. \dfrac{1-e^{-aT}}{z-e^{-aT}}\right|_{z=z_1}=1\\
	B = (z-z_2) \left. \dfrac{F(z)}{z} \right|_{z=z_2} =\left. \dfrac{1-e^{-aT}}{z-1}\right|_{z=z_2}= -1
	$$
	
2. （==有重根情形==）求Z反变换：$E(z) = \dfrac{-3z^2+z}{z^2-2z+1}$

	答案：对于==有重根==的情形，也可以使用留数定理来求系数：
	$$
	\begin{aligned}
	& \dfrac{E(z)}{z} = \dfrac{-3z+1}{(z-1)^2} = \dfrac{A}{(z-1)^2}+\dfrac{B}{z-1}\\
	&\begin{cases}
	A = \left.(z-1)^2 \dfrac{E(z)}{z}\right|_{z=1} = -3+1 = -2\\
	B = \left.\dfrac{d}{dz}\left(  (z-1)^2\dfrac{E(z)}{z} \right) \right|_{z=1} = -3|_{z=1}=-3
	\end{cases}
	\\
	&\Rightarrow \\
	&E(z) = \dfrac{-2z}{(z-1)^2} + \dfrac{-3z}{z-1} \\
	&e(nT) = -2\dfrac{nT}{T} -3 = -2n-3
	\end{aligned}
	$$
	

### 练习

1. 求Z反变换$f(nT)$：$F(z)=\dfrac{z}{(z-1)(z-0.8)}$
2. 求Z反变换$f(nT)$：$F(z) =  \dfrac{z^2+z}{(z-1)^2(z-0.6)}$；
3. 求Z反变换$f(nT)$：$F(z)=\dfrac{z^2}{(z-0.4)(z+0.2)}$；

**答案**

1. 答：
	$$
	\begin{aligned}
	\dfrac{F(z)}{z} &= \dfrac{A}{z-1} + \dfrac{B}{z-0.8} = \dfrac{5}{z-1}-\dfrac{5}{z-0.8} \\
	F(z) &= \dfrac{5z}{z-1}-\dfrac{5z}{z-0.8} \\
	f(nT) &= 5 - 5*0.8^n \\
	f^*(t) &= \sum_{n=0}^{\infty} (5-5*0.8^n)\delta(t-nT)
	\end{aligned}
	$$
	
2. 答：
	$$
	\begin{aligned}
	& F(z) = z\left(  \dfrac{A}{z-1}+\dfrac{B}{(z-1)^2} + \dfrac{C}{z-0.6}   \right)  \\
	& z+1 = A(z-1)(z-0.6)+B(z-0.6)+C(z-1)^2 \\
	& A = -10, B=5, C=10 \\
	& F(z) = -\dfrac{10 z}{z-1} + \dfrac{5z}{(z-1)^2} + \dfrac{10z}{z-0.6}\\
	&f(nT) = -10 + 5n + 10*0.6^n
	\end{aligned}
	$$
	
3. 答：
	$$
	\begin{aligned}
	& F(z) = z\left(   \dfrac{A}{z-0.4}+\dfrac{B}{z+0.2}   \right)\\
	& z=A(z+0.2) + B(z-0.4)  = (A+B)z +0.2A-0.4B \\
	& A=2/3; B=1/3 \\
	& F(z) = \dfrac{2}{3}\dfrac{z}{z-0.4} + \dfrac{1}{3}\dfrac{z}{z+0.2} \\
	& f(nT) = \dfrac{2}{3} (0.4)^n + \dfrac{1}{3}(-0.2)^n
	\end{aligned}
	$$

## 方法2：长除法

将Z变换函数<span style="color:red">按 $z^{-1}$ 升幂排列为有理分式</span>，然后做除法即可。
$$
F(z) = \dfrac{b_0+b_1z^{-1}+b_2z^{-2}+\cdots +b_m z^{-m}}{1+a_1z^{-1}+a_2z^{-2}+\cdots+a_n z^{-n}}
$$
做除法可得到$z^{-1}$的升幂排列的幂级数：
$$
F(z) = c_0+c_1z^{-1} +c_2z^{-2}+\cdots = \sum_{n=0}^{\infty}c_nz^{-n}
$$
根据Z变换定义可知：
$$
c_n=f(nT)
$$

### 例题

1. 求Z反变换：$E(z)=\dfrac{10z}{(z-1)(z-2)}$
2. 求Z反变换：$F(z)=\dfrac{z^3+2z^2+1}{z^3-1.5z^2+0.5z}$
3. 的求Z反变换：$F(z)=\dfrac{z}{z-a}$

**答案**

1. 答
	$$
	E(z)= \dfrac{10z}{z^2-3z+2}=\dfrac{10z^{-1}}{1-3z^{-1}+2z^{-2}} \Rightarrow e^*(t)= 10\delta(t-T) +30 \delta(t-2T) + 70 \delta(t-3T)+\cdots 
	$$
	
2. 答
	$$
	\begin{aligned}
	&F(z) = \dfrac{1+2z^{-1}+z^{-3}}{1-1.5z^{-1}+0.5z^{-2}} \\
	&F(z) = 1+3.5z^{-1}+4.75z^{-2}+\cdots \\
	&f(nT)= \delta(t)+3.5 \delta(t-T) + 4.75 \delta(t-2T)+\cdots
	\end{aligned}
	$$
	
3. 答
	$$
	F(z) = 1+az^{-1}+a^2z^{-2}+\cdots \\
	f(nT) = a^n\\
	f(t) = a^{t/T}
	$$

---

## 方法3：留数法

$$
\color{red}f(nT) = \sum_{i=1}^k  \Res[ F(z)z^{n-1} ]_{z\to z_i}
$$

其中，$z_i$ 为$F(z)z^{n-1}$ 的极点。
$$
\begin{aligned}
& ①若z_i是单极点，则：  \Res[ F(z)z^{n-1} ]_{z\to z_i} = \lim_{z\to z_i} [(z-z_i) F(z) z^{n-1} ] \\
& ②若z_i是m重极点，则：  \Res[ F(z)z^{n-1} ]_{z\to z_i} = \lim_{z\to z_i} \dfrac{1}{(m-1)!} \dfrac{d^{m-1} [[(z-z_i)^m F(z) z^{n-1} ]] }{dz^{m-1}}
\end{aligned}
$$

### 例题

1. 求Z反变换：$F(z)=\dfrac{z}{z-a}$；
2. 求Z反变换：$F(z)= \dfrac{z^2}{(z-1)(z-0.5)}$
3. 求Z反变换：$F(z) =  \dfrac{z^2+z}{(z-1)^2(z-0.6)}$
4. 求Z反变换：$F(z)= \dfrac{0.5z}{(z-1)(z-0.5)}$

**答案**

1. 答
	$$
	F(z)z^{n-1} = \dfrac{z^n}{z-a},只有一个极点 z_1 = a,则 f(nT) = \left[ (z-a)\dfrac{z^n}{z-a}\right ]_{z\to a} = a^n \\
	$$
	
2. 答
	$$
	F(z)z^{n-1} = \dfrac{z^{n+1}}{(z-1)(z-0.5)},z_1 = 1, z_2 = 0.5，则f(nT) = \left[   \dfrac{1}{0.5} + \dfrac{0.5^{n+1}}{-0.5}   \right ] = 2-0.5^{n}\\
	$$
	
3. 答
	$$
	F(z)z^{n-1} = \dfrac{z^{n+1}+z^n}{(z-1)^2(z-0.6)},	z_1 = z_2 = 1; z_3 = 0.6\\
		f(nT) = \left[  \dfrac{d}{dz}    \dfrac{z^{n+1}+z^n}{(z-0.6)} \right]_{z\to 1}  + \left[ \dfrac{z^{n+1}+z^n}{(z-1)^2} \right]_{z \to 0.6} = 5n-10+10*0.6^n
	$$
	
4. 答
	$$
	F(z)z^{n-1} = \dfrac{0.5 z^n}{(z-1)(z-0.5)},则 f(nT) = \dfrac{0.5}{1-0.5} + \dfrac{0.5*0.5^n}{0.5-1} = 1-0.5^n
	$$
	





