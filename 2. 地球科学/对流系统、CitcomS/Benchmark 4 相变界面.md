# Benchmark 4 相变界面

密度扰动的原因可以分为三部分：热膨胀、相变以及物质不均一：
$$
\delta \rho = -\alpha\rho_0 (T-T_0) + \delta\rho_{ph}\Gamma + \delta\rho_{ch}C
$$
这部分考虑**相变导致的密度扰动**



## CitcomS中相变的数学处理

这部分考虑**相变的密度扰动的处理**：

相变产生的密度变化近乎跳变，数学上的处理方式是使用Sigmond函数:
$$
S(x) = \frac{1}{1+e^{-x}}
$$
其函数图像如下：

![sigmond函数](C:\Users\Administrator\Documents\md\Citcom\sigmond函数.png)

可见，其函数值在x>0与x<0处迅速趋近于1和0，是对于跳变函数：
$$
f(x)= \left\{ 
\begin{array}{11}
0   &  x<0\\
1 & x>1
\end{array}
\right .
$$
的近似。

Citcom中，使用的函数为：
$$
\frac{1}{2}(1+tanhx) = \frac{1}{2}(1+ \frac{e^x - e^{-x}}{e^x +e^{-x}})\\
= \frac{e^x}{e^x+e^{-x}} = \frac{1}{1 + e^{-2x}} = S(2x)
$$
$\Gamma$则定义为：
$$
\frac{1}{2}(1+tanh(\frac{\pi}{\rho_0gw_{ph}})) = S(2\frac{\pi}{\rho_0gw_{ph}})
$$
其中分母$\rho_0gw_{ph}$应该是一个scale，其中$w_{ph}$为相变界面的厚度。

**分子$\pi$则为反映考察范围是否为相变界面之下的量**。推导如下：

假设P-T相图上的相变界面曲线斜率（即克拉伯龙斜率为固定），相变曲线方程为：
$$
\rho_0g(r-1+d_{ph}) = \gamma_{ph}(T-T_{ph})
$$
于是定义：
$$
\pi \xlongequal{\Delta} = \rho_0g(1-d_{ph}-r)+\gamma(T-T_{ph})
$$
该量在相变曲线上为0，**大于零时表示该位置比相变界面深，发生相变。**

- 分母的scale：$\rho_0gw_{ph}$:

在相变界面的上下边界,即
$$
 r = 1-d_{ph} \pm r/2
$$
时，有：
$$
2\frac{\pi}{\rho_0gw_{ph}}=  \pm 1
$$



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc0MzkxODkxM119
-->