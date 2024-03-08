---
layout:     post
title:     梯度下降与镜像下降线性耦合 
subtitle:  Linear Coupling:An Ultimate Unification of Gradient and Mirror Descent
date:       2019-04-25
author:     Tintin
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - optimization
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

本文来源于[^footnote]Allen-Zhu的Gradient-Mirror Coupling，将$$MirrorDescent$$和$$GradientDescent$$组合起来设计了一个全新的加速方法，并将其扩展到不能用moment等加速方法的场景下。

### 1. Online Learning Regret

在线学习最主要的一个限制是只能看到当前和过去的数据，未来是未知的，有可能完全颠覆现在的认知。因此，在线学习所追求的是直到所有数据所能设计的最优策略。同这个最优策略的差异成为regret。

$$Regret_T(h^*) = \sum_{t=1}^T (l(h(x_t),y_t)-l(h^*(x_t),y_t))  $$

$$l$$表示损失函数,$$h,h^*$$分别表示当前策略和最优策略。$$T$$表示当前第$$T$$个回合，online learning每个回合都会有一个新数据，随着时间增加数据变多，$$Regret$$也会变小。

作者引入此概念用以描述求解过程中，当前更新的点$$x_t$$与最优点$$x^*$$之间的目标函数$$f(x)$$的差值：

$$\begin{split}
R_k(x^*)  \equiv \sum_{t=0}^{k-1} (f(x_t)-f(x^*))& = \sum_{t=0}^{k-1} f(x_t)- T f(x^*) \\
& =   k f( \overline{x})- k f(x^*)  \\
& =   k(f( \overline{x})-f(x^*)  )
\end{split}
$$

而根据Convexity：

$$\begin{split}
f(x^*)&\geq f(x)+ \left \langle \triangledown f(x),x^*-x  \right \rangle \\

&\geq \frac{1}{k}\sum_{t=0}^{k-1}f(x_t)+ \frac{1}{k}\sum_{t=0}^{k-1}\left \langle \triangledown f(x_t),x^*-x_t  \right \rangle \\
&\geq f(\overline{x})+\frac{1}{k}\sum_{t=0}^{k-1}\left \langle \triangledown f(x_t),x^*-x_t  \right \rangle 【琴生不等式】 \\
f(\overline{x})-f(x^*)& \leq \frac{1}{k}\sum_{t=0}^{k-1}\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle
\end{split} 
$$

所以：

$$R_T(x^*)  \equiv \sum_{t=0}^{T-1} (f(x_t)-f(x^*)) =  T(f( \overline{x})-f(x^*)  ) \leq \sum_{t=0}^{T-1}\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle \quad (1.1)  $$

后面的收敛速率都是由该式推导而出的。只用求出$$f( \overline{x})-f(x^*) \leq \epsilon $$时所需步数$$T$$即可。
 

### 2. 对偶问题：镜像下降

**【Mirror Descent】**：

$$\widetilde{x}=Mirr_x(\alpha *\partial f(x)) $$

$$Mirr_x(\varepsilon )=\min_y \{D(x,y)+\left \langle \varepsilon,y-x\right \rangle  \}$$

由上节可知，求收敛速度，很重要一步是求出$$\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle$$,根据**三点性质**有：

$$
\begin{split}
||x_{k+1}-x^*||^2
&=||x_{k}-\alpha \triangledown f(x_k)-x^*||^2\\
&= ||x_{k}-x^*||^2-2\alpha\triangledown f(x_k)(x_{k}-x^*)+\alpha^2||\triangledown f(x_k)||^2 \\
\end{split}
$$

$$\therefore \alpha \triangledown f(x_k)(x_{k}-x^*) = \frac{\alpha^2}{2}||\triangledown f(x_k)||^2 + D(x_k,x^*) -D(x_{k+1},x^*)$$

