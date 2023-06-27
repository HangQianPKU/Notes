# Benchmark 1.控制方程

## 控制方程

### 连续性方程

使用不可压缩流体的假设：
$$
\frac{d \rho}{dt} = 0
$$
注意不是说密度均一，而是给定流体微元的密度不随时间改变

### 动量方程

deried from N-S方程的动量方程：
$$
\rho\frac{d\vec v}{dt} = 2(\mu S -\frac{1}{3}\mu(\nabla·\vec v))+\mu'(\nabla·\vec v)-pI + \rho \vec f
$$


使用的假设：

1）.不可压缩流体的假设（化简应力张量项），参数第二黏性系数$\lambda'$没了

2）.惯性项忽略（黏性无穷大假设）
$$
\frac{d \vec v}{dt} = 0
$$
3.）体力项为重力：
$$
\vec f = \rho \vec g = \rho \nabla \Phi
$$
这部分又可以进一步分解为基准值与扰动：
$$
\rho \vec g = (\rho _0 + \delta \rho)(\nabla \Phi _0 + \nabla \delta \Phi ) \\
\xlongequal{舍去二阶小量} \rho _0 \nabla \Phi _0 +( \delta \rho \nabla \Phi _0 +\rho _0 \nabla\delta\Phi)
$$
其中：
$$
\Delta \Phi _0 = -4\pi G \rho _0 \\
\Delta \delta \Phi = -4\pi G \delta \rho
$$
上面的式子需要引入$\delta(·)$来严格的证明。（积分意义下的广义函数）

**这里奇怪的一点是，上面关于$\delta \Phi$ 的式子看出，它只和和这一点的密度扰动有关**

**这很反直觉啊？不是应该和整个地球的各个位置的密度变化都有关吗？而且文章里也将了关于重力势的扰动和Surface以及CMD都有关啊？**



另外
$$
\rho _0 \nabla \Phi _0 +( \delta \rho \nabla \Phi _0 +\rho _0 \nabla\delta\Phi)
$$
的前一项
$$
\rho _0 \nabla \Phi _0
$$
与无运动时的静压强是平衡的，即：
$$
-\nabla p_0I +\rho_0\nabla \phi_0 = 0
$$
于是方程化为：
$$
\mu\nabla((\nabla\vec v+\nabla ^T \vec{v})-pI)+(\delta \rho \nabla \Phi _0 +\rho _0 \nabla\delta\Phi) = 0
$$
**注意这里的压强$p$已经是该状态下的压强静压强之差了**,大概就是叫他dynamics pressure的原因八





接下来，我们可以定义reduced pressure
$$
P = p + \rho _0  \delta\Phi _0
$$
所以说这里的压强经过两次约化，大概叫他reduced dynamics pressure比较合理（x)



结合以上几点假设和化简，就有：




$$
0 = 2\mu S -pI + \rho \vec f
   = \mu \nabla(\nabla \vec v + \vec v \nabla) - \nabla P -\alpha \delta T \rho _0 \nabla \Phi _0
$$
这里把密度扰动写成了仅由温度引起。



考察密度扰动$\delta \rho$更精细结构，密度扰动主要是三部分导致：

1.热膨胀导致的密度下降，这部分可以写成：
$$
-\alpha (T-T_0) \rho _0
$$
其中$\alpha$ (热膨胀系数)的定义：
$$
\alpha = \frac{1}{V}\frac{\partial V}{\partial T}\\
= \frac{\rho}{m}\frac{\partial{m/ \rho}}{\partial T} \\
= - \frac{1}{\rho}\frac{\partial \rho}{\partial T}
$$
2.物质不均一引起的密度变化

3.相变引起的密度变化

后面两点放在之后讨论

### 能量方程

N-S方程的能量方程原本比较复杂：
$$
\rho \frac{dU}{dt} = P:S +\nabla (\kappa \nabla T)+ \rho q
$$
化简为：
$$
\rho _0 C_p(\frac{\partial T}{\partial t}+ \vec v\nabla T) = \kappa \Delta T + \rho _0 q
$$


这里采用的假设：

- 流体变形的面力做功$P:S$为0（有什么实际的物理背景吗？）
- 内能完全由温度决定，并且比热$C_p$为常数
- 温度用基准温度$\rho _0 $代替

我们在这里忽略了阻碍流动产生的**摩擦生热**以及流体运动时压力做功导致的**压缩生热**，这几项大概被包括在$P:S$里面了。

$$
n \mathrm{~d} t=\frac{r \mathrm{~d} r}{a \sqrt{a^{2} e^{2}-(a-r)^{2}}}
$$
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDc3OTEyODIzXX0=
-->