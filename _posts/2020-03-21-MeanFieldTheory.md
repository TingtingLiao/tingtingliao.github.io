---
layout:     post
title:      Mean Field Theory Solution of the Ising Model
subtitle:    
date:       2020-03-21
author:     Tintin
header-img: img/home-bg.jpg
catalog: true
tags:
    - Physics
    - Main Filed Theory
    - Boltzman Distribution 
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>


 <!-- https://www.lagou.com/lgeduarticle/3296.html -->
 <!-- https://zhuanlan.zhihu.com/p/31426458 -->

### 一、相变
* **相**： 具有均匀物理性质的热力学系统。如水固相、液相、气相。
* **相变**： 从一种相转变成另一种相。如冰融化成水是固-液相变，铁磁-顺磁相变，超导体-正常导体相变，几何相变（渗流），量子相变等。
* **相变产生因素？**  
导致相变产生的物理量有：温度T、压强P等。这些物理量本质上影响的是①物质内部粒子间的相互作用 ②粒子自身的热运动。
* **如何描述相？**    
1. 状态转换成能量。 Hamiltonian 哈密尔顿量 $$H$$ 表示某一状态 $$\Gamma$$ 下系统能量。  

$$H=H(\Gamma)$$

2. 能量转换成概率。即出现这一状态的概率，这就是玻尔兹曼分布。

$$F(state) \propto  e^{-\frac{H(state)}{kT}}$$ 

$$ p =  \frac{e^{-\frac{H}{kT}}}{Z}    \quad \quad Z = \sum_{\Gamma} e^{-\frac{H(\Gamma)}{kT}}$$ 

* $$k$$ 为玻尔兹曼常数。 
* $$Z$$ 为配分函数(partition function). 所有状态下能量和，可看做是归一化函数。 $$Z$$ 为定值。
* $$p$$ 与$$ H$$ 成反比。系统中粒子间对抗性越强，能量越高，相变概率高，该状态出现概率低。
* $$p$$ 与 $$T$$ 成正比。

### 二、 Ising model
#### 2.1 哈密尔顿量 H
伊辛模型用于描述物质磁铁性模型。
![](\img\MeanFieldTheory\ising1.png)
假设有 $$N $$个粒子，每个粒子有两个自旋方向$$ s \in \{-1,+1\} $$分别表示向下和向上，系统共有 $$2^N$$ 种状态。状态 $$s$$ 下系统哈密尔顿量由两部分组成：

$$H(s)=-J\sum_{ \left \langle i,j \right \rangle }s_is_j -h\sum_{i=1}^N s_i \\$$

* 相邻粒子对间作用。 $$J $$是正常数，表示粒子对之间的磁铁性， $$\left \langle i,j \right \rangle $$ 表示所有响铃自旋对。若 $$i, j $$同号能量小$$ -s_is_j $$ 为负数。若 $$i, j $$异号对抗性大，能量大 $$-s_is_j $$正数。
* 外场对粒子作用。 $$h$$ 为沿 $$z$$ 方向的磁场。 

#### 2.2 配分函数 Z

$$Z = \sum_{\Gamma} e^{-\beta H} \quad \quad (\beta=\frac{1}{kT})$$

$$\sum_{\Gamma} = \sum_{s_1=\pm 1}\sum_{s_2=\pm 1}\dots\sum_{s_N=\pm 1} =\prod_{i=1}^N\left ( \sum_{s_i \in \{+1,-1\}} \right )$$

$$\begin{split} \sum_{\Gamma} \rightarrow Tr \end{split}$$

$$Tr$$表示所有可能状态之和。

$$Z = Tr(e^{-\beta H})$$ 

**配分函数$$ Z$$ 需要计算$$ 2^N $$个状态，随着 N 增加，计算量呈指数增长。而且当维度扩展到高维空间时，这种计算方式就无法求解。有没有一种方法能够在任意维度下都是通用的，且计算更简单？平均场理论。**

### 三、Weiss Molecular Filed Theory
#### 3.1 分解Hamiltonian

$$\begin{split} s_i=\left \langle s_i \right \rangle\:&+ \quad  \delta_i\\  [mean] &\quad  [fluctuation] \\  \delta_i = s_i-\: &\left \langle s_i \right \rangle \\ \left \langle s_i \right \rangle  = m\quad&  \end{split} 
$$

将自旋取值用均值和波动表示，均值是整个系统粒子取值的期望，那么每个粒子不同的则是波动部分。根据上式，粒子间的作用可以转化为：

