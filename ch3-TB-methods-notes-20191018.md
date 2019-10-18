# Notes for Chapter 3 in the book entitled "Computational Materials Science" by Ohno

注：

–	我个人没有TB背景，读这个chapter纯粹为了娱乐和自我科普。如果有错误的笔记，希望experts不吝赐教，鄙人一定知错就改。

–	下文中，如果我说作者，代指 Ohno sensei。如果说“我”，那就是我自己本人。

–	我阅读的是Ohno sensei写的第二版，2018年Springer出版。

–	当你在豆瓣笔记阅读我的笔记有困难的时候，你可以直接将我的笔记贴到markdown工具中来阅读，那样公式就会被很好地渲染。

-------------------

# 3.1 Introduction

作者在introduction主要介绍了TB存在的意义。ab initio或者first-principles theory, 特别是density functional theory（DFT for short)在当前固体计算中已经十分精确，但是在处理大体系（上百个atoms）的时候，计算很吃力。华人Wang Linwang的Pwmat和Guo Hong的RESCUE据称可以从头算上千个原子的，我体验过RESCUE，在大体系上计算效率要高于VASP，但是pseudopotential的缺乏，对magnetic ordering的poor description似的这种方法的广泛应用还是需要时间的。对于那些由经典力学统治的体系，molecular dynamics simulations不失为一种好方法，但是如果要既快又省得知道这种体系的电子结构的时候，TB这种根植于量子力学的方法就有意义了。在量子化学领域，有一种比较省力的方法，MNDO (modified neglect of differential overlap) ，它非常接近Hartree Forck方法，但是采用了很多半经验的公式，因此求解Hamiltonian和overlap matrix elements就省力了许多。

TB接近这种MNDO方法，因为他们的matrix elements都需要通过ab initio计算数据拟合得到的公式中得到。在TB中，通常需要利用原子轨道线性组合方法（LCAO）。尽管这些basis function的确切解析形式是未知的，不过我们可以计算这个basis中的matrix elements和Hamiltonian，然后电子的本征值和总能就可以从这些martix elements中推导得到。此外，利用Hellmann–Feynman theorem，通过把能量对原子坐标求导，我们还能够得到作用在原子上的Hellmann–Feynman forces。有了力，我们就可以做一些其他的事情了。例如采用Verlet method或者predictor–corrector method来做分子动力学模拟。在这种类型的模拟中，计算时间主要花在了寻找本征值和哈密顿量的本征矢，以及计算粒子的力。

作者在接下来的小节中将主要描述紧束缚理论的原理，计算specturm和forces的方法，以及如何做parametrization。作者也提供了一些案例来将TB方法应用到简单的半导体中，并在本章最后来概述TB方法的优点和缺点。

# 3.2 Tight-Binding Formalism

TB原理可由Kohn-Sham类型的Hamiltonian中推导得到，这意味着它是一种单体的平均长理论。这里稍微引申一下讲讲Kohn-Shame方程。它是一种effective single-body theory，但是不是说这种方法就考虑不了many body effect，它的计算还是可以通过exchange functional term包含在里面，更多的信息，我个人认为可以参看Capelle的[A Bird’s-Eye View of Density-Functional Theory](https://arxiv.org/pdf/cond-mat/0211443.pdf)中的第四节：DFT as an effective single-body theory: The Kohn-Sham equations。

回到我们的TB formalism。Slater和Koster是TB方法的两位伟大的设计师，这里我不禁想要歌颂一下他们，在密度泛函理论和计算机远不普及的时代，他们开拓的这一方法成功地解决了许多简单晶体的电子结构计算问题。尽管随着时代的发展，精确的DFT计算逐渐取代了TB，但是近些年人们对于大体系计算的需求渐增，使得这种方法又得以被人们重视和利用。