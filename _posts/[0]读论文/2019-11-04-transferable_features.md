---
layout: post
title: 【读论文】How transferable are features in deep neural networks
categories:
tags: 0_读论文
keywords:
description:
order: 1
---



- **How transferable are features in deep neural networks?** (2014), J. Yosinski et al. [[pdf]](http://papers.nips.cc/paper/5347-how-transferable-are-features-in-deep-neural-networks.pdf)
- 镜像地址 [pdf](https://github.com/guofei9987/pictures_for_blog/tree/master/papers)

## abstract&introduction

很多做图像 DNN 都有这么一个特点：第一层很像 Gabor filters and color blobs

Transferability 受两种 issue 的负面影响
- 做一个任务的网络，用到另一个任务上，而两个任务目的有差别
- 优化困难。


很多做图像 DNN 都有这么一个特点：第一层很像 Gabor filters and color blobs。这些DNN如此相似，以至于如果看到不相似的，人们第一怀疑是不是参数给错了，或者代码bug了。  
而且这一现象普遍存在与不同的 datasets，training objectives，甚至是不同的网络：(supervised image classification, unsupervised density learning, unsuperived learning of sparse represetations)。

除此之外，网络的最后一层显然没有这个现象，比如做分类，最后一层是softmax层，那么每一个 output 对应一个你事先规定好的label。  
我们把有这个现象叫做 **general**，例如前面提到的神经网络第一层。没有这个现象叫做 **specific**，例如前面提到的神经网络最后一层。  

自然想要提出几个问题：
- general 和 specific 的程度如何量化（quantify）
- transition 是在一层出现的，还是在多层出现的（Does the transition occur suddenly at a single layer, or is it spread out over several layers?）
- transition 在哪里出现：第一层附近，中间，还是最后几层？

对这些问题的研究，有助于 transfer learning 的研究。

### transfer learning
文章第二页介绍了 transfer learning 的做法，不多说。注意两个叫法：先训练的数据集叫做 base dataset，后训练的数据集叫做 target dataset

显然，如果target datasets 的数量远少于 base datasets ，transfer learning 是一个很好用的手段，且能防止过拟合。

transfer learning 可以这样：
-保留前n层，后面的层可以重新设计（也可以用base network的），然后后面的层初始化，然后
- 一起训练（fine-tune ）。
- 前n层不参与训练（frozen）
