---
title: "UCB-CS70_离散数学_个人笔记：Proofs 和 EECS 的联系及几种常见证明方法"
excerpt_separator: "离散数学中Proofs 和 EECS 的联系及几种常见证明方法"
categories:
  - UCB-CS70 离散数学
tags:
  - UCB-CS70
  - 离散数学
---
## 有趣的命题

在note1中，提出了两个关于“至少”和“至多”的命题：

1. There are **at least** three distinct integers *x* that satisfy *P(x)*.
2. 有 **最多** 三个不同的整数*x*这满足*p（x）*。 
 


对于这两个命题，可以分别用下面两个式子表达：

1. $ \exists x \exists y \exists z (x \neq y \land x \neq z \land y \neq z \land P(x) \land P(y) \land P(z))$

2. 
   $$
   \begin{align}
   &\exists x \exists y \exists z \forall d(P(d) \implies d = x \lor d = y \lor d = z)\\
   \equiv & \forall x \forall y \forall v \forall z \  (\ (x \neq y \land x \neq v \land  x \neq z \land y \neq v \land y \neq z \land v \neq z) \implies \neg (\ P(x) \land P(y) \land P(v) \land P(z)\ )\ )
   \end{align}
   $$

第一个命题很好理解。第二个命题则相对复杂。首先看(1)， 它指出：存在三个*x*、*y*、*z*，使得任意一个*d*，如果*P(d)*成立，那么*d=x*或*d=y*或*d=z*。命题2的另一种表达是(2)，它指出：对于任意的*x*、*y*、*v*、*z*，如果四个变量互不相同，那么*P(x)*、*P(y)*、*P(v)*、*P(z)*不同时成立。

可以想到，如果想要表达命题：恰好存在三个不同的整数*x*满足*P(x)*。它的数学表达就是把上面两个命题用“$\land$”连接起来。