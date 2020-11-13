---
layout: post
title: 一切的源头
date: 2020-10-16
tag: [summary]
---

> Wir müssen wissen, wir werden wissen.<br/>
我们必须知道，我们必将知道。<br/>
—— 大卫·希尔伯特<br/><br/>
我看到了它，却不敢相信它。<br/>
—— 康托尔<br/><br/>
计算机是数学家一次失败思考的产物。<br/>
—— 无名氏

### 【提出问题和解决问题的人】
计算无处不在。

走进一个机房，在服务器排成的一道道墙之间，听着风扇的鼓噪，似乎能嗅出0和1在CPU和内存之间不间断的流动。从算筹算盘，到今天的计算机，我们用作计算的工具终于开始量到质的飞跃。计算机能做的事情越来越多，甚至超越了它们的制造者。上个世纪末，深蓝凭借前所未有的搜索和判断棋局的能力，成为第一台战胜人类国际象棋世界冠军的计算机，但它的胜利仍然仰仗于人类大师赋予的丰富国际象棋知识；而仅仅十余年后，Watson却已经能凭借自己的算法，先“理解”问题，然后有的放矢地在海量的数据库中寻找关联的答案。长此以往，工具将必在更多的方面超越它的制造者。而这一切，都来源于越来越精巧的计算。

计算似乎无所不能，宛如新的上帝。但即使是这位“上帝”，也逃不脱逻辑设定的界限。

第一位发现这一点的，便是图灵。