$$\begin{split} s_is_j &= (\left \langle s_i \right \rangle + \delta_i)(\left \langle s_j \right \rangle + \delta_j)\\ & \approx \left \langle s_i \right \rangle\left \langle s_j \right \rangle + \left \langle s_i \right \rangle \delta_j +\left \langle s_j \right \rangle \delta_i \quad \quad (\delta_i\delta_j很小忽略不计)\\ &=m^2 + m(s_j - m) + m(s_i-m)\\ &=m(s_i+s_j-m)\\ \end{split}$$

将其代入 $$H$$ 中，

$$\begin{split} H&=-J\sum_{\left \langle i,j \right \rangle}s_is_j-h\sum_{i}^{N}s_i\\ &= -J\sum_{\left \langle i,j \right \rangle}m(s_i+s_j-m)-h\sum_{i}^{N}s_i\\ &= -mJ\sum_{\left \langle i,j \right \rangle}(2s_i-m)-h\sum_{i}^{N}s_i\\ \end{split} $$

其中 $$\sum_{\left \langle i,j \right \rangle} $$是对所有相邻粒子对求和，可以进一步表示为以下形式， $$neighbor(i) $$表示粒子$$i$$的邻居，所有的点都会计算两次所以系数为 $$\frac{1}{2}$$ 。而求和只与$$i$$ 有关，因此 $$\sum_{j\in neighor(i)} $$为邻居个数 $$q$$ ,一维空间 $$q=2$$ ，二维空间 $$q=4 $$，三维空间 $$q=6$$ .

$$ \sum_{\left \langle i,j \right \rangle}=\frac{1}{2}\sum_{i=1}^N\sum_{j\in neighor(i)}=\frac{q}{2}\sum_{i=1}^N$$

继续对 $$H$$ 进行简化： 

$$\begin{split} H&=-mJ\sum_{\left \langle i,j \right \rangle}(2s_i-m)-h\sum_{i}^Ns_i\\ &=-\frac{qmJ}{2}\sum_{i=1}^N(2s_i-m)-h\sum_{i}^{N}s_i\\ &=\frac{Nqm^2J}{2}-(qmJ+h)\sum_{i=1}^Ns_i\\ \end{split}$$

原来的哈密尔顿量$$ H$$ 需要对电子对之间作用$$ s_is_j $$以及外场作用 $$s_i $$分别求和。经过平均场变换过后，只需对 $$s_i $$求和，无需考虑电子对之间的相互作用。**简而言之，用平均场代替所有其它粒子对该粒子的相互作用，粒子之间无需进行交互，所有粒子只与平均场交互，使得多体问题转变为单体问题。**

$$H =\frac{Nqm^2J}{2}-h_{eff}\sum_{i=1}^Ns_i\quad [h_{eff}=qmJ+h]$$

注：整个式子中m是未知的。

#### 3.2 配分函数 Z

$$\begin{split} Z&=Tr(e^{-\beta H})\\  &=\prod_{i=1}^{N}\sum_{s_i \in \{-1, 1\}} e^{-\frac{\beta NqJm^2}{2}}e^{\beta h_{eff}\sum_{i=1}^{N}s_i} \quad  \quad \\ &=e^{-\frac{\beta NqJm^2}{2}} \sum_{s_1\in \{-1, 1\}}\dots\sum_{s_n \in \{-1, 1\}}(e^{\beta h_{eff}s_1}e^{\beta h_{eff}s_2}\dots e^{\beta h_{eff}s_n})\\ &=e^{-\frac{\beta NqJm^2}{2}}\prod_{i=1}^{N}\sum_{s_i \in \{-1, 1\}}  e^{\beta h_{eff}s_i}& \\ &=e^{-\frac{\beta NqJm^2}{2}}\prod_{i=1}^{N}( e^{\beta h_{eff}} + e^{-\beta h_{eff}}) \\ &=e^{-\frac{\beta NqJm^2}{2}}(2\cosh(\beta h_{eff}))^N\quad \quad[\cosh x=\frac{e^{x}+e^{-x}}{2}]\\ \end{split}$$

#### 3.3 求解平均场M

$$m$$ 表示整个系统的平均值, $$\left \langle s_i \right \rangle$$ 表示粒子$$ i$$ 的平均值，注：这里 $$\left \langle s_i \right \rangle $$不再是$$ m $$而是粒子$$ i $$实际计算出来的均值。

$$m=\frac{1}{N}\sum_{i=1}^N \left \langle s_i \right \rangle$$

