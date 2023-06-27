# Data assimilation
weather research and forecast data assimilation( WRFDA)
基本问题：天气系统具有强烈非线性，哪怕模型完美，任意小的初值误差在足够长的演化后误差也会足够大，因此在天气预报等问题中，演化的动力学方程需要不断进行修正。
另一个问题是，观测值本身也存在误差

# 贝叶斯滤波

## 随机变量的贝叶斯统计
用一个例子来引入几个概念：先验概率，后验概率、似然概率
我们用离散的版本来简化问题。
假定需要测量某处的海平面温度（SST），服从一个分布（单位略去，摄氏度）：
$$
\left\{\begin{array}{l}
p(T=23)=0.8 \\
p(T=22)=0.2
\end{array}\right.
$$
这个分布不管是通过经验观察，还是通过模型计算得到，总之是在我们观测之前就知道的。因此称为**先验分布**。
接下来进行观测，例如观测值是 $T_m$ =22.2，那么要考虑的问题是：基于该观测值，该地海温是23和22的概率分别是多少？
我们先把公式写出来再解释;
由贝叶斯公式：
$$
p\left(B_i \mid A\right)=\frac{p\left(A \mid B_i\right) p\left(B_i\right)}{p\left(A\right)}=\frac{p\left(A \mid B_i\right) p\left(B_i\right)}{\sum_{j=1}^n p\left(A \mid B_j\right) p\left(B_j\right)}
$$
得到：
$$
p\left(T=23 \mid T_m=22.2\right)=\frac{p\left(T_m=22.2 \mid T=23\right) p(T=23)}{p\left(T_m=22.2\right)}
$$
$p\left(T_m=22.2\right)$再用全概率展开为：
$$p\left(T_m=22.2\right)=p\left(T_m=22.2 \mid T=23\right) p(T=23)+p\left(T_m=22.2 \mid T=22\right) p(T=22)$$
$p\left(T=23 \mid T_m=22.2\right)$称为后验概率，表达的意思是根据已有观测，估计现实世界；$p\left(T_m=22.2 \mid T=23\right)$称为似然概率，表达的是在现实世界发生某时间后，得到特定观测值的概率——这个概率一般是用来描述观测的精度。

分母的一大串是个归一化常数，直接记为$\eta$，则贝叶斯统计的基本想法可以看成：
$$
后验概率 =  \eta \times 先验概率 \times 似然概率$$

## 随机过程的贝叶斯统计

前面考虑的是随机变量的贝叶斯统计。

现在考虑一族具有前后相关性的随机变量，也就是随机过程的统计问题。在我们的建模中，这个随机过程一般是用来描述某个动力学过程，只不过在原本封闭（封闭意味着初值确定则任意时刻变量都唯一确定）的体系中，加入了考虑到模型精度和观测精度的随机因素。

模型是这样的：对于给定初值的随机变量$X-0$，会随着时间发展出一族随机变量$X-k$。
这些$X_k$表示的是真值，而每一个真值都会产生一个观测值$Y_k$

如果我们认为通过固定的观测方式和仪器，似然概率是确定的（事实上通常直接假设为一个高斯分布）。

同样，我们直接给出建模再进行解释：

$\left\{\begin{array}{l}X_k=f\left(X_{k-1}\right)+Q_k \\ Y_k=h\left(X_k\right)+R_k\end{array}\right.$

函数f表示从k-1状态到k状态的传递函数，描述该系统的动力学过程，例如在流体力学问题中，$X_{k-1}$就是k-1时刻系统的初值，f就是N-S方程以该调节为初值的方程的求解算子。
$Q_k$称为预测噪声，表达的是由于模型简化、机器精度等因素导致的预测误差。
函数h一般称为观测算子，用来在数学上描述观测这个过程，抽象的说是一个从状态空间到观测空间的映射。
最后是$R_k$，表示的是观测噪声。

这里比较重要的思想是，我们在一个确定轮的体系中加入了概率论的观点，在系统中引入随机性。

完整的滤波过程有两步;
首先是通过第一个方程，将上一步的不确定性传递到下一步，这个过程称为预测步；
在下一步中，则根据观测到的数据来减少对该时刻变量的不确定性，这个过程称为更新补；
最后用算出的后验概率作为下一步的先验概率。

对于具体的滤波过程推导，我们只简述一下。
假设：随机变量$X_0, Q_1, \cdots, Q_k, R_1, \cdots, R_k$相互独立。

还是用离散的版本推，连续的版本只需要把点概率和求和换成概率密度和积分即可。
对预测步：
$\begin{aligned} p\left(X_1=u\right) &=\sum_{v=-\infty}^{+\infty} p\left(X_1=u \mid X_0=v\right) p\left(X_0=v\right) \\ &=\sum_{v=-\infty}^{+\infty} p\left(X_1-f\left(X_0\right)=u-f(v) \mid X_0=v\right) p\left(X_0=v\right) \\ &=\sum_{v=-\infty}^{+\infty} p\left(Q_1=u-f(v) \mid X_0=v\right) p\left(X_0=v\right) \quad \text { (代入状态方程) } \\ &=\sum_{v=-\infty}^{+\infty} p\left(Q_1=u-f(v)\right) p\left(X_0=v\right) \quad\left(Q_k \text { 与 } X_{k-1} \text { 相互独立) }\right.\end{aligned}$

连续形式，用概率密度表达：

$$\tilde{f}_1(x)=\frac{d}{d x}\left(p\left(X_1<x\right)\right)=\int_{\mathbb{R}} f_{Q_1}[u-f(v)] f_0(v) d v$$

对于更新步，根据观测数据$Y = y_1$，先计算似然概率：

$\begin{aligned} f_{Y_1 \mid X_1}\left(y_1 \mid x\right) &=\lim _{\varepsilon \rightarrow 0} \frac{p\left(y_1<Y_1<y_1+\varepsilon \mid X_1=x\right)}{\varepsilon} \\ &=\lim _{\varepsilon \rightarrow 0} \frac{p\left(y_1-h(x)<Y_1-h\left(X_1\right)<y_1-h(x)+\varepsilon \mid X_1=x\right)}{\varepsilon} \\ &=\lim _{\varepsilon \rightarrow 0} \frac{p\left(y_1-h(x)<R_1<y_1-h(x)+\varepsilon \mid X_1=x\right)}{\varepsilon} \\ &=\lim _{\varepsilon \rightarrow 0} \frac{p\left(y_1-h(x)<R_1<y_1-h(x)+\varepsilon\right)}{\varepsilon} \quad\left(R_k \text { 与 } X_k \text { 相互独立 }\right) \\ &=f_{R_1}[y-h(x)] \end{aligned}$

以及连续变量贝叶斯公式：
$$f_{X \mid Y}(x \mid y)=\frac{f_{Y \mid X}(y \mid x) f_X(x)}{f_Y(y)}$$
得到：
$$\widehat{f_1}(x)=\eta f_{R_1}\left[y_1-h(x)\right] \tilde{f}_1(x)$$
$\eta$依然是归一化常数，具体写出来：
$$
\eta=\left[\int_{\mathbb{R}} f_{R_1}\left[y_1-h(x)\right] \widetilde{f_1}(x)\right]^{-1}
$$

总结起来，贝叶斯滤波的流程是：
$$
\begin{aligned}
f_0(x) & \rightarrow \tilde{f}_1(x)=\int_{\mathbb{R}} f_{Q_1}[x-f(v)] f_0(v) d v \quad(\text { 预测步) }\\
& \rightarrow \widehat{f}_1(x)=\eta_1 f_{R_1}\left[y_1-h(x)\right] \tilde{f}_1(x) \quad(\text { 更新步) }\\
& \rightarrow \tilde{f}_2(x)=\int_{\mathbb{R}} f_{Q_2}[x-f(v)] \widehat{f}_1(x) d v \quad(\text { (预测步 }) \\
& \rightarrow \widehat{f}_2(x)=\eta_2 f_{R_2}\left[y_2-h(x)\right] \tilde{f}_2(x) \quad(\text { 更新步 }) \\
& \rightarrow \cdots
\end{aligned}
$$

## 模型的实现
困难主要在于更新步需要求一个无穷积分。
如果是简单系统，积分的求解问题还不是很大，但当我们考虑地球的各个动力学系统时，状态传递函数f一般是NS方程或其他复杂非线性方程，这时候复杂表达式的无穷积分很难处理。
处理该问题有两个方向，一个是将f和h，以及观测噪声和预测噪声都做大量简化，这就是卡尔曼滤波一系列算法；
另一种角度则是想办法处理无穷积分的数值求解，这个角度则是粒子滤波算法的起源

## 卡尔曼滤波
在贝叶斯滤波的框架下：
$\left\{\begin{array}{l}X_k=f\left(X_{k-1}\right)+Q_k \\ Y_k=h\left(X_k\right)+R_k\end{array}\right.$
只需要假设f与h都是线性，并且Q和R都服从高斯分布，就得到了卡尔曼滤波。
即：
$$
\begin{aligned}
&\left\{\begin{array}{l}
X_k=F X_{k-1}+Q \\
y_k=h X_k+R
\end{array}\right. \\
&Q \sim N\left(0, Q^2\right), R \sim N\left(0, R^2\right)
\end{aligned}
$$
一维版本：
假设
$$
X_{k-1} \sim N\left(\widehat{\mu}_{k-1}, \widehat{\sigma}_{k-1}^2\right)
$$
由贝叶斯滤波的预测步公式：
$$
\begin{aligned}
\tilde{f}_k(x) &=\int_{\mathbb{R}} f_Q[x-f(v)] \widehat{f}_{k-1}(v) d v \\
&=\int_{\mathbb{R}}(2 \pi Q)^{-\frac{1}{2}} \exp \left(-\frac{(x-F v)^2}{2 Q^2}\right) \cdot\left(2 \pi \widehat{\sigma}_{k-1}\right)^{-\frac{1}{2}} \exp \left(-\frac{\left(v-\widehat{\mu}_{k-1}\right)^2}{2 \widehat{\sigma}_{k-1}^2}\right) d v
\end{aligned}
$$
得到：
$$\tilde{f}_k(x) \sim N\left(F \widehat{\mu}_{k-1}, F^2 \widehat{\sigma}_{k-1}^2+Q^2\right)$$
再设：
$$\tilde{f}_k(x) \sim N\left(\widetilde{\mu}_k, \tilde{\sigma}_k^2\right)$$
于是：
$\begin{aligned} \widehat{f}_k(x) &=\eta f_k\left(y_k-h x\right) \tilde{f}_k(x) \\ &=\eta(2 \pi R)^{-\frac{1}{2}} \exp \left[-\frac{\left(y_k-h x\right)^2}{2 R^2}\right]\left(2 \pi \tilde{\sigma}_k\right)^{-\frac{1}{2}} \exp \left[-\frac{\left(x-\tilde{\mu}_k\right)^2}{2 \tilde{\sigma}_k^2}\right] \\ \eta &=\left[\int_{\mathbb{R}}(2 \pi R)^{-\frac{1}{2}} \exp \left[-\frac{\left(y_k-h x\right)^2}{2 R^2}\right]\left(2 \pi \tilde{\sigma}_k\right)^{-\frac{1}{2}} \exp \left[-\frac{\left(x-\widetilde{\mu}_k\right)^2}{2 \tilde{\sigma}_k^2}\right] d x\right]^{-1} \end{aligned}$

用一个很有意思的引理：
Theorem 1：若 $f_X(x) \sim N\left(\mu_1, \sigma_1^2\right) ， f_{Y \mid X}(y \mid x) \sim N\left(\mu_2, \sigma_2^2\right)$ ，则 $f_{X \mid Y}(x \mid y) \sim N\left(\frac{\sigma_1^2}{\sigma_1^2+\sigma_2^2} \mu_2+\frac{\sigma_2^2}{\sigma_1^2+\sigma_2^2} \mu_1, \frac{\sigma_1^2 \sigma_2^2}{\sigma_1^2+\sigma_2^2}\right)$
可以得到：
$$
\widehat{f}_k(x) \sim N\left(\frac{h \tilde{\sigma}_k^2 y_k+R^2 \widetilde{\mu}_k}{h^2 \tilde{\sigma}_k^2+R^2}, \frac{R^2 \tilde{\sigma}_k^2}{h^2 \tilde{\sigma}_k^2+R^2}\right)
$$

记$K=\frac{h \tilde{\sigma}_k^2}{h^2 \tilde{\sigma}_k^2+R^2}$，称K为卡尔曼增益，得到卡尔曼滤波（一维版本）：
$$
\left\{\begin{array}{l}
\tilde{\mu}_k=F \widehat{\mu}_{k-1} \\
\tilde{\sigma}_k^2=F^2 \widehat{\sigma}_{k-1}^2+Q^2 \\
\widehat{\mu}_k=\widetilde{\mu}_k+K\left(y_k-h \widetilde{\mu}_k\right) \\
\widehat{\sigma}_k^2=(1-K h) \tilde{\sigma}_k^2
\end{array}\right.
$$
高维版本涉及比较麻烦的矩阵化简：

$$
\left\{\begin{array}{l}
\widetilde{\boldsymbol{\mu}}_k=\mathbf{F} \widehat{\boldsymbol{\mu}}_{k-1} \\
\widetilde{\Sigma}_k=\boldsymbol{F} \widehat{\Sigma}_{k-1} \boldsymbol{F}^T+Q \\
K=\widetilde{\Sigma}_{k-1} \boldsymbol{H}^T\left(\boldsymbol{H} \widetilde{\Sigma}_{k-1} \boldsymbol{H}^T+R\right)^{-1} \\
\widehat{\boldsymbol{\mu}}_k=\widetilde{\boldsymbol{\mu}}_k+K\left(\boldsymbol{y}_k-\boldsymbol{H} \widetilde{\boldsymbol{\mu}}_k\right) \\
\widehat{\Sigma}_k=(I-K \boldsymbol{H}) \widetilde{\Sigma}_k
\end{array}\right.
$$

# 变分
WRFAD中的变分方法本质上是在进行一个泛函的优化——不知道是不是和卡尔曼滤波的等价最小二乘描述有联系

<!--stackedit_data:
eyJoaXN0b3J5IjpbOTU5MDg4ODFdfQ==
-->