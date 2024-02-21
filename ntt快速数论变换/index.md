# NTT（快速数论变换）


## 1 算法介绍

### 1.1 多项式乘法引入

#### 1.1.1 多项式的两种表示方法

一般$n﻿$​次多项式如公式（1）所示。多项式具有两种表示方法分别是系数表示法和点值表示法。

$$
\begin{equation}
A(x)=a_0+a_1x^1+a_2x^2+\ldots +a_{n-1}x^{n-1}+a_nx^n=\sum^{n}_{i=0}a_ix^i
\end{equation}
$$

###### 系数表示法：

任意多项式都可以通过一组系数所确定，而这组系数所组成的向量也叫做系数向量,通过系数向量表示一个多项的方式也叫做系数表示法。例如公式（1）中 $A(x)$ 的系数向量为：$\mathbf{a}=[a_0  ,a_1,\ldots,a_{n-1},a_n]$。

###### 点值表示法：

对于一个已知的多项式例如公式（1）中的 $A(x)$ ，将 $x_i$ 代进去可以得到一个确定的值 $y_i$ ，如：$y_0=A(x_0)$ 。且可将 $(x_0,y_0)$ 看作是坐标系上的一个点。可将任意多（互不相等）的自变量 $( x_1,x_2,\ldots,x_{n-1}, x_n)$ 代入到 $A(x)$ 中，从而得到更多的点：$(x_1,y_1),(x_2,y_2),\ldots(x_n,y_n)$。通过 n+1 个不同点组成的点集 $P=\{(x_0,y_0),(x_1,y_1),\ldots,(x_n,y_n)\} $ ，唯一确定一个 n 次多项式，该方式也叫做多项式的点值表示法。

#### 1.1.2 多项式乘法

已知两个多项式 $A(x)$ 和 $B(x)$，分别是 $n$ 次多项式和 m 次多项式。

<div>
$$
\begin{align}
A(x)=\sum^{n}_{i=0}a_ix^i\\
B(x)=\sum^{m}_{i=0}b_ix^i
\end{align}
$$
</div>



公式（2）和公式（3）相乘得到一个最高为 $n+m$ 次的多项式 $C(x)$。系数向量：$\mathbf{c}=[c_0  ,c_1,\ldots,c_{m+n}]$。

<div>
$$
\begin{align}
C(x)=\sum^{n+m}_{i=0}c_ix^i
\end{align}
$$
</div>



###### 系数乘法：

系数乘法，将两个多项式的系数相乘，系数相乘，如公式（5）所示。不难看出时间复杂为 $O(n^2)$。整理可得公式（6）。

$$
\begin{align}
C_{i+j}=  \sum_{i=0}^{n} \sum_{j=0}^{m} a_i b_j 
\end{align}
$$

<div>
$$
\begin{align}
C_i=\sum^{i}_{j=0}a_jb_{i-j}
\end{align}
$$
</div>



###### 点值乘法：

点值乘法只需将要将对应的纵坐标相乘即可，但是因为新得到的多项式次数更高，所以每个因子多项式都需要提供 $m+n+1$ 个点参与运算。时间复杂度为 $O(n)$。

###### 小结

在计算多项式乘法时，点值表示的时间复杂度低。但是在计算系统中系数表示更加的常用。于是很自然的想到，在进行乘法时从系数表示转换到点值表示，使用点值表示法进行乘法运算，然后再转换为系数表示法。

但是很不幸的是，从系数表示转换到点值表示，以及点值表示到系数表示，这两个过程的时间复杂度均为 $O(n^2)$。

![资源 2](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191953350.svg "图1 多项式乘法总结图")

#### 1.1.3 转换优化

如果通过减少求值的点数可以较少计算时间。根据函数的奇偶性我们可以只求一半的点值，从而减少一半的计算量。这里为了方便表示设一个 $n-1$ 次的一般多项式，$n=2^a , a\in \N$。

$$
P(x)=p_0+p_1x^1+p_2x^2+\ldots +p_{n-1}x^{n-1}=\sum^{n-1}_{i=0}p_ix^i
$$

将偶次项和奇数项分别组合：

$$
P(x)=(p_0+p_2x^2\ldots+p_{n-2}x^{n-2})+(p_1x^1+\ldots +p_{n-1}x^{n-1})
$$

对奇次项提取公因式 $x$:

$$
P(x)=(p_0+p_2x^2\ldots+p_{n-2}x^{n-2})+x(p_1x^0+\ldots +p_{n-1}x^{n-2})
$$

化简为：

$$
\begin{align}
P(x)=P_e(x^2)+xP_o(x^2)
\end{align}
$$

则我们可以取 $\frac{n}{2}$ 对相反数点，这样我们只需要计算一半的点值。

$$
P(x_i)=P_e(x_i^2)+xP_o(x_i^2) \\
P(-x_i)=P_e(x_i^2)-x_iP_o(x_i^2)
$$