粒子 $$i$$ 的平均值表示，所有状态下粒子 i 取值与其概率乘积总和。以下忽略下标 $$i$$ ,粒子 $$s $$的均值： 

$$\begin{split}  \left \langle s \right \rangle & = \sum_{\Gamma} ps\\ &= Tr(ps)\\  &=Tr( \frac{e^{-\beta H}}{Z}s)\\  &=\frac{1}{Z}Tr(se^{-\beta H}) \\  \end{split} \\ \begin{split}  m&=\frac{1}{N}\sum_{i=1}^N \left \langle s_i \right \rangle\\  &=\frac{1}{N}\frac{1}{Z}\sum_{i=1}^N Tr(s_ie^{-\beta H})\\   &=\frac{1}{N}\frac{1}{Z} Tr(\sum_{i=1}^Ns_ie^{-\beta H})\\  \end{split} $$

到这里好像不知道怎么进行化简了。而 $$Z=Tr(e^{-\beta H}) $$与[1]式有相似的形式，少了一项 $$\sum_{i=1}^N s_i$$， 可以对$$ H $$求导得到$$\sum_{i=1}^N s_i = -\frac{\partial H}{\partial h_{eff}} $$

$$\begin{split} Tr(\sum_{i=1}^N(s_ie^{-\beta H}))&\rightarrow  Tr(e^{-\beta H}\sum_{i=1}^Ns_i ) \quad  [1]\\ &\rightarrow Tr(-e^{-\beta H} \frac{\partial H}{\partial h_{eff}} )\\ &\rightarrow Tr(\frac{1}{\beta}\frac{\partial e^{-\beta H} }{\partial h_{eff}} )\\ &\rightarrow \frac{1}{\beta}\frac{\partial Tr(e^{-\beta H}  )}{ \partial h_{eff}}\\ & \rightarrow\frac{1}{\beta} \frac{\partial Z}{ \partial h_{eff}} \end{split}$$

从而 

$$\begin{split} m&=\frac{1}{N}\frac{1}{Z}\frac{1}{\beta} \frac{\partial Z}{ \partial h_{eff}}\\ &=\frac{1}{N}\frac{1}{\beta} \frac{\partial \ln Z}{ \partial h_{eff}}\\ \end{split} 
$$

对 Z 的化简形式取对数求导： 

$$\begin{split} Z &=e^{-\frac{\beta NqJm^2}{2}}(2\cosh(\beta h_{eff}))^N\quad \\  \ln Z &=-\frac{\beta NqJm^2}{2}+N\ln2 + N\ln \cosh(\beta h_{eff})\\ \frac{\partial \ln Z}{ \partial h_{eff}}&= N\beta \tanh (\beta h_{eff})  \end{split}$$

即可得到最终的 $$m$$ ，形式特别优美

$$\begin{split} m&=\tanh (\beta h_{eff})\\ m&=\tanh (\beta(qJm+h)) \quad \beta=\frac{1}{kT} \end{split}$$

这个式子中右边也有 $$m$$ , 很难对方程直接求解，给定参数 $$\beta, q,J,h $$可以通过画图的方式寻找两函数交点. 我们考虑 $$h=0$$ 即没有外场作用的情况，这时方程求解有两种情况：

* $$\beta qJ\leq 1\:(kT\geq qT)$$：有唯一解 $$m=0$$ 系统为顺磁态。
* $$\beta qJ> 1\:(kT<qT)$$：有三个解 $$m=0$$ 以及$$m=\pm m_0, m_0\leq 1， m_0$$ 时候系统为铁磁态。而 $$m=0 $$时，系统中粒子不再与平均场交互，系统随机性很强不稳定，因此我们只考虑 $$m=\pm m_0$$ 两种解。

上述我们可以看到，临界状态为 $$\beta qJ=1$$,即 

$$kT=qJ$$

一维空间 $$q=2,kT=2J$$ ；二维空间 $$kT=4J$$ ；
#### 3.4 总结 
根据上面的推导过程，给定参数，从而确定平均场 $$m$$ 的值，进而计算出Hamiltonian和配分函数 

$$\begin{split} m&=\tanh (\beta h_{eff})\\ H &=\frac{Nqm^2J}{2}-h_{eff}\sum_{i=1}^Ns_i\\ Z&=e^{-\frac{\beta NqJm^2}{2}}(2\cosh(\beta h_{eff}))^N \\ \end{split}$$

通过平均场理论， $$H$$ 与 $$Z$$ 的计算大大简化了，并且可以适用于高维空间2D，3D等。

