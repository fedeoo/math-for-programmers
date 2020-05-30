# 概率分析

在对算法分析的过程中，我们会发现部分算法的运行时间依赖于输入数据，比如插入排序，在最好的情况下运行时间是 $O(n)$ 最坏情况下运行时间是 $O(n^2)$。对于这种情况，我们更关注的是它的平均情况运行时间。这篇文章就带大家回忆下高中部分的排列组合以及概率。

## 排列组合
对于古典概率来说，我们需要经常用到排列组合知识计算样本空间数量，所以需要先回忆一下这部分内容。
从 n 个不同元素中，任意取出 m 个进行排序，总的排列数目为 

$A_{n}^{m} = \frac{n!}{(n-m)!}$

从 n 个不同元素中取出 m 个元素构成一个集合的话，则集合的数目为

$C_{n}^{m} = \frac{n!}{m!(n-m)!}$

思考：n 个人坐在圆形会议桌的方法有多少种？

### 插入排序概率分析
当对 n 个不同的数进行排序时，待处理的数据就是 n 个数的排列，可能的情况有 $n!$  这个数据刚好有序的概率是 $1/n!$。    
插入排序的运行时间与逆序对数目有关，逆序对的数目期望是为 n(n-1)/4 可以这样理解，总共有 n(n - 1)/2 对，每一对是逆序对的概率是 1/2。

### 快排概率分析
快排最坏情况下划分子问题时刚好每次划分 n - 1 个元素和 0 个元素。用递归式表示就是

$T(n) = T(n-1) + \Theta(n)$

这时运行时间复杂度为 $\Theta(n^2)$ 

最好情况下的划分两个子问题规模分别是 $\left \lfloor n/2 \right \rfloor$ 和 $\left \lceil n/2 \right \rceil -1$ 递归式为 

$T(n) = 2T(n/2) + \Theta(n)$

根据主定理，$T(n)=\Theta(nlgn)$

假设每次划分总是 9 : 1 这种情况下，我们有递归式

$T(n) = T(9n/10) + T(n/10) + \Theta(n)$

比较容易证明这种情况下算法的运行时间依然是 $O(nlgn)$

事实上，80% 以上的划分都比 9 : 1 更平衡。这个比较容易证明，对于更一般的情况，对于任何常数 $0 < \alpha \leqslant 1/2$，算法产生比 $1 - \alpha : \alpha$ 更平衡的划分的概率约为 $1 - 2\alpha$。证明过程比较简单，我们考察一下区间 $[0, 0.5]$， 落在区间 $[\alpha, 0.5]$ 的概率为 $\frac{0.5 - \alpha}{0.5 - 0} = 1 - 2\alpha$，落在这个区间的划分都要比以 $\alpha$ 为界划分的好。 对于 $\alpha = 0.1$ 的情况，我们可以说 80% 以上的划分都比 9 : 1 更平衡。

快速排序还有一种改进的划分：三数取中。假设我们定义一个好的划分介于 [n/3, 2n/3] 之间，我们想比较一下，改进的算法获得好划分的概率增加了多少。对于平凡实现，获得好划分的概率是 1/3。下面我们来计算下改进的划分是好划分的概率。   
我们假定 A 是数组，$A'$ 是已排好序的数组，用三数取中法选择主元 x，并定义 $p_i=Pr{x = A'[i]}$。$p_i$ 也就是划分位置在排序数组$A'$ i 的概率，当然 $p_1 = p_n = 0$。 我们知道 n 数取 3 总共有 $C_n^{3}$，划分在 i 位置的种类有 $C_{i - 1}^{1} * C_{n - i}^{1}$ 所以有

$p_i = \frac{C_{i - 1}^{1}C_{n - i}^{1}}{C_n^{3}} = \frac{6(i - 1)(n - i)}{n(n - 1)(n - 2)}$

好划分的概率为 

$$\begin{aligned}
P & = \sum_{i = n/3}^{2n/3}\frac{C_{i - 1}^{1}C_{n - i}^{1}}{C_n^{3}} \\\\
  & \approx \int_{n/3}^{2n/3}\frac{6(n-x)(x-1)}{n(n - 1)(n - 2)}dx \\\\
  & = \frac{6(3n^3/18 - 7n^3/81 + 3n^2/18 - n^2/3)}{n(n - 1)(n - 2)} \\\\
\end{aligned}$$

当 n 趋于无穷大时，概率为 $\frac{13}{27}$


## 总结
概率论在人工智能中经常会用到，所涉及的内容也更深入一些。算法分析中涉及到的概率论部分比较简单，大多数的分析用高中所学的概率知识已经可以对付了。这一篇还会继续更新，后续会探讨更多的概率论与统计相关的应用。