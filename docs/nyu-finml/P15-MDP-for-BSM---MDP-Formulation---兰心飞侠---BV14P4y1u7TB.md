# P15：BSM的MDP - MDP表述 - 兰心飞侠 - BV14P4y1u7TB

现在，我们将把离散时间的BSL模型重新表述为市场决策过程，或者MDP模型。这意味着现在我们将以期权定价和对冲问题为视角来考虑问题。

![](img/274343fd76e2da59ee02438052df31c2_1.png)

作为离散时间的随机最优控制问题。

![](img/274343fd76e2da59ee02438052df31c2_3.png)

在这种情况下，被控制的系统将是一个对冲组合，控制变量将是该对冲组合中的股票头寸。这个问题将通过顺序最大化某些奖励来解决。正如我们稍后将看到的，这些奖励实际上是对冲组合的负方差与风险厌恶系数lambda的乘积，再加上一个微分项。

当以这种方式处理问题时，我们需要区分两种情况。第一种情况是当世界模型已知时。在这种情况下，我们可以使用动态规划方法（DP）或基于模型的强化学习来解决问题。第二种情况是当世界模型未知时。

在这种情况下，我们可以使用不依赖于世界模型的强化学习方法，直接依赖于数据样本来解决最优控制问题。这两种情况对应于两种不同的方式来求解贝尔曼最优性方程。因此，我们将从适合动态规划设置的表述开始。

然后我们将展示如何将其调整为强化学习的设置。我们首先在模型中定义状态变量。事实证明，虽然我们可以使用股票价格作为状态变量，但它们并不太方便使用，因为它们随着时间的推移不断上涨，导致动态不稳定。然而，这是可以轻松解决的。为此，我们引入了与股票价格ST相关的状态变量Xt，如下所示。

在这里的第一个方程式中。正如第二个方程所示，Xt仅仅是一个重新缩放的标准布朗运动。

![](img/274343fd76e2da59ee02438052df31c2_5.png)

第三个方程展示了如何通过添加确定性漂移并进行指数化计算股票价格ST与Xt的关系。现在，我想稍微偏离一下，做一个一般性的评论。原则上，我们可以考虑离散状态或连续状态的最优控制和金融问题。虽然连续状态的表述在实践中更为相关，但离散状态的表述也有其应用场景。

更容易理解。所以我想在这里评论一下，我们如何在模型表述中来回切换，介于连续状态和离散状态的MDP问题之间。

![](img/274343fd76e2da59ee02438052df31c2_7.png)

通常我们假设我们处理的是一个连续状态空间。但如果我们想要处理离散状态空间，我们可以简单地使用黑色显示模型的离散化版本，并直接在这个离散模型中构建最优对冲。这可以通过多种方式完成，其中一种最简单的方式是使用马尔可夫。

由Duann和Sonata在2001年提出的黑色显示模型的链式近似。尽管在我们的课程中不会使用这种离散化的动态，但我认为至少应该提到它，因为离散空间的MGP问题简化了一些事情。例如，我们可以为这样的系统使用简单的算法，如Q-learning，接下来我们会介绍。

我们将在下一课中讨论。所以让我们记住，我们在这里讨论的所有内容都可以在离散状态空间中进行表述，并且我们想要指定我们的MGP问题。首先是我们的新状态变量Xt，而不是St。我们将引入一个新的动作变量A(Xt)。原始的动作变量U(St)可以通过动作A(Xt)得到，前提是我们只是...

将Xt重新表达为St。接下来，为了描述世界不同状态下的动作A，我们引入了确定性策略pi的概念，该策略将状态Xt和时间t映射到动作A(t)，该动作由策略函数pi(t)和Xt给出。现在我们准备好为我们的模型指定贝尔曼方程。

我们已经介绍了值函数Vt，它是期权价格的负值，并且由这里的第一个方程给出。在第二行中，我将对应t'等于t的项在求和中拆分，这样现在求和的范围从t+1到C。但现在我们可以将方程中的最后一项替换为未来的值函数V(t+1)。

如果我们使用V(t+1)的定义以及这个关系中的三个范围项，我们可以得到这张幻灯片上的第二个方程。在这个方程中，我们还引入了一个离散时间折扣因子gamma，它与连续时间的无风险利率R和时间步长delta t相关。现在我们可以将第二个关系代入第一个方程。

这产生了值函数Vt的贝尔曼方程。这个贝尔曼方程就是这里的第一个方程，并且它具有标准形式，正如它应该的那样。我们这里模型的特定之处在于奖励函数R(X，T，A，T和X或T+1)的形式。这个函数在这里的第二个方程中给出。

正如这个方程所示，瞬时奖励Rt有两个贡献。第一个贡献与股票价格的变化成正比，并与股票中的头寸相乘。第二项与lambda成正比，是时间步t的负风险，衡量方式为H-程序在时间t的方差。方程中的第二行显示了它对动作的显式函数依赖性。

在这个第二个方程中，我将随机变量的方差表示为该变量的平方差值的期望。因此，这里出现的头部值，如pi头或s头，代表的是差值量。如果我们现在取这个瞬时奖励Rt的时间t期望，我们将得到这个方程中显示的期望奖励。现在，这个关系中有两件事特别突出。

首先，这个关系在行动变量AT上是二次的。因此，关于AT的优化变得更容易。

![](img/274343fd76e2da59ee02438052df31c2_9.png)

其次，这个公式显示，由第二项给出的二次风险奖励被作为一步期望纳入。因此，一旦我们对期望奖励进行了这种修改，我们可以直接使用标准的风险中立MDP方法来解决问题。你可能还记得在之前的课程中我们谈到过标准风险中立MDP公式对于金融问题的不适用性，因为后者关注的是风险。

现在我们在这里给出的公式提供了一个简单的解决方案。我们只需要在奖励函数中加入期望的二次风险，然后在风险中立估值中使用这样的风险调整奖励。值得注意的是，如果我们将这个表达式中的lambda取为零，那么我们。

可以看到，在这个极限下，期望奖励在AT上变为线性。所以，这意味着在这个极限下，如果我们专注于期望奖励，就不再有任何需要最大化的东西。虽然在MDP问题的意义上没有目标函数，但当然不意味着就无法找到任何最优的H。

最优的H仍然可以通过我们之前所做的二次风险最小化找到，但唯一的不同是，在这个极限下，风险最小化与任何与期权价格相关的目标函数之间不再有联系。换句话说，在lambda趋近于零的极限下，对冲和定价问题变得。

解耦。谢谢。[沉默]。
