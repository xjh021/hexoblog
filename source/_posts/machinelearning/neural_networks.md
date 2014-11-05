---
layout: post
date: 2014-10-31
title: neural network
category: machine learning
tags: [ml]
mathjax: true
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
## 神经元模型`models of a neuron`
**神经元**是神经网络操作的基本信息处理单位。他是人工神经网络的设计基础。我们在这里给出神经元模型的三种基本元素:
1. **突触**`synapses`或**连接链**`connecting`:每一个都由其*权值*`weight`或*强度*`strength`作为特征,具体来说，在连到神经元k的突触j上的输入信号 \\(x\_j\\) 被乘以k的突触权值 \\(w\_{kj}\\) 。注意突触权值 \\(w_{kj}\\)下标的写法很重要。第一个下标指正在研究的这个神经元，第二个下标指权值所在的突触的输入端。和人脑中的突触不一样，人工神经元的突触权值有一个范围，可以取正值也可以取负值。
2. **加法器**`adder`:用于求输入信号被神经元的相应突触加权的和，这个操作构成一个线性组合器`linear combiner`
3. **激活函数**`activation function`: 用来限制神经元输出振幅。由于它将输出信号压制(限制)到允许范围之内的一个定值，故而激活函数也称为**压制函数**`squashing function`。通常一个神经元输出的正常幅度范围可写成单闭区间[0, 1]或者另一种区间[-1, +1]。
![图5 神经元的非线性模型，标记为第k个神经元]({{BASE_PATH}}/image/machinelearning/neural_networks/imag1.png)
图中的神经元模型也包括一个外部偏置`bias`，记为\\(b\_k\\)。偏置\\(b\_k\\)的作用是根据其为正或为负，相应地增加或降低激活函数的网络输入。用数学术语表示，我们可以用如下一对方程描述图中神经元k:
$$u\_k=\sum\_{j=0}^m w\_{kj}x\_j$$                  (1)
$$y\_k=\varphi(u\_k + b\_k)$$                       (2)
其中\\(x\_1\\), \\(x\_2\\),..., \\(x\_m\\)是输入信号，\\(w\_{k1}\\),\\(w\_{k2}\\),...,\\(w\_{km}\\)是神经元\\(k\\)的突触权值，\\(u\_k\\)是输入信号的*线性组合器*的输出，\\(b\_k\\)为偏置，**激活函数**为\\(\varphi(\cdot)\\)，\\(y\_k\\)是神经元输出信号。偏置\\(b\_k\\)的作用是对图中模型的线性组合器的输出\\(u\_k\\)作*仿射变换*`affine transformation`，如下所示：
$$v\_k=u\_k + b\_k$$                                (3)
特别地，根据偏置\\(b\_k\\)取正或取负，神经元\\(k\\)的诱导局部域`induced local field`或激活点位`activation potential`\\(v\_k\\)和线性组合器输出\\(u\_k\\)的关系如图所示。注意到由于仿射变换的作用，\\(v\_k\\)与\\(u\_k\\)的图形不再经过原点。![图6 偏置产生的仿射变换，注意\(u_k\)=0时\(v_k\)=\(v_k\)=\(b_k\)]({{BASE_PATH}}/image/machinelearning/neural_networks/imag2.png)
偏置\\(b\_k\\)是人工神经元\\(k\\)的外部参数。我们可以像(2)中一样考虑它，同样可以结合式(1)和式(3)得到如下公式：
$$v\_k=\sum\_{j=0}^{m} w\_{kj}x\_{j}$$              (4)
$$y\_k=\varphi(v\_k)$$                              (5)
在(4)中，我们加上一个新的突触，其输入是
$$x\_0 =+ 1$$                                       (6)
权值是\\(w\_{k0} = b\_k\\)                          (7)
因此得到了神经元k的新模型。如图所示。在这个图中，偏置起两种作用:(1)添加新的固定输入+1; (2)添加新的等于偏置\\(b\_k\\)的突触权值。虽然形式上图5和图7的模型不相同，但在数学上它们是等价的。![图7 神经元的另一个非线性模型，\(w_{k0}\)代替了偏置\(b_k\)]({{BASE_PATH}}/image/machinelearning/neural_networks/imag3.png)

