---
layout:     post
title:      梯度下降收敛速度
subtitle:    
date:       2019-03-29
author:     Tintin
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - Optimization
---
<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>
### 1. 梯度下降收敛速度

梯度下降迭代更新公式 

$$x_{k+1}=x_k-t\triangledown f(x_k)$$

**$$\bullet \quad$$根据Lipschitz连续**

$$
\begin{split}
f(x_{k+1}) \leq f(x_{k}) + \triangledown f(x_k)|x_{k+1}-x_k|+ \frac{L}{2}||x_{k+1}-x_k||^2
\end{split}
$$

$$ \leq f(x_{k}) - t||\triangledown f(x_k)||^2 + \frac{Lt^2}{2}||\triangledown f(x_k)||^2$$

令$$L=\frac{1}{t}$$

$$f(x_{k+1})\leq f(x_{k}) -\frac{t}{2}||\triangledown f(x_k)||^2 $$

根据一阶导数性质,替换$$f(x_k)$$为$$f(x^{*})$$： 

$$f(x^*)\geq f(x_k) +\triangledown f(x_k)(x^*-x_k)$$ 

$$f(x_k)\leq f(x^*)-\triangledown f(x_k)(x^*-x_k)$$

$$\therefore f(x_{k+1})-f(x^*) \leq \triangledown f(x_k)(x_k-x^*)-\frac{t}{2}||\triangledown f(x_k)||^2$$

$$\bullet \quad$$**不等式右边由以下推导**

$$
\begin{split}
||x_{k+1}-x^*||^2
&=||x_{k}-t\triangledown f(x_k)-x^*||^2\\
&= ||x_{k}-x^*||^2-2t\triangledown f(x_k)(x_{k}-x^*)+t^2||\triangledown f(x_k)||^2 \\
&= ||x_{k}-x^*||^2-2t (\triangledown f(x_k)(x_{k}-x^*)-\frac{t}{2}||\triangledown f(x_k)||^2) \\

\triangledown f(x_k)(x_k -x^*)-\frac{t}{2}||\triangledown f(x_k)||^2 &= \frac{1}{2t}(||x_{k}-x^*||^2-||x_{k+1}-x^*||^2 )
\end{split}
$$

$$\therefore f(x_{k+1})- f(x^*)\leq \frac{1}{2t}(||x_{k}-x^*||^2-||x_{k+1}-x^*||^2 )$$  

【注：mirror　descent“三点”性质可由该式推导出】

$$\bullet \quad$$**对每步迭代求和**<br>

$$
\begin{split}
\sum_{i=1}^{K}(f(x_{i})- f(x^*))&\leq \frac{1}{2t}(||x_{0}-x^*||^2-||x_{K}-x^*||^2)\\
&\leq \frac{1}{2t}||x_{0}-x^*||^2
\end{split}
$$

迭代完的位置与最优值之间的差值可以忽略不计，第一项占主导位置。

$$
\begin{split}
f(x_{k})- f(x^*)& \leq f(x_{k-1})- f(x^*)\leq ...f(x_{0})- f(x^*)\\
\\
f(x_{k})- f(x^*)& \leq \frac{1}{K}\sum_{i=1}^{K}(f(x_{i})- f(x^*)) \\
& \leq\frac{1}{2tK}||x_{0}-x^*||^2 =\frac{C}{K}
\end{split}
$$

$$\bullet \quad$$**如果想获得精度为$$\epsilon $$的解**，那么：

$$f(x_{k})- f(x^*)=\frac{C}{K}\leq \epsilon $$

$$K\geq \frac{C}{\epsilon}$$

即需要迭代$$O(\frac{1}{\epsilon})$$次，收敛速度为$$O(K)$$ <br>
注：步长$$t=\frac{1}{L}$$.上文中直接用

### 2. 次梯度下降收敛速度<br>
$$\bullet \quad$$**由凸函数一阶性质**：

$$f(x^*)\geq f(x_k)+\triangledown f(x_k)(x_{k+1}-x_k)$$

$$f(x_k) - f(x^*)\leq \triangledown f(x_k)(x_{k}-x_{k+1})$$

**$$\bullet \quad$$【同上】** 

$$
\begin{split}
||x_{k+1}-x^*||^2
&=||x_{k}-t\triangledown f(x_k)-x^*||^2\\
\\
&= ||x_{k}-x^*||^2-2t\triangledown f(x_k)(x_{k}-x^*)+t^2||\triangledown f(x_k)||^2
\end{split}
$$

$$ 2t\triangledown f(x_k)(x_{k}-x^*) = ||x_{k}-x^*||^2 - ||x_{k+1}-x^*||^2 +t^2||\triangledown f(x_k)||^2$$

**$$\bullet \quad$$【同上】求和**,其中根据lipschitz有$$\triangledown f(x)\leq G$$：

$$
\begin{split}
\sum_{i=1}^{K}(f(x_{i})- f(x^*))\leq \frac{||x_{0}-x^*||^2+t^2G^2k}{2t}
\end{split}
$$
 
$$f(x_{k})- f(x^*) \leq \frac{1}{k}\sum_{i=1}^{k}(f(x_{i})- f(x^*))=\frac{||x_{0}-x^*||^2+t^2G^2k}{2tk}
$$

$$\bullet \quad$$**$$\epsilon-$$最优解：**

$$\frac{R^2}{2tk}+\frac{tG^2}{2}\leq \epsilon $$

令两部分都等于$$\frac{\epsilon}{2}$$,那么：

$$
\begin{split}
t&=\frac{\epsilon}{G^2}\\
k&=\frac{R^2}{\epsilon t}=\frac{G^2R^2}{2\epsilon^2}
\end{split}
$$

收敛速度为$$O(\frac{1}{\epsilon^2})$$

### 3. 总结

|        |适用问题 |Lipschitz约束|$$f(x_{k})- f(x^*) \leq$$| 步长$$t$$ | 收敛速度 |
| ------ |:------: | :------: | :------: | :------: |
| $$GD$$| 凸函数且可导  | $$\|f'(x)-f'(y)\|\leq L\|x-y\|$$ | $$\frac{\|\|x_{0}-x^*\|\|^2}{2tk}$$ | $$\frac{1}{L}$$ | $$O(\frac{1}{\epsilon})$$ |
| $$subGD$$| 凸函数不可导 | $$\|f(x)-f(y)\|\leq G\|x-y\|$$ | $$\frac{\|\|x_{0}-x^*\|\|^2+t^2G^2k}{2tk}$$ | $$\frac{\epsilon}{G^2}$$ | $$O(\frac{1}{\epsilon^2})$$ |