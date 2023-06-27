## LDA和PCA的优劣？

## 发明者
Fisher搞出来的（方差分析也是他搞出来的），算是方差分析在机器学习中的应用？

## 和方差分析、f分析的联系
https://www.matongxue.com/madocs/2118/

## 与瑞利商、广义瑞利商的联系：

https://zhuanlan.zhihu.com/p/79696530

首先来看瑞利商的定义。瑞利商是指这样的函数 $R(A, x)$ :
$$
R(A, x)=\frac{x^H A x}{x^H x}
$$
其中， $x$ 是非零向量，而 $A$ 为 $n \times n$ 的Hermitan矩阵。所谓的Hermitan矩阵就是满足共轭转置 矩阵和自己相等的矩阵，即 $A^H=A$ ，例如， $A=\left(\begin{array}{cc}1 & 2+i \\ 2-i & 1\end{array}\right)$ 的共轭转置等于其本 身。当 $\mathrm{A}$ 为实矩阵时，如果满足 $A^T=A$ ，则 $A$ 为Hermitan矩阵Q。
瑞利商 $R(A, x)$ 有一个非常重要的性质，即它的最大值等于矩阵 $\mathrm{A}$ 最大的特征值，而最小值等于 矩阵 $A$ 的最小的特征值，也就是满足
$$
\lambda_{\min } \leq \frac{x^H A x}{x^H x} \leq \lambda_{\max }
$$
当向量 $x$ 是标准正交基 $\mathrm{Q}$ ，即满足 $x^H x=1$ 时，瑞利商退化为 $R(A, x)=x^H A x$ 。
下面看一下广义瑞利商。广义瑞利商是指这样的函数 $R(A, B, x)$ :
$$
R(A, B, x)=\frac{x^H A x}{x^H B x}
$$
其中 $x$ 为非零向量，而 $A, B$ 为 $n \times n$ 的 Hermitan矩阵。 $\mathrm{B}$ 为正定矩阵。它的最大值和最小值 是什么呢? 其实我们只要通过将其通过标准化就可以转化为瑞利商的格式。令 $x=B^{-1 / 2} x^{\prime}$ ，则 分母转化为:
$$
x^H B x=x^{\prime H}\left(B^{-1 / 2}\right)^H B B^{-1 / 2} x^{\prime}=x^{\prime H} B^{-1 / 2} B B^{-1 / 2} x^{\prime}=x^{\prime H} x^{\prime}
$$
而分子转化为:
$$
x^H A x=x^{\prime H} B^{-1 / 2} A B^{-1 / 2} x^{\prime}
$$
此时我们的 $R(A, B, x)$ 转化为 $R\left(A, B, x^{\prime}\right)$ :
$$
R\left(A, B, x^{\prime}\right)=\frac{x^{\prime H} B^{-1 / 2} A B^{-1 / 2} x^{\prime}}{x^{\prime H} x^{\prime}}
$$
利用前面的瑞利商的性质，我们可以很快的知道， $R\left(A, B, x^{\prime}\right)$ 的最大值为矩阵 $B^{-1 / 2} A B^{-1 / 2}$ 的最大特征值，或者说矩阵 $B^{-1} A$ 的最大特征值，而最小值为矩阵 $B^{-1} A$ 的最小特征值。

## 基本思想

组内方差最小，组件方差最大

基本基于正态分布

## 直观的例子

https://www.zhihu.com/question/34305879/answer/2182905513

## 投影的数学表达

出现投影的矩阵表达的地方：  PCA  LDA   Krylov子空间


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM4MDgyNjczN119
-->