**激活函数的类型**`Types of Activation Function`
激活函数，记为\\(\varphi(v)\\)，通过诱导局部域\\(v\\)定义神经元输出。这里我们给出两种基本的激活函数:
1. *阀值函数*`Threshold Function`: 这种激活函数如图8a所示，可写为：
$$\varphi(x)=\left\\{
\begin{array}{rcl}
1 & & {v \geq 0}\\\
0 & & {v < 0}
\end{array}\right.$$            (9)
在工程文献中，这种函数一般称为Heaviside函数。相应地，在神经元k上使用这种阀值函数，其输出可表示为
$$y\_k=\left\\{
\begin{array}{rcl}
1 & & {v\_k \geq 0}\\\
0 & & {v\_k < 0}
\end{array}\right.$$
其中\\(v\_k\\)是神经元的诱导局部域，即
$$v\_k=\sum\_{j=1}^{m}w\_{kj}x\_j + b\_k$$         (10)
在神经计算中，这样的神经元在文献中称为McCulloch-Pitts模型，以纪念McCulloch and Pitts的开拓性工作。在模型中，如果神经元诱导局部域非负，则输出为1,否则0.这描述了McCulloch-Pitts模型的皆有或皆无特性(all-or-none property)
2. `sigmoid`函数。此函数的图形是“S”形的，在构造人工神经网络中是最常用的激活函数。它是严格递增函数，在线性和非线性行为之间显现出较好的平衡。`sigmoid`函数的一个例子是`logistic`函数，定义如下:
$$\varphi(v)=\frac{1}{1+exp(-av)}$$
其中a是sigmoid函数的*倾斜参数*。修改参数a就可以改变倾斜程度。如图8b所示。实际上，在原点的斜度等于a/4。在极限情况下，倾斜参数趋于无穷，sigmoid就变成了简单的阈值函数。阈值函数仅取值0或1,而sigmoid的值域是0到1的连续区间。还要注意到sigmoid函数是可微分的，而阈值函数不是。(可微性是神经网络理论的一个重要特征)
![图8a 阈值函数;b 具有不同倾斜参数a的sigmoid函数]({{BASE_PATH}}/image/machinelearning/neural_networks/imag4.png)
在式(8)、(11)中定义的激活函数的值域是0到+1。有时也期望激活函数的值域是-1到+1，这种情况下激活函数是诱导局部域的奇函数。具体来说，阈值函数的另一种形式是
$$\varphi(v)=\left\\{
\begin{array}{rcl}
1 & & {v > 0}\\\
0 & & {v = 0}\\\
-1 & & {v < 0}
\end{array}\right.$$            (12)
通常称之为signum函数。为了与sigmoid函数相对应，我们可以使用双曲正切函数
$$\varphi(v)=tanh(v)$$          (13)
它允许sigmoid型的激活函数取负值，这时候会产生比式(11)的logistic函数更好的实际利益。

**神经元的统计模型**`Stochastic Model of a Neuron`
图7的神经元模型是确定的，它的输入输出行为对所有的输入精确定义。但在一些神经网络的应用中，基于随机神经模型的分析更符合需求。使用一些解析处理方法，McCulloch-Pitts模型的激活函数用概率分布来实现。一个神经允许有两个可能的状态值+1或-1。一个神经元激发(即它的状态开关从“关”到“开”)是随机决定的。用x表示神经元的状态。\\(P(v)\\)表示激发的概率，其中v是诱导局部域。我们可以设定
$$x=\left\\{
\begin{array}{rcl}
+1 & & 概率为 P(v)\\\
-1 & & 概率为 1 - P(v)
\end{array}\right.$$
一个标准选择是sigmoid型的函数:
$$P(v)=\frac{1}{1 + exp(-v/T)}$$
其中T是伪温度(pseudotemperature)，用来控制激发中噪声水平即不确定性。但是，不管神经网络是生物的或人工的，T都不是神经网络的物理温度，认识到这一点很重要。进一步，正如所说明的一样，我们仅仅将T看作是一个控制表示突触噪声效果的热波动参数。注意当T趋于0时，式(14)和式(15)所描述的随机神经元就变为无噪声(即确定性)形式，也就是McCulloch-Pitts模型。

## 被看成有向图的神经网络`Neural networks viewed as directed graphs`
图 5 或 7 的方框图提供了构成人工神经元模型各个要素的功能描述。我们可以在不牺牲模型功能细节的条件下用信号流图来简化模型外观。Mason开发了线性网络的一套信号流图，并带有定义好的规则。神经元的非线性限制了它们在神经网络中的应用范围。不过信号流图在描述神经网络信号流时为我们提供了简洁的方法。
**信号流图**是一个由在一些特定的称为节点的点之间相连的有向连接(分支)组成的网络。一个典型的节点j有一个相应的节点信号\\(x\_j\\)。一个典型的有向连接从节点j开始，到k节点结束。它有相应的*传递函数*或*传递系数*以确定节点k的信号\\(y\_k\\)依赖于节点j的信号\\(x\_j\\)的方式。图形中各部分的信号流动遵循三条基本规则。

**规则1**: 信号仅仅沿着定义好的箭头方向在连接上流动
两种不同类型的连接可以区别开来:
  * *突触连接*: 它的行为由线性输入输出关系决定。具体来说，如图9a，节点信号\\(y\_k\\)由节点信号\\(x\_j\\)乘以突触权值\\(w\_{kj}\\)产生。
  * *激活连接*: 它的行为一般由非线性输入输出关系决定。如图9b所示，其中\\(\varphi(\cdot)\\)为非线性激活函数。

**规则2**: 节点信号等于经由连接进入的有关节点的所有信号的代数和。
这个规则通过如图9c所示的突触汇聚或扇入的情形来说明。

**规则3**: 节点信号沿每个外向连接向外传递，此时传递的信号完全独立于外向连接的传递函数
第三个规则通过如图9d所示的突触散发或扇出的情形来说明
图10比图7形式更简单，但可以包含所有的描绘的功能细节，注意，在两个图中，输入\\(x\_0=+1\\)和相关的突触权值\\(w\_{k0}=b\_k\\),其中\\(b\_k\\)是神经元k的偏置。![]({{BASE_PATH}}/image/machinelearning/neural_networks/imag5.png)
根据图10的信号流图所显示的神经元模型，我们可以给出一个神经网络的下列数学定义:
神经网络是由具有互相连接的突触节点和激活连接构成的有向图，具有4个主要特征:
  1. 每个神经元可以表示为一个组成线性的突触连接，一个外部应用偏置，以及可能的非线性激活连接。偏置由和一个固定为+1的输入连接的突触连接表示。(Each neuron is represented by a set of linear links, an externally applied bias, and a possibly nonlinear activation link. The bias is represented by a synaptic link connected to an input fixed at +1)
  2. 神经元的突触连接给它们相应的输入信号加权。(The synaptic links of a neuron weight their repective input signals)
  3. 输入信号的加权和构成该神经元的诱导局部域。(The weighted sum of the input signals defines the induced local field of the neuron in question)
  4. 激活连接压制神经元的诱导局部域产生输出。(The activation link squashes the induced local fields of the neuron to produce an output)

一如此定义的有向图是完全的，这是指它不仅仅描述了神经元的信号流，也描述了每个神经元内部的信号流。但是当我们的注意集中在神经元之间的信号流上时，可以使用这个图的一个简略形式，它省略神经元内部的信号流的细节。这样的有向图是局部完全的，它的特征是:
  1. 源节点向图提供输入信号
  2. 每个神经元由称为计算节点的单个节点表示
  3. 联接图中源节点和计算节点之间的通信连接没有权值，它们仅仅提供图中信号流的方向。

## 反馈`feedback`
当系统中的一个元素的输出能够部分地影响作用于该元素的输入，从而造成一个或多个围绕该系统进行信号传输的封闭路径时，我们说动态系统中存在着**反馈**。