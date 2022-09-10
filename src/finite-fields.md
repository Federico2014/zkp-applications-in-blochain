# Finite Field
Finite Field（有限域）$\mathbb{F}_p$为一种代数结构，是很多密码学协议的基础要素。若$a\in \mathbb{F}_p$，表示$a$的取值范围为$0$到$p-1$。若$p$为素数，则称$\mathbb{F}_p$为素数域。

域内支持2种运算类型：[1]
- 加法运算：满足$\mathbb{F}_p + \mathbb{F}_p \mapsto \mathbb{F}_p$，即若$a\in\mathbb{F}_p, b\in\mathbb{F}_p$，有$(a+b\mod p)\in \mathbb{F}_p$。
- 乘法运算：满足$\mathbb{F}_p \times \mathbb{F}_p \mapsto \mathbb{F}_p$，即若$a\in\mathbb{F}_p, b\in\mathbb{F}_p$，有$(a\times b\mod p)\in \mathbb{F}_p$。

域内计算满足如下公理：
- 结合性：加法运算为$a+(b+c)=(a+b)+c$；乘法运算为$a\times (b\times c)=(a\times b)\times c$。
- 交换性：加法运算为$a+b=b+a$；乘法运算有$a\times b=b\times a$。
- 单位元：加法运算的单位元为$0$，满足$a+0=a$；乘法运算的单位元为$1$，满足$a\times 1=a$。
- 逆运算：加法运算中，$a$的逆运算为$-a$，使得$a+(-a)=0$；乘法运算中，对于$a\neq 0$的逆运算为$a^{-1}$，使得$a\times a^{-1}=1$。
- 分配性：对于乘法运算，有$a\times (b+c)=(a\times b)+(a\times c)$。

# Group
Group（群）$\mathbb{G}$比域更简单，仅支持$\cdot$一种运算类型。[2]

群可分为加法群和乘法群：
- 加法群：$[k] A = \underbrace{A + A + \cdots + A}_{k \text{ times}}$，其中$k$为非零值，将该运算称为scalar multiplication（标量乘法）。单位元为$0$或$\mathcal{O}$，即有$[-k]A+[k]A=\mathcal{O}$。
- 乘法群：$a^k = \underbrace{a \times a \times \cdots \times a}_{k \text{ times}}$，称为exponentiation（幂乘）运算。单位元为$1$，即有$a^{-k}\times a^{k}=1$。

群的order（阶）是指群内的总元素数，如有限域$\mathbb{F}_p$的非零元素——$\{1,2,3,\cdots,p-1\}$即可构成一个乘法群$\mathbb{F}^{\times}$，该群的阶为$p-1$。对于群内某元素$a$，使$a^k=1$或$[k]a=\mathcal{O}$的最小$k$值，称为$a$的order（阶）。

通常基于某个generating set（生成元集）运算可生成群内的任意元素。以乘法群为例，存在生成元集$g_1,\cdots,g_k$，通过选择不同的$a_1,\cdots,a_k$，经$\prod_{i=1}^{k}g_i^{a_i}$运算可生成群内的任意元素。每个群可有多个不同的生成元集。

若某群的生成元集仅有一个元素$g$组成，则称其为cyclic循环群。此时，可认为由$g$生成了整个群，$g$的阶即为该群的阶。



# 参考资料
[1] [维基百科——域的定义](https://en.wikipedia.org/wiki/Field_(mathematics)#Classic_definition)

[2] [ZCash Halo2文档](https://zcash.github.io/halo2/background/fields.html)