---
layout: post
title: 【论文笔记】Learning and transferring mid-Level image representations CNN
categories:
tags: 0_读论文
keywords:
description:
order: 4
---


- **Learning and transferring mid-Level image representations using convolutional neural networks** (2014), M. Oquab et al. [[pdf]](http://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Oquab_Learning_and_Transferring_2014_CVPR_paper.pdf)
- 镜像地址 [pdf](https://github.com/guofei9987/pictures_for_blog/tree/master/papers)

## abstract&introduction
CNN最近获得了成功，这得益于它能学到大量的 mid-Level image representations  

然而，CNN需要巨大的参数量，以及大量的标记图片，限制了CNN在有限训练集上的应用。

这篇论文展示了，可以在一个训练集上训练得到数据，然后 transfer 到其它训练集。

## 相关进展
名词：原本的训练叫做 `source task`，后来的训练叫做 `target task`
### 1. transfer learning 有一些用法：
- 弥补某些类别数据不足的问题
- 同一类别，source domain和target domain 数据情况不一致（例如图片的亮度、背景和视角不一样）

### 2. visual object classification
例如，一些 clustering 方法，（K-means，GMM），Histogram encoding，spatial pooling，等。

这些方法实践证明挺好，但还不清楚是否是最优的方案。
### 3. Deap Learning
不多说

## transfering CNN weights
CNN 有 60M parameters，所以往往需要 transfer learning 来训练它

- 显式的做一个映射，把 source task 的 label 映射到 target task
- 借助一些 sliding window detectors 方法，下面会详细说


举例来说，你的 source task 是识别不同狗的种类，而 target task 仅需要把狗识别出来。  
那么具体做法是，把最后一层softmax层（记为FC8）拿掉，然后加上一层ReLU（FCa）和一层softmax（FCb）