关于**Bregman divergence**的距离函数$$D(x,y)$$，详细解释见[这里](http://127.0.0.1:4000/2019/04/09/Proximal-Descent/).

根据<font color="blue">(1.1)式</font>，$$MD$$中的$$Regret$$：

$$
\begin{split}
\alpha T(f( \overline{x})-f(x^*)  ) &\leq \alpha \sum_{t=0}^{T-1}\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle \\
& \leq  \frac{\alpha^2}{2}\sum_{k=0}^{T-1} ||\triangledown f(x_k)||^2 + D(x_0,x^*) -D(x_{T},x^*) \\
& \leq  \frac{\alpha^2}{2}\sum_{k=0}^{T-1} ||\triangledown f(x_k)||^2 + D(x_0,x^*) -D(x_{T},x^*) 
\end{split}
$$

令$$\rho = \frac{1}{T}\sum_{k=0}^{T-1}\|\triangledown f(x_k)\|^2$$为梯度平方均值，那么：

$$\begin{split}
\alpha T(f( \overline{x})-f(x^*)) \leq \frac{\alpha^2}{2} T \rho^2 + D(x_0,x^*) \quad (2.1)
\end{split}
$$

* 若$$f(x)$$为$$L-smooth$$,取$$\alpha = \frac{1}{L}$$,有：

$$f( \overline{x})-f(x^*) \leq \frac{\rho^2}{2L} + \frac{LD(x_0,x^*)}{T} $$

令$$\frac{LD(x_0,x^*)}{T}\leq \epsilon$$，收敛速率$$T \geq \frac{LD(x_0,x^*}{\epsilon})$$

* 若$$f(x)$$为$$non-smooth$$,令$$\alpha = \frac{\sqrt{2D(x_0,x^*)}}{\rho \sqrt{T}}$$,由（2.1）式得：

$$f( \overline{x})-f(x^*) \leq \frac{2D(x_0,x^*)}{\alpha T} = \frac{\sqrt{2D(x_0,x^*)}· \rho}{ \sqrt{T}}  $$

达到$$\epsilon$$精度，收敛速率$$T \geq \frac{2D(x_0,x^*) \rho^2}{\epsilon^2 }  $$。

在非光滑的情况下，$$MD$$的收敛速度是$$O(\frac{1}{\epsilon^2})$$，比$$GD$$慢一个量级，而在光滑的情况下两者都是$$O(\frac{1}{\epsilon})$$。



#### Mirror Descent与Gradient Descent联系

为了更好的$$MD$$与$$GD$$之间的联系，这里假设它们的目标函数都是$$L-smooth$$的：

$$ f(x_{k+1}) \leq f(x_{k}) + \triangledown f(x_k)|x_{k+1}-x_k|+ \frac{L}{2}||x_{k+1}-x_k||^2$$

$$
\begin{split}
GD(x)&= argmin_y \{  \triangledown f(x)|y-x|+ \frac{L}{2}||y-x||^2\} \\
x_{k+1} &\leftarrow x_k -  \frac{1}{L} \triangledown f(x_k) \\
\\
\\
MD(\alpha\triangledown f(x))&=argmin_y \{  \alpha\triangledown f(x)|y-x|+ D(x,y)\} \\
&= argmin_y \{  \alpha\triangledown f(x)|y-x|+ \frac{L}{2}||y-x||^2\}
\end{split} \\
x_{k+1} \leftarrow x_k - \alpha \frac{1}{L} \triangledown f(x_k)
$$

$$GD$$的收敛定理是minimize一个quadratic upper bound[证明见这里](https://tintin.space/2019/03/29/Convergence/)，而$$MD$$是minimize dual averaging lower bound.

$$ f(x_k)-f(x^*)\leq \frac{L\|x_0-x^*\|^2}{2T} 【GD】$$

$$\alpha T(f( \overline{x})-f(x^*)  )  \leq \alpha \sum_{t=0}^{T-1}\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle  
  \leq  \frac{\alpha^2}{2}\sum_{k=0}^{T-1} ||\triangledown f(x_k)||^2 + D(x_0,x^*) -D(x_{T},x^*) 【MD】 $$

当$$f(x)$$光滑，距离函数为欧式距离且$$\alpha=1$$时，$$MD$$和$$GD$$等价。由此我们也可以从一个high level角度解释$$MD$$和$$GD$$，当$$\|\triangledown f(x_k)\|$$特别大的时候，$$GD$$更新的距离也很大，而当$$\|\triangledown f(x_k)\|$$很小即函数很平缓时，相反$$MD$$会表现更好，此时它的$$Regret$$比较小bound也更加严格，而$$\alpha$$也是随着时间的变化在逐步调节获得更好的收敛。

因此我们很容易想到，是否可以<font color='red'>结合梯度下降和镜像下降得到一个更快速的一阶算法？</font>

### 3. Linear Coupling
![](/img/LinearCoupling/Algorithm.png)

$$
\begin{split}
MD\&GD \quad x_{k+1}& \leftarrow \tau_k MD(x_{k})+(1-\tau_k)GD(x_{k}) \\
 x_{k+1}&\leftarrow \tau_k z_{k+1}+(1-\tau_k)y_{k+1}
\end{split}
$$

同样地，我们需要先求出$$\alpha \triangledown f(x_{k+1})(x_{k+1}-x^*)$$,但Linear Coupling不是$$MD$$,而是$$MD$$和$$GD$$的线性组合，因此我们需要将其拆开：

$$\alpha \triangledown f(x_{k+1})(x_{k+1}-x^*) = \alpha \triangledown f(x_{k+1})(x_{k+1}-z_{k}) +\alpha \triangledown f(x_{k+1})(z_{k}-x^*)$$

**第二项**$$z_k$$满足$$MD$$(2.1)式:

$$
\begin{split}
\alpha \triangledown f(x_{k+1})(z_{k}-x^*) &= \frac{\alpha^2}{2}||\triangledown f(x_{k+1})||^2 + D(z_k,x^*) -D(z_{k+1},x^*)  \\
& \leq \alpha^2 L (f(x_{k+1})-f(y_{k+1})) + D(z_k,x^*) -D(z_{k+1},x^*) 
\end{split}
$$

$$L-smooth$$梯度下降中满足([证明见这里](https://tintin.space/2019/03/29/Convergence/)):

$$f(x_k)-f(x_{k+1})\geq \frac{1}{2L}||\triangledown f(x_k)||^2$$

Linear Coupling中$$y_{k+1}$$是$$x_{k+1}$$经过$$GD$$的下一步，因此：

$$f(x_{k+1})-f(y_{k+1})\geq \frac{1}{2L}||\triangledown f(x_{k+1})||^2$$

$$\therefore 第二项 \leq \alpha^2 L (f(x_{k+1})-f(y_{k+1})) + D(z_k,x^*) -D(z_{k+1},x^*) $$

**第一项**：

$$
\begin{split}
\alpha \triangledown f(x_{k+1})(x_{k+1}-z_{k}) =& \alpha \triangledown f(x_{k+1})(x_{k+1}-\frac{x_{k+1}-(1-\tau_k)y_{k} }{\tau_k})  \\
=& \alpha \triangledown f(x_{k+1}) \frac{1-\tau_k }{\tau_k}(y_{k}-x_{k+1})\\
\leq & \alpha \frac{1-\tau_k }{\tau_k} (f(y_k) -f(x_{k+1})) 【一阶凸性质】
\end{split} 
$$

令$$\frac{1-\tau_k }{\tau_k} = \alpha L $$,两式相加得:

$$\alpha \triangledown f(x_{k+1})(x_{k+1}-x^*) \leq \alpha^2 L (f(y_k)-f(y_{k+1})) + D(z_k,x^*) -D(z_{k+1},x^*)   $$

根据<font color="blue">(1.1)式</font>, Linear Coupling的$$Regret$$:

$$
\begin{split}
\alpha T(f( \overline{x})-f(x^*)) &\leq \alpha \sum_{t=0}^{T-1}\left \langle \triangledown f(x_t),x_t-x^*  \right \rangle  \\
 &\leq \alpha^2 L (f(y_0)-f(y_{T})) + D(z_0,x^*) -D(z_{T},x^*) \\
f( \overline{x})-f(x^*) &\leq \frac{1}{T} (\alpha L d + \frac{D(z_0,x^*)}{\alpha} )
\end{split} 
$$

其中$$d=f(y_0)-f(y_{T})$$,令$$\alpha = \sqrt{\frac{ D(z_0,x^*)}{Ld}}$$,

$$f( \overline{x})-f(x^*) \leq \frac{2\sqrt{LdD(z_0,x^*)}}{T}  $$

当$$T = 4\sqrt{\frac{LD(z_0,x^*)}{d}}$$时，$$f( \overline{x})-f(x^*) \leq \frac{d}{2}$$,即当前点的距离到最优点距离变成一半.即每个epoch的距离依次为$$d,\frac{d}{2},\frac{d}{4},...$$,那么收敛速率：

$$T = O(\sqrt{\frac{LD(z_0,x^*)}{\epsilon}}+\sqrt{\frac{LD(z_0,x^*)}{2\epsilon}}+\sqrt{\frac{LD(z_0,x^*)}{4\epsilon}}+...) =O(\sqrt{\frac{LD(z_0,x^*)}{\epsilon}})$$

Linear Coupling的收敛速度$$\frac{1}{\sqrt{\epsilon}}$$和Nesterov加速方法一样，比$$GD$$快了一个量级。其中步长是固定的$$\alpha = \sqrt{\frac{ D(z_0,x^*)}{Ld}}$$随着时间的增加$$\alpha$$变大，我们取$$\tau=\frac{1}{\alpha L+1} $$逐渐减小，也就是说随着当前点离最优点越来越近的时候，$$\alpha$$变小，$$\tau$$变大，$$GD$$更新能力逐渐变弱而$$MD$$逐渐变强。

<!-- 这也印证了[赵老师](https://zhuanlan.zhihu.com/p/35323828)椭圆的例子，在求解凸且光滑的二次函数$$\min_{x,y} \frac{1}{2}(0.09x^2+y^2)$$时，强凸$$\mu=0.09$$,光滑$$L=1$$,$$GD$$在一开始会以$$y$$轴为主导方向进行梯度更新步长为$$\frac{1}{L}=1$$,在$$y$$趋近于0之再沿着$$x$$方向更新，这时梯度变小，本应该用一个更大的步长$$\frac{1}{0.09}$$，那么mirror descent这时就派上用场了，根据上面的分析，在凸且光滑的情况下它的步长是$$GD$$的$$\alpha$$倍即$$\alpha \frac{1}{L}$$。根据这样理解的话我觉得$$Gradient-Mirror Coupling$$可以解释为一种自适应步长的算法，通过调整步长在$$GD$$后期达到加速的作用。 -->

但是它有三个缺点：1）$$\alpha$$的值取决于$$D(z_0,x^*)$$;2)$$d$$初始化好坏会影响收敛；3）算法需要重新开始，即在每个epoch开始时都需要重新初始化参数。

为了解决这几个问题，Zheyuan将$$\alpha，\tau$$设为随着时间变化的参数不需要额外初始化其它参数,$$\alpha_{t+1} = \frac{t+2}{2L},\tau_t=\frac{1}{\alpha L+1}=\frac{2}{t+2}$$,收敛速率为$$O(\sqrt{\frac{4LD(z_0,x^*)}{\epsilon}})$$

优点：1）Linear Coupling不同于常见的momentum加速方法，但是它和一般的加速方法有着相同的收敛速度； 2）利用Mirror Descent应用于非Euclidean Distance场景中。

[^footnote]: [Allen-Zhu Z, Orecchia L. Linear coupling: An ultimate unification of gradient and mirror descent[J]. arXiv preprint arXiv:1407.1537, 2014.](https://arxiv.org/pdf/1407.1537.pdf).