![Turing](https://mmbiz.qpic.cn/mmbiz/951TjTgiabkw6W502ykIhQPsFBa0nefU1CCk0s2siaFXribRakY5icpLY1sPW4Rg6ZBJWGgKOowCT4vEqrbUs0aGtA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


> ### [零]一切从逻辑开始

1900年的巴黎，在世纪交替之际，大卫·希尔伯特提出了他著名的23个问题。其中第二个问题——算术系统的相容性——正是他那雄心勃勃的“希尔伯特计划”的最后一步。这位数学界的巨人，打算让整个数学体系矗立在一个坚实的地基上，一劳永逸地解决所有关于对数学可靠性的种种疑问。一切都为了回答三个问题：
1. 数学是**完备**的(complete)吗？也就是说，面对那些正确的数学陈述，我们是否总能找出一个证明？数学真理是否总能被证明？
2. 数学是**一致**的(consistent)吗？也就是说，数学是否前后一致，不会得出某个数学陈述又对又不对的结论？数学是否没有内部矛盾？
3. 数学是**可判定**的(decidable)吗？也就是说，能够找到一种方法，仅仅通过机械化的计算，就能判定某个数学陈述是对是错？数学证明能否机械化？

希尔伯特明确提出这三个问题时，已是28年后的1928年。在这28年间，数学界在算术系统的相容性上没有多少进展。但希尔伯特没有等太久，仅仅三年后，哥德尔(Gödel)就得到了前两个问题的答案，尽管这个答案不是希尔伯特所希望看到的。

哥德尔告诉我们，
>**任何包含了算术的数学系统都不可能同时拥有完备性和一致性！**

其中，“算术”有着精确的含义，就是[皮亚诺公理](https://en.wikipedia.org/wiki/Peano_axioms)，一组描述了自然数的公理。
也就是说，如果一个数学系统包含了算术的话，要么它是自相矛盾的，要么存在一些命题，它们是真的，但我们却无法证明。这说明，希尔伯特的前两个问题不可能同时为真。

这就是著名的[哥德尔不完备性定理](https://en.wikipedia.org/wiki/G%C3%B6del%27s_incompleteness_theorems)所宣称的，与其说它回答了希尔伯特的前两个问题，不如说它阐述了为什么我们根本不可能解决这两个问题。

哥德尔的证明非常的长，达到了200多页纸，但其中很大的成分是用在了一些辅助性的工作上面，比如占据超过1/3纸张的是关于一个**形式系统如何映射到自然数**，也就是说，如何把一个形式系统中的所有公式都表示为自然数，并可以从一自然数反过来得出相应的公式。这其实就是**编码**，在我们现在看来是很显然的，因为一个程序就可以被编码成二进制数，反过来也可以**解码**。但是在当时这是一个全新的思想，也是最关键的辅助性工作之一，另一方面，这正是“**程序即数据**”的最初想法。

<!-- 很多现在看来很显然的观点当时却是非常创造性的思想！ -->

希尔伯特的前两个问题已经解决，只剩下最后一个问题。然而，如果一个数学系统不完备的话，它显然不可能是可判定的，因为机械化的计算本身也可以看成一种证明，而在一个不完备的系统中，真理不总能被证明。所以，最后一个问题只对完备的数学系统有意义。

所幸，完备的数学系统是存在的。同样是哥德尔，他证明了**一阶谓词演算**的逻辑系统是完备的，这被称为哥德尔完备性定理。[一阶谓词演算](https://en.wikipedia.org/wiki/First-order_logic)是一个比较弱的逻辑系统，在其中我们甚至不能有效唯一地描述算术。比如说，自然数系统符合皮亚诺公理的一阶版本，但它并不是唯一的，还有无数种所谓“非标准模型”同样符合这套一阶系统。在一阶谓词演算中，对于一套公理系统，如果一个命题在所有的模型中都正确，那么必定可以形式地证明这个命题，这就是一阶谓词演算的完备性。**在一阶谓词演算中，真理总能被证明。**

在这个弱得多的逻辑系统中，我们有了完备性，真的命题必定可以被证明。那么，它是不是可判定的？我们能不能找到一种**机械计算**的方法，判定其中数学陈述的对错？数学称述的真假，是否可判定的？这个问题，就是希尔伯特的可判定性问题。

### [一]复杂的简单机器——图灵机
在纽曼教授的数理逻辑课上，图灵(Turing)第一次听到希尔伯特的可判定性问题以及哥德尔不完备性定理。那是1935年的春天，他刚刚完成在剑桥国王学院的四年本科学习，以优异的成绩被选为学院研究员，正准备在数学界大显身手，数理逻辑自然而然吸引了他的兴趣。图灵清楚地意识到，解决可判定性问题的关键，在于对“**机械计算**”的严格定义。考究希尔伯特的原意，这个词大概意味着“**依照一定的有限的步骤，无需计算者的灵感就能完成的计算**”。这在没有电子计算机的当时，算是相当有想象力又不失准确的定义。

但图灵的想法更为单纯。什么是“机械计算”？机械计算就是**一台机器可以完成的计算**，这就是图灵的回答。
而这个可以完成计算的机器也就是后来以他名字命名的[图灵机](https://en.wikipedia.org/wiki/Turing_machine)。

图灵机的结构非常简单，只有以下几个结构
* **无限**的纸带(tape)
* 读写头(head)
* 状态寄存器(state register)
* **有限**状态表(table)

整台图灵机的秘密在于读写头的状态转移表，它指示着读写头的状态和当前读写头正对格子的符号如何变化。它只有一种非常简单的规则，就是“如果在状态A的读写头对着符号x，那么对当前格子写入符号y，将纸带左移一格/右移一格/保持不动，然后转移到状态B”。状态转移表就是由一系列这样的简单规则组成的。可以说，状态转移表就相当于图灵机的**源代码**。

实际上，我们平时笔算乘法的思维过程，跟一台图灵机的运转非常相似：在每个时刻，我们只将注意力集中在一个地方，根据已经读到的信息移动笔尖，在纸上写下符号；而指示我们写什么怎么写的，则是早已背好的九九乘法表，以及简单的加法。如果将一个笔算乘法的人看成一台图灵机，纸带就是用于记录的纸张，读写头就是这个人和他手上的笔，读写头的状态就是大脑的精神状态，而状态转移表则是笔算乘法的规则，包括九九表、列式的方法等等。这种模式似乎也适用于更复杂的机械计算任务。如此看来，图灵机虽然看起来简单，但它足以作为机械计算的定义。

既然图灵机如此简单，能不能将它“升级”，赋予更多的硬件和自由度，使它变得更强大呢？比如说，让它拥有多条纸带和对应的读写头，而纸带上也不再限定两种符号，而是三种四种甚至更多种符号？的确，放宽限制之后，在某种程度上，对于相同的任务我们能设计出更快的图灵机，但从本质上来说，“升级”后的图灵机能完成的任务，原来的图灵机也能完成，虽然也许会慢些。也就是说，这种“升级”在**可计算性**上并没有意义，放宽限制后的机器能计算的，原来的机器也能完成。既然计算能力没有质的变化，无论采取什么样的结构，用多少种符号，都无所谓。

图灵机的一大优点，就是它的简单。只要给出状态转移表，任何一个人都可以模拟一台图灵机的计算。对工程师而言，在现实中用机械建造一台图灵机也并非什么难事。对于程序员来说，写一个模拟图灵机的简单程序更是不在话下。

但如此简单的机器，能做的事情远远超出一般的想象。只要有足够长的纸带和足够好的耐心，今天的电脑能做的计算，一台精心设计的图灵机也能完成。诀窍在于，电脑中的电路是有限的，电路的状态也是有限的，我们可以用图灵机去模拟电脑中的电路状态。只要有足够长的纸带，那就可以模拟出足够大的寄存器、内存和硬盘；而CPU中的电路，虽然所有可能的状态极其多，但终究是有限的，可以用图灵机模拟，虽然这台图灵机的状态转移表将会有着令人头痛的大小，以及令人偏头痛的复杂程度。但是，从原则上来说，用图灵机模拟一台电脑是完全可能的，虽然每次“读写内存”时，读写头都需要花长得令人咋舌的时间在纸带上来回奔波。

也就是说，从原则上来说，只要配备适当的输入和输出设备，以及极其好的耐心，我们完全可以用图灵机上网、玩游戏甚至执行自己写的程序。特别地，存在一台特定的编写程序专用的图灵机T，我们可以在纸带上写程序，将它输入到T，然后T就能执行这个程序。那么，如果我们将可以模拟任意图灵机运行的程序（暂且把它称为程序P），写在纸带上输入到T中，让T去执行的话，原本的机器T就摇身一变，变成了一台可以根据输入的状态转移表来模拟任何一台图灵机的图灵机。

更精确地说，因为程序P的长度是有限的，我们可以将它直接写进原来机器的状态转移表，得到一台新的机器UTM。UTM会在纸带上读取两样东西：一台图灵机M的状态转移表的二进制编码，以及作为M的初始输入的纸带数据。然后，UTM会根据M的状态转移表和初始输入数据，在纸带上模拟M的运作过程。换言之，UTM是一台可以模拟任何图灵机的图灵机。它是所有机器的机器，所谓的通用图灵机(Universal Turing Machine)。当然，通用图灵机并不是唯一的，只要一台图灵机能完成根据状态转移表模拟任意图灵机的任务，它就是通用图灵机。

但单纯的直觉终究不能令人信服，数学家讲究的是逻辑和证明。而要证明通用图灵机的存在，最直接的方法莫过于直接给出一个通用图灵机的实例。尽管证明细节非常复杂，但是现代电子计算机的发展，相当于验证了通用图灵机的存在：每一台电脑都相当于一台通用图灵机。

通用图灵机的存在，从侧面说明了图灵机这个计算模型的强大之处：图灵机作为一类机器，其中一个特例就可以模拟整个类别中的任意一台机器，宛如能折射大千世界的一滴水珠。但在这种强大的背后，隐隐也暗藏着不安定的因素。哥德尔不完备性定理告诉我们，有时候越强大的数学理论，因为能表达的概念太多，甚至连理论的命题和证明都能表达，反而会导致不能被证明的真命题的存在。如果一个系统足以描述它自己，那魔法般的自指将是不可避免的。图灵机如此强大，它的其中一台就可以模拟所有图灵机，会不会导致不能用计算来回答的问题存在呢？

很不凑巧，答案是会。

### 无法计算的问题
在哥德尔不完备性定理的证明中，哥德尔构造了一个描述了本身不可证明性的自指命题，通过这个命题完成了他的证明。要想照葫芦画瓢的话，那些关于图灵机**本身**的问题，将会是很好的候补。
一个自然的问题即：
> 一台图灵机什么时候会停机呢？

更严格地说，会不会停机并不是图灵机本身的属性，它跟纸带的初始输入也有关系。对于同一台图灵机，不同的纸带输入也可能导致不同的结果和行为。比如说，我可以设计一台图灵机，它的任务只有一个：一步一步向右移动，寻找输入中的第一个1。如果输入纸带上全是0的话，那么，这台图灵机自然不会停止；但只要纸带上有一个1，那么它就会停止。所以，真正严谨的问题是：给定一台图灵机M以及一个输入I，如果我们将I输入M，然后让M开始运行，这时M是会不停运转下去，还是会在一段时间后停止？我们将这个问题称为**停机问题**。

初看起来，停机问题并不难。既然我们有通用图灵机这一强大的武器，那么只需要用它一步步模拟M在输入I上的计算过程就可以了。如果模拟过程在一段时间后停止了，我们当然可以得出“M在输入I上会停止”这个结论。问题是，在模拟过程停止之前，我们不可能知道整个计算过程到底是不会停止，它可能会在3分钟后停止，可能要等上十年八载，更有可能永远都不会停止。换句话说，用模拟的方法，我们只能知道某个程序在某个输入上会停止，但永远不能确定那些不停止的状况。所以说，单纯的模拟是不能解决停机问题的。

实际上，停机问题比我们想象中要复杂得多。
数学中的很多猜想，比如说3x+1猜想、黎曼猜想等，都可以转化为判断一个程序是否会停止的问题。如果存在一个程序，能判断所有可能的图灵机在所有可能的输入上是否会停止的话，那么只要利用这个程序，我们就能证明一大堆重要的数学猜想。我们可以说，停机问题比所有这些猜想更难更复杂，因为这些困难的数学猜想都不过是一般的停机问题的一个特例。如果停机问题可以被完全解决，我们能写出一个程序来判断任意图灵机是否会停机的话，那么相当多的数学家都要丢饭碗了。

停机问题如此复杂，机械的计算看起来没有足够的力量来完全解决它。停机问题似乎是不可计算的。但要想严格证明这个结论，还是要借助于那魔法般的**自指**。

用**反证法**，假设存在这样的图灵机P能够判断任意问题是否会停机。
考虑一台新的图灵机R，它通过调用P来判定“另一台图灵机M在其自身的编码<M>上”是否会停机，如果P判定M停机，则R陷入死循环（不停机）；若P判定M不停机，则R停机。

（这里可以这样想，R相当于现代计算机**完整的程序**，P相当于程序内部的**函数**，而M相当于**输入的数据**；这里也可以看出**程序和数据没有任何区别**，程序其实只是一种特殊的数据）

那么，考虑R在自身编码<R>上的运行情况。如果R停机的话，那说明P判断R不停机；而R不停机的话，那说明P判断R停机！
R既停机又不停机，这显然是不可能的。**自我指涉**结合**自我否定**直接导致了不可调和的矛盾！

如此便证明了那个能够完美解决停机问题的图灵机P根本不存在！也即图灵机是不可计算的。

实际上，图灵一开始并没有证明停机定理。他证明的是在一阶谓词演算这个完备的系统中，不存在这样的计算过程，可以在有限步运算内，判断这个系统中每个命题的真假。

也就是说
> **即便是完备的系统，它也是不可判定的！**

图灵确信自己解决了希尔伯特的判定问题后，很快将他的想法写成了论文，它的题目是：

《**论可计算数，及其在可判定性问题上的应用**》（On Computable Numbers, With an Application to the Entscheidungsproblem）

他将论文交给了数理逻辑课的纽曼教授。这篇论文在纽曼教授的桌上放了几个星期。当教授终于有时间细读图灵的论文时，一开始根本不敢相信希尔伯特的问题竟然能通过对如此简单的机器的论证而解决，但无懈可击的逻辑论证最终战胜了怀疑。这无疑是划时代的工作，最终埋葬了希尔伯特的宏伟计划。


《计算的极限》是一个系列，本文摘自其中的前几篇，后续有空再进行更新。
## 参考资料
1. 方弦，[计算的极限](https://mp.weixin.qq.com/s?__biz=MzUyMDQzNDM3MQ==&mid=2247484689&idx=1&sn=192957af3bad3ad3359faa691a48e56a&chksm=f9eb22bdce9cabab64080ca5c1ff59d6634322a906ad6f2249cfbba531b76a59a71c7a69cf4a&scene=21#wechat_redirect) ,http://songshuhui.net/archives/70194