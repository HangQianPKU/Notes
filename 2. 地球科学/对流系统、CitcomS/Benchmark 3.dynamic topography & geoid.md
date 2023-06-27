# Benchmark 3- dynamic topography & geoid

### dynamics topography:

定义为：
$$
s = -\frac{\sigma _{rr-t}}{\delta \rho g_0}\\
b = \frac{\sigma _{rr-b}}{\delta \rho g_0}\\
$$
![](C:\Users\Administrator\Documents\md\Citcom\动力地形.png)

这玩意儿逻辑是这样的：在数值计算中，我们是不追踪网格边界的变化的——当然也有追踪网格边界变化的算法，但是比较复杂，内存需求非常大。主要是我不会（x)。现在考虑速度场的第二类齐次边界条件（也就是法向应力为零，free slip），那么如果边界没有变化——边界处的应力就应该是0。但事实上不是——因为实际上边界是在运动的嘛。于是我们可以认为，在地表，高出的部分有：

$$
\delta\rho g_0 s = \sigma_{rr-t}
$$
于是：
$$
s =\frac{\sigma _{rr-t}}{\delta \rho g_0}
$$
不过上式取了负号，表示负地形？也可能是密度差表示上层物质（这里是空气）与下层物质之差，于是就带一个负号了。



关于地幔就比较奇怪了，
$$
\sigma_{rr-b} = \delta\rho g_0 b - \rho_{core}\varphi_b
$$


### 大地水准面：

参考内容是《Geodynamics》



![](C:\Users\Administrator\Documents\md\Citcom\大地水准面.jpg)

我们给大地水准面异常geoid的定义为：
$$
h = \frac{\varphi}{g_0}
$$
上图中，U_mo是在**参考重力势定义出的大地水准面高度测得**的重力势，U_0是参考重力势实际对应的等势面，于是有：
$$
U_0 = U_{m0} + \frac{\partial U}{\partial r}\Delta N \approx U_{m0} + g_0 h
$$
而重力势异常根据定义为：
$$
\Delta U = U_{m0}-U_0=-g_0 h
$$
**因此大的密度（导致重力势异常为负、别忘了势能前面有负号）会导致大地水准面的升高**
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0MDYwODAxMl19
-->