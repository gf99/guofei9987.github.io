---
layout: post
title: 常见统计分布(1)
categories:
tags: 4_2_概率论
keywords:
description:
order: 9501
---

<h2>离散分布</h2>
<table>
<tr><th>名称</th><th>表示</th><th>概率分布</th><th>特征</th><th>性质</th><th>特点</th></tr>
<tr><th>0-1分布Bernoulli distribution</th></tr>


<tr><th>二项分布Binomial distribution</th>

<td>$X \sim b(n,p)$</td>

<td>$P(X=k)=\dbinom{n}{k}p^k(1-p)^{n-k}$</td>
<td>$EX=np$<br>$DX=np(1-p)$</td>
<td>可加性：
$b(n_1,p)+b(n_2,p)=b(n_1+n_2,p)$
</td>
<td>
0-1分布是一种特殊的二项分布<br>
二项分布是0个独立同分布的0-1分布的加和
</td>
</tr>


<tr>
<th>负二项分布
(帕斯卡分布)
</th>
<td></td>
<td>
$P(X=x,r,p)=\dbinom{x-1}{r-1}p^r(1-p)^{x-r}$<br>
$x \in [r,r+1,r+2,...,\infty]$
</td>
<td></td>
<td>
如果r=1，就是几何分布
</td>

<td>对于一系列独立同分布的实验，每次实验成功概率为p，实验直到r次成功为止，总实验次数的概率分布。</td>
</tr>
<tr>




<tr><th>泊松分布</th>

<td>$X\sim \pi(\lambda)$</td>
<td>
$P(X=k)=\dfrac{\lambda^k e^{-\lambda}}{k!}$<br>
$(k=0,1,2,...)$
</td>

<td>
$EX=\lambda$<br>
$DX=\lambda$
</td>

<td>
可加性：
$\pi(\lambda_1)+\pi(\lambda_2)=\pi(\lambda_1+\lambda_2)$
</td>
<td>泊松分布有广泛的应用，  <br>
某一服务设施一定时间内到达的人数<br>
电话交换机接到的呼叫次数<br>
汽车站台的后可人数<br>
机器出现的故障数<br>
自然灾害发生的次数<br>
一块产品上的缺陷数<br>
显微镜下单位分区内的细菌数<br>
某放射性物质单位时间发射的粒子数</td></tr>
<tr>
<table></table>



<h2>常用连续分布</h2>
<table>
<tr><th>名称</th><th>表示</th><th>概率分布</th><th>特征</th><th>性质</th><th>特点</th></tr>

<tr>
  <th>均匀分布Uniform distribution</th>
  <td>$X \sim U(n,p)$</td>
</tr>








<tr><th> 指数分布Exponential distribution</th>
<td></td>
<td>
$f(x)=\left \{ \begin{array}{ccc}
\lambda e^{-\lambda x}&x>0 \\
0&o/w
\end{array}\right.$
</td>
<td>
$EX=1/\lambda$  <br>
$DX=1/\lambda$
</td>

<td>
无记忆性(Memoryless)<br>

$$P(x>s+t \mid x>s )=P(x>t)$$
$$s,t>=0$$
</td>
</tr>

<tr>
<th>正态分布
Normal distribution
Gaussian distribution  </th>
<td>$X \sim N(\mu,\sigma^2)$</td>
<td>$f(x)=\dfrac{1}{\sqrt{2\pi}\sigma}e^{-\dfrac{(x-\mu)^2}{2 \sigma^2}}$</td>
<td>
$$EX=\mu$$
$$DX=\sigma^2$$
</td>

<td>
可加性：  <br>
$X_i \sim N(\mu_i,\sigma_i^2)$，并且相互独立  <br>
那么$\sum X_i \sim N(\sum\mu_i,\sum\sigma_i^2)$  <br>
</td>

<td>
如果同时满足以下两条：  <br>
$X_i \sim(i.i.d)N(\mu,\sigma^2)$ 独立同分布  <br>
$S^2=\dfrac{1}{n-1}\sum(X_i - \bar X)^2$  <br>
那么，  <br>
$\bar X \sim N(u,\dfrac{\sigma^2}{n})$  <br>
$\dfrac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$  <br>
$ES^2=\sigma^2$  <br>
$\bar X, S^2$相互独立  <br>
$\dfrac{\bar X-\mu}{S/\sqrt{n}} \sim t(n-1)$
</td>
<tr>
<table></table>

<br>
<br>
点击继续看...<a href='/2017/05/26/distribution1.html'>常见统计分布(2)</a>



## 引入Gamma Function
$\Gamma(\alpha)=\int_0^{+\infty} x^{\alpha-1}e^{-x}dx$

### Gamma Function的性质
$\Gamma(1)=1,\Gamma(0.5)=\sqrt{(\pi)}$  
$\Gamma(\alpha+1)=\alpha\Gamma(\alpha)$  
$\int_0^{-\infty}x^{\alpha-1}e^{-x}=\Gamma(\alpha)/\lambda^\alpha$  



## Gamma distribution
$$f(x)=\left \{ \begin{array}{ccc}
\dfrac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\beta x}&x \geq 0\\
0&others
\end{array}\right.$$

### 特征

$EX=\dfrac{\alpha}{\lambda}$  
$DX=\dfrac{\alpha}{\lambda^2}$  

### 性质

#### 指数分布

$Ga(1,\lambda)=exp(\lambda)$  

#### 卡方分布

$Ga(n/2,1/2)=\chi^2(n) \sim f(x)=\dfrac{1}{2^{n/2}\Gamma(n/2)}x^{n/2-1}e^{-x/2}(x>0)$   

顺便得到卡方分布的特征  
$E\chi^2=n$  
$DX=2n$  


## Beta distribution

$f=\dfrac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)+\Gamma(\beta)}x^{\alpha-1}(1-x)^{\beta-1}$  

### 特征
$EX^k=\dfrac{\Gamma(\alpha+k)\Gamma(\alpha+\beta)}{\Gamma(\alpha)+\Gamma(\alpha+\beta+k)}$  

$EX=\dfrac{\alpha}{\alpha+\beta}$   
$DX=\dfrac{\alpha\beta}{(\alpha+\beta)^2(\alpha+\beta+1)}$  

### 用途
在一些机器学习模型中，有时把先验分布定位beta distribution  

## Fisher Z
$f=\dfrac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}\dfrac{x^{a-1}}{(1+x)^{a+b}}$
### 特征

$EX^k=\dfrac{(a+k-1)(a+k-2)...a}{(b-1)(b-2)...(b-k)}$  
(k<b)

$EX=\dfrac{a}{b-1}$  
(b>1)  
$DX=\dfrac{a(a+b-1)}{(b-1)^2(b-2)}$  
(b>2)  

### 性质

#### F分布
如果，$X \sim Z(n_1/2,n_2/2)$  
那么，  
$Y=\dfrac{n_2}{n_1}X \sim F(n_1,n_2)$  

关于F分布，
$F=\dfrac{\chi^2(n_1)/n_1}{\chi^2(n_2)/n_2}$    

$EF=\dfrac{n_2}{n_2-2}$  

$DF=\dfrac{2n_2^2(n_1+n_2-2)}{n_1(n_2-2)(n_2-4)}$  
(n_2>4)  

## t distribution
$t=\dfrac{N}{\sqrt{\chi^2/n}}$    
$Et=0$  
$Dt=\dfrac{n}{n-2}$  


## The exponential family

$P(y;\eta)=b(y)exp(\eta^T T(y)-a(\eta))$  

### 性质
以下都是exponential family：  
- Bernoulli distribution
- Binomial distribution  
- poisson distribution
- normal distribution
