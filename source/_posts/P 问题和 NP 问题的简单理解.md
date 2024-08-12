[P/NP问题 | 维基百科](https://zh.wikipedia.org/zh-cn/P/NP问题)

### P 问题

P 问题的定义是：所有可以由一个确定型图灵机在多项式表达的时间内解决的问题

P 代表 Polynomial-time (adj. 多项式时间)

> 简单理解：答案可以很快被计算出来的问题

### NP 问题

NP 问题的定义是：所有可以在多项式时间内验证它的解是否正确的决定问题

N 代表 Non-deterministic (非确定性的)

> 简单理解：问题的答案可以很快验证的问题。或者说，问题的答案不一定可以很快计算出来，但是如果给你一个问题的答案，你可以很快验证这个答案对不对

现在科学家们不确定 P 问题是否和 NP 问题相等，即 P = NP 是否成立，或者 P ≠ NP 是否成立。也就是说，科学家不确定如果一个问题的解能够很快被验证，那么这个问题的解是否也能很快地被求出来。

### NP-Complete 问题

NP-Complete 问题（亦称 NPC 问题，NP 完全问题），指的是那些在 NP 问题中最不像在 P 中的问题。也就是说，NPC 问题是那些看起来解最不可能很快求出来的问题。但同时他们的解一定能很快被验证。

![NPC](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Complexity_classes.svg/2560px-Complexity_classes.svg.png)

### NP-Hard 问题

[NP困难 | 维基百科](https://zh.wikipedia.org/wiki/NP困难)

如果所有 NP 问题都可以多项式时间内归约到某个问题，则称该问题为 NP 困难问题。

> 关于归约 (Reducibility)：
>
> 简单的说，一个问题 A 可以约化为问题 B 的含义是，可以用问题 B 的解法解决问题 A（个人感觉也就是说，问题 A 是 B 的一种特殊情况）。标准化的定义是，如果能找到一个变化法则，对任意一个 A 程序的输入，都能按照这个法则变换成 B 程序的输入，使两程序的输出相同，那么我们说，问题 A 可以约化为问题 B。
>
> 例如求解一元一次方程这个问题可以约化为求解一元二次方程，即可以令对应项系数不变，二次项的系数为 0，将 A 的问题的输入参数带入到 B 问题的求解程序去求解。
>
> 参考：[什么是P、NP、NPC、NP-Hard问题 | Ji Hu's Blog](https://hujichn.github.io/2016/07/14/什么是P、NP、NPC、NP-Hard问题/)

另外，约化还具有传递性，A 可以化约为 B，B 可以约化为 C，那么 A 也可以约化为 C。

![NP-Hard](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/P_np_np-complete_np-hard.svg/2560px-P_np_np-complete_np-hard.svg.png)

左边是在假设 P = NP 的情况下，描述 P，NP，NP 完全，以及 NP 困难问题之间关系的欧拉图。右边则是假设 P ≠ NP 的情况下三者之间关系的欧拉图。

注意，虽然 NP-Hard 问题的名字里带了 “NP” 俩字，但是 NP-Hard 问题并不一定是 NP 问题（即并不一定能很快验证 NP-Hard 问题的解，也就是说，NP-Hard 问题是那些不一定能很快求出解，也不一定能很快验证解的问题）。

NP-Hard 问题至少和 NPC 问题一样难。