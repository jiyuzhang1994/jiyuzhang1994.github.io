---
layout: default
permalink: /derandlogspace/
title: Derandomizing Log Space vs. Fooling Log Space
tags: Study Diary
---
上周的ToC reading group上讲到了最近的一篇ITCS paper [CHLT18]，主要是讲怎么通过functions with bounded second-level fourier tail， 把它转化成针对那个function family的PRG(Pseudorandom Generator)。 特别地通过这篇paper的一个conjecture，我们可以得到AC[$\oplus$]的PRG。 在meeting上，Michael就问了AC[$\oplus$]是不是在BPL里，原因是我们知道怎么构建对于BPL的PRG(而且并不optimal，且仅仅是nonuniformly），但我们好像不知道怎么去fool所有的log space 的计算（实际上应该是L/poly)，而AC[$\oplus$]看起来是不像是在BPL里面的。因为之前对pseudorandomness for space-bounded computation了解还比较少，于是我也就找了一些相关的笔记看，这次就主要是想写下derandomizing BPL 和 fool L/poly的区别。

首先我们来看看L/poly。回想一下log space的图灵机是怎么运行的：我们有一个只读（read-only)的输入带（input tape)，还有一个可以读写（read-write）的只有log长度的工作带（work tape），和一个write-only的 output tape。对于一个长度为n的input，log space的图灵机可以在input tape 上来回读取输入，然后在 $\log n$ space的work tape上进行计算，最后在output tape上输出结果。能被这种图灵机计算的complexity class我们称之为 L。注意在这个计算过程中图灵机的每一步计算的配置（configuration)都可以由它的 1.读取头在input tape上的位置 2.work tape上的内容 3.读取头里的状态 这个triple表示。那么什么是L/poly？回想P/poly是用一个combinatorial model，也就是circuits来non-uniformly模拟polynomial time的图灵机，这个circuit有polynomial size的description，也就相当于是一个图灵机有了polynomial size的advice，所以才写作P/poly。那么同样地，我们想用一个polynomial size 的 combinatorial model来模拟log space的图灵机的计算并称之为L/poly。这次我们用的model是branching program。  

一个branching program是一个acyclic graph。在这个图里面每一个node都有个label是 $x_i$，表示input的第$i$个variable。这个图有一个start node和两个end node。每个node（除了end node）都有两个ouput edge，一个由1标注，一个由0标注：如果node的variable的值是1，就沿1的边走，如果是0，就沿0的边走。 两个end nodes一个由0标注一个由1标注。 这样我们大概就可以想象一个长度为n的输入在这个model上是怎么计算的了：对于此输入，我们从start node看起，如果node上标注的variable的值为1的话就沿标注1的边看下一个node，如果是0就沿标注0的边看下一个node，沿着这样的一条计算路径（path)后最终会落到要么是0或是1的end nodes上。输出最后到达的end node的值。这个branching program的size就是这个图的node的数量

要说明为什么一个polynomial size 的 branching program和L/poly 图灵机计算一样，试想我们上面说的一个configuration，是不是正好就和这个branching program里的一个node一样：node的标注（label）就是图灵机在input tape上的读取头的位置；我们一共有polynomial个nodes，正好对应work tape上的$2^{\log n}=poly(n)$种不同的binary string。这样就证明了poly-size branching program能计算L/poly。 另一个方向就仅仅是log space machine被给予这个branching program的description(which is poly-size)。

Fooling L/poly 就是指我们要构造一个PRG G，使得对于任意的branching program B，$|Pr[B(G(U_z))=1]-Pr[B(U_m)=1]|<\epsilon$。也就是说我们要把一个长度为z的truly random string，stretch成一个长度为|G(U_z)|=m 的 string $G(U_z)$，使得所有的distinguisher B都不能区别它与长度为m的uniformly random string的区别。  


再来看看BPL, 为了model BPL non-uniformly我们用一个特殊的branching program -- Read-Once Branching Program。回想一下log space的



**References**  
[CHLT18]Eshan Chattopadhyay. Pooya Hatami. Shachar Lovett. Avishay Tal. Pseudorandom generators from the second Fourier level and applications to AC0 with parity gates
 