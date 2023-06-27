# Benchmark 2 无量纲化与边界条件

## 无量纲化：

-  **引入的基本物理量**：
 $$
  R:地球半径 \\
  \kappa : thermal \quad diffusity \\
  \eta:黏度
  $$
  **我们可以看到，引入这三个物理量后Ra的表达式就是我们熟悉的形式——是否在自然对流问题中，一般都是这样来无量纲化呢？**

$$
r = Rr'
$$

- **距离（标准）尺度R为地球半径,注意，不是地幔深度**。这在之后计算Ra时有影响

$$
t = \frac{R^2}{\kappa}t'
$$

- 这里的时间尺度采用的是热扩散——**这样的结果是，无量纲化后的时间数值往往很小**

$$
\vec u = \kappa /R \vec u'
$$

- 速度尺度与时间尺度一致

$$
T = \Delta T T' + T_0
$$

- **$\Delta T$为CMD与surface temperature $T_0$ 之差**

$$
\eta = \eta _r \eta  '
$$

- **$\eta _r$ 是需要引入的参考黏度**

$$
P = \frac{\eta _r \kappa}{R^2}P'
$$

这里值得注意一下，压力的参考尺度中含有热扩散系数$\kappa$

Zhong.2008中，其值定义为，温度为CMD与surface温度的平均值时的黏度：
$$
\eta = \exp{E(0.5-T)}
$$
这个关系还假定了黏度与温度的指数依赖关系。

一般地，CitcomS程序中的  rheol   参数定义了黏度与其他量的依赖关系



通过标准尺度的引入，方程可化为：

连续性方程：
$$
\nabla ·\vec u = 0
$$
动量方程：
$$
\nabla·(-PI+\eta(\nabla \vec v + \nabla ^{T}\vec v)) + \xi Ra(T-BC)\hat z = 0
$$
其中：
$$
Ra = \frac{\alpha\rho_0g_0\Delta T d^3 }{\kappa \eta_r} \\
B = \frac{\Delta \rho}{\alpha \rho_0 \Delta T} \\
\xi = (\frac{R}{d})^3
$$


## 边界条件

待解的物理量：温度；速度；压强

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUzMzg3MDI5MF19
-->