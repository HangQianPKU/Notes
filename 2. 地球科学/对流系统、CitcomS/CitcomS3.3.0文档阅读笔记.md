# CitcomS3.3.0文档阅读笔记

## 一.一些重要的问题

1. 在安装CitcomS3.1.1时，直接.\configure会报错（忘记复制报错内容了，大概是下载某些python的扩展内容失败），需要在configure时添加参数:

   ```shell
   $ ./configure --without-pyre --without-hdf5
   ```

   并且要注意的是，由于没有安装hdf5，尽管文档80页提到输入文件参数output_format可以设置ascii, ascii_gz或者hdf5作为参数，但选择参数hdf5会报错——相反，如果想要出图，vtk格式是可以的。

## 二.文档输出



2.

testcase.coord.*

96个文件，*从0到95

文档中说只有初始时间步输出一次，不懂为啥会输出那么多

按照文档的说法，三列分别是colatitude  longitude radius

每个文件35938行，好家伙

偶数文件输出的为下半层，奇数文件输出的为上半层

哦我明白了，**每个进程输出一个文件，表示这个进程计算的区域**



3.

textcase.domain 

只有第一步，0号进程会输出

```shell
$ file s5.domain
s5.domain: data
```

用vi打开是乱码——为二进制文件



但是比如前面的coord文件：

```shell
$ file s5.coord.00
s5.coord.00: ASCII text
```



4.

textcase.geoid.1.0

**太阴间了，参考文档写的是textcase.geoid.10——文档里出现了大量的10，咋回事儿，是版本的问题吗**

一共八列，分别是

输出232行，因为阶数到20

1+2+3+...+20= 231，第一行说明

degree(l)  

order(m) 

cosine efficient of total geoid at at l,m

sine coefficient of total geoid at l,m

cosine due to surface topography

sine due to surface topography

cosine due to perturbation of \rho

sine due to perturbation of \rho

小朋友脑子里充满了问号——为啥没有CMB的dynamical topography引起的扰动

密度扰动不是还需要一个参数r 吗，光给L,m也不够啊



5.

testcase.horiz.acg.*

文件有两个，分别为0.0与1.0

前者是深部（半径数值小）

每个文件33行——0.0的最后一行和1.0的第一行数据是一样的，合理

半径也就是深度方向分成了64个node嘛



四列——

radius 

temprature

RMS1

RMS2

现在还不知道RMS是啥

话说为啥你这个标题，平均，水平，会把温度的数据放在这里啊可索



6.

textcase.stress.*

共96个文件，a.0 a从0到95



每个文件39939行，去掉前两行的无效信息，刚好和前面的coord对上了,看来也是每个进程输出一个文件

文件的编号就是进程的编号——嗯，连输出顺序也是那个很挫的顺序，看来没错。

6列，代表应力张量的六个分量——我现在已经觉得应力矩阵说起来很别扭了

但是它这个顺序是真的诡异

\theta \theta

\phi \phi

r r

\theta \phi

\theta r

\phi r

为什么不按照r,\theta,\phi的顺序来排啊真是的



7.

textcase.surf.*  和  textcase.botm.*

直接放在一起说吧，差不多的

一共48个文件——隔一个进程输出一个进程

有意思的是，**偶数进程输出botm，奇数进程输出surf**，这是有什么讲究吗

一共是四列，输出的是边界的信息

topograpy

heat flux

volecity_colatitude

volecity_longitude



8.

textcase.visc.*

一共96个文件，从0到95

只有一列，输出黏度

35937行——对应于coordinate



9.

textcase.velo.*

一共96个文件，从0到95

四列：

v_\theta

v_\phi

v_r

还有一个temperature

自己内部的顺序倒是挺一致的

**这里的temperature是和坐标一一对应的，前面的温度是一层的平均值**，奥这么一想人家那个命名也没问题

hori_avg，确实应该把水平层的平均温度也加进来嘛



10.

文档里提到的pressure没有提到哎



11.

草，我把textcase.time删掉了

这个B真的没有数字后缀啊

溜了溜了，下次再肝

继续啊





<!--stackedit_data:
eyJoaXN0b3J5IjpbODU2MTk4NjEyXX0=
-->