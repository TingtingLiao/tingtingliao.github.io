---
layout:     post
title:      Variance Reduction 
subtitle:    
date:       2019-05-07
author:     Tintin
header-img: img/post-bg-swift.jpg
catalog: true
tags:
    - Optimization
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

本文证明了在凸与非凸两种情况下，方差缩减算法的梯度方差期望的bound。 以及在目标函数分别为：强凸、非强凸、非凸组合$$f_i(x)$$与非强凸函数$$f(x)$$三种情况下SVRG的收敛速率。

# 1. Lemma  

#### **Lemma 1**：【$$L-smooth$$】

  $$g(x_{k+1}) \leq g(x_k)-\frac{1}{2L}\|\triangledown g(x_k)\|^2$$  

**Proof**:

$$
\begin{split}
 g(x_{k+1})&\leq g(x_k) + \triangledown g(x_k)(x_{k+1}-x_k) + \frac{L}{2}||x_{k+1}-x_k||^2\\
&\leq g(x_k)-\eta ||\triangledown g(x_k)||^2 + \frac{L\eta^2}{2}||\triangledown g(x_k)||^2\\
& \leq g(x_k)-\frac{1}{2L}||\triangledown g(x_k)||^2
\end{split}
$$

第一个不等式根据$$L-smooth$$,第二个不等式根据$$x_{k+1}-x_k = -\eta \triangledown g(x_k)$$,第三个不等式取$$\eta = \frac{1}{L}$$.


#### **Lemma 2**：【凸】 

$$  \|\triangledown f(x_k)- \triangledown f(x^*)\|^2 \leq 2L (f(x_k)-f(x^*)-\triangledown f(x^*)(x_k-x^*))$$

**Proof**: 假若有凸函数$$f(x)$$,$$令 g(x) = f(x)-f(x^*)-\triangledown f(x^*)(x-x^*) $$,根据凸性质$$\forall x\in R^d$$有$$ g(x)\geq 0$$,根据定理1:

 $$
 \begin{split}
 0=g(x^*)&\leq g(x_{k+1}) =  g(x_k)-\frac{1}{2L}||\triangledown g(x_k)||^2\\
 ||\triangledown f(x_k)- \triangledown f(x^*)||^2 &\leq 2L (f(x_k)-f(x^*)-\triangledown f(x^*)(x_k-x^*))
 \end{split}
 $$

#### **Lemma 3**： 【非凸】 

 $$  \|\triangledown f(x_k)- \triangledown f(x^*)\|^2 \leq 4(L+l) (f(x_k)-f(x^*)-\triangledown f(x^*)(x_k-x^*)) + (4l^2+2Ll)\|x_k-x^*\|^2$$

下面先回顾非凸定义再给出证明。我们知道函数$$f(x)$$是L-smooth满足：

$$- \frac{L}{2}\|y-x\|^2\leq f(y) - f(x)+\left \langle \triangledown f(x),y-x \right \rangle \leq \frac{L}{2}\|y-x\|^2$$

下面我们定义$$L-$$upper smooth以及$$l-$$lower smooth满足：

$$- \frac{l}{2}\|y-x\|^2\leq f(y) - f(x)+\left \langle \triangledown f(x),y-x \right \rangle \leq \frac{L}{2}\|y-x\|^2$$

举个例子，凸函数有$$0-$$ lower smooth,$$L-smooth$$函数有$$L-$$upper smooth和$$L-$$lower smooth,一个凸且L光滑函数有$$0-$$ lower smooth和$$L-$$upper smooth。

这里需要注意的是，当函数$$f(x)$$的lower smooth$$l=0$$时函数是凸的，当$$l>0$$时是非凸的。

