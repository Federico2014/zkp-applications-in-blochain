# Finite Fields
Finite Field（有限域）$\mathbb{F}_p$为一种代数结构，是很多密码学协议的基础要素。若$a\in \mathbb{F}_p$，表示$a$的取值范围为$0$到$p-1$。若$p$为素数，则称$\mathbb{F}_p$为素数域。
域主要有2种类型：
- 加法域：满足$\mathbb{F}_p + \mathbb{F}_p \mapsto \mathbb{F}_p$
- 乘法域：满足$\mathbb{F}_p \times \mathbb{F}_p \mapsto \mathbb{F}_p$

域具有如下特性：
- 结合性：加法域有$a+(b+c)=(a+b)+c$；乘法域有$a\cdot (b\cdot c)=(a\cdot b)\cdot c$。
- 交换性：加法域有$a+b=b+a$；乘法域有$a\cdot b=b\cdot a$。
- 单位元：加法域的单位元为$0$，满足$a+0=a$；乘法域的单位元为$1$，满足$a\cdot 1=a$。
- 逆运算：加法域中，$a$的逆运算为$-a$，使得$a+(-a)=0$；乘法域中，对于$a\neq 0$的逆运算为$a^{-1}$，使得$a\cdot a^{-1}=1$。
- 分配性：对于乘法域，有$a\cdot (b+c)=(a\cdot b)+(a\cdot c)$。

