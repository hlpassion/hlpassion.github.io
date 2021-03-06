---
layout:    post
title:    "Machine Learning Yearning 翻译，动手写起来!"
subtitle:    "chapter05-chapter07"
date:    2017-06-05
author:    "Heron"
header-img:    "img/post-bg-mly.webp"
header-mask:    0.3
catalog:    true
tags:
    - 博客
    - 翻译
    - 机器学习
---
## 5.Your development and test sets
&emsp;&emsp;让我们回到早些时候提到的从图片中识别猫的例子：运行一个移动应用，用户上传许多不同事物的照片到应用中。你想要自动的找出猫的图片。
&emsp;&emsp;你的团队通过不同的网站下载了大量的训练集数据，猫的图片(正例),不是猫的图片(负例)。他们将数据分成70%训练集和30%测试集。使用这些数据，他们建立了一个在测试集上表现良好的猫图片检测器。
&emsp;&emsp;但是当你将这个分类器应用在移动应用中，你发现分类器的表现非常差。
![black](https://cloud.githubusercontent.com/assets/12608255/26761096/cb44d708-495b-11e7-891a-b965cd38bb51.jpg)

&emsp;&emsp;发生了什么?
&emsp;&emsp;你发现用户上传的图片与你们下载的训练集数据不同：用户上传的图片是通过手机拍摄，分辨率低、模糊并且光线也不好。然后你的训练和测试集是由网络图片构成，你的算法在你关心的智能手机拍摄的图片上的泛化性弱。
&emsp;&emsp;在现代大数据之前，在机器学习中使用随机的70%/30%将数据分成训练/测试集是一个通用的规则。这样的经验依然是可以起作用的，但是在训练数据(网络图片)和测试数据(手机图片)不均衡的情况下这就是一个坏的想法。
&emsp;&emsp;我们通常定义：
- 训练集--运行在算法上的数据
- 开发集(我认为就是验证集)--用来调和参数、选择特征和决定一些学习算法其他的一些属性。
- 测试集--用来验证算法的表现情况，但是不用作任何算法参数的调整。

&emsp;&emsp;当你定义了开发集和测试集，你的团队将尝试各种想法，比如不同的学习算法参数，来看看算法的表现结果。开发集和测试集可以让你的团队快速的看到算法的表现情况。
&emsp;&emsp;换言之，开发集和测试集用来指导你的团队对机器学习系统进行最重要的改变。
&emsp;&emsp;因此，你应该按照下面来做：
  **Choose dev and test sets to reflect data you expect to get in the future and want to do well on.**
自行理解，感觉翻译不出深度。
&emsp;&emsp;换言之，你的测试集不应该简单的设置成数据的30%，尤其是你的期望数据和你训练的数据不同的时候。(这部分翻译的一般。)
&emsp;&emsp;如果你还没有开发一个APP，你也可能还没有用户，因此我们也不能知道未来的数据表现。比如，让你的朋友用手机拍照发给你。一旦app完成，你可以更新的开发/测试集使用真实的用户数据。
&emsp;&emsp;如果你真的没有任何方式来获取你未来你期望的数据，可能你可以使用网络图片开始。但是你应该注意这样会导致你的系统的泛化能力不是很好。
&emsp;&emsp;我们需要判断决定到底要使用多少开发集和测试集。但是不要假设你的训练分布和测试分布必须是相同的。尝试去挑选能反映你最终想要表现很好的数据作为测试样本，而不是你碰巧有的任何数据。

## 6.Your dev and test sets should come from the same distribution
&emsp;&emsp;你已经有了应用中的图片数据并分割成了四个区域，美国、中国、印度、其他。用这些数据组成开发集和测试集，我们可以随机定义区域中的两个作为开发集，另外两个作为
测试集，是么？比如美国和印度作为开发集；中国和其他作为测试集。
![image](https://cloud.githubusercontent.com/assets/12608255/26761963/9684aa88-496b-11e7-897f-490e55621b17.png)
&emsp;&emsp;一旦你定义了开发集和测试集，你的团队将会关注在提高开发集的表现。因此，开发集应该反映你真正想要提高性能的任务：在四个区域上都应该做的很好，而不仅仅是在两个区域上表现的好。
&emsp;&emsp;开发集和测试集分布不同时还有第二个问题：有可能你的团队建立一个在开发集上表现很好的东西，但是却发现它在测试集上表现很差。我曾经在很多挫折和浪费了努力后看到过这个结果。要避免这件事发生在你身上。:blush:
&emsp;&emsp;例如，假设你团队开发的系统在开发集上表现很好但是在测试集上却相反。如果你的开发集和测试集来自相同的分布，那么你就会有非常清晰的诊断是哪里出了错：在开发集上发生了过拟合。最显而易见的解决方法就是获取更多的开发集数据。
&emsp;&emsp;但是如果开发集和测试集来自不同的分布，这样你的选择就不清晰了。可能是几种情况出了错：
1. 开发集数据产生了过拟合。
2. 测试集的数据比开发集数据要更加复杂。因此你的算法可能做得和预期的一样好，因此也没法做出进一步的改进。
3. 测试集数据并不是很难区分，但是和开发集分布不同。因此，在开发集上表现好但是在测试集上表现的不好。这种情况下，对开发集做许多努力来提高算法的表现都是在浪费努力。

&emsp;&emsp;机器学习应用已经很难了。使用错误的开发集和测试集引入了额外的关于是否改进开发集分布或提高测试集表现性能的不确定性。使用错误的开发集和测试集使得我们很难确定到底什么work了什么不work，因此更难确定选择的优先级。
&emsp;&emsp;如果你面对的是第三方基准测试（benchmark）的问题，它们的创造者可能已经指定了开发集和测试集来自不同的分布。这种情况下，运气，而不是技能，将会对算法性能产生更大的影响。当然，开发那些能够在一个分布上训练并能很好地泛化到另一个分布的学习算法是一个很重要的研究方向。但如果你的目标是开发能够在特定机器学习应用中得到不错效果的系统的话，我建议你选择开发集和测试集服从同一分布。这将使您的团队更有效率。
## 7. How large do the dev/test sets need to be?
&emsp;&emsp;开发集应该足够大，大到可以区分你所尝试的不同算法之间的差异。比如，分类器A的精度是90%，分类器B的精度是90.1%，这样只有100个样本数据的开发集是不能检测出这0.1%的差异。与我所见到过得机器学习算法相比，100个样本数据的开发集很小。常见的开发集在1000到10,000个样本数据。使用10,000个样本数据你将有机会检测到那0.1%的性能提升。
&emsp;&emsp;对于成熟、重要的应用--比如广告、搜索引擎、商品推荐-我看到许多团队为了0.01%的性能提升而努力，因为这将直接影响到公司的收益。在这个例子中，为了检测更小的性能提升，开发集数据可能要比10,000多的多。
  那么测试集的大小呢？它也应该足够大，大到在你对系统整体性能做评估的时候可以提供更高的可信度。比较流行的启发是使用你数据的30%作为测试集。当你拥有适当的样本数据(100-10,000)时，它可以work的很好。但是在大数据时代，我们的机器学习问题有时会有超过数十亿的样本数据，分配给开发/测试集的数据比例一直在减小，但是开发/测试集的绝对数量一直在增加。在给开发/测试集分配数据时，没必要超出评估算法性能所需要的数据量。