**Proof**: 对于非凸函数$$f(x)$$,$$令 g(x) = f(x)-f(x^*)-\triangledown f(x^*)(x-x^*)+\frac{l}{2}\|x_k-x^*\|^2$$这时$$g(x)$$依旧为凸函数且为$$(L+l)-smooth$$：

 $$
 \begin{split}
 0 &\leq   g(x_k)-\frac{1}{2(L+l)}||\triangledown g(x_k)||^2\\
 ||\triangledown f(x_k)- \triangledown f(x^*)+l(x_k-x^*)||^2 &\leq 2(L+l) (f(x_k)-f(x^*)-\triangledown f(x^*)(x_k-x^*)+\frac{l}{2}\|x_k-x^*\|^2)
 \end{split}
 $$

下面计算$$\|\triangledown f(x_k)- \triangledown f(x^*)\|^2$$

$$
 \begin{split}
 \|\triangledown f(x_k)- \triangledown f(x^*)]\|^2 &\leq 2 \|\triangledown f(x_k)- \triangledown f(x^*)+l(x_k-x^*)\|^2 +2\|l(x_k-x^*)\|^2 \\
 &\leq 4(L+l) (f(x_k)-f(x^*)-\triangledown f(x^*)(x_k-x^*)) + (2Ll+4l^2)\|x_k-x^*\|^2
\end{split}
 $$

# 2. Varience Bound 

**Proof**:

$$\tilde{\triangledown}f(x_k) = \triangledown_i f(x_k) - \triangledown_i f(\tilde{x})+\triangledown f(\tilde{x})$$

$$
\begin{split}
E||\tilde{\triangledown}f(x_k) - \triangledown f(x_k)||^2 &= E ||\triangledown_i f(x_k) - \triangledown_i f(\tilde{x})+\triangledown f(\tilde{x})-\triangledown f(x_k)||^2 \\
&  \leq  E ||\triangledown_i f(x_k) - \triangledown_i f(\tilde{x})||^2 \\
& \leq 2E ||\triangledown_i f(x_k) - \triangledown_i f(x^*)||^2 +2E||\triangledown_i f(\tilde{x}) -\triangledown_i f(x^*)||^2 \\
&=  2 ||\triangledown  f(x_k) - \triangledown  f(x^*)||^2 +2 ||\triangledown  f(\tilde{x}) -\triangledown  f(x^*)||^2 \\
\end{split}\\
$$

第一个不等式根据$$ E\|\zeta - E\zeta \|^2 = E\|\zeta\|^2 - \|E \zeta\|^2\leq E\|\zeta\|^2$$,第二个不等式根据$$\|a^2\pm b^2\| \leq 2\|a\|^2+2\|b\|^2$$
 
#### 2.1 Convex

当$$f(x)$$为凸函数时，根据Lemma 2，其中$$\triangledown f(x^*) = 0$$可以得到：