观察公式（7）不难发现，是将是将 $P(x)$ 拆成了两个规模为 $\frac{n}{2}$ 的 $P_e(x)$,$P_o(x）$。自然而然想到了递归执行。但是问题是相反数平方运算后所得结果均为正数，无法构造相反数对，无法实现递归。如果能一直保持相反数点对，那么时间复杂度可以表示为$T(n)=T(2n)+O(n)$​，即时间复杂度为 $O(n\log n)$。

#### 1.1.4 单位根（复平面）

既然实数域没有办法解决上面的问题，可以将起扩展到复平面。我们希望取一些点使其平方后的结果依然存在相反数对。

![资源 6](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191953363.svg "图2 复平面图")

既然点是我们自己选的，那不如使最后一组相反数点对为 $\{1,-1\}$。我们取 n 等于 8 作为演示。不难看出最后选取的 8 个点均是 $x^8=1$ 的解。

![资源 5](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191953795.svg "图3 n=8选点示例图")

对于任意 $n=2^a , a\in \N$，来说即为 $x^n=1$。

由欧拉公式:

$$
e^{i \theta}=\cos\theta+i\sin\theta
$$

可以得到公式：

$$
z^n=1=\cos2k\pi+i\sin2k\pi=e^{2k\pi i},k \in[ 0,n)
$$

所以:

$$
z=\sqrt[n]{e^{2k\pi i}}=e^\frac{2k\pi i}{n} ,k \in[ 0,n)
$$

上面是 $n$ 次单位复数根的推导。当 $k=1$ 时，值 $\omega_n=e^\frac{2\pi i}{n}$ 被成为主 $n$ 次单位复数根，其他的 $n$ 次单位根都是 $\omega_n$ 的幂次。其中 $\theta$ 表示为复平面单位圆上的弧长。因此 $e^\frac{2\pi i}{n}$ 表示将一个单位圆均分 $n$ 份。$n=8$ 时如下图所示。

![资源 14@800x](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402192044572.webp "图4  8次单位根示意图")

###### $n$ 次单位根的性质：

**消去引理：**

$$
\omega_{dn}^{dk}=\omega_n^k\\
\omega_{n}^{\frac{n}{2}}=-\omega_2=-1\\
\omega_{n}^{k+\frac{n}{2}}=-\omega_n^k
$$

**折半引理：**

$$
(\omega_n^k)^2=\omega_{\frac{n}{2}}^k
$$

**求和引理：**

<div>
$$
\sum_{i=0}^{n-1} (\omega_n^k)^i =
\begin{cases}
0, & k \neq mn, \ m \in \mathbb{Z} \\
n, & k = mn, \ m \in \mathbb{Z}
\end{cases}
$$
</div>
### 1.2 $DFT$ 与 $IDFT$

经过 1.1 节的介绍已经对多项式乘法的相关内容有了相应的了解，下面开始正式介绍 $DFT$ 与 $IDFT$，将在 1.3 节介绍 $FFT$ 与 $IFFT$ 的相关内容。我们是从多项式乘法引入的$DFT$和$IDFT$，但是$DFT$和$IDFT$的应用的场景有很多，多项式求值、大数乘法，拉格朗日插值、矩阵乘法、中国剩余定理以及环同态。

#### 1.2.1 离散傅里叶变换（$DFT$）

设有一个 $n-1$ 次的多项式 $P(x)$:

$$
P(x) = a_0 + a_1x + a_2x^2 + \ldots + a_{n-1}x^{n-1}
$$

多项式 $P(x)$ 的 $DFT$ 在单位根 $\omega_n^k = e^{2\pi i k / n}  $ 上的值计算如下：

$$
P_k = P(\omega_n^k) = \sum_{j=0}^{n-1} a_j \omega_n^{kj} \quad  k = 0, 1, \ldots, n-1
$$

#### 1.2.2 逆离散傅里叶变换（$IDFT$）

对 $DFT$ 过程我们可以通过矩阵进行描述 $y=Wa$：

<div>
$$
\left[ \begin{array}{c}
y_0 \\
y_1 \\
y_2 \\
\vdots \\
y_{n-1} \\
\end{array} \right]
=
\left[ \begin{array}{cccc}
1 & 1 & \ldots & 1 \\
1 & \omega_n & \ldots & \omega_n^{n-1} \\
1 & \omega_n^2 & \ldots & \omega_n^{2(n-1)} \\
\vdots & \vdots & \ddots & \vdots \\
1 & \omega_n^{n-1} & \ldots & \omega_n^{(n-1)^2}
\end{array} \right]
\left[ \begin{array}{c}
a_0 \\
a_1 \\
a_2 \\
\vdots \\
a_{n-1} \\
\end{array} \right]
$$
</div>



其中 $V_n$ 是一个范德蒙矩阵，是一个可逆矩阵，因此 $IDFT$：$a=W^{-1}y$。

下面求取:$W_n^{-1}$​。矩阵 $W$可以表示为：$W_{jk}=ω\_ n^{jk}$​。$W_{jk}^{-1}$​的值应该是$W_{jk}$​的共轭除以 $n$。由 $\omega\_n$​的性质，我们知道 $\omega\_n^k$​ 的共轭是 $\omega\_n^{-k}$​。因此 $W\_{jk}^{-1}=\frac{1}{n}ω\_ n^{-jk}$​。所以 $DFT$ 和 $DFT^{-1}$ 的时间复杂一致，甚至计算流程基本一致。

$$
a_j= \frac{1}{n}\sum_{j=0}^{n-1} y_j ω_ n^{-jk} \quad  k = 0, 1, \ldots, n-1
$$

### 1.3 快速傅里叶变化（$FFT$）

其实 $FFT$ 的思路在 1.1.3 节中进行介绍只是并不是很完善，下面进行一个较为细致的描述。详细介绍 FFT 的递归实现和迭代实现。$IFFT$ 与 $FFT$ 区别不大不再详细介绍了。

#### 1.3.1 $FFT$ 递归实现

将单位根带入公式（7）得：

$$
\begin{align}
P(\omega_n^k)=P_e((\omega_n^k)^2)+{\omega_n^k}P_o((\omega_n^k)^2) \quad k \in[ 0,n)
\end{align}
$$

根据消去引理推导出得对称性以及折半引理对公式（8）进行化简。取 $k \in[0,\frac{n}{2})$ 。

<div>
$$
\begin{align*}
P(\omega_{n}^{k})&=P_{e}((\omega_{n}^{k})^2)+{\omega_{n}^{k}}P_{o}((\omega_{n}^{k})^2)\\
 & =P_{e}(\omega_{\frac{n}{2}}^{k})+{\omega_{n}^{k}}P_{o}(\omega_{\frac{n}{2}}^{k})\\ 
 \end{align*}
$$
</div>

<div>
$$
\begin{align*}
P(\omega_{n}^{k+m})&=P_{e}((\omega_{n}^{k+m})^2)+\omega_{n}^{k+m}P_{o}((\omega_{n}^{k+m})^2)\\
 & =P_{e}(\omega_{\frac{n}{2}}^{k+m})+\omega_{n}^{k+m}P_{o}(\omega_{\frac{n}{2}}^{k+m})\\ 
& =P_{e}(-\omega_{\frac{n}{2}}^{k})-\omega_{n}^{k}P_{o}(-\omega_{\frac{n}{2}}^{k})\\
& =P_{e}(\omega_{\frac{n}{2}}^{k})-\omega_{n}^{k}P_{o}(\omega_{\frac{n}{2}}^{k})
 \end{align*}
$$
</div>

即公式（9-10），这两个式子也被称为 CT(Cooley-Tukey)蝶形操作，$\omega_{n}^{k}$ 被成为转换因子。

<div>
$$
\begin{align}
P(\omega_{n}^{k})=P_{e}(\omega_{\frac{n}{2}}^{k})+{\omega_{n}^{k}}P_{o}(\omega_{\frac{n}{2}}^{k})\\
P(\omega_{n}^{k+m})=P_{e}(\omega_{\frac{n}{2}}^{k})-\omega_{n}^{k}P_{o}(\omega_{\frac{n}{2}}^{k})
\end{align}
$$
</div>

现在给出递归版本得算法：

<div>
$$
\begin{array}{ll}
\hline
\text{\textbf{Algorithm 1}} & \text{ Recursion   FFT}\\ 
\hline
\textbf{Require:} & P=[p_0,p_1,\ldots,p_{n-1}]\quad n=2^{a},a\in\mathbb{N}\\ 
\text{\textbf{function}} & \text{FFT\_R}(P)\\ 
&n\leftarrow len(P)\\
&\text{if}\quad n==1 \\
&\quad \text{return}\quad P\\
&\text{endif}\\
&\omega_n \leftarrow e^{\frac{2\pi i}{n}}\\
&P_e\leftarrow [p_0,p_2,\ldots,p_{n-2}]\\
&P_o\leftarrow [p_1,p_3,\ldots,p_{n-1}]\\
&y_e\leftarrow  FFT\_R(P_e)\\
&y_o\leftarrow  FFT\_R(P_o)\\
& \text{for} \quad k \quad \text{to} \quad \frac{n}{2}-1\\
&\quad y[k]=y_e[k]+\omega_n^ky_o[k]\\
&\quad y[k+\frac{n}{2}]=y_e[k]-\omega_n^ky_o[k]\\
&\text{endfor}\\
&\text{return} \quad y \\
\textbf{end function} & \\ 
\hline & 
\end{array}
$$
</div>


###### 蝶形操作：

蝶形操作得名于其数据流图的形状，上面的推导过程出现的是 CT 蝶形变换，还有一种 GS 蝶形变换。

![资源 9](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402192017314.svg "图5 CT蝶形操作图")

![资源 10](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191954300.svg "图6 CT蝶形操作简化图")

通过公式（9-10）我们可以推导出 GS 蝶形变换的形式。将公式（9）和（10）分别加减运算得：

<div>
$$
P(\omega_{n}^{k}) + P(\omega_{n}^{k+m}) = 2P_{e}(\omega_{\frac{n}{2}}^{k})
\\
P(\omega_{n}^{k}) - P(\omega_{n}^{k+m}) = 2\omega_{n}^{k}P_{o}(\omega_{\frac{n}{2}}^{k})
$$
</div>

整理得公式（11-12）不难看出，GS 操作是 CT 操作得逆过程，可以用于 $IFFT$ 中，不过本文介绍的 $IFFT$ 方案没有采用这种思路,当然也可以用在$FFT$中，毕竟两者本质上没啥区别。

<div>
$$
\begin{align}
P_e(\omega_{\frac{n}{2}}^{k}) &= \frac{1}{2}(P(\omega_{n}^{k}) + P(\omega_{n}^{k+m})) \\
P_o(\omega_{\frac{n}{2}}^{k}) &= \frac{1}{2\omega_{n}^{k}}(P(\omega_{n}^{k}) - P(\omega_{n}^{k+m})) 
\end{align}
$$
</div>



![资源 11](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191954666.svg "图7 GS蝶形操作")

#### 1.3.2 $FFT$ 迭代实现

继续优化。依然拿 n=8 举例。递归数据操作如图 7 所示，在递归操作中是自定向下层层展开，然后逐层向上收缩。每一次递归都会消耗堆栈资源，影响效率。如果可以从底向上计算，那么可以省去堆栈资源的消耗，提高程序运行效率。于是现在的问题就变为了如何确定递归树叶子节点的元素排序。

![资源 11](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191954478.svg "图8 n=8时递归示意图")

###### **位逆序**

将叶子节点元素顺序和原始元素顺序均使用二进制表示。发现二进制表示发生了左右对称反转，称之为位逆序变换。

```cpp
//位逆序
Layer 4:	0	4	2	6	1	5	3	7
Binary:		000 100 010 110 001 101 011 111
Reverse:	000 001 010 011 100 101 110 111
Decimal:	0	1	2	3	4	5	6	7
```

下面是 Bit-Reverse-Copy 的实现:

<div>
$$
\begin{array}{ll}
     \hline
    \text{\textbf{Algorithm 2}} & \text{Bit-Reverse-Copy} \\ 
    \hline
    \text{\textbf{Require:}}    & A \\ 
    \text{\textbf{function}}    & \text{BitReverseCopy}(A) \\  
    & n\leftarrow len(A) \\  
    & b\leftarrow\log_2(n) \\ 
    & \text{for}\quad i=0\quad\text{to}\quad n-1 \\  
    & \quad r\leftarrow\text{ReverseBits}(i,b) \\  
    & \quad B[r]\leftarrow A[i] \\  
    & \text{endfor} \\  
    & \text{return}\quad B \\ 
    \text{\textbf{end function}} & \\
    \hline 
\end{array}
$$
</div>


函数 ReverseBits 的实现如下,基本过程就是取原值低位赋值给目标值低位，然后原值右移一位，目标值左移一位，直到循环结束。

<div>
$$
\begin{array}{ll}
\hline
\text{\textbf{Function}} & \text{ReverseBits}(i, b) \\
\hline
& result \leftarrow 0 \\
& \text{for}\quad j = 0\quad\text{to}\quad b - 1 \\
& \quad result \leftarrow result \ll 1 \\
& \quad bit \leftarrow (i \gg j) \& 1 \\
& \quad result \leftarrow result \, | \, bit \\
& \text{endfor} \\
& \text{return}\quad result \\
\hline
\end{array}
$$
</div>



###### 迭代算法实现

算法 3 是迭代版版本的 $FFT$ 实现。$s $ 可以看作是合并轮数。$m$ 是当前合并次数下每个单元的规模。

<div>
$$
\begin{array}{ll}\hline\text{\textbf{Algorithm 3}} & \text{Iteration FFT}\\ 
\hline
\text{\textbf{Require:}} & P=[p_0,p_1,\ldots,p_{n-1}]\quad n=2^{a},a\in\mathbb{N}\\
    \text{\textbf{function}} & \text{FFT\_I}(P)\\  
& n\leftarrow len(P)\\  
& A\leftarrow BitReverseCopy(P)\\
& \text{for}\quad s=1\quad\text{to}\quad \log
{n}\\  
& \quad m=2^s\\
& \quad \omega_m=e^{\frac{2\pi i}{m}}\\
& \quad\text{for}\quad k=0\quad \text{to} \quad n-1 \quad \text{by}\quad m\\  
& \quad\quad \omega=1\\
& \quad\quad\text{for}\quad j=0\quad\text{to} \quad \frac{m}{2}-1\\  
& \quad\quad\quad t=\omega A[k+j+\frac{m}{2}]\\
& \quad\quad\quad u= A[k+j]\\
& \quad\quad\quad A[k+j]= u+t\\
& \quad\quad\quad A[k+j+\frac{m}{2}]= u-t\\
& \quad\quad\quad \omega=\omega \omega_m\\
& \quad\quad\text{endfor}\\ 
& \quad\text{endfor}\\ 
& \text{endfor}\\  
& \text{return}\quad A\\
 \text{\textbf{end function}} 
& \\ 
\hline 
& \end{array}
$$
</div>


下面是 n=8 时，迭代 $FFT$ 的数据流图。

![资源 9](https://cdn.jsdelivr.net/gh/yinxiangkai/ImageBed@main/202402191954181.svg "图9 n=8迭代FFT数据流图")

### 1.4 快速数论变换（$NTT$）

$FFT$ 存一些问题。首先 FFT 是在复数域上的表示，而计算机系统中表示复数，需要比实数花费更多的资源，且复数运算也比实数运算更加复杂。此外 FFT 涉及大量的正余弦运算，对于精度有影响。于是我们期望在实数域内寻找类似单位根性质的数学概念。有限域上的原根满足相应的要求。

#### 1.4.1 数学基础

##### 欧拉函数

欧拉函数，即 $\varphi(n)$,表示的是小于等于 $n$ 且和 $n$ 互素的数的个数。当 n 是素数时 $\varphi(n)=n-1$。

##### 欧拉定理

对于 $a \in \Z$ ,$m\in \N^*$,若 $\gcd(a,m)=1$,则 $a^{\varphi(m)} \equiv 1 \pmod{m}$。

##### 费马小定理

若 $p $ 为素数，$\gcd(a,p)=1$,则 $a^{p-1} \equiv 1 \pmod{p}$。

也可表达为，对于任意整数 $a $，有 $a^p \equiv a \pmod{p}$。

利用费马小定理求逆元:$a^{-1}=a^{p-2}$

##### 阶

由欧拉定理可知，若 $\gcd(a,p)=1$，一定存在一个最小的正整数 $n$ 满足同余式 $a^n \equiv 1 \pmod{m}$。这个 $n$ 被称作 $a$ 模 $m$ 的阶记作 $\delta_m(a) $ 或者 $ord_m(a)$。具有以下性质：

性质 1：$a,a^2,\ldots,a^{\delta_m(a)}$ 模 $m $ 两两不同余，之后进入周期。

性质 2：若 $a^n \equiv 1 \pmod{m}$,则 $\delta_m(a)|n$。可以推导出若 $a^p \equiv a^q \pmod{m} $，则有 $p \equiv q \pmod{\delta_m(a)}$。

##### 原根

给定  $n \in \N^*$ ，$g \in \Z$，满足  $\gcd(g, m) = 1$，且  $\delta_m(g) = \varphi(m)$ ，则称  $g$  为模 $m$ 的原根。

即 $g$ 满足 $\delta_m(g)=| \Z_m^* |=\varphi(m)$ 。当 $m$ 是质数时，我们有 $g^i\mod m$，$0<i<m$ 的结果互不相同

原根个数：若一个数 $m$ 有原根，则它原根的个数为 $\varphi(\varphi(m))$。

原根存在定理：一个数 $m$ 存在原根当且仅当 $m=2,4,p^a,2p^a$ ，其中 $p$ 为奇素数，$a\in \N^*$。

###### 原根的性质

原根具有以下性质，不难看与单位根具有类似的性质。所以可以用于替代单位根，用于简化 $DFT$ 计算。

不重性：$\forall 0 \leq i < j < \varphi(p),\ g^i \not\equiv g^j \pmod{p} $

折半性：定义 $g_n = g^{\frac{1}{n}} \equiv g^{\frac{p-1}{n}}$,$g_n^k = (g_n)^k$ 则 $g_{an}^{ak} \equiv g_n^k \pmod{p}$。

对称性：$g_{2n}^{k+n} \equiv - g_{2n}^k \pmod{p}$

求和性：$\sum_{i=0}^{n-1} (g_n)^{ki} \equiv n[k=0] \pmod{p} $，$k=0$ 为真 $[k=0]=1$,否则为 $0$。

###### 原根与模数的选择

为了实现多次二分，模数 $p$ 应选可以拆分为 $q\times 2^k +1$ 的素数,$q$ 为奇素数，$k$ 为整数,$2^k$ 是最大长度。可以看下表的例子。

<div>
$$
\text{表 1：原根和模数的相关数据}\\
\begin{array}{cccc}
\hline
\text{原根 } g & \text{模数 } p & \text{分解 } p & \text{模大小表示} \\
\hline
3 & 469762049 & 7 \times 2^{26} + 1 & 2^{26} \\
3 & 998244353 & 119 \times 2^{23} + 1 & 2^{23} \\
3 & 2281701377 & 17 \times 2^{27} + 1 & 2^{27} \\
\hline
\end{array}
$$
</div>



#### 1.4.2 $NTT$ 的递归实现

将原根带入公式（7）得：

$$
\begin{align}
P(g_n^k)=P_e((g_n^k)^2)+{g_n^k}P_o((g_n^k)^2)\mod p \quad k \in[ 0,n)
\end{align}
$$

根据消去引理推导出得对称性以及折半引理对公式（13）进行化简。取 $k \in[0,\frac{n}{2})$ 。

<div>
$$
\begin{align*}
P(g_{n}^{k})&=P_{e}((g_{n}^{k})^2)+{g_{n}^{k}}P_{o}((g_{n}^{k})^2) \mod p\\
 & =P_{e}(g_{\frac{n}{2}}^{k})+{g_{n}^{k}}P_{o}(g_{\frac{n}{2}}^{k}) \mod p\\ 
 \end{align*}
$$
</div>

<div>
$$
\begin{align*}
P(g_{n}^{k+m})&=P_{e}((g_{n}^{k+m})^2)+g_{n}^{k+m}P_{o}((g_{n}^{k+m})^2) \mod p\\
 & =P_{e}(g_{\frac{n}{2}}^{k+m})+g_{n}^{k+m}P_{o}(g_{\frac{n}{2}}^{k+m}) \mod p\\ 
& =P_{e}(-g_{\frac{n}{2}}^{k})-g_{n}^{k}P_{o}(-g_{\frac{n}{2}}^{k}) \mod p\\
& =P_{e}(g_{\frac{n}{2}}^{k})-g_{n}^{k}P_{o}(g_{\frac{n}{2}}^{k}) \mod p
 \end{align*}
$$
</div>



简化为：

<div>
$$
P(g_{n}^{k})=P_{e}(g_{\frac{n}{2}}^{k})+{g_{n}^{k}}P_{o}(g_{\frac{n}{2}}^{k})\\
P(g_{n}^{k+m})=P_{e}(g_{\frac{n}{2}}^{k})-g_{n}^{k}P_{o}(g_{\frac{n}{2}}^{k})
$$
</div>



现在给出递归版本得算法,不难发现除了将单位根替换为原根，增加模运算，以及增加了参数 ，原根 $g$，模数 $p $ 外与 $FFT$ 并无太大区别。

<div>
$$
\begin{array}{ll}
\hline
\text{\textbf{Algorithm 4}} & \text{ Recursion Number Theoretic Transform (NTT)}\\ 
\hline
\textbf{Require:} & A=[a_0,a_1,\ldots,a_{n-1}],\quad n=2^{k},\quad g ,\quad p \\
\text{\textbf{function}} & \text{NTT\_R}(A, g, p)\\ 
&n\leftarrow \text{len}(A)\\
&\text{if}\quad n==1 \\
&\quad \text{return}\quad A\\
&\text{endif}\\
&g_n \leftarrow g^{\frac{p-1}{n}} \mod p\\
&A_e\leftarrow [a_0,a_2,\ldots,a_{n-2}]\\
&A_o\leftarrow [a_1,a_3,\ldots,a_{n-1}]\\
&Y_e\leftarrow  \text{NTT\_R}(A_e, g_n^2, p)\\
&Y_o\leftarrow  \text{NTT\_R}(A_o, g_n^2, p)\\
& \text{for} \quad k \quad \text{from} \quad 0 \quad \text{to} \quad n/2-1\\
&\quad Y[k]= (Y_e[k] + g_n^k Y_o[k]) \mod p\\
&\quad Y[k+n/2]= (Y_e[k] - g_n^k Y_o[k]) \mod p\\
&\text{end for}\\
&\text{return} \quad Y \\
\textbf{end function} & \\ 
\hline & 
\end{array}
$$
</div>



> 注意NTT里的$g^{-1}$是取逆元操作，在计算$INTT$时请注意。

#### 1.4.3 $NTT$ 的迭代实现

$NTT$ 的迭代实现同理，因此不再赘述，直接给出相应的算法。

<div>
$$
\begin{array}{ll}
\hline
\text{\textbf{Algorithm 5}} & \text{Iteration NTT}\\ 
\hline
\text{\textbf{Require:}} & A=[a_0,a_1,\ldots,a_{n-1}],\ n=2^{k},\quad  g, \quad p\\ 
\text{\textbf{function}} & \text{NTT\_I}(A, g, p)\\  
& n \leftarrow \text{len}(A)\\  
& A \leftarrow \text{BitReverseCopy}(A)\\
& \text{for}\ s=1\ \text{to}\ \log{n}\\  
& \quad m=2^s\\
& \quad g_m=g^{\frac{p-1}{m}} \mod p\\
& \quad \text{for}\ k=0\ \text{to}\ n-1\ \text{by}\ m\\  
& \quad\quad g=1\\
& \quad\quad \text{for}\ j=0\ \text{to}\ \frac{m}{2}-1\\  
& \quad\quad\quad t=g A[k+j+\frac{m}{2}] \mod p\\
& \quad\quad\quad u= A[k+j]\\
& \quad\quad\quad A[k+j]= (u+t) \mod p\\
& \quad\quad\quad A[k+j+\frac{m}{2}]= (u-t) \mod p\\
& \quad\quad\quad g=(g \cdot g_m) \mod p\\
& \quad\quad \text{endfor}\\ 
& \quad \text{endfor}\\ 
& \text{endfor}\\  
& \text{return}\ A\\
\text{\textbf{end function}} & \\ 
\hline
& 
\end{array}
$$
</div>



### 1.5  Cooley-Tukey 算法

现在介绍另外一种算法，这种算法可以更好的并发执行。

如果 $n=a\cdot b$ 我们可以用 $i_a\in[0,a)$ 和 $i_b \in [0,b)$ 重写$i=i_a+i_b\cdot a$ 。这将创建一个对应 $[0,n) \backsimeq[0,a) \times[0,b)$。另一种对应关系是 $k=k_b+k_a\cdot b$ 。将$i$和$k$带入下式。

$$
P_{k}=P(x_{n}^{k})=\sum_{i=0}^{n-1}a_{i}x_{n}^{ki}\quad k=0,1,\ldots,n-1
$$

需要提前明确的一点是公式中$x$既可以是原根又可以是单位根,不过为了统一表示省去了原根的取模操作。替换可得：

$$
\begin{align}
P_{k_{b}+k_{a}\cdot b}=\sum_{i_{a}=0}^{a-1}\sum_{i_{b}=0}^{b-1}a_{i_{a}+i_{b}\cdot a} \cdot x_{a\cdot b}^{(k_{b}+k_{a}\cdot b)(i_{a}+i_{b}\cdot a)}
\end{align}
$$

将$x$的幂次展开：

<div>
$$
\begin{align*}
x_{a\cdot b}^{(k_{b}+k_{a}\cdot b)(i_{a}+i_{b}\cdot a)}&=x_{a\cdot b}^{i_{a}k_{b}+i_{a}k_{a}\cdot b+i_{b}k_{b}\cdot a+i_{b}k_{a}\cdot ab} \\&=x_{a\cdot b}^{i_{a}k_{b}}\cdot x_{a\cdot b}^{i_{a}k_{a}\cdot b}\cdot x_{a\cdot b}^{i_{b}k_{b}\cdot a}\cdot x_{a\cdot b}^{i_{b}k_{a}\cdot ab}
\end{align*}
$$
</div>



根据原根的折半性质或者单位根的消去引理$x_{an}^{ak}=x_n^k$，可得：

$$
x_{a\cdot b}^{i_{a}k_{b}}\cdot x_{a\cdot b}^{i_{a}k_{a}\cdot b}\cdot x_{a\cdot b}^{i_{b}k_{b}\cdot a}\cdot x_{a\cdot b}^{i_{b}k_{a}\cdot ab}=x_{n}^{i_{a}k_{b}}\cdot x_{a}^{i_{a}k_{a}}\cdot x_{b}^{i_{b}k_{b}}\cdot1
$$

则公式(14)变为如下形式：

$$
P_{k_{b}+k_{a}\cdot b}=\sum_{i_{a}=0}^{a-1}\sum_{i_{b}=0}^{b-1}a_{i_{a}+i_{b}\cdot a}\cdot x_{n}^{i_{a}k_{b}}\cdot x_{a}^{i_{a}k_{a}}\cdot x_{b}^{i_{b}k_{b}}
$$

添加一些括号来确定运算顺序则变成了：

$$
P_{k_{b}+k_{a}\cdot b}=\sum_{i_{a}=0}^{a-1}\Bigg[\bigg(\sum_{i_{b}=0}^{b-1}a_{i_{a}+i_{b}\cdot a}\cdot x_{b}^{i_{b}k_{b}}\bigg)x_{n}^{i_{a}k_{b}} \Bigg] x_{a}^{i_{a}k_{a}}
$$

首先是做了一个长度为$b$的$DFT$操作，对结果乘上一个旋转因子后，对结果又做了一个长度为$a$的$DFT$操作。



‍

‍

## 参考文献

‍[1]七海. 基础知识：FFT - 简单入门[EB/OL]//七海の参考書. (2021-03-29)[2024-01-21]. https://shiraha.cn/2021/The-concept-of-fft-introducing-edition/.

[2]快速数论变换（NTT）及蝴蝶操作构造详解[EB/OL]//知乎专栏. [2024-01-21]. https://zhuanlan.zhihu.com/p/80297169.

[3]蒙哥马利算法(Montgomery Algorithm)|蒙哥马利约简、模乘、模幂_montgomery算法-CSDN博客[EB/OL]. [2024-01-28]. https://blog.csdn.net/weixin_46395886/article/details/112988136.

[4]蒙哥马利算法详解_蒙德马利法-CSDN博客[EB/OL]. [2024-01-28]. https://blog.csdn.net/zgzczzw/article/details/52712980.

[5]COOLEY J W, TUKEY J W. An Algorithm for the Machine Calculation of Complex Fourier Series[J].

[6]TOM NURKKALA. Cache-Friendly Matrix Transpose[Z/OL]. (2020-05-05)[2024-01-30]. https://www.youtube.com/watch?v=huz6hJPl_cU.

[7]BAILEY D H. FFTs in external of hierarchical memory[C/OL]//Proceedings of the 1989 ACM/IEEE conference on Supercomputing  - Supercomputing ’89. Reno, Nevada, United States: ACM Press, 1989: 234-242[2024-01-30]. http://portal.acm.org/citation.cfm?doid=76263.76288. DOI:[10.1145/76263.76288](https://doi.org/10.1145/76263.76288).

[8]Finite Field Implementations[Z/OL]. [2024-01-26]. https://docs.google.com/presentation/d/1I5QS58LtA3iiiPiVHHcN7oufCoo8sIDh9UvnJHWjA2Q.

[9]RISC ZERO. Finite Field Implementations: Barrett & Montgomery[Z/OL]. (2023-02-17)[2024-01-26]. https://www.youtube.com/watch?v=hUl8ZB6hpUM.

[10]AGARWAL R C, COOLEY J W. Fourier transform and convolution subroutines for the IBM 3090 Vector Facility[J/OL]. IBM Journal of Research and Development, 1986, 30(2): 145-161. DOI:[10.1147/rd.302.0145](https://doi.org/10.1147/rd.302.0145).

[11]董晓算法. G41 快速傅里叶变换 FFT算法 多项式乘法_哔哩哔哩_bilibili[EB/OL]. [2024-01-21]. https://www.bilibili.com/video/BV1Le4y1V78D/.

[12]董晓算法. G43 快速数论变换 NTT算法_哔哩哔哩_bilibili[EB/OL]. [2024-01-21]. https://www.bilibili.com/video/BV1a3411Z7vL/.

[13]HEIDEMAN M, JOHNSON D, BURRUS C. Gauss and the history of the fast fourier transform[J/OL]. IEEE ASSP Magazine, 1984, 1(4): 14-21. DOI:[10.1109/MASSP.1984.1162257](https://doi.org/10.1109/MASSP.1984.1162257).

[14]Hardcaml Zprize[EB/OL]. [2024-01-30]. https://zprize.hardcaml.com/ntt-top-level.html.

[15]BURRUS C. Index mappings for multidimensional formulation of the DFT and convolution[J/OL]. IEEE Transactions on Acoustics, Speech, and Signal Processing, 1977, 25(3): 239-242. DOI:[10.1109/TASSP.1977.1162938](https://doi.org/10.1109/TASSP.1977.1162938).

[16]MONTGOMERY P L. Modular Multiplication Without Trial Division[J].

[17]LIANG Z, ZHAO Y. Number Theoretic Transform and Its Applications in Lattice-based Cryptosystems: A Survey[M/OL]. arXiv, 2022[2024-01-30]. http://arxiv.org/abs/2211.13546.

[18]KNAUTH C, ADAS B, WHITFIELD D, 等. Practically efficient methods for performing bit-reversed permutation in C++11 on the x86-64 architecture[M/OL]. arXiv, 2017[2024-01-29]. http://arxiv.org/abs/1708.01873.

[19]KRAPIVENSKY V. Speeding up decimal multiplication[M/OL]. arXiv, 2020[2024-01-21]. http://arxiv.org/abs/2011.11524. DOI:[10.48550/arXiv.2011.11524](https://doi.org/10.48550/arXiv.2011.11524).

[20]LONGA P, NAEHRIG M. Speeding up the Number Theoretic Transform for Faster Ideal Lattice-Based Cryptography[A/OL]. (2016)[2024-01-30]. https://eprint.iacr.org/2016/504.

[21]REDUCIBLE. The Fast Fourier Transform (FFT): Most Ingenious Algorithm Ever?[Z/OL]. (2020-11-15)[2024-01-21]. https://www.youtube.com/watch?v=h7apO7q16V0.