$$E||\tilde{\triangledown}f(x_k) - \triangledown f(x_k)||^2 \leq  4L (f(x_k)-f(x^*) + f(\tilde{x})-f(x^*)  $$


#### 2.2 Nonconvex 

当$$f(x)$$为非凸函数时，根据Lemma 3，其中$$\triangledown f(x^*) = 0$$可以得到：

$$E||\tilde{\triangledown}f(x_k) - \triangledown f(x_k)||^2 \leq 8(L+l) (f(x_k)-f(x^*)+f(\tilde{x})-f(x^*)) + (8l^2+4Ll)(\|x_k-x^*\|^2 +\|\tilde{x}-x^*\|^2)  $$


# 3. SVRG++收敛速率

### 3.1 Non Strongly Convex Objective

**Problem Definition：**

$$ \min_x F(x) := \frac{1}{n} \sum_{i=1}^{n} f_i (x) + \psi(x)$$

$$f(x)$$为凸函数且$$L-smooth$$,$$\psi(x)$$不光滑.当$$\psi(x)=0$$上式依旧成立。

**Key Lemma:**

$$F(x_{k+1})-F(x^*)\leq \frac{ \eta}{2(1-\eta L)}\|\tilde{\triangledown} f(x_k)-\triangledown f(x_k)\|^2 + \frac{\|x_k-x^*\|^2-\|x_{k+1}-x^*\|^2}{2\eta} $$

**Proof**：

$$
\begin{split}
F(x_{k+1})-F(x^*) &= f(x_{k+1})-f(x^*)+\psi(x_{k+1})-\psi (x^*)\\
&\leq f(x_k) - \triangledown f(x_k)(x_{k+1}-x_k) +\frac{L}{2}\|x_{k+1}-x_k \|^2-f(x^*)+\psi(x_{k+1})-\psi (x^*) \\
&\leq  \left \langle \triangledown f(x_k), x_k-x^* \right \rangle - \triangledown f(x_k)(x_{k+1}-x_k) +\frac{L}{2}\|x_{k+1}-x_k \|^2+\psi(x_{k+1})-\psi (x^*)
\end{split} 
$$

第一个不等式根据**L-smooth**，第二个不等式根据**凸性值**。

$$
\begin{split}
& \left \langle \triangledown f(x_k), x_k-x^* \right \rangle +\psi(x_{k+1})-\psi (x^*)\\
\leq& \left \langle \tilde{\triangledown} f(x_k), x_k-x_{k+1} \right \rangle +\left \langle \tilde{\triangledown} f(x_k), x_{k+1}-x^* \right \rangle +\left \langle  g, x_{k+1}-x^* \right \rangle \\
=& \left \langle \tilde{\triangledown} f(x_k), x_k-x_{k+1} \right \rangle + \left \langle \tilde{\triangledown} f(x_k)+g, x_{k+1}-x^* \right \rangle \\
=   & \left \langle \tilde{\triangledown} f(x_k), x_k-x_{k+1} \right \rangle +  \left \langle -\frac{1}{\eta}(x_{k+1} -x_k), x_{k+1}-x^* \right \rangle\\ 
=&\left \langle \tilde{\triangledown} f(x_k), x_k-x_{k+1} \right \rangle + \frac{\|x_k-x^*\|^2}{2\eta}-\frac{\|x_{k+1}-x^*\|^2}{2\eta}-\frac{\|x_{k+1}-x_{k}\|^2}{2\eta}
\end{split} 
$$

其中第一个不等式根据$$ \psi(x)$$的凸性质，$$g\in \partial \psi(x)$$是次梯度;第二个等号根据近似梯度$$x_{k+1} = \min_y \{ \triangledown f(x_k)(y-x_k) + \frac{1}{2\eta} \|y-x_k\|^2 + \psi(y) \}$$，其满足$$\triangledown f(x_k) +\frac{1}{\eta}(x_{k+1} -x_k) +g = 0 $$。当$$\psi(x)=0$$时，$$\triangledown f(x_k) +\frac{1}{\eta}(x_{k+1} -x_k)= 0 $$，上式依成立。结合上式子可以得到：

$$
\begin{split}
&F(x_{k+1})-F(x^*)\\
\leq & \left \langle \tilde{\triangledown} f(x_k)-\triangledown f(x_k), x_k-x_{k+1} \right \rangle-\frac{1-\eta L}{2\eta}\|x_{k+1}-x_{k}\|^2 + \frac{\|x_k-x^*\|^2-\|x_{k+1}-x^*\|^2}{2\eta}\\
\leq & \frac{ \eta}{2(1-\eta L)}\|\tilde{\triangledown} f(x_k)-\triangledown f(x_k)\|^2 + \frac{\|x_k-x^*\|^2-\|x_{k+1}-x^*\|^2}{2\eta}
\end{split}
$$

第二个不等式根据杨氏不等式$$\left \langle a, b \right \rangle \leq \lambda \frac{\|a\|^2}{2}+ \frac{1}{\lambda}\frac{\|b\|^2}{2}$$，最后利用SVRG方差**Key Lemma**替换第一项即可得到：

<!-- $$ F(x_{k+1})-F(x^*)\leq  \frac{ 2\eta L}{ (1-\eta L)}(F(x_k)-F(x^*)+F(\tilde{x})-F(x^*))   + \frac{\|x_k-x^*\|^2-\|x_{k+1}-x^*\|^2}{2\eta} $$

后面再对内循环外循环进行求和即可得到最终收敛速度。 -->

<!-- ### 3.2 Strongly-Convex  

**Problem Definition:**

$$ \min_x \frac{1}{n} \sum_{i=1}^{n} f_i (x) +\psi(x) $$

**Proof:**

$$
\begin{split}
E||x_{k+1}-x^*||^2 &= E||x_{k}-\eta \tilde{\triangledown} f(x_k)-x^*||^2\\
&=||x_{k}-x^*||^2-2\eta {\triangledown} f(x_k)(x_{k}-x^*)+\eta^2E||\tilde{\triangledown} f(x_k)||^2 \\
&\leq ||x_{k}-x^*||^2-2\eta(f(x_k)-f(x^*))+4L\eta^2 (f(x_k)-f(x^*)+f(\tilde{x})-f(x^*))\\
& = ||x_{k}-x^*||^2-2\eta (1-2L\eta)(f(x_k)-f(x^*))+4L\eta^2 (f(\tilde{x})-f(x^*)) 
\end{split} 
$$

不等式第二项根据凸函数性质，第三项根据上面方差的bound。

我们固定外循环$$s$$,内循环经过$$m$$次迭代可得：


$$||x_{s+1}-x^*||^2 + 2m\eta (1-2L\eta)(f(x_{s+1})-f(x^*)) \leq|| {x_s}-x^*||^2 +4Lm\eta^2 (f(x_s)-f(x^*))$$ 

等式第一项根据**强凸**性质，等式左边第一项消去。

$$2m\eta (1-2L\eta)(f(x_{s+1})-f(x^*)) \leq (\frac{2}{\gamma} + 4Lm\eta^2) (f(x_s)-f(x^*)) $$

$$f(x_{s+1})-f(x^*) \leq (\frac{1}{\gamma m\eta (1-2L\eta)}+\frac{2L\eta}{1-2L\eta} ) (f(x_s)-f(x^*))$$

根据等比数列即可得：

$$E[f(\tilde{x}_s)-f(x^*)] \leq \alpha^s [f(\tilde{x}_0)-f(x^*)]$$

$$SVRG$$的推导主要利用估计方差期望的bound，之后根据强凸性质得到循环是等差序列，对单次内循环求和后得到相邻外循环是等比数列，并得到最终结果。凸函数的bound是利用目标函数差值作为bound。而非凸函数是对点间距离作为bound。

 -->



### 3.3 $$\sigma-$$Strongly Convex

本节中讨论当$$f_i(x)$$为非凸函数，而它的平均$$f(x):=\frac{1}{n}\sum_{i=0}^{n-1}f_i(x)$$为凸函数且为$$\sigma-$$强凸时的收敛速率。

**Key Lemma**:

$$F(x_{k+1})-F(x^*)\leq \frac{ \eta}{2(1-\eta L)}\|\tilde{\triangledown} f(x_k)-\triangledown f(x_k)\|^2 + \frac{(1-\sigma\eta)\|x_k-x^*\|^2-\|x_{k+1}-x^*\|^2}{2\eta} $$

**Proof**:

$$
\begin{split}
F(x_{k+1})-F(x^*) &= f(x_{k+1})-f(x^*)+\psi(x_{k+1})-\psi (x^*)\\
&\leq f(x_k) - \triangledown f(x_k)(x_{k+1}-x_k) +\frac{L}{2}\|x_{k+1}-x_k \|^2-f(x^*)+\psi(x_{k+1})-\psi (x^*) \\
&\leq  \left \langle \triangledown f(x_k), x_k-x^* \right \rangle -\frac{\sigma}{2}\|x_k-x^*\|^2- \triangledown f(x_k)(x_{k+1}-x_k) +\frac{L}{2}\|x_{k+1}-x_k \|^2+\psi(x_{k+1})-\psi (x^*)
\end{split} 
$$

第一个不等式根据**L-smooth**，第二个不等式根据$$\sigma-$$**强凸性质**。比非强凸多了一项$$\frac{\sigma}{2}\|x_k-x^*\|^2$$,后面相同即只需在3.1节**Key Lemma**中加上这一项即可。

